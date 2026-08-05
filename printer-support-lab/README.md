# Windows Print Server and IT Service Desk Support Lab

This project extends an Active Directory lab into a small Windows print-support environment. It was built to practice the operational skills described in an entry-level IT Analyst role: Windows Server printing, printer and plotter support, Active Directory access control, networking fundamentals, troubleshooting, ticket prioritization, documentation, and customer communication.

## Lab objective

Demonstrate a repeatable support workflow for shared office printing:

1. Establish a known-good domain client and domain-controller connection.
2. Install and manage Windows Print and Document Services.
3. Create shared office-printer and engineering-plotter queues.
4. Control printer access through an Active Directory security group.
5. Connect a domain client to a shared printer.
6. Simulate user, service, queue, and network failures.
7. Capture evidence and document the complete incident lifecycle.

## Environment

| Component | Lab role |
| --- | --- |
| `DC-01` | Windows Server 2022, Active Directory Domain Services, DNS, and Print Server |
| `Client-01` | Domain-joined Windows client used for end-user validation |
| `soclab.local` | Lab Active Directory and DNS namespace shown in the evidence |
| `KH-Printer-Users` | AD security group used to control printer access |
| `KH-DAL-Office-Printer-01` | Simulated shared office-printer queue |
| `KH-DAL-Engineering-Plotter-01` | Simulated engineering-plotter queue |
| `10.10.1.50:9100` | Plotter TCP/IP endpoint used for connectivity testing |

## Evidence captured

The screenshots document the project progressively:

- Domain client validation and DNS/connectivity to `DC-01`
- Print Server role installation
- Print Management queues
- Office-printer sharing configuration
- Plotter TCP/IP port configuration
- AD printer-group membership and least-privilege Print permission
- Client connection to the shared printer
- Single-user access denied and successful restoration
- Print Spooler outage, offline queue symptom, and recovery verification
- Plotter connectivity diagnostic failure
- Jira intake/triage and resolution history

The screenshot index is in [`screenshots/README.md`](screenshots/README.md).

## Incidents demonstrated

### Single-user printer access

One user receives Access Denied while other users may remain operational. The troubleshooting path focuses on scope, account identity, AD group membership, printer permissions, token refresh, and user verification.

### Print-service outage

The Print Spooler is stopped, the shared queue appears offline, and printing is restored after service recovery. The workflow emphasizes service scope, queue state, user impact, safe recovery, and validation.

### Network plotter unreachable

The simulated plotter uses a Standard TCP/IP port with Raw printing on port 9100. Connectivity testing shows the configured endpoint is unreachable, which supports a network, address, port, or device-isolation troubleshooting path.

## Operational method

Every incident follows the same support sequence:

> Reported symptom → clarify → determine scope → assess business impact → identify dependencies → isolate the cause → apply the safest change → test → communicate → document → escalate when appropriate

This keeps technical troubleshooting connected to the user’s productivity and any project or client deadline.

## Security and production considerations

- Printer access is assigned through an AD security group rather than individual user permissions.
- Normal users receive Print permission only; printer-management permissions remain restricted.
- The lab places the Print Server role on `DC-01` for efficiency. A production environment should normally separate domain-controller and print-server responsibilities.
- The simulated queues do not replace testing with physical multifunction printers, copiers, or engineering plotters.
- Production changes would require approval, maintenance planning, driver testing, security review, rollback planning, and vendor-specific validation.

## Limitations

This is a focused personal lab. It does not reproduce regional office scale, real print hardware, vendor drivers, print-server redundancy, enterprise monitoring, change-management approvals, or every Microsoft 365 dependency. The screenshots show what was configured and tested in this environment; they are not claims of production experience.

## Why this matters for IT support

The project demonstrates initiative through a self-identified skills gap, but the larger lesson is operational: isolate the affected scope, prioritize based on business impact, communicate clearly, make the smallest safe change, verify with the user, and leave a useful record for the next technician.
