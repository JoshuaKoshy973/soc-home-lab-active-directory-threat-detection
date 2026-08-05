# Troubleshooting Runbook

Use this sequence for a Windows shared-folder access issue. Record the result of each check before changing permissions.

## Troubleshooting sequence

1. Confirm the user, device, share path, exact error, and scope.
2. Confirm `DC-01` is reachable from the client.
3. Confirm the SMB share exists and is available.
4. Determine whether the user can browse, read, create, edit, rename, or delete.
5. Check the user’s Active Directory group membership.
6. If membership changed, have the user sign out and back in to refresh the security token.
7. Check NTFS permissions in the folder’s Security tab.
8. Check share permissions in Advanced Sharing.
9. Check inheritance and remove unintended broad inherited entries when appropriate.
10. Determine the effective permission from the user’s group memberships and both permission layers.
11. Correct only the faulty layer.
12. Retest using the affected user and the same UNC path.
13. Document the root cause, resolution, and verification.

## Use the scope to narrow the search

- **One user affected:** Start with account state, group membership, logon token, or user-specific permissions.
- **An entire department affected:** Check department group permissions, inheritance, share permissions, and NTFS configuration.
- **All users affected:** Check server reachability, SMB service, network connectivity, and share availability.

## Practical checks

Compare the reported action with the permission required. A user may be able to browse and read while still lacking permission to create or modify files. Do not assume a successful connection proves write access.

Use the affected user’s UNC path for the final test. A local administrator test may bypass the user’s effective permissions and can produce a misleading result.

## Documentation standard

Close the ticket with the observed symptom, affected scope, checks performed, exact root cause, corrective action, and a user-facing verification. Keep lab incidents clearly identified as controlled exercises rather than production outages.
