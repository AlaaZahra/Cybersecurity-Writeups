# CyberDefenders Write-ups

![Platform](https://img.shields.io/badge/Platform-CyberDefenders-1679A7)
![Focus](https://img.shields.io/badge/Focus-Blue%20Team%20%26%20SOC-0A66C2)
![Categories](https://img.shields.io/badge/Categories-Network%20%7C%20Cloud%20Forensics-6f42c1)
![Status](https://img.shields.io/badge/Status-Active-brightgreen)

## Overview

This directory contains my hands-on investigations completed on the **CyberDefenders** platform.

The write-ups focus on the investigation methodology, supporting evidence, tools, queries, Wireshark filters, attack timelines, indicators of compromise, and defensive recommendations—not only the final answers.

---

## Investigations

| Lab | Category | Primary Tool | Investigation Focus |
|---|---|---|---|
| [AWSRaid](./AWSRaid/) | Cloud Forensics | Splunk | AWS CloudTrail compromise and S3 activity |
| [JetBrains](./JetBrains/) | Network Forensics | Wireshark | TeamCity authentication bypass and web-shell deployment |
| [RetailBreach](./RetailBreach/) | Network Forensics | Wireshark | Stored XSS, session theft, and path traversal |
| [Poisoned Credentials](./PoisonedCredentials/) | Network Forensics | Wireshark | Name-resolution poisoning and NTLM credential exposure |
| [PsExec Hunt](./PsExecHunt/) | Network Forensics | Wireshark | SMB-based lateral movement using PsExec |
| [Tomcat Takeover](./TomcatTakeover/) | Network Forensics | Wireshark | Tomcat Manager compromise, WAR deployment, and reverse shell |

---

## Lab Summaries

### AWSRaid

An AWS CloudTrail investigation performed with Splunk.

The investigation identified:

- Repeated failed AWS console logins.
- A successful login to a compromised IAM account.
- S3 bucket enumeration and object access.
- Modification of an S3 bucket's public-access configuration.
- Creation of a new IAM user for persistence.
- Addition of the attacker-created user to an administrative group.

[View the AWSRaid investigation](./AWSRaid/)

---

### JetBrains

A network-forensics investigation into the compromise of a JetBrains TeamCity server.

The investigation identified:

- Exploitation of `CVE-2024-27198`.
- Authentication bypass against TeamCity.
- Creation of a global administrator account.
- Deployment of a malicious TeamCity plugin.
- Execution of commands through a JSP web shell.
- Modification of a credentials file.
- An attempted Docker container escape.

[View the JetBrains investigation](./JetBrains/)

---

### RetailBreach

A network-forensics investigation into the compromise of an online retail application.

The investigation identified:

- Automated web-directory enumeration using Gobuster.
- Injection of a stored XSS payload through a product-review form.
- Execution of the payload when an administrator visited the affected page.
- Exfiltration of the administrator's session cookie.
- Reuse of the stolen cookie to access administrative functionality.
- Path traversal through the application's log viewer.
- Unauthorized access to `/etc/passwd`.

[View the RetailBreach investigation](./RetailBreach/)

---

### Poisoned Credentials

A network-forensics investigation into local name-resolution poisoning and NTLM authentication exposure.

The investigation identified:

- Incorrect local name-resolution requests.
- A malicious system responding to the requests.
- Redirection of the victim toward an attacker-controlled SMB service.
- NTLM authentication data transmitted during SMB session setup.
- The affected user and workstation.
- The relationship between multicast name resolution, SMB, and credential capture.

[View the Poisoned Credentials investigation](./PoisonedCredentials/)

---

### PsExec Hunt

A network-forensics investigation into SMB-based lateral movement using PsExec.

The investigation identified:

- The initially compromised machine.
- The first internal system targeted by the attacker.
- The account used for NTLM authentication.
- Access to the `ADMIN$` administrative share.
- Creation and transfer of `PSEXESVC.exe`.
- Remote service-control activity.
- Communication through the `IPC$` share and PsExec named pipes.
- A second lateral-movement target.

[View the PsExec Hunt investigation](./PsExecHunt/)

---

### Tomcat Takeover

A network-forensics investigation into the compromise of an Apache Tomcat server.

The investigation identified:

- Active TCP port scanning.
- Apache Tomcat Manager exposed on TCP port `8080`.
- Automated directory enumeration using Gobuster.
- Discovery of the `/manager` administrative interface.
- Successful HTTP Basic Authentication.
- Deployment of a malicious WAR archive.
- Root-level reverse-shell execution.
- Cron-based persistence with a recurring callback.

[View the Tomcat Takeover investigation](./TomcatTakeover/)

---

## Investigation Workflow

The investigations generally follow this methodology:

1. Establish a baseline using capture statistics or log summaries.
2. Identify active endpoints, services, users, and protocols.
3. Isolate suspicious behavior using filters or SIEM queries.
4. Reconstruct the relevant network streams or event timeline.
5. Confirm each finding using request, response, authentication, or command evidence.
6. Record indicators of compromise.
7. Map the observed behavior to MITRE ATT&CK.
8. Document detection and remediation opportunities.

---

## Tools Used

- Wireshark
- Splunk
- AWS CloudTrail
- CyberChef
- IP intelligence services
- MITRE ATT&CK
- External vulnerability references

---

## Repository Structure

```text
CyberDefenders/
├── AWSRaid/
│   ├── README.md
│   ├── AWSRaid-CloudTrail-Investigation-Report.docx
│   ├── AWSRaid-CloudTrail-Investigation-Report.pdf
│   └── images/
├── JetBrains/
│   ├── README.md
│   ├── Network_Forensics_JetBrains_Report.pdf
│   └── images/
├── PoisonedCredentials/
│   ├── README.md
│   ├── PoisonedCredentials-Network-Forensics-Report.docx
│   ├── PoisonedCredentials-Network-Forensics-Report.pdf
│   └── images/
├── PsExecHunt/
│   ├── README.md
│   ├── PsExecHunt-Network-Forensics-Report.docx
│   ├── PsExecHunt-Network-Forensics-Report.pdf
│   └── images/
├── RetailBreach/
│   ├── README.md
│   ├── RetailBreach-Network-Forensics-Report.pdf
│   └── images/
└── TomcatTakeover/
    ├── README.md
    ├── TomcatTakeover-Network-Forensics-Report.docx
    ├── TomcatTakeover-Network-Forensics-Report.pdf
    └── images/
```

---

## Documentation Approach

Each investigation may include:

- A detailed GitHub `README.md`.
- A complete PDF investigation report.
- An editable Word report.
- Screenshots of relevant evidence.
- Wireshark display filters or Splunk queries.
- A reconstructed attack timeline.
- Indicators of compromise.
- MITRE ATT&CK mapping.
- Detection opportunities.
- Remediation recommendations.
- A visual attack-chain diagram.

---

## Disclaimer

All investigations in this directory were completed in authorized training environments using intentionally vulnerable systems and supplied evidence files.

No real-world systems were targeted.

---

**Author:** Alaa Zahra  
**Track:** SOC Analyst and Digital Forensics  
**Platform:** CyberDefenders