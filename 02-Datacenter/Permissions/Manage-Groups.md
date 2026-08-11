# Manage Groups

---

## Overview

Groups allow administrators to organize multiple users into a single logical unit. Instead of assigning permissions to individual users, permissions can be assigned to a group, making user administration simpler and more consistent.

When a user is added to a group, the user inherits the permissions assigned to that group.

---

## When to Use

Use groups when you need to:

- Organize users by department or team.
- Simplify permission management.
- Assign the same permissions to multiple users.
- Reduce administrative overhead.
- Manage user access consistently.

---

## Prerequisites

Before managing groups, ensure that:

- You have administrator privileges.
- The required users have already been created.
- The required access roles have been identified.

---

# Access the Groups Page

1. Log in to the VM2Cloud web interface.
2. Select **Datacenter**.
3. Click **Permissions**.
4. Click **Groups**.

The Groups page displays all configured groups.

---

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

# Create a Group

## Step 1: Open the Add Group Window

1. Click **Create**.

The Create Group window opens.

---

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

## Step 2: Configure the Group

Enter the required information.

Typical fields include:

- Group Name
- Comment (Optional)

Review the information.

---

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

## Step 3: Create the Group

1. Click **Create**.

The new group appears in the Groups list.

---

### Screenshot 4

```text
[ Place Screenshot Here ]
```

---

# Edit a Group

## Step 1: Select the Group

1. Select the required group.
2. Click **Edit**.

---

### Screenshot 5

```text
[ Place Screenshot Here ]
```

---

## Step 2: Update the Group

Modify the required information.

Typical editable fields include:

- Comment

> **Note:** The group name cannot be changed after the group has been created.

Click **OK** to save the changes.

---

### Screenshot 6

```text
[ Place Screenshot Here ]
```

---

# Add Users to a Group

## Step 1: Select the Group

1. Select the required group.

---

## Step 2: Add Members

1. Click **Members**.
2. Click **Add**.
3. Select one or more users.
4. Click **Add**.

The selected users are added to the group.

---

### Screenshot 7

```text
[ Place Screenshot Here ]
```

---

# Remove Users from a Group

1. Select the required group.
2. Open the **Members** tab.
3. Select the user.
4. Click **Remove**.
5. Confirm the operation.

Removing a user from a group immediately removes any permissions inherited from that group.

---

### Screenshot 8

```text
[ Place Screenshot Here ]
```

---

# Delete a Group

> **Warning:** Deleting a group removes the group and any permissions assigned to it. Users who were members of the group will lose the permissions inherited from the group.

## Steps

1. Select the required group.
2. Click **Remove**.
3. Confirm the deletion.

---

### Screenshot 9

```text
[ Place Screenshot Here ]
```

---

# Verification

Verify the following:

- The group appears in the Groups list.
- Users are correctly added or removed from the group.
- Group information is updated successfully.
- Assigned permissions are inherited by group members.
- Deleted groups no longer appear in the Groups list.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Unable to create a group | Verify that the group name is unique and all required fields are completed. |
| User cannot be added to the group | Confirm that the user account exists and is available in the selected authentication realm. |
| Group permissions are not applied | Verify that the appropriate role and permissions have been assigned to the group. |
| Unable to delete the group | Ensure that you have administrator privileges and that the correct group is selected. |
| User still has access after being removed from the group | Verify whether the user has direct permissions or belongs to another group with the same access. |

---

# Summary

Groups simplify user administration by allowing permissions to be assigned to multiple users at once. By organizing users into logical groups, administrators can efficiently manage access to VM2Cloud resources while reducing the complexity of individual permission assignments.
