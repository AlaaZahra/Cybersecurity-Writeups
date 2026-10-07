# CyberDefenders — Blue Team Investigations

![Platform](https://img.shields.io/badge/Platform-CyberDefenders-1679A7)
![Focus](https://img.shields.io/badge/Focus-Blue%20Team%20%26%20DFIR-0A66C2)
![Tools](https://img.shields.io/badge/Tools-Wireshark%20%7C%20Splunk%20%7C%20VirusTotal-success)
![Status](https://img.shields.io/badge/Status-Active-brightgreen)

This directory contains my CyberDefenders lab investigations and technical write-ups.

Each investigation focuses on the complete analytical process: understanding the scenario, examining the available evidence, building and validating hypotheses, reconstructing the attack chain, and documenting the findings.

## Investigations

| Lab | Category | Investigation Focus | Main Tools |
|---|---|---|---|
| [AWSRaid](./AWSRaid/) | Cloud Forensics | AWS account compromise, S3 activity, IAM persistence, and CloudTrail analysis | Splunk, AWS CloudTrail |
| [GrabThePhisher](./GrabThePhisher/) | Malware Analysis | Phishing-kit source-code analysis, credential collection, Telegram exfiltration, and attacker infrastructure | Kali Linux, grep, static analysis |
| [IcedID](./IcedID/) | Threat Intelligence | Malicious document analysis, payload infrastructure, threat-actor attribution, and malware execution behavior | VirusTotal, Malpedia, Recorded Future Triage |
| [JetBrains](./JetBrains/) | Network Forensics | TeamCity compromise, authentication bypass, malicious plugin deployment, and command execution | Wireshark |
| [PsExec Hunt](./PsExecHunt/) | Network Forensics | SMB authentication, administrative shares, PsExec service installation, and lateral movement | Wireshark |
| [RetailBreach](./RetailBreach/) | Network Forensics | Web enumeration, stored XSS, session theft, administrative access, and path traversal | Wireshark |
| [Tomcat Takeover](./TomcatTakeover/) | Network Forensics | Port scanning, web enumeration, Tomcat credential attacks, WAR deployment, and reverse-shell activity | Wireshark |

## Investigation Summaries

### AWSRaid — AWS CloudTrail Incident Investigation

Investigation of suspicious activity within an AWS environment using CloudTrail logs ingested into Splunk.

The analysis covers:

- Failed and successful AWS Console login attempts.
- Identification of the compromised IAM account.
- S3 bucket and object enumeration.
- Access to sensitive objects.
- Modification of S3 public-access settings.
- Creation of a persistence IAM account.
- Addition of the new account to a privileged IAM group.

[View the AWSRaid investigation](./AWSRaid/)

---

### GrabThePhisher — Phishing Kit Investigation

Static investigation of a cryptocurrency-wallet phishing kit recovered from an archived evidence package.

The analysis covers:

- Identification of the impersonated cryptocurrency wallet.
- Review of the phishing page and server-side PHP code.
- Collection and storage of wallet seed phrases.
- Victim machine-information collection.
- Telegram-based data exfiltration.
- Extraction and safe handling of exposed tokens and identifiers.
- Separation between developer aliases and verified real-world attribution.

[View the GrabThePhisher investigation](./GrabThePhisher/)

---

### IcedID — Malware Threat Intelligence Investigation

Threat-intelligence investigation of a malicious macro-enabled document associated with an IcedID infection chain.

The analysis covers:

- Identification of the malicious document filename.
- Discovery of a disguised GIF payload.
- Analysis of the payload-delivery infrastructure.
- Review of contacted domains and registration information.
- Threat-actor attribution using intelligence sources.
- Identification of the Windows function used to retrieve additional payloads.
- Differentiation between evidence observed directly and information obtained from external intelligence sources.

[View the IcedID investigation](./IcedID/)

---

### JetBrains — Network Forensics Investigation

Network-forensics analysis of a compromised JetBrains TeamCity server.

The investigation reconstructs:

- Initial access to the TeamCity server.
- Exploitation of an authentication-bypass vulnerability.
- Creation of an unauthorized administrator account.
- Upload and deployment of a malicious TeamCity plugin.
- JSP web-shell activity.
- Operating-system command execution.
- Credential-file modification.
- Attempts to escape from the containerized environment.

[View the JetBrains investigation](./JetBrains/)

---

### PsExec Hunt — SMB Lateral Movement Investigation

Analysis of SMB traffic associated with PsExec-style lateral movement across a Windows environment.

The investigation covers:

- Identification of the initially compromised machine.
- SMB negotiation and session establishment.
- NTLM user authentication.
- Access to administrative network shares.
- Transfer of the PsExec service executable.
- Remote service creation and control.
- IPC communication using named pipes.
- Identification of additional systems targeted for lateral movement.

[View the PsExec Hunt investigation](./PsExecHunt/)

---

### RetailBreach — Network Forensics Investigation

Investigation of a web-application compromise reconstructed from captured network traffic.

The analysis follows the attacker through:

- Web-directory enumeration.
- Identification of Gobuster activity.
- Submission of a stored XSS payload.
- Execution of the payload in an administrator's browser.
- Theft and reuse of an administrative session cookie.
- Access to restricted administration pages.
- Exploitation of a path-traversal vulnerability.
- Unauthorized retrieval of a sensitive operating-system file.

[View the RetailBreach investigation](./RetailBreach/)

---

### Tomcat Takeover — Apache Tomcat Compromise Investigation

Network-forensics investigation of an Apache Tomcat web-server compromise.

The investigation covers:

- Identification of the scanning source.
- Analysis of the attacker's port-scanning activity.
- Discovery of the exposed Tomcat administration interface.
- Web-content enumeration using an automated tool.
- Brute-force authentication activity.
- Identification of the successful credentials.
- Upload of a malicious WAR archive.
- Deployment of a reverse-shell payload.
- Identification of the callback destination.

[View the Tomcat Takeover investigation](./TomcatTakeover/)

## Investigation Methodology

The following methodology is used throughout these write-ups:

1. Understand the incident scenario and available evidence.
2. Preserve the original evidence and work from a safe copy.
3. Establish a baseline of hosts, protocols, users, and services.
4. Identify suspicious behavior without assuming the final answer.
5. Create focused filters or queries to test each hypothesis.
6. Correlate activity using timestamps, identities, systems, and network indicators.
7. Distinguish confirmed evidence from analytical assumptions.
8. Reconstruct the attack timeline.
9. Document screenshots, filters, commands, and supporting evidence.
10. Produce defensive recommendations based on the confirmed findings.

## Skills Practiced

- Network traffic analysis
- Digital forensics and incident response
- Malware triage and static analysis
- CloudTrail investigation
- Splunk Search Processing Language
- SMB and NTLM analysis
- HTTP request and response analysis
- Threat-intelligence research
- Attack-chain reconstruction
- Indicator-of-compromise extraction
- MITRE ATT&CK mapping
- Evidence-based technical reporting

## Documentation Structure

Each completed lab may contain:

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

The lab `README.md` contains the technical write-up, while the PDF or Word document provides a formal incident-investigation report when available.

## Evidence Standard

Findings are documented using:

- The exact filter, query, or command used.
- The relevant result and its surrounding context.
- A clear explanation of what the evidence proves.
- Any limitations or alternative interpretations.
- A screenshot showing the query and the supporting result.
- A timeline connecting the finding with the rest of the incident.

## Disclaimer

All investigations in this directory were completed using authorized CyberDefenders training labs and intentionally provided evidence.

No unauthorized systems were accessed or tested. Any exposed credentials, tokens, infrastructure indicators, or malicious code are documented strictly for defensive education and incident-response training.

---

**Author:** Alaa Zahra  
**Track:** SOC Analyst — Network, Cloud, and Malware Investigations