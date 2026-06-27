# SOC Home Lab: Active Directory Threat Detection

## Overview

This project is an Azure-based SOC home lab designed to simulate a small enterprise Windows environment and practice security monitoring, log analysis, detection engineering, and incident documentation.

The lab includes a Windows Server domain controller, an Active Directory domain, a domain-joined Windows endpoint, Sysmon endpoint telemetry, Splunk Enterprise as the SIEM, and Splunk Universal Forwarder for log forwarding. The goal of the project was to generate realistic security events, ingest them into Splunk, write SPL detections, configure alerts, build a SOC-style dashboard, and document findings using incident reports mapped to MITRE ATT&CK.

## Lab Objectives

* Build a Windows Active Directory environment in Azure
* Configure centralized log collection with Splunk Enterprise
* Collect Windows Security logs and Sysmon endpoint telemetry
* Simulate suspicious Windows and Active Directory activity
* Write SPL detections for common SOC use cases
* Configure a scheduled Splunk alert for continuous monitoring
* Build a SOC-style Splunk dashboard
* Document incidents with evidence, severity, recommended response, and MITRE ATT&CK mapping

## Lab Architecture

The lab was built in Microsoft Azure using a private virtual network and subnet.

### Core Components

* `DC-01`

  * Windows Server 2022
  * Active Directory Domain Services
  * DNS
  * Domain Controller for `soclab.local`
  * Splunk Enterprise
  * Local Windows Security log collection

* `WIN-CLIENT-01`

  * Domain-joined Windows endpoint
  * Joined to `soclab.local`
  * Sysmon installed for endpoint telemetry
  * Splunk Universal Forwarder installed
  * Forwarded Windows Security, System, Application, and Sysmon logs to Splunk

### Network

* Resource Group: `rg-soc-lab-ad-detection`
* Virtual Network: `vnet-soc-lab`
* Subnet: `subnet-soc-lab`
* Domain Controller IP: `10.10.1.10`
* Windows Client IP: `10.10.1.30`
* Splunk receiving port: `9997`
* Splunk web interface: `8000`

## Tools and Technologies

* Microsoft Azure
* Windows Server 2022
* Active Directory Domain Services
* DNS
* Sysmon
* SwiftOnSecurity Sysmon configuration
* Splunk Enterprise
* Splunk Universal Forwarder
* Windows Event Viewer
* Windows Security Logs
* Splunk SPL
* MITRE ATT&CK
* GitHub

## Log Sources Collected

The lab collected and analyzed the following log sources:

* Windows Security logs
* Windows System logs
* Windows Application logs
* Sysmon Operational logs
* Active Directory security events from the domain controller

## Detection Use Cases

The lab included detections for the following activity:

### Windows Reconnaissance Commands

Detected common post-compromise discovery commands such as:

* `whoami`
* `ipconfig`
* `net user`
* `net group`
* `nltest`

These commands can be used by attackers to identify the current user, system information, available accounts, domain groups, and trust relationships.

Detection file:

`splunk-detections/windows-recon-commands.spl`

### Failed Windows Logons

Detected failed authentication attempts using Windows Security Event ID 4625.

Detection file:

`splunk-detections/failed-windows-logons.spl`

### Active Directory Account and Group Changes

Detected account creation and group membership changes using Windows Security Event IDs:

* `4720` — User account created
* `4728` — Member added to a security-enabled global group
* `4732` — Member added to a security-enabled local group

Detection file:

`splunk-detections/ad-account-group-changes.spl`

### PowerShell Activity

Detected PowerShell-related activity in Sysmon logs.

Detection file:

`splunk-detections/powershell-activity.spl`

### Sysmon Process Creation

Detected process creation activity from Sysmon logs.

Detection file:

`splunk-detections/sysmon-process-creation.spl`

## Splunk Alerting

A scheduled Splunk alert was configured for Windows reconnaissance commands.

Alert name:

`Possible Windows Reconnaissance Commands`

The alert was configured to run every five minutes and trigger when reconnaissance command activity appeared in Sysmon logs.

This simulated how a SOC can use saved searches and scheduled alerts for continuous monitoring.

## SOC Dashboard

A Splunk dashboard was created to provide a centralized monitoring view for the lab.

Dashboard panels included:

* Windows Reconnaissance Commands
* Failed Windows Logons
* Active Directory Account and Group Changes
* Sysmon Process Creation Events

Screenshot reference:

`screenshots/splunk-soc-dashboard.png`

## Incident Reports

Structured incident reports were created to document the simulated activity, detection logic, evidence, MITRE ATT&CK mapping, severity, and recommended response.

Reports included:

* `incident-reports/recon-activity-incident-report.md`
* `incident-reports/ad-account-group-change-incident-report.md`
* `incident-reports/failed-logon-incident-report.md`

## MITRE ATT&CK Mapping

This project used MITRE ATT&CK to map observed behavior to known attacker tactics and techniques.

Examples:

* Discovery

  * System Owner/User Discovery — T1033
  * System Information Discovery — T1082
  * Account Discovery — T1087
  * Permission Groups Discovery — T1069
  * Domain Groups — T1069.002

* Credential Access

  * Brute Force — T1110
  * Password Guessing — T1110.001

* Persistence / Privilege Escalation

  * Account Manipulation — T1098
  * Create Account — T1136
  * Domain Account — T1136.002

## Key Screenshots

Screenshots were captured throughout the lab to document configuration and results, including:

* Azure resource group and virtual network setup
* Domain controller promotion
* Active Directory users, groups, and OU structure
* Windows client domain join
* Sysmon logs
* Splunk Enterprise installation
* Splunk receiving port configuration
* Universal Forwarder installation
* Sysmon log ingestion in Splunk
* Recon command detection
* Scheduled Splunk alert
* Failed logon detection
* Active Directory account/group change detection
* SOC dashboard

Screenshots are stored in:

`screenshots/`

## What I Learned

Through this project, I learned how endpoint and identity logs are generated, forwarded, searched, and analyzed in a SIEM environment. I gained hands-on experience with Active Directory, Sysmon, Splunk Universal Forwarder, SPL detections, scheduled alerts, dashboards, and incident documentation.

Key lessons included:

* How Active Directory centralizes users, groups, computers, and authentication
* How Sysmon provides deeper endpoint visibility than default Windows logs
* How Splunk Universal Forwarder sends selected logs to Splunk Enterprise
* How `inputs.conf` defines what logs are collected
* How Splunk receiving port `9997` is used for forwarded data
* How SPL can be used to detect suspicious behavior
* How scheduled alerts turn SPL searches into monitoring rules
* How dashboards summarize multiple security searches
* How incident reports communicate findings clearly
* How MITRE ATT&CK maps raw log activity to attacker behavior

## Future Improvements

Future improvements for this lab could include:

* Add a dedicated Splunk server instead of running Splunk on the domain controller
* Add a Kali Linux attacker VM for Nmap/network reconnaissance testing
* Add Windows Firewall logging for stronger network scan visibility
* Improve Sysmon field extraction with Splunk add-ons
* Add more detections for suspicious PowerShell, account lockouts, lateral movement, and credential access
* Configure additional scheduled alerts
* Expand the dashboard with charts, counts, and trend visualizations
* Add more incident reports and response playbooks

## Project Status

Core lab build is complete. Detection files, scheduled alerting, dashboard creation, and incident reporting were completed as part of the SOC monitoring workflow.

