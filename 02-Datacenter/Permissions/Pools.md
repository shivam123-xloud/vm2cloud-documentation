# Pools

---

## Overview

A **pool** is a logical grouping of virtual machines, containers, and storage that can be managed and permissioned as a single unit.

Pools solve a problem that grows with the environment: as guest count rises, granting permissions guest by guest becomes unmanageable. Assigning a role on a pool grants it across every member, and adding a guest to the pool extends that access automatically.

Pools are defined at **Datacenter → Permissions → Pools**.

### What pools are for

* **Permissions.** Grant a team access to their own guests without giving them access to everything.
* **Organisation.** Group by department, customer, project, or environment.
* **Backup selection.** A backup job can target a pool, so pool membership determines backup coverage. See [Create Backup Job](../Backup/Create-Backup-Job.md).
* **Navigation.** The resource tree can be viewed by pool.

A pool is an organisational and permission boundary, not a technical one. Members do not become isolated from each other, do not share resources, and are not placed together on a node. For placement control, see [Node Affinity](../HA/Node-Affinity.md) and [Resource Affinity](../HA/Resource-Affinity.md). For network isolation, see [Firewall Overview](../Firewall/Firewall-Overview.md).

---

## When to Use

Create pools when:

* Different teams manage different sets of guests.
* Guests belong to identifiable customers, departments, or projects.
* Permissions would otherwise be assigned guest by guest.
* Backup coverage should follow a logical grouping rather than a fixed list.
* You need a self-service boundary — letting a team manage their own guests only.

A single-team environment with a handful of guests may not need pools at all.

---

## Prerequisites

Before creating pools, ensure that:

* You have administrator privileges.
* You know which guests and storage belong in the pool.
* You know which users or groups need access.
* You have identified the role to grant. See [Roles](Roles.md).
* The cluster has quorum.

---

# Procedure

## Step 1: Open the Pools Panel

1. Log in to the VM2Cloud web interface.
2. Select **Datacenter** in the resource tree.
3. Click **Permissions**.
4. Click **Pools**.

The existing pools are listed.

---

### Screenshot 1

**Pools Panel**

```text
[ Place Screenshot Here ]
```

> **Capture:** Datacenter → Permissions → Pools, showing the pool list with the **Create**,
> **Edit**, and **Remove** buttons visible.

---

## Step 2: Create the Pool

1. Click **Create**.
2. In **Name**, enter an identifier such as `finance` or `customer-acme`.
3. In **Comment**, describe what the pool is for and who owns it.
4. Click **Create**.

The pool is created empty.

---

### Screenshot 2

**Create Pool Dialog**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Create Pool dialog, showing the Name and Comment fields.

---

## Step 3: Add Members

1. Select the pool.
2. Click **Add**.
3. Choose what to add:
   - **Virtual Machine** — select one or more guests.
   - **Storage** — select one or more storages.
4. Confirm.

A guest belongs to one pool at a time. Adding it to a second pool moves it.

> **Verify:** Confirm the exact controls used to add members to a pool, and whether
> virtual machines and containers are added through the same dialog.

---

### Screenshot 3

**Adding Guests to a Pool**

```text
[ Place Screenshot Here ]
```

> **Capture:** The dialog used to add virtual machines or containers to a pool, showing
> the selectable guest list.

---

### Screenshot 4

**Pool With Members**

```text
[ Place Screenshot Here ]
```

> **Capture:** A pool selected in Datacenter → Permissions → Pools, showing its member
> guests and storage in the members list.

---

## Step 4: Grant Permissions on the Pool

This is where pools earn their value.

1. Click **Permissions** under **Datacenter**.
2. Click **Add**, then select **Group Permission** or **User Permission**.
3. In **Path**, select the pool path, for example `/pool/finance`.
4. Select the **Group** or **User**.
5. Select the **Role** to grant.
6. Confirm.

Every member of the pool inherits that permission, and every guest added later inherits it automatically.

See [Assign Permissions](Assign-Permissions.md) for the full permission model.

---

### Screenshot 5

**Granting a Role on a Pool**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Add Permission dialog with a pool path selected, showing the Path,
> Group, and Role fields.

