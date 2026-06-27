# Lab Architecture

This SOC home lab was built in Microsoft Azure to simulate a small enterprise Windows environment.

## Network

- Resource Group: `rg-soc-lab-ad-detection`
- Virtual Network: `vnet-soc-lab`
- Subnet: `subnet-soc-lab`
- Address Space: `10.10.0.0/16`
- Subnet Range: `10.10.1.0/24`

## Systems

| Host | Role | IP Address |
|---|---|---|
| `DC-01` | Domain Controller, DNS, Active Directory, Splunk Enterprise | `10.10.1.10` |
| `WIN-CLIENT-01` | Domain-joined Windows endpoint, Sysmon, Splunk Universal Forwarder | `10.10.1.30` |

## Log Flow

`WIN-CLIENT-01` generated Windows Event Logs and Sysmon logs. Splunk Universal Forwarder sent those logs to Splunk Enterprise on `DC-01` over port `9997`.

`DC-01` generated Active Directory and Windows Security logs locally. Splunk collected those logs to detect account creation and group membership changes.

## Detection Pipeline

1. Activity was generated on the endpoint or domain controller.
2. Windows Security logs and Sysmon logs recorded the activity.
3. Logs were ingested into Splunk.
4. SPL searches were used to detect suspicious activity.
5. A scheduled Splunk alert monitored reconnaissance commands.
6. A Splunk dashboard summarized key SOC activity.
7. Incident reports documented findings and MITRE ATT&CK mapping.
