# CyberDefenders Write-ups

![Platform](https://img.shields.io/badge/Platform-CyberDefenders-1679A7)
![Focus](https://img.shields.io/badge/Focus-Blue%20Team-0A66C2)
![Status](https://img.shields.io/badge/Status-Active-brightgreen)

This directory contains my network, cloud, and digital-forensics investigations completed on the CyberDefenders platform.

Each lab documents the investigation process, evidence, queries or filters, reconstructed attack chain, indicators of compromise, MITRE ATT&CK mapping, and defensive recommendations.

## Labs

| Lab | Category | Primary Tool | Report |
|---|---|---|---|
| [AWSRaid](./AWSRaid/) | Cloud Forensics | Splunk / AWS CloudTrail | [PDF](./AWSRaid/AWSRaid-CloudTrail-Investigation-Report.pdf) |
| [JetBrains](./JetBrains/) | Network Forensics | Wireshark | [PDF](./JetBrains/Network_Forensics_JetBrains_Report.pdf) |
| [RetailBreach](./RetailBreach/) | Network Forensics / Web Attacks | Wireshark | [PDF](./RetailBreach/RetailBreach-Network-Forensics-Report.pdf) |

## Investigation Focus

- Network traffic analysis and HTTP stream reconstruction.
- AWS CloudTrail investigation using Splunk SPL.
- Authentication and session-compromise analysis.
- Web-shell, XSS, path traversal, and cloud persistence detection.
- Evidence-based timelines and MITRE ATT&CK mapping.

---

**Author:** Alaa Zahra  
**Track:** SOC Analyst / Cloud and Network Forensics
