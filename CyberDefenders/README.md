# CyberDefenders — Blue Team Investigations

![Platform](https://img.shields.io/badge/Platform-CyberDefenders-1679A7)
![Focus](https://img.shields.io/badge/Focus-Blue%20Team%20%26%20DFIR-0A66C2)
![Investigations](https://img.shields.io/badge/Investigations-9-blueviolet)
![Status](https://img.shields.io/badge/Status-Active-brightgreen)

This directory contains my CyberDefenders lab investigations and technical write-ups.

Each investigation documents the complete analytical process: understanding the scenario, examining the available evidence, developing and validating hypotheses, reconstructing the attack chain, and reporting the confirmed findings.

## Investigation Portfolio

| Lab | Category | Investigation Focus | Main Tools |
|---|---|---|---|
| [AWSRaid](./AWSRaid/) | Cloud Forensics | AWS account compromise, S3 access, configuration changes, and IAM persistence | Splunk, AWS CloudTrail |
| [FileShare](./FileShare/) | Network Forensics | Name-resolution poisoning, NTLM authentication, and credential interception | Wireshark |
| [GrabThePhisher](./GrabThePhisher/) | Malware Analysis | Cryptocurrency phishing-kit analysis and Telegram exfiltration | Kali Linux, Static Analysis |
| [IcedID](./IcedID/) | Threat Intelligence | Malicious-document analysis, payload infrastructure, and threat attribution | VirusTotal, Malpedia, Triage |
| [JetBrains](./JetBrains/) | Network Forensics | TeamCity compromise, malicious plugin deployment, and command execution | Wireshark |
| [PsExec Hunt](./PsExecHunt/) | Network Forensics | SMB authentication, PsExec execution, and lateral movement | Wireshark |
| [RetailBreach](./RetailBreach/) | Network Forensics | Web enumeration, stored XSS, session theft, and path traversal | Wireshark |
| [Tomcat Takeover](./TomcatTakeover/) | Network Forensics | Port scanning, brute-force authentication, WAR deployment, and reverse shell | Wireshark |
| [Yellow Cockatoo](./YellowCockatoo/) | Threat Intelligence | SolarMarker malware identification, dropped files, and C2 infrastructure | VirusTotal, Hybrid Analysis, Red Canary |

## Investigations

### AWSRaid — AWS CloudTrail Incident Investigation

Investigation of suspicious activity within an AWS environment using CloudTrail logs ingested into Splunk.

The analysis covers:

- Reviewing the distribution of AWS service events.
- Investigating failed and successful AWS Console login attempts.
- Identifying the compromised IAM account.
- Following the compromised identity across later API calls.
- Examining S3 bucket and object access.
- Detecting changes to S3 public-access settings.
- Identifying the creation of a persistence account.
- Confirming that the new account was added to a privileged IAM group.
- Reconstructing the cloud attack timeline.

[View the AWSRaid investigation](./AWSRaid/)

---

### FileShare — Name-Resolution Poisoning Investigation

Network-forensics investigation of suspicious name-resolution and SMB authentication traffic.

The analysis covers:

- Establishing a protocol and endpoint baseline.
- Examining multicast and local name-resolution requests.
- Identifying the system that responded to a name-resolution query.
- Distinguishing the requesting victim from the responding system.
- Following the subsequent SMB connection.
- Examining SMB2 Session Setup traffic.
- Identifying NTLM authentication information.
- Extracting the authenticated username and workstation name.
- Correlating name-resolution poisoning with credential interception.
- Separating confirmed packet evidence from investigative assumptions.

[View the FileShare investigation](./FileShare/)

---

### GrabThePhisher — Phishing Kit Investigation

Static investigation of a cryptocurrency-wallet phishing kit recovered from an archived evidence package.

The analysis covers:

- Safely extracting and inventorying the supplied evidence.
- Identifying the cryptocurrency wallet impersonated by the kit.
- Reviewing the phishing page and server-side PHP code.
- Understanding how wallet seed phrases were collected.
- Identifying the service used to retrieve victim machine information.
- Reviewing previously collected seed phrases.
- Identifying the exfiltration channel used by the attacker.
- Extracting Telegram-related configuration from the source code.
- Distinguishing developer handles from verified real-world identities.
- Documenting sensitive indicators safely.

[View the GrabThePhisher investigation](./GrabThePhisher/)

---

### IcedID — Malware Threat Intelligence Investigation

Threat-intelligence investigation of a malicious macro-enabled document associated with an IcedID infection chain.

The analysis covers:

- Searching for a malware sample using its hash.
- Identifying the malicious document filename.
- Discovering a disguised GIF payload.
- Examining the relationships between the document, URLs, domains, and payloads.
- Counting the domains used for payload delivery.
- Reviewing domain-registration information.
- Correlating the sample with threat-intelligence sources.
- Identifying the associated threat actor.
- Investigating the execution behavior of the malware.
- Identifying the Windows function used to download additional payloads.
- Distinguishing direct evidence from external intelligence enrichment.

[View the IcedID investigation](./IcedID/)

---

### JetBrains — Network Forensics Investigation

Network-forensics analysis of a compromised JetBrains TeamCity server.

The investigation covers:

- Identifying the suspicious source and destination systems.
- Examining HTTP activity directed at the TeamCity server.
- Investigating exploitation of an authentication-bypass vulnerability.
- Identifying the creation of an unauthorized administrator account.
- Reviewing the upload of a malicious TeamCity plugin.
- Examining JSP web-shell activity.
- Following operating-system command execution.
- Identifying credential-file modification.
- Reviewing attempts to escape from the containerized environment.
- Reconstructing the complete server-compromise timeline.

[View the JetBrains investigation](./JetBrains/)

---

### PsExec Hunt — SMB Lateral Movement Investigation

Analysis of SMB traffic associated with PsExec-style lateral movement across a Windows environment.

The investigation covers:

- Identifying the initially compromised machine.
- Examining SMB negotiation and session establishment.
- Reviewing NTLM authentication traffic.
- Identifying the account used for authentication.
- Examining access to administrative network shares.
- Tracking the transfer of the PsExec service executable.
- Confirming the executable creation and write operations.
- Examining remote service creation and control.
- Reviewing IPC communication through named pipes.
- Identifying additional systems targeted for lateral movement.

[View the PsExec Hunt investigation](./PsExecHunt/)

---

### RetailBreach — Network Forensics Investigation

Investigation of a web-application compromise reconstructed from captured network traffic.

The analysis covers:

- Establishing an overview of HTTP activity.
- Identifying the suspicious external source.
- Detecting automated web-directory enumeration.
- Identifying the enumeration tool through its User-Agent.
- Examining the submission of a stored XSS payload.
- Following the execution of the payload in an administrator’s browser.
- Investigating session-cookie exposure.
- Detecting reuse of the stolen administrative session.
- Examining access to restricted administration pages.
- Identifying exploitation of a path-traversal vulnerability.
- Confirming unauthorized retrieval of a sensitive operating-system file.

[View the RetailBreach investigation](./RetailBreach/)

---

### Tomcat Takeover — Apache Tomcat Compromise Investigation

Network-forensics investigation of an Apache Tomcat web-server compromise.

The investigation covers:

- Establishing the PCAP timeline and traffic baseline.
- Identifying scanning activity directed at the server.
- Determining the source responsible for the suspicious requests.
- Reviewing the open ports discovered during the scan.
- Identifying the Tomcat administration interface.
- Detecting automated web-content enumeration.
- Identifying the enumeration tool from the HTTP User-Agent.
- Examining requests for administrative directories.
- Investigating brute-force authentication attempts.
- Identifying the successful login.
- Tracking the upload of a malicious WAR archive.
- Identifying the reverse-shell callback destination.
- Reconstructing the web-server compromise chain.

[View the Tomcat Takeover investigation](./TomcatTakeover/)

---

### Yellow Cockatoo — Malware Threat Intelligence Investigation

Threat-intelligence investigation of a suspicious SHA-256 hash associated with the threat cluster tracked by Red Canary as Yellow Cockatoo and commonly associated with SolarMarker activity.

The investigation covers:

- Searching for the supplied SHA-256 hash.
- Comparing malware classifications from different security vendors.
- Identifying the threat-cluster name used by Red Canary.
- Identifying the malware sample’s common filename.
- Reviewing the sample’s compilation timestamp.
- Identifying the sample’s first submission date to VirusTotal.
- Investigating files dropped inside the Windows `AppData` directory.
- Identifying the dropped `.dat` component.
- Extracting and safely defanging the command-and-control server.
- Distinguishing vendor naming from verified malware behavior.
- Documenting threat-intelligence findings and their supporting sources.

[View the Yellow Cockatoo investigation](./YellowCockatoo/)

## Investigation Methodology

The following methodology is used throughout these investigations:

1. Understand the scenario and identify the supplied evidence.
2. Preserve the original evidence and work from a safe copy.
3. Establish a baseline of hosts, users, services, protocols, and timestamps.
4. Identify suspicious activity without assuming the final answer.
5. Develop a focused hypothesis for each investigative question.
6. Use an appropriate filter, query, command, or intelligence source.
7. Examine the result in its original context.
8. Correlate activity using identities, systems, timestamps, and indicators.
9. Separate confirmed evidence from assumptions and external enrichment.
10. Reconstruct the attack timeline.
11. Capture screenshots of the evidence supporting each conclusion.
12. Produce detection, containment, and remediation recommendations.

## Skills Practiced

- Network traffic analysis
- Digital forensics and incident response
- Malware triage and static analysis
- Threat-intelligence research
- AWS CloudTrail investigation
- Splunk Search Processing Language
- SMB and NTLM analysis
- HTTP request and response analysis
- Name-resolution poisoning analysis
- Attack-chain reconstruction
- Indicator-of-compromise extraction
- MITRE ATT&CK mapping
- Evidence-based technical reporting

## Tools Used

| Tool | Purpose |
|---|---|
| Wireshark | Packet analysis, protocol filtering, and stream reconstruction |
| Splunk | CloudTrail searching, correlation, aggregation, and timeline analysis |
| VirusTotal | Hash reputation, file metadata, relations, and vendor detections |
| Hybrid Analysis | Malware behavior and dropped-file investigation |
| Malpedia | Malware-family and threat-actor intelligence |
| Recorded Future Triage | Dynamic behavior and API-call investigation |
| Kali Linux | Safe evidence examination and command-line static analysis |
| Git and GitHub | Version control and publication of technical write-ups |

## Documentation Structure

Each completed investigation may contain:

```text
LabName/
├── README.md
├── LabName-Investigation-Report.pdf
├── LabName-Investigation-Report.docx
└── images/
    ├── 01-initial-evidence.png
    ├── 02-investigation-step.png
    └── 03-confirmed-finding.png
```

The lab-level `README.md` contains the technical write-up, while the PDF or Word document provides a formal incident-investigation report when available.

## Evidence Standard

Each conclusion should be supported by:

- The exact filter, query, command, or source used.
- The relevant result and its surrounding context.
- A clear explanation of what the evidence proves.
- Any limitations or possible alternative interpretations.
- A screenshot containing the method and supporting result.
- A timeline connecting the finding to the broader incident.

## Disclaimer

All investigations in this directory were completed using authorized CyberDefenders training labs and intentionally supplied evidence.

No unauthorized systems were accessed or tested. Any credentials, tokens, infrastructure indicators, malware samples, or malicious code are documented strictly for defensive education and incident-response training.

---

**Author:** Alaa Zahra  
**Track:** SOC Analyst — Network, Cloud, Malware, and Threat Intelligence Investigations