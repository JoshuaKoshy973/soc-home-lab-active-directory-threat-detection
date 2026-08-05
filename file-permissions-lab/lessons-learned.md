# Lessons Learned

- Share and NTFS permissions are separate layers; Windows evaluates both for network access.
- The more restrictive effective permission wins.
- Adding an AD group does not remove existing inherited access.
- Restricted parent permissions are cleaner in a new design, while existing environments may require inheritance cleanup.
- Users can belong to multiple access groups, so effective access must be evaluated across all memberships.
- Group-based permissions are easier to audit and manage than user-by-user assignments.
- Avoid explicit Deny unless there is a strong, documented reason to use it.
- Group membership changes may require a new logon token before the user sees the change.
- Successful troubleshooting requires reproducing and verifying the affected user’s experience.
- A controlled blind challenge is useful because it tests the troubleshooting process rather than recall of the change that was made.
