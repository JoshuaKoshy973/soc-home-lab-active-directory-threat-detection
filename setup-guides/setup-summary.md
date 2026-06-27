# Setup Summary

This file summarizes the main setup steps completed for the SOC home lab.

## 1. Azure Setup

- Created resource group
- Created virtual network and subnet
- Created Windows Server virtual machines
- Assigned static private IP addresses

## 2. Domain Controller Setup

- Created `DC-01`
- Installed Active Directory Domain Services
- Promoted `DC-01` to domain controller
- Created domain: `soclab.local`
- Configured DNS for domain resolution

## 3. Active Directory Setup

Created organizational units:

- `Users`
- `Groups`
- `Workstations`
- `Servers`
- `Service Accounts`

Created test users, groups, and a service account for the lab.

## 4. Windows Client Setup

- Created `WIN-CLIENT-01`
- Joined the endpoint to `soclab.local`
- Moved the computer object into the `Workstations` OU

## 5. Sysmon Setup

- Installed Sysmon on `WIN-CLIENT-01`
- Used the SwiftOnSecurity Sysmon configuration
- Verified Sysmon logs in Event Viewer

## 6. Splunk Setup

- Installed Splunk Enterprise on `DC-01`
- Configured Splunk receiving port `9997`
- Installed Splunk Universal Forwarder on `WIN-CLIENT-01`
- Configured the forwarder to send logs to `10.10.1.10:9997`
- Configured Windows Event Log and Sysmon inputs

## 7. Detection and Monitoring

- Verified logs were searchable in Splunk
- Created SPL detection files
- Configured a scheduled Splunk alert
- Built a SOC monitoring dashboard
- Created incident reports mapped to MITRE ATT&CK
