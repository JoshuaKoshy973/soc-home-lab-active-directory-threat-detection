# Print Service Outage

## Scenario

The Windows Print Spooler was stopped on the print server. The shared office-printer queue then presented an Offline symptom, preventing normal print processing until the service was restored.

## Initial scope and impact

- Affected service: Windows Print Spooler on `DC-01`
- Queue: `KH-DAL-Office-Printer-01`
- Scope to confirm during intake: multiple users or queues versus one client
- Potential impact: shared office printing unavailable and engineering work potentially delayed
- Priority guidance: raise priority when multiple users, a deadline, or a required plotter is affected

## Investigation path

1. Ask whether one user or multiple users are affected.
2. Check whether the shared printer is visible and whether its queue is Offline.
3. Confirm that `DC-01` is reachable.
4. Inspect the Print Spooler service state and startup type.
5. Review the PrintService Operational log and System log for supporting evidence.
6. Check for blocked jobs and whether the queue is paused.
7. Restore the service according to the lab procedure.
8. Reopen the queue and submit a test job.
9. Confirm recovery with the affected user or users.

## Evidence

The following evidence shows the outage and recovery sequence:

![Print Spooler stopped](../screenshots/11-print-spooler-stopped.png)

![Shared printer offline](../screenshots/12-shared-printer-offline.png)

![Print service restored](../screenshots/13-print-service-restored.png)

## Root cause and resolution

The simulated root cause was a stopped Print Spooler service. Recovery consisted of restoring the service and verifying that the shared office-printer queue was available again.

For a production incident, record the service-state change, event timestamps, affected queues, user impact, any queued jobs, the reason for the service interruption if known, and whether a restart or change approval was required.

## Customer communication

Users should receive a concise update stating the scope, current workaround if available, next update or expected test point, and confirmation after printing is restored. Avoid closing the ticket solely because the service restarted; validate the user-facing workflow.
