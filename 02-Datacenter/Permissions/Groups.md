# Groups

---

## Overview

Groups allow administrators to organize multiple users into a single logical unit. Instead of assigning permissions to individual users, permissions can be assigned to a group, making access management simpler and more consistent.

Users inherit the permissions assigned to the groups they belong to. This approach reduces administrative effort and helps maintain a standardized access control model.

---

## When to Use

Use groups to:

- Organize users by department or team.
- Assign the same permissions to multiple users.
- Simplify user administration.
- Reduce repetitive permission assignments.
- Manage user access more efficiently.

---

## Prerequisites

Before managing groups, ensure that:

- You have administrator privileges.
- The required users have already been created.
- The required roles and permissions have been identified.

---

# Access the Groups Page

1. Log in to the VM2Cloud VE web interface.
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

## Step 1: Open the Create Group Window

1. Click **Create**.

The **Create: Group** window opens.

---

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

## Step 2: Configure the Group

Enter the required information.

| Field | What it does |
|---|---|
| **Name** | Group identifier, used when assigning permissions. Cannot be changed after creation. |
| **Comment** | Free text. Record which team the group represents and who owns it. |

Name the group after the team or role it represents rather than the permission it currently holds — permissions change, teams persist.

Review the configuration before continuing.

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

> **Note:** The Group Name cannot be modified after the group has been created.

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

| Field | Editable |
|---|---|
| **Name** | No. Fixed at creation. To rename, create a new group, move the members and permissions, then remove the old one. |
| **Comment** | Yes. |

After making the required changes:

1. Click **OK**.

The updated information is saved.

---

### Screenshot 6

```text
[ Place Screenshot Here ]
```

---

# Add Users to a Group

Adding users to a group allows them to inherit the permissions assigned to that group.

## Step 1: Select the Group

1. Select the required group.

## Step 2: Add Members

1. Click **Members**.
2. Click **Add**.
3. Select one or more users.
4. Click **Add**.

The selected users become members of the group.

---

### Screenshot 7

```text
[ Place Screenshot Here ]
```

---

# Remove Users from a Group

Removing a user from a group immediately removes the permissions inherited from that group.

## Steps

1. Select the required group.
2. Open the **Members** tab.
3. Select the user.
4. Click **Remove**.
5. Confirm the operation.

---

### Screenshot 8

```text
[ Place Screenshot Here ]
```

---

# Delete a Group

> **Warning:** Deleting a group removes the group, its memberships, and any permissions assigned to it. Users who were members of the group will lose the permissions inherited from it.

## Steps

1. Select the required group.
2. Click **Remove**.
3. Review the confirmation message.
4. Click **Yes**.

The group is permanently removed.

---

### Screenshot 9

```text
[ Place Screenshot Here ]
```

---

# Verification

Verify the following:

- The group appears in the Groups list.
- Users are correctly added to or removed from the group.
- Group information is updated successfully.
- Group members inherit the expected permissions.
- Deleted groups no longer appear in the Groups list.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Unable to create a group | Verify that the group name is unique and all required fields are completed. |
| User cannot be added to the group | Confirm that the user account exists and is available in the selected authentication realm. |
| Group permissions are not applied | Verify that the appropriate role and permissions have been assigned to the group. |
| Unable to edit the group | Ensure that you have sufficient administrative privileges. |
| User still has access after being removed from the group | Check whether the user has direct permissions or belongs to another group with the same access. |
| Unable to delete the group | Verify that you have administrator privileges and that the correct group is selected. |

---

# Summary

Groups simplify access control by allowing administrators to manage permissions for multiple users through a single group. By creating groups, adding or removing members, updating group information, and deleting unused groups, administrators can maintain a secure and well-organized VM2Cloud VE environment.
