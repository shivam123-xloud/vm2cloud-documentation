# Assign Permissions

---

## Overview

Permissions determine which users or groups can access specific VM2Cloud resources and what actions they are allowed to perform. A permission is created by assigning a **Role** to a **User** or **Group** for a particular resource path.

VM2Cloud uses Role-Based Access Control (RBAC) to manage permissions, allowing administrators to securely control access across the virtualization environment.

---

## When to Use

Assign permissions when you need to:

- Grant users access to VM2Cloud resources.
- Assign administrative privileges.
- Restrict access to specific resources.
- Delegate management responsibilities.
- Control access using predefined or custom roles.

---

## Prerequisites

Before assigning permissions, ensure that:

- You have administrator privileges.
- The required user or group already exists.
- The required role is available.
- You know the resource that the permission should apply to.

---

# Access the Permissions Page

1. Log in to the VM2Cloud web interface.
2. Select **Datacenter**.
3. Click **Permissions**.

The Permissions page displays all configured permission entries.

---

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

# Create a Permission

## Step 1: Open the Add Permission Window

1. Click **Add**.

The Add Permission window opens.

---

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

## Step 2: Configure the Permission

Configure the required settings.

Typical options include:

- Path
- User or Group
- Role
- Propagate

Review the selected configuration before continuing.

### Path

Select the resource where the permission will apply.

Examples include:

- `/`
- `/nodes`
- `/storage`
- `/vms`

### User or Group

Select whether the permission will be assigned to:

- A User
- A Group

### Role

Select the required role that defines the available privileges.

### Propagate

Enable **Propagate** if the permission should automatically apply to child resources beneath the selected path.

---

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

## Step 3: Create the Permission

1. Click **Add**.

The permission appears in the Permissions list.

---

### Screenshot 4

```text
[ Place Screenshot Here ]
```

---

# Edit a Permission

> **Note:** Existing permissions cannot be edited directly. To change a permission, remove the existing permission and create a new one with the required configuration.

---

# Remove a Permission

## Step 1: Select the Permission

1. Select the required permission from the Permissions list.

---

## Step 2: Remove the Permission

1. Click **Remove**.
2. Confirm the operation.

The permission is removed immediately.

---

### Screenshot 5

```text
[ Place Screenshot Here ]
```

---

# Permission Scope

Permissions can be assigned to different resource levels.

| Resource Path | Applies To |
|--------------|------------|
| `/` | Entire VM2Cloud environment |
| `/nodes` | All nodes |
| `/nodes/<node>` | Specific node |
| `/storage` | All storage resources |
| `/storage/<storage>` | Specific storage |
| `/vms` | All virtual machines and containers |
| `/vms/<VMID>` | Specific virtual machine or container |

> **Note:** The available resource paths depend on your VM2Cloud configuration.

---

# Verification

Verify the following:

- The permission appears in the Permissions list.
- The correct user or group is assigned.
- The correct role is displayed.
- The selected resource path is correct.
- Users receive the expected level of access.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| User cannot access a resource | Verify that the correct permission has been assigned to the appropriate resource path. |
| Permission has no effect | Ensure that the selected role contains the required privileges. |
| User receives excessive access | Review inherited permissions and the Propagate option. |
| Unable to add a permission | Verify that the selected user, group, role, and resource path exist. |
| Access is still available after removing a permission | Check whether the user inherits permissions from another group or a higher-level resource path. |

---

# Best Practices

- Grant only the permissions required for a user's responsibilities.
- Assign permissions to groups whenever possible instead of individual users.
- Use the **Propagate** option carefully to avoid granting unintended access.
- Regularly review user permissions and remove unused entries.
- Follow the principle of least privilege.

---

# Summary

Permissions control access to VM2Cloud resources by assigning a role to a user or group for a specific resource path. Proper permission management helps secure the environment while ensuring users have the appropriate level of access to perform their responsibilities.
