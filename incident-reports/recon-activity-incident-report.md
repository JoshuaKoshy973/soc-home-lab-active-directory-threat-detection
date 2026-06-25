# Incident Report: Windows Reconnaissance Activity

## Summary

Suspicious reconnaissance activity was simulated and detected on the Windows endpoint `WIN-CLIENT-01`. The activity included common discovery commands 
such as `whoami`, `ipconfig`, `net user`, `net group`, and `nltest`. These commands are often used by attackers after gaining access to a system to understand
the current user context, network configuration, available accounts, group memberships, and domain relationships.

The activity was captured by Sysmon on the endpoint, forwarded to Splunk using Splunk Universal Forwarder, and identified through a Splunk SPL search.

## Environment

* Host: `WIN-CLIENT-01`
* Domain: `soclab.local`
* Log Source: `WinEventLog:Microsoft-Windows-Sysmon/Operational`
* SIEM: Splunk Enterprise
* Detection Type: Sysmon process activity / command-line discovery

## Detection Logic

```spl
index=* source="WinEventLog:Microsoft-Windows-Sysmon/Operational"
("whoami" OR "ipconfig" OR "net.exe" OR "net user" OR "net group" OR "Domain Admins" OR "nltest")
| table _time host source sourcetype
```

## MITRE ATT&CK Mapping

* Tactic: Discovery
* Technique: System Owner/User Discovery — T1033
* Technique: System Information Discovery — T1082
* Technique: Account Discovery — T1087
* Technique: Permission Groups Discovery — T1069
* Sub-technique: Domain Groups — T1069.002

## Evidence

The Splunk search returned Sysmon events from `WIN-CLIENT-01` showing reconnaissance commands executed from the endpoint. These events included process activity related to commands used to 
identify the current user, system information, local/domain accounts, and domain group membership.

Screenshot reference:

`screenshots/splunk-recon-command-detection.png`

## Analysis

The observed commands are not malicious by themselves, but they are commonly used during post-compromise discovery. An attacker who gains access to a Windows endpoint may run these 
commands to determine what account they are using, what system they are on, what network/domain they are connected to, and whether privileged groups or domain relationships exist.

In this lab, the activity was intentionally generated for testing. In a real environment, this behavior would require additional context, including:

* Whether the user normally performs administrative work
* Whether the commands were run from PowerShell, Command Prompt, or another parent process
* Whether the activity occurred shortly after a suspicious login
* Whether multiple discovery commands occurred within a short time window
* Whether the same user or host showed additional suspicious behavior

## Severity

Medium

## Recommended Response

1. Review the user account that executed the commands.
2. Confirm whether the activity was expected administrative behavior.
3. Review related process creation events before and after the discovery commands.
4. Check for suspicious parent processes such as Office applications launching PowerShell or Command Prompt.
5. Review authentication logs for unusual successful or failed logins.
6. If unauthorized activity is suspected, isolate the endpoint and reset affected credentials.

## Conclusion

This simulated incident demonstrated how Sysmon and Splunk can be used to detect Windows reconnaissance behavior. The detection showed how common discovery commands can be 
identified through endpoint telemetry and mapped to MITRE ATT&CK Discovery techniques.
