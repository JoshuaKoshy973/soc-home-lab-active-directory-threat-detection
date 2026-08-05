# Troubleshooting Runbook

## Purpose

This runbook provides a repeatable first-response process for shared printers, engineering plotters, and the Windows print path in this lab.

## First-response sequence

1. Clarify the exact symptom and capture the exact error message.
2. Identify the user, workstation, printer or plotter, server, and office/location.
3. Determine scope: one user, one workstation, one application, one queue, or multiple users.
4. Assess business impact and urgency. Consider project deadlines, engineering drawings, client deliverables, and available workarounds.
5. Confirm the expected printer and whether the issue is connection, permission, queue, service, driver, network, or device related.
6. Check the least disruptive dependency first: client mapping, queue state, Print Spooler, server reachability, then printer port/device reachability.
7. Apply the smallest safe correction.
8. Submit a test job or open the shared queue from the affected user’s account.
9. Confirm the result with the user.
10. Document symptoms, scope, impact, root cause, resolution, verification, and communication.
11. Escalate when the evidence points to a vendor driver, physical device, network change, or unsupported application behavior.

## Decision guide

```text
One user affected?
├─ Yes → Verify account, group membership, printer permissions, mapping, profile, and token refresh.
└─ No  → Check queue, Print Spooler, print server, network path, and device status.

Only one application affected?
├─ Yes → Test another application and check application-specific print settings or driver behavior.
└─ No  → Continue through the shared Windows print path.

Queue offline or jobs blocked?
├─ Check queue pause state and job status.
├─ Check Print Spooler service and PrintService logs.
└─ Retest after the safest correction.

Network plotter unreachable?
├─ Confirm configured address and protocol.
├─ Test the expected TCP port; do not rely on ping alone.
├─ Check routing, subnet, VLAN, firewall, and recent changes.
└─ Confirm the physical device or escalate to the network/vendor team.
```

## Scenario procedures

### Single-user Access Denied

Evidence to collect:

- Exact error and printer share name
- `whoami` output and user identity
- Whether another user can access the same queue
- AD group membership for `KH-Printer-Users`
- Printer Security permissions
- Whether the user signed out and back in after access was corrected

Likely causes include missing group membership, missing Print permission, stale logon token, broken mapping, or local profile state. Do not grant Manage this printer or Manage documents to solve an ordinary Print-permission problem.

### Print Spooler outage or offline queue

Evidence to collect:

- Number of affected users and queues
- Queue state and any blocked jobs
- Print Spooler service state and startup type
- PrintService Operational and System event logs
- Whether the print server itself is reachable
- Test result after recovery

Restarting a service can be an appropriate lab recovery step, but production changes should follow local procedures and include user communication, impact assessment, and verification.

### Network plotter unreachable

Evidence to collect:

- Configured printer address and port
- Protocol, such as Raw/9100 or LPR
- Source workstation/server and interface
- `ping` result as one diagnostic signal
- `Test-NetConnection` result for the expected TCP port
- Recent office, VLAN, firewall, IP-address, or device changes

A failed ping is not conclusive because some devices block ICMP. A failed TCP connection to the configured printing port is stronger evidence that the configured network path is unavailable, but the device and network owner should still be consulted before changing production settings.

## Ticket documentation template

```text
Title:
Reported by:
User/device:
Affected service:
Exact symptom/error:
Scope:
Business impact:
Priority and reasoning:
Troubleshooting performed:
Evidence collected:
Root cause:
Resolution:
Verification:
User communication:
Escalation or follow-up:
```

## Escalation standard

Escalate with a useful evidence package rather than a vague statement that printing is broken. Include the affected users, device names, queue and server names, driver or port details, exact tests and results, relevant timestamps, business impact, workaround, and the specific decision needed from the next support level.
