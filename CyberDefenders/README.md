# CyberDefenders Write-ups

![Platform](https://img.shields.io/badge/Platform-CyberDefenders-1679A7)
![Focus](https://img.shields.io/badge/Focus-Blue%20Team%20%26%20DFIR-0A66C2)
![Status](https://img.shields.io/badge/Status-Active-brightgreen)

This directory contains my hands-on CyberDefenders investigations across network forensics, cloud security, incident response, threat hunting, and phishing-kit analysis. Each write-up documents the investigative reasoning, evidence, commands or queries, attack reconstruction, and defensive lessons—not only the final answers.

## Investigations

| Lab | Category | Primary Tools | Investigation Focus |
|---|---|---|---|
| [AWSRaid](./AWSRaid/) | Cloud Forensics | Splunk, AWS CloudTrail | Compromised IAM identity, S3 access, public-access changes, and persistence |
| [GrabThePhisher](./GrabThePhisher/) | Phishing Kit Analysis | Kali Linux, static source review | Seed-phrase theft, IP enrichment, Telegram exfiltration, and local logging |
| [JetBrains](./JetBrains/) | Network Forensics | Wireshark | TeamCity authentication bypass, web-shell deployment, and container-escape attempt |
| [PsExec Hunt](./PsExecHunt/) | Network Forensics | Wireshark | SMB authentication, PsExec service deployment, and lateral movement |
| [RetailBreach](./RetailBreach/) | Network Forensics | Wireshark | Stored XSS, session theft, cookie reuse, and path traversal |
| [TomcatTakeover](./TomcatTakeover/) | Network Forensics | Wireshark | Port scanning, enumeration, Tomcat compromise, WAR upload, and reverse shell |

## Repository Structure

```text
CyberDefenders/
├── AWSRaid/
├── GrabThePhisher/
├── JetBrains/
├── PsExecHunt/
├── RetailBreach/
├── TomcatTakeover/
└── README.md
```

Each lab directory may contain:

- A detailed `README.md` investigation write-up.
- Screenshots of relevant evidence.
- Splunk queries, Wireshark display filters, or command-line analysis steps.
- A reconstructed attack chain or investigation diagram.
- Indicators of compromise and MITRE ATT&CK mappings.
- Detection and remediation recommendations.
- A PDF investigation report when available.

## Investigation Principles

- Start from evidence and avoid assuming the answer.
- Correlate multiple artifacts before reaching a conclusion.
- Distinguish confirmed facts, reasonable inferences, and unresolved questions.
- Preserve sensitive evidence and redact secrets from public documentation.
- Explain why each command, query, or filter was used.
- Translate technical findings into practical detection and response actions.

## Disclaimer

All investigations were completed in authorized training environments using intentionally vulnerable systems or supplied forensic artifacts. No real-world systems were targeted. Sensitive values may be redacted from public documentation.

---

**Author:** Alaa Zahra  
**Focus:** SOC Analysis, Cloud Security, Network Forensics, and DFIR

