# SOC Home Lab: Active Directory Threat Detection

## Project Overview

This project is an Azure-based SOC home lab designed to simulate an enterprise Active Directory environment and detect suspicious Windows activity using Splunk Enterprise.

The lab includes a Windows Server domain controller, a Windows endpoint, a Kali Linux attacker machine, Sysmon logging, Splunk Universal Forwarder, and Splunk Enterprise for centralized log analysis.

The goal of this project is to practice real SOC analyst workflows, including log ingestion, detection engineering, alert triage, incident investigation, MITRE ATT&CK mapping, and incident reporting.

## Lab Objectives

- Build a small enterprise-style Windows domain environment
- Configure Active Directory on a Windows Server domain controller
- Join a Windows endpoint to the domain
- Install and configure Sysmon for endpoint telemetry
- Forward Windows and Sysmon logs into Splunk
- Simulate common attack activity using Kali Linux
- Build SPL detections for suspicious behavior
- Map detections to MITRE ATT&CK techniques
- Create SOC-style incident reports for each simulation

## Lab Architecture

Coming soon.

## Tools and Technologies

- Microsoft Azure
- Windows Server
- Active Directory
- Windows endpoint
- Kali Linux
- Splunk Enterprise
- Splunk Universal Forwarder
- Sysmon
- PowerShell
- Nmap
- MITRE ATT&CK
- GitHub

## Planned Detections

| Detection | MITRE Technique | Status |
|---|---|---|
| Brute-force login attempts | T1110 | Planned |
| Account lockout activity | Valid Accounts / Account Access | Planned |
| New user creation | T1136 | Planned |
| Privileged group membership changes | T1098 | Planned |
| Domain admin escalation | T1078 | Planned |
| Suspicious PowerShell execution | T1059.001 | Planned |
| Network reconnaissance | T1046 | Planned |
| Failed login analysis | T1110 | Planned |

## Incident Reports

Incident reports will be added as attack simulations are completed.

## Lessons Learned

Coming soon.

## Disclaimer

This lab is for educational and defensive security purposes only. All attack simulations are performed in a controlled lab environment owned and managed by the project author.
