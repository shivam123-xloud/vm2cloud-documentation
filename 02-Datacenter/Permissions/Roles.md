# Roles

---

## Overview

Roles define the set of privileges that determine what actions a user or group can perform within VM2Cloud VE. A role itself does not grant access to any resource. Instead, roles are assigned through permissions, which specify where the privileges apply.

VM2Cloud VE includes several predefined roles for common administrative tasks. Custom roles can also be created to meet specific operational requirements.

---

## When to Use

Use roles when you need to:

- Control user privileges.
- Delegate administrative tasks.
- Apply the principle of least privilege.
- Create custom privilege sets.
- Simplify permission management.

---

## Prerequisites

Before managing roles, ensure that:

- You have administrator privileges.
- You understand the privileges required for the intended users.
- The required users or groups have already been created, if applicable.

---

# Access the Roles Page

1. Log in to the VM2Cloud VE web interface.
2. Select **Datacenter**.
3. Click **Permissions**.
4. Click **Roles**.

The Roles page displays all predefined and custom roles.

---

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

# View Available Roles

The Roles page displays information such as:

- Role ID
- Assigned Privileges

Select a role to view the privileges included in that role.

> **Note:** System roles are predefined by VM2Cloud VE and are available immediately after installation.

---

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

# Create a Custom Role

## Step 1: Open the Create Role Window

1. Click **Create**.

The Create Role window opens.

---

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

## Step 2: Configure the Role

Enter the required information.

Typical fields include:

- Role ID
- Privileges

Select the privileges that should be included in the role.

Review the configuration before continuing.

---

### Screenshot 4

```text
[ Place Screenshot Here ]
```

---

## Step 3: Create the Role

1. Click **Create**.

The new role appears in the Roles list.

---

### Screenshot 5

```text
[ Place Screenshot Here ]
```

---

# Edit a Custom Role

> **Note:** Only custom roles can be modified. System-defined roles cannot be edited.

## Steps

1. Select the custom role.
2. Click **Edit**.
3. Add or remove the required privileges.
4. Click **OK**.

---

### Screenshot 6

```text
[ Place Screenshot Here ]
```

---

# Delete a Custom Role

> **Warning:** Deleting a role removes it from VM2Cloud VE. Users or groups assigned to this role will lose the associated privileges.

## Steps

1. Select the custom role.
2. Click **Remove**.
3. Confirm the operation.

> **Note:** System-defined roles cannot be removed.

---

### Screenshot 7

```text
[ Place Screenshot Here ]
```

---

# Common System Roles

The following roles are commonly available in VM2Cloud VE.

| Role | Description |
|------|-------------|
| Administrator | Full administrative access to VM2Cloud VE resources. |
| Auditor | Read-only access for monitoring and auditing. |
| Resource Administrator | Manages virtual infrastructure resources. |
| Virtual Machine Administrator | Manages virtual machines. |
| Datastore Administrator | Manages storage resources. |
| Backup Operator | Performs backup and restore operations. |

> **Verify:** Capture the full list of predefined roles from Datacenter → Permissions → Roles
> and document the privileges each one grants.

---

# Verification

Verify the following:

- The custom role appears in the Roles list.
- The selected privileges are assigned correctly.
- Users or groups assigned to the role receive the expected privileges.
- Deleted custom roles no longer appear in the Roles list.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Unable to create a role | Verify that the Role ID is unique and at least one privilege has been selected. |
| Edit option is unavailable | Confirm that the selected role is a custom role and not a system-defined role. |
| Remove option is unavailable | System-defined roles cannot be deleted. Only custom roles can be removed. |
| Users cannot perform expected actions | Verify that the correct role has been assigned through the appropriate permission. |
| Privilege changes are not reflected | Confirm that the updated role has been saved and is assigned to the correct user or group. |

---

# Verification

After managing roles, verify that:

- The required role exists.
- The assigned privileges are correct.
- Users or groups inherit the expected privileges after the role is assigned through permissions.

---

# Summary

Roles define the privileges available within VM2Cloud VE and provide a flexible way to control administrative access. By combining roles with users, groups, and permissions, administrators can implement secure and well-structured access control throughout the virtualization environment.
