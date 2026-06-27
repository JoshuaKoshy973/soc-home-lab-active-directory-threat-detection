# Simulated Activity

This folder documents the simulated security activity generated during the SOC home lab. The purpose of these simulations was to create realistic Windows endpoint and Active Directory events, ingest those events into Splunk, and detect them using SPL searches.

All activity was performed in a controlled lab environment for defensive security monitoring, detection engineering, and incident response practice.

## Lab Environment

* Domain: `soclab.local`
* Domain Controller: `DC-01`
* Windows Endpoint: `WIN-CLIENT-01`
* SIEM: Splunk Enterprise
* Endpoint Telemetry: Sysmon
* Log Forwarding: Splunk Universal Forwarder
* Primary Log Sources:

  * `WinEventLog:Security`
  * `WinEventLog:Microsoft-Windows-Sysmon/Operational`

## Simulation 1: Windows Reconnaissance Commands

### Scenario

This simulation represents post-compromise discovery activity. After gaining access to a Windows endpoint, an attacker may run basic discovery commands to understand the current user context, system information, network configuration, local users, administrator groups, and domain relationships.

### Host

`WIN-CLIENT-01`

### Commands Executed

```powershell
whoami
hostname
ipconfig /all
net user
net localgroup administrators
net group "Domain Admins" /domain
nltest /domain_trusts
```

### Purpose of Commands

* `whoami` — identifies the current user context
* `hostname` — identifies the computer name
* `ipconfig /all` — displays network configuration, DNS settings, and domain-related information
* `net user` — lists local user accounts
* `net localgroup administrators` — lists local administrators
* `net group "Domain Admins" /domain` — attempts to list members of the Domain Admins group
* `nltest /domain_trusts` — checks domain trust relationships

### Detection Logic

```spl
index=* source="WinEventLog:Microsoft-Windows-Sysmon/Operational"
("whoami" OR "ipconfig" OR "net.exe" OR "net user" OR "net group" OR "Domain Admins" OR "nltest")
| table _time host source sourcetype
```

### Expected Log Source

`WinEventLog:Microsoft-Windows-Sysmon/Operational`

### MITRE ATT&CK Mapping

* Tactic: Discovery
* Technique: System Owner/User Discovery — T1033
* Technique: System Information Discovery — T1082
* Technique: Account Discovery — T1087
* Technique: Permission Groups Discovery — T1069
* Sub-technique: Domain Groups — T1069.002

### Screenshot Reference

`screenshots/splunk-recon-command-detection.png`

### Notes

The commands used in this simulation are not malicious by themselves. However, when several discovery commands occur close together, especially from an unusual user or after suspicious logon activity, they can indicate post-compromise reconnaissance.

## Simulation 2: Failed Windows Logons

### Scenario

This simulation represents failed authentication activity. Failed logons may occur because of normal user error, but repeated failed logons can indicate password guessing, brute-force attempts, expired credentials, misconfigured services, or suspicious authentication behavior.

### Host

`WIN-CLIENT-01` and/or `DC-01`

### Activity Performed

Incorrect credentials were entered multiple times to generate failed Windows logon events.

### Relevant Windows Event ID

* `4625` — An account failed to log on

### Detection Logic

```spl
index=* source="WinEventLog:Security" EventCode=4625
| table _time host source sourcetype EventCode
```

### Expected Log Source

`WinEventLog:Security`

### MITRE ATT&CK Mapping

* Tactic: Credential Access
* Technique: Brute Force — T1110
* Sub-technique: Password Guessing — T1110.001
* Related Tactic: Initial Access
* Related Technique: Valid Accounts — T1078

### Screenshot Reference

`screenshots/splunk-failed-logon-detection.png`

### Notes

A small number of failed logons may be normal. In a real SOC environment, failed logons become more suspicious when they occur repeatedly, target privileged accounts, come from unusual hosts, or are followed by a successful logon.

## Simulation 3: Active Directory Account and Group Changes

### Scenario

This simulation represents identity and privilege-related changes in Active Directory. Attackers may create new accounts or modify group memberships to maintain persistence, escalate privileges, or expand access in a domain environment.

