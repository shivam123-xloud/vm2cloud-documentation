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

# Predefined Roles

These roles exist by default and cannot be edited or deleted. Most access can be granted without ever creating a custom role.

| Role | Grants |
|---|---|
| **Administrator** | Every privilege. Unrestricted. |
| **NoAccess** | No privileges. Used deliberately to block inherited access on a specific path. |
| **PVEAdmin** | Almost everything except system-level node operations and realm management. |
| **PVEAuditor** | Read-only across the environment. The right choice for monitoring and reporting. |
| **PVEVMAdmin** | Full control of virtual machines and containers — create, configure, delete. |
| **PVEVMUser** | Use guests without reconfiguring them: console, power, backup, snapshot. |
| **PVETemplateUser** | View templates and create guests from them. |
| **PVEDatastoreAdmin** | Full storage administration, including adding and removing storage. |
| **PVEDatastoreUser** | Allocate space on storage and view its contents. Cannot reconfigure it. |
| **PVEPoolAdmin** | Manage pools and their membership. |
| **PVEPoolUser** | View pools. |
| **PVESysAdmin** | Node and system administration — services, network, updates. |
| **PVEUserAdmin** | Manage users, groups, and their permissions. |
| **PVESDNAdmin** | Manage SDN zones and VNets. |
| **PVESDNUser** | Use SDN networks without configuring them. |

**NoAccess** is the one worth understanding. Permissions inherit down the path hierarchy, so a role granted on `/` reaches everything below it. Assigning **NoAccess** on a narrower path carves out an exception — the usual way to say "this team administers everything except that one guest".

> **Verify:** Confirm this list against Datacenter → Permissions → Roles in your deployment.
> These are the platform's standard roles; a specific build may add or omit some. Also
> capture the privileges each one contains.

---

# Privileges

Privileges are what a role contains. You choose from these when creating a custom role.

| Group | Privileges | Controls |
|---|---|---|
| **VM** | `VM.Allocate`, `VM.Audit`, `VM.Clone`, `VM.Console`, `VM.Migrate`, `VM.Monitor`, `VM.PowerMgmt`, `VM.Backup`, `VM.Snapshot`, `VM.Snapshot.Rollback` | Creating, viewing, and operating guests |
| **VM config** | `VM.Config.CPU`, `VM.Config.Memory`, `VM.Config.Disk`, `VM.Config.Network`, `VM.Config.CDROM`, `VM.Config.Options`, `VM.Config.HWType`, `VM.Config.Cloudinit` | Changing individual parts of guest hardware |
| **Storage** | `Datastore.Allocate`, `Datastore.AllocateSpace`, `Datastore.AllocateTemplate`, `Datastore.Audit` | Managing storage and consuming capacity |
| **System** | `Sys.Audit`, `Sys.Console`, `Sys.Modify`, `Sys.PowerMgmt`, `Sys.Syslog`, `Sys.Incoming` | Node-level operations |
| **Access** | `User.Modify`, `Group.Allocate`, `Realm.Allocate`, `Realm.AllocateUser`, `Permissions.Modify` | Managing who can do what |
| **Pools** | `Pool.Allocate`, `Pool.Audit` | Creating and viewing pools |
| **SDN** | `SDN.Allocate`, `SDN.Audit`, `SDN.Use` | Software-defined networking |

The `VM.Config.*` split is the useful one for delegation. Granting `VM.Config.Memory` but not `VM.Config.Disk` lets a team resize memory on their own guests without touching storage allocation.

> **Verify:** Capture the privilege list from the Create Role dialog and confirm these
> names and groupings.

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

Verify the following:

- The role appears in the Roles list.
- Its privileges match what was intended.
- A user assigned the role through a permission gains exactly the expected access.
- That user **cannot** perform actions the role does not grant.
- Deleted custom roles no longer appear.

Test what the role does **not** allow, not only what it does. A role granting more than intended is only discovered when someone uses it.

---

# Best Practices

- Use a predefined role wherever one fits. Custom roles are another thing to maintain.
- Start from the least privileged role that could work, then add privileges if it proves insufficient.
- Use **PVEAuditor** for monitoring and reporting accounts rather than a custom read-only role.
- Use **NoAccess** to carve exceptions out of broad grants rather than restructuring the whole permission set.
- Name custom roles after the job they enable, not the person or team who first needed them.
- Review custom roles when responsibilities change — they tend to accumulate privileges and never lose them.
- Grant roles to groups on paths, never directly to individuals where a group would do.

---

# Summary

Roles define the privileges available within VM2Cloud VE and provide a flexible way to control administrative access. By combining roles with users, groups, and permissions, administrators can implement secure and well-structured access control throughout the virtualization environment.
