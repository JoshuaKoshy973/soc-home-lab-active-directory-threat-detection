# Screenshots

Evidence from the file-permissions lab is indexed below. Each file was captured during setup, validation, or controlled troubleshooting.

1. **01-company-shares-folder-structure.png** — Shows the Public, Finance, and Engineering folders under `C:\CompanyShares`.
2. **02-public-share-created.png** — Shows the Public folder configured as an SMB share.
3. **03-public-share-read-only-test.png** — Shows a client-side write attempt denied in the Public share.
4. **04-ad-file-access-groups.png** — Shows the Active Directory file-access security groups.
5. **05-engineering-group-membership.png** — Shows Alex Carter as a member of `KH-Engineering-Modify`.
6. **06-public-ntfs-modify-permission.png** — Shows the Public NTFS Modify permission for `KH-Public-Users`.
7. **07-finance-ntfs-modify-permission.png** — Shows the Finance NTFS Modify permission for `KH-Finance-Modify`.
8. **08-engineering-ntfs-modify-permission.png** — Shows the Engineering NTFS Modify permission for `KH-Engineering-Modify`.
9. **09-finance-restricted-ntfs-access.png** — Shows the restricted Finance ACL with explicit group-based access.
10. **10-engineering-restricted-ntfs-access.png** — Shows the restricted Engineering ACL with explicit group-based access.
11. **11-public-inheritance-disabled.png** — Shows Public inheritance cleanup and explicit permission entries.
12. **11-finance-access-denied-for-unauthorized-user.png** — Shows an unauthorized user denied access to Finance.
13. **12-engineering-modify-access-verified.png** — Shows a file successfully present in the Engineering share after write validation.
14. **13-engineering-access-denied.png** — Shows a single-user Engineering access failure.
15. **14-engineering-group-membership-missing.png** — Shows the Engineering access group without the affected user.
16. **15-engineering-access-restored.png** — Shows Engineering access restored and a test file created.
17. **16-powershell-permission-challenge-applied.png** — Shows the blind permission challenge completed without revealing its fault.
18. **17-jira-engineering-write-incident-open.png** — Shows the initial Jira ticket for the Engineering write issue.
19. **18-engineering-write-access-failure.png** — Shows the Access Denied result when writing to Engineering.
20. **19-engineering-ntfs-permission-root-cause.png** — Shows the Engineering group with Read and Execute but not Modify.
21. **20-engineering-write-access-restored.png** — Shows successful file creation after restoring write access.
22. **21-jira-engineering-incident-resolved.png** — Shows the resolved Jira incident with root cause and verification.
