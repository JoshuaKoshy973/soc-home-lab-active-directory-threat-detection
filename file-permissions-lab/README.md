# File Permissions Lab

## Overview

This lab demonstrates group-based access control for Windows SMB shares and NTFS folders in a small Active Directory environment. It focuses on designing permissions, validating the user experience from a client, and troubleshooting access failures.

## Skills demonstrated

- Active Directory security groups and user access
- SMB share and NTFS permission separation
- Inheritance cleanup and least-privilege access
- UNC-path validation from a Windows client
- Structured incident troubleshooting and documentation

## Environment

| Component | Value |
|---|---|
| Domain | `SOCLAB.LOCAL` |
| Server | `DC-01` |
| Client | `WIN-CLIENT-01` |
| Test user | Alex Carter (`acarter`) |
| Local storage | `C:\CompanyShares` |

## Folder and share overview

| Folder | Share | Access group |
|---|---|---|
| Public | `\\DC-01\Public` | `KH-Public-Modify` |
| Finance | `\\DC-01\Finance` | `KH-Finance-Modify` |
| Engineering | `\\DC-01\Engineering` | `KH-Engineering-Modify` |

## Access model

| Principal | Public | Finance | Engineering |
|---|---:|---:|---:|
| Alex Carter | Modify | No access | Modify |
| Department group | Modify | Modify | Modify |
| SYSTEM / Administrators | Full Control | Full Control | Full Control |

Share permissions were kept broad with `Everyone` set to Full Control. NTFS permissions provide the practical restrictions, with no explicit Deny permissions used. Finance and Engineering use explicit permissions after inheritance cleanup.

## Incidents

Two controlled incidents were documented: a single-user Engineering access failure caused by missing group membership, and a blind Engineering write-access challenge caused by a read-only NTFS group permission. The second scenario was intentionally introduced in the lab and was not a production outage.

## Key outcome

Alex’s access was validated against all three shares, the intended restrictions were confirmed, and both incidents were resolved and documented through a repeatable troubleshooting process.

## Documentation

- [Setup and validation](setup-and-validation.md)
- [Troubleshooting runbook](troubleshooting-runbook.md)
- [Single-user access incident](incidents/single-user-access-denied.md)
- [Department permission incident](incidents/department-permission-error.md)
- [Lessons learned](lessons-learned.md)
- [Screenshot index](screenshots/README.md)
