# Delete User

---

## Overview

Deleting a user permanently removes the user account from VM2Cloud. After deletion, the user can no longer authenticate or access any VM2Cloud resources using that account.

> **Warning:** Deleting a user does not automatically remove permissions that were previously assigned to the account from audit logs or historical task records. Ensure that the account is no longer required before proceeding.

---

## When to Use

Delete a user when you need to:

- Remove access for a user who has left the organization.
- Remove temporary or expired accounts.
- Clean up unused user accounts.
- Remove accounts that are no longer required.

---

## Prerequisites

Before deleting a user, ensure that:

- You have administrator privileges.
- The user account exists.
- The account is no longer required.
- Any required data or audit information has been reviewed.

---

# Delete a User

## Step 1: Open the Users Page

1. Log in to the VM2Cloud web interface.
2. Select **Datacenter**.
3. Click **Permissions**.
4. Click **Users**.

The Users page displays all configured user accounts.

---

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

## Step 2: Select the User

1. Select the user account that you want to remove.
2. Review the account details to ensure the correct user is selected.

---

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

## Step 3: Remove the User

1. Click **Remove**.
2. Review the confirmation message.
3. Click **Yes** to confirm the deletion.

The user account is removed from VM2Cloud.

---

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

# Verification

Verify the following:

- The user no longer appears in the Users list.
- The account cannot be used to log in.
- Any direct permissions assigned to the user have been removed.
- Group memberships for the deleted user no longer exist.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Remove button is unavailable | Verify that your account has administrator privileges. |
| User cannot be deleted | Ensure that the correct user is selected and that no administrative restriction is preventing the deletion. |
| User still appears in the list | Refresh the page and verify that the deletion task completed successfully. |
| Login still works after deletion | Confirm that the user was deleted from the correct authentication realm and that another account with the same username does not exist in a different realm. |
| Permissions still appear in reports | Historical audit records may retain references to deleted users; this is expected behavior. |

---

# Alternative: Disable Instead of Delete

If you may need the account again in the future, consider disabling it instead of deleting it.

To disable a user:

1. Open the user account.
2. Click **Edit**.
3. Clear **Enable Account**.
4. Click **OK**.

Disabling preserves the account configuration while preventing login access.

---

# Summary

Deleting a user permanently removes the account from VM2Cloud and prevents further access to cluster resources. Before deleting an account, verify that it is no longer required and consider disabling it if future access may be needed.
