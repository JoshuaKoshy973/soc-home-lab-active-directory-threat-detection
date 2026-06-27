# Incident Report: Active Directory Account and Group Change Activity

## Summary

Active Directory account and group membership changes were simulated and detected on the domain controller `DC-01`. The activity included creating a test user account and adding a user to a security group. These actions are important to monitor because attackers may create new accounts or modify group memberships to maintain persistence, escalate privileges, or expand access within a domain environment.

The activity was recorded in Windows Security logs on the domain controller and identified in Splunk using SPL searches for account creation and group membership change event IDs.

## Environment

* Host: `DC-01`
* Domain: `soclab.local`
* Log Source: `WinEventLog:Security`
* SIEM: Splunk Enterprise
* Detection Type: Active Directory account and group membership change monitoring

## Detection Logic

```spl
index=* source="WinEventLog:Security" (EventCode=4720 OR EventCode=4728 OR EventCode=4732)
| table _time host source sourcetype EventCode
```

## Relevant Windows Event IDs

* Event ID 4720: A user account was created
* Event ID 4728: A member was added to a security-enabled global group
* Event ID 4732: A member was added to a security-enabled local group

## MITRE ATT&CK Mapping

* Tactic: Persistence
* Technique: Account Manipulation — T1098
* Tactic: Privilege Escalation
* Technique: Account Manipulation — T1098
* Related Technique: Create Account — T1136
* Sub-technique: Domain Account — T1136.002

## Evidence

The Splunk search returned Windows Security events from `DC-01` showing Active Directory account and group-related activity. The events included account creation and group membership changes, such as a user account being created and a user being added to a security group.

Screenshot reference:

`screenshots/splunk-ad-account-group-change-detection.png`

## Analysis

The observed activity was intentionally generated for lab testing. In a real environment, account creation and group membership changes should be reviewed carefully because they can indicate administrative activity, privilege escalation, or attacker persistence.

A newly created domain account may be legitimate if it was created by an administrator as part of normal onboarding. However, if the account was created unexpectedly, outside normal business hours, or by an unusual administrator account, it could indicate suspicious activity.

Group membership changes are especially important when privileged or security-sensitive groups are involved. If an attacker adds a user to a group with elevated permissions, they may gain broader access to systems, data, or administrative functions.

Important investigation questions would include:

* Who created the account or modified the group?
* Was the change approved or expected?
* What group was modified?
* Was the added user a normal user, service account, or privileged account?
* Did the activity occur shortly after suspicious logon activity?
* Did the account perform additional activity after being created or added to the group?

## Severity

Medium to High

The severity depends on the context. A normal approved account creation may be low severity. An unexpected account creation or privileged group membership change should be treated as medium or high severity, especially if it involves administrative groups.

## Recommended Response

1. Identify the account that performed the change.
2. Confirm whether the user creation or group change was authorized.
3. Review the affected account and group permissions.
4. Check whether the new or modified account performed any additional activity.
5. Review recent logon events for the user and the administrator account involved.
6. Remove unauthorized accounts or group memberships if confirmed suspicious.
7. Reset credentials for affected accounts if compromise is suspected.
8. Review domain administrator and privileged group membership for unexpected changes.

## Conclusion

This simulated incident demonstrated how Splunk can detect Active Directory account and group changes using Windows Security logs from the domain controller. Monitoring Event IDs 4720, 4728, and 4732 helps identify potential persistence, privilege escalation, and unauthorized identity changes in a Windows domain environment.