---

## Step 5: Verify Access

1. Log in as a user in the granted group, or ask them to.
2. Confirm they see the pool's guests.
3. Confirm they can perform the actions the role permits.
4. Confirm they cannot see or act on guests outside the pool.

The last check matters most. Verify the boundary holds, rather than assuming it.

---

## Step 6: Edit or Remove

**To change the comment:**

1. Select the pool.
2. Click **Edit**.
3. Update and click **OK**.

**To remove a member:**

1. Select the pool.
2. Select the member.
3. Click **Remove** and confirm.

Removing a member from a pool removes any access that was granted through it, and removes it from any backup job selecting that pool.

**To delete a pool:**

1. Remove all members.
2. Select the pool.
3. Click **Remove** and confirm.

> **Warning:** Deleting a pool removes every permission granted on it. Users who reached their guests only through the pool lose access immediately. It also removes those guests from any backup job that selected the pool — leaving them unprotected. Check both before deleting.

---

# Configuration / Options

| Option | Description |
|---|---|
| **Name** | Pool identifier, used in permission paths as `/pool/<name>`. Must be unique. |
| **Comment** | Description of the pool's purpose and owner. |

### Members

| Member type | Notes |
|---|---|
| **Virtual machines and containers** | A guest belongs to one pool at a time. |
| **Storage** | Storage may be added so pool members can use it. |

> **Verify:** Confirm the exact field labels in the Create Pool dialog and whether a
> pool name may be changed after creation.

---

# Verification

Verify the following:

* The pool appears in the Pools list.
* The intended guests and storage appear as members.
* The pool path is selectable when assigning permissions.
* Users in the granted group can access pool members.
* Those users **cannot** access guests outside the pool.
* A guest added to the pool becomes accessible to those users without further change.
* Backup jobs selecting the pool cover the current members.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| User cannot see pool guests | Confirm a role is granted on the pool path, not just that the pool exists. Membership alone grants nothing. |
| User sees more than expected | Another permission grants broader access, for example a role on `/`. Review all their permissions. |
| Guest disappeared from another pool | A guest belongs to one pool at a time. Adding it elsewhere moved it. |
| Cannot delete the pool | Members remain. Remove them first. |
| Access lost after deleting a pool | Permissions granted on the pool went with it. Re-grant at another path. |
| A guest is no longer backed up | It was removed from a pool that a backup job selects. See [Manage Backup Job](../Backup/Manage-Backup-Job.md). |
| New guest not accessible to the team | It was never added to the pool. Pool membership is not automatic. |
| Pool path not available when assigning permissions | Create the pool first; the path only exists once the pool does. |

---

# Best Practices

- Name pools after the team, customer, or project that owns them.
- Grant permissions on the pool, never on individual guests inside it — that defeats the purpose.
- Add a guest to its pool as part of creating it, so permissions and backup coverage apply from day one.
- Use pool-based selection in backup jobs so coverage follows membership.
- Grant the least privileged role that lets the team do their work. See [Roles](Roles.md).
- Verify the boundary by testing what a user *cannot* reach, not only what they can.
- Comment every pool with its owner, so the grouping still makes sense later.
- Review pool membership when guests are decommissioned.
- Check backup job coverage before removing a guest from a pool.

---

# Related Documentation

- [Permissions Overview](Permissions-Overview.md)
- [Assign Permissions](Assign-Permissions.md)
- [Users](Users.md)
- [Groups](Groups.md)
- [Roles](Roles.md)
- [Create Backup Job](../Backup/Create-Backup-Job.md)
- [Manage Backup Job](../Backup/Manage-Backup-Job.md)
- [Permissions Troubleshooting](Permissions-Troubleshooting.md)

---

# Summary

A pool groups guests and storage so they can be permissioned and managed together. Granting a role on the pool path gives a team access to everything in it, including guests added later, which turns per-guest permission management into a single assignment. Backup jobs can also select a pool, so membership drives backup coverage.

Membership alone grants nothing — a role must be assigned on the pool path. And deleting a pool takes its permissions with it and drops its guests out of any backup job selecting it, so check both before removing one.
