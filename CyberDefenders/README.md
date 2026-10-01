# CyberDefenders Write-ups

![Platform](https://img.shields.io/badge/Platform-CyberDefenders-1679A7)
![Focus](https://img.shields.io/badge/Focus-Blue%20Team%20%26%20DFIR-0A66C2)
![Labs](https://img.shields.io/badge/Completed%20Labs-5-6F42C1)
![Status](https://img.shields.io/badge/Status-Active-brightgreen)

## Overview

This directory contains my hands-on investigations completed on the **CyberDefenders** platform. The labs cover network forensics, cloud incident response, web-attack analysis, credential attacks, and Windows lateral movement.

Each write-up documents the investigation methodology, supporting evidence, filters or queries, reconstructed attack chain, indicators of compromise, MITRE ATT&CK mapping, detection opportunities, and remediation recommendations.

> The emphasis is on explaining how each conclusion was reached—not only presenting the final answers.

---

## Labs

| Lab | Investigation Focus | Primary Tools | Report |
|---|---|---|---|
| [AWSRaid](./AWSRaid/) | AWS CloudTrail investigation covering suspicious console access, S3 activity, bucket exposure, and IAM persistence | Splunk, AWS CloudTrail | [PDF](./AWSRaid/AWSRaid-CloudTrail-Investigation-Report.pdf) |
| [JetBrains](./JetBrains/) | TeamCity compromise involving authentication bypass, administrator creation, malicious plugin upload, web-shell execution, and a container-escape attempt | Wireshark | [PDF](./JetBrains/Network_Forensics_JetBrains_Report.pdf) |
| [PoisonedCredentials](./PoisonedCredentials/) | NBNS name-resolution poisoning followed by SMB and NTLM credential exposure | Wireshark | [PDF](./PoisonedCredentials/PoisonedCredentials-Network-Forensics-Report.pdf) |
| [RetailBreach](./RetailBreach/) | Web compromise involving directory discovery, stored XSS, session-cookie theft and reuse, and path traversal | Wireshark | [PDF](./RetailBreach/RetailBreach-Network-Forensics-Report.pdf) |
| [PsExec Hunt](./PsExecHunt/) | SMB-based lateral movement using NTLM authentication, administrative shares, PsExec service execution, and named pipes | Wireshark | [PDF](./PsExecHunt/PsExecHunt-Network-Forensics-Report.pdf) |

---

## Investigation Coverage

### Cloud Forensics

- AWS CloudTrail event analysis using Splunk SPL.
- Console-login and authentication investigation.
- IAM account and group activity.
- S3 bucket discovery, object access, and configuration changes.
- Persistence and privilege-escalation analysis.

### Network Forensics

- Protocol hierarchy and endpoint analysis.
- HTTP, SMB2, NTLMSSP, DCE/RPC, and SVCCTL investigation.
- TCP and application-stream reconstruction.
- Identification of attacker, victim, and pivot systems.
- Timestamp normalization and evidence-based timeline creation.

### Web Attack Analysis

- Directory and endpoint discovery.
- Authentication-bypass exploitation.
- Stored cross-site scripting.
- Session-cookie theft and session hijacking.
- Path traversal and sensitive-file access.
- Web-shell upload and command execution.

### Windows Lateral Movement

- SMB negotiation and Session Setup analysis.
- NTLM user and workstation identification.
- Access to `ADMIN$` and `IPC$`.
- PsExec executable transfer.
- Remote service creation and execution.
- Named-pipe communication and pivot detection.

---

## Investigation Workflow

Each lab follows a consistent evidence-driven process:

1. Understand the scenario and define the investigation objectives.
2. Establish a baseline of the available traffic or log data.
3. Identify suspicious users, hosts, IP addresses, events, or endpoints.
4. Build focused Wireshark filters or Splunk queries.
5. Validate each finding using packet fields, event fields, or response data.
6. Reconstruct the attack chain in chronological order.
7. Extract indicators of compromise.
8. Map confirmed behavior to MITRE ATT&CK.
9. Document detection and remediation recommendations.
10. Record any limitations instead of making unsupported assumptions.

---

## Repository Structure

```text
CyberDefenders/
├── README.md
├── AWSRaid/
│   ├── README.md
│   ├── AWSRaid-CloudTrail-Investigation-Report.pdf
│   └── images/
├── JetBrains/
│   ├── README.md
│   ├── Network_Forensics_JetBrains_Report.pdf
│   └── images/
├── PoisonedCredentials/
│   ├── README.md
│   ├── PoisonedCredentials-Network-Forensics-Report.pdf
│   └── images/
├── RetailBreach/
│   ├── README.md
│   ├── RetailBreach-Network-Forensics-Report.pdf
│   └── images/
└── PsExecHunt/
    ├── README.md
    ├── PsExecHunt-Network-Forensics-Report.pdf
    └── images/
```

---

## Skills Demonstrated

- Wireshark display-filter construction.
- HTTP and SMB stream analysis.
- Windows authentication and lateral-movement analysis.
- AWS CloudTrail threat hunting with Splunk.
- Attack-chain and timeline reconstruction.
- IOC extraction and documentation.
- MITRE ATT&CK technique mapping.
- Evidence-based technical reporting.
- Detection engineering and remediation planning.

---

## Documentation Standard

Every investigation aims to include:

- A detailed `README.md` write-up.
- Screenshots placed directly below the relevant query or filter.
- Clear explanations of what each filter does and why it was used.
- A concise answer-and-evidence matrix.
- A reconstructed attack timeline.
- Indicators of compromise.
- MITRE ATT&CK mappings.
- Detection and remediation recommendations.
- A complete PDF investigation report when available.

---

## Disclaimer

All investigations were performed in intentionally vulnerable, authorized training environments. The material is provided for educational and defensive-security purposes only. No real-world systems were targeted.

---

**Author:** Alaa Zahra  
**Track:** SOC Analyst / Cloud and Network Forensics

