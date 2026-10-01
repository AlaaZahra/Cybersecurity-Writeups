# Cybersecurity Write-ups

![Focus](https://img.shields.io/badge/Focus-Blue%20Team%20%26%20SOC-0A66C2)
![Domains](https://img.shields.io/badge/Domains-Network%20%7C%20Cloud%20%7C%20Web-6F42C1)
![Tools](https://img.shields.io/badge/Tools-Wireshark%20%7C%20Splunk%20%7C%20AWS-1679A7)
![Status](https://img.shields.io/badge/Status-Active-brightgreen)

## Welcome

Welcome to my cybersecurity learning and investigation portfolio.

This repository documents my hands-on work across network forensics, cloud security, web-attack analysis, incident investigation, and Capture The Flag challenges. Each write-up focuses on the investigation process, evidence, tools, attack-chain reconstruction, and defensive recommendations—not only the final answers.

> My current focus is developing practical SOC and DFIR skills through evidence-driven investigations.

---

## Featured Investigations

### CyberDefenders

| Lab | Investigation | Category | Primary Tool |
|---|---|---|---|
| [AWSRaid](./CyberDefenders/AWSRaid/) | AWS CloudTrail incident investigation covering suspicious console access, S3 activity, bucket exposure, and IAM persistence | Cloud Forensics | Splunk |
| [JetBrains](./CyberDefenders/JetBrains/) | TeamCity compromise involving authentication bypass, administrator creation, malicious plugin upload, web-shell execution, and a container-escape attempt | Network Forensics | Wireshark |
| [PoisonedCredentials](./CyberDefenders/PoisonedCredentials/) | NBNS name-resolution poisoning followed by SMB and NTLM credential exposure | Network Forensics | Wireshark |
| [RetailBreach](./CyberDefenders/RetailBreach/) | Web compromise involving directory discovery, stored XSS, session hijacking, and path traversal | Network Forensics / Web Attacks | Wireshark |
| [PsExec Hunt](./CyberDefenders/PsExecHunt/) | SMB lateral movement involving NTLM authentication, administrative shares, service execution, and PsExec named pipes | Network Forensics | Wireshark |

➡️ [View all CyberDefenders investigations](./CyberDefenders/)

---

## Platforms

- [CyberDefenders](./CyberDefenders/) — Blue Team, SOC, cloud, and network-forensics investigations.
- [Hack The Box](./HackTheBox/) — Hands-on penetration-testing and security labs.
- [TryHackMe](./TryHackMe/) — Guided cybersecurity learning paths and practical rooms.

---

## Focus Areas

- Security Operations and incident investigation.
- Network and digital forensics.
- Cloud security and AWS CloudTrail analysis.
- Windows authentication and lateral movement.
- Web application attack investigation.
- Threat hunting and attack-chain reconstruction.
- Indicators of compromise extraction.
- MITRE ATT&CK mapping.
- Detection and remediation planning.

---

## Repository Structure

```text
Cybersecurity-Writeups/
├── README.md
├── CyberDefenders/
│   ├── README.md
│   ├── AWSRaid/
│   ├── JetBrains/
│   ├── PoisonedCredentials/
│   ├── PsExecHunt/
│   └── RetailBreach/
├── HackTheBox/
└── TryHackMe/
```

Each completed lab normally contains:

```text
LabName/
├── README.md
├── Investigation-Report.pdf
└── images/
```

---

## Investigation and Documentation Approach

Each investigation follows a structured process:

1. Understand the scenario and define the objectives.
2. Establish a baseline of the supplied evidence.
3. Identify suspicious hosts, users, IP addresses, events, or endpoints.
4. Build focused Wireshark filters, Splunk searches, or analysis commands.
5. Validate every finding using packet or log evidence.
6. Reconstruct the attack chain in chronological order.
7. Extract indicators of compromise.
8. Map confirmed behavior to MITRE ATT&CK.
9. Document detection and remediation recommendations.
10. Record evidence limitations instead of making unsupported assumptions.

Each completed write-up may include:

- A detailed `README.md`.
- Screenshots placed below the relevant filter or query.
- Explanations of why each analysis step was performed.
- A reconstructed attack timeline.
- An answer-and-evidence matrix.
- Indicators of compromise.
- MITRE ATT&CK mappings.
- Detection and remediation recommendations.
- A downloadable PDF investigation report.

---

## Tools and Technologies

| Area | Tools and Technologies |
|---|---|
| Network Forensics | Wireshark, TCP/IP, HTTP, SMB2, NTLMSSP, DCE/RPC |
| SIEM and Log Analysis | Splunk, SPL |
| Cloud Investigation | AWS CloudTrail, IAM, S3 |
| Threat Analysis | MITRE ATT&CK, IOC extraction, attack-chain reconstruction |
| Reporting | Markdown, Git, GitHub, Microsoft Word, PDF |

---

## Skills Demonstrated

- Packet and protocol analysis.
- SIEM query construction.
- Cloud audit-log investigation.
- Authentication and session-compromise analysis.
- Windows lateral-movement investigation.
- Web-attack and vulnerability analysis.
- Evidence-based incident reporting.
- Technical documentation with reproducible steps.
- Defensive detection and remediation planning.

---

## Disclaimer

All write-ups are based on intentionally vulnerable systems, legal training platforms, and authorized lab environments. The content is provided for educational and defensive-security purposes only. No real-world systems were targeted.

---

**Author:** Alaa Zahra  
**Track:** SOC Analyst / Cloud and Network Forensics

⭐ If you find these write-ups useful, feel free to star the repository.