### Host

`DC-01`

### Activity Performed

* Created a test Active Directory user account
* Added a test user to a security group

### Relevant Windows Event IDs

* `4720` — A user account was created
* `4728` — A member was added to a security-enabled global group
* `4732` — A member was added to a security-enabled local group

### Detection Logic

```spl
index=* source="WinEventLog:Security" (EventCode=4720 OR EventCode=4728 OR EventCode=4732)
| table _time host source sourcetype EventCode
```

### Expected Log Source

`WinEventLog:Security`

### MITRE ATT&CK Mapping

* Tactic: Persistence
* Technique: Account Manipulation — T1098
* Tactic: Privilege Escalation
* Technique: Account Manipulation — T1098
* Related Technique: Create Account — T1136
* Sub-technique: Domain Account — T1136.002

### Screenshot Reference

`screenshots/splunk-ad-account-group-change-detection.png`

### Notes

Active Directory account and group changes should be monitored closely. A normal account creation may be part of expected administration, but unexpected user creation or group membership changes can indicate persistence or privilege escalation.

## Simulation 4: PowerShell and Process Activity

### Scenario

This simulation represents endpoint process execution monitoring. PowerShell and process creation activity are important in SOC investigations because attackers often use built-in Windows tools to perform discovery, execute commands, or run scripts.

### Host

`WIN-CLIENT-01`

### Activity Performed

PowerShell was used to launch basic commands and applications such as `notepad.exe`. Sysmon captured process creation events showing the executed process and parent process relationship.

### Example Activity

```powershell
whoami
ipconfig
notepad
```

### Detection Logic

```spl
index=* source="WinEventLog:Microsoft-Windows-Sysmon/Operational"
("powershell.exe" OR "PowerShell")
| table _time host source sourcetype
```

### Additional Process Creation Search

```spl
index=* source="WinEventLog:Microsoft-Windows-Sysmon/Operational" "Process Create"
| table _time host source sourcetype
```

### Expected Log Source

`WinEventLog:Microsoft-Windows-Sysmon/Operational`

### Relevant Sysmon Event ID

* Sysmon Event ID 1 — Process Creation

### MITRE ATT&CK Mapping

* Tactic: Execution
* Technique: Command and Scripting Interpreter — T1059
* Sub-technique: PowerShell — T1059.001
* Related Tactic: Discovery

### Screenshot References

`screenshots/splunk-sysmon-notepad-process-search.png`

`screenshots/splunk-sysmon-logs-ingested.png`

### Notes

PowerShell is not malicious by itself. It is a legitimate administration tool. However, unusual PowerShell execution, suspicious parent processes, encoded commands, or PowerShell activity tied to other suspicious behavior should be investigated.

## Summary of Simulations

| Simulation                                 | Host                      | Primary Log Source | Key Detection                                                             |
| ------------------------------------------ | ------------------------- | ------------------ | ------------------------------------------------------------------------- |
| Windows Reconnaissance Commands            | `WIN-CLIENT-01`           | Sysmon Operational | Discovery commands such as `whoami`, `ipconfig`, `net user`, and `nltest` |
| Failed Windows Logons                      | `WIN-CLIENT-01` / `DC-01` | Windows Security   | Event ID `4625`                                                           |
| Active Directory Account and Group Changes | `DC-01`                   | Windows Security   | Event IDs `4720`, `4728`, `4732`                                          |
| PowerShell and Process Activity            | `WIN-CLIENT-01`           | Sysmon Operational | PowerShell and Sysmon process creation events                             |

## Defensive Value

These simulations helped validate that the lab could detect both endpoint and identity-based activity:

* Endpoint discovery behavior through Sysmon
* Failed authentication through Windows Security logs
* Active Directory changes through domain controller Security logs
* PowerShell and process activity through Sysmon process creation events

The simulated activity was used to create SPL detections, configure a scheduled Splunk alert, build a SOC-style dashboard, and write incident reports mapped to MITRE ATT&CK.
