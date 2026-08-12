# Container Permissions

---

## Overview

The **Permissions** tab shows who has access to this container, and lets you grant access to it directly.

Permissions are assigned on a **path** in the resource hierarchy. This tab works on the path for this container — `/vms/<vmid>` — so grants made here apply to this container alone.

It also lists permissions **inherited** from higher up: a role granted on `/` or on a pool containing this container appears here too.

For the permission model itself, see [Assign Permissions](../02-Datacenter/Permissions/Assign-Permissions.md). For the virtual machine equivalent of this panel, see [VM Permissions](../04-Virtual-Machines/VM-Permissions.md) — the two behave identically.

> **Note:** Containers and virtual machines share the same `/vms/<vmid>` path namespace, because guest IDs are unique across both. A permission on `/vms/101` applies to whichever guest holds ID 101.

---

## When to Use

Use this tab when you need to:

* See who currently has access to a container.
* Give one person or group access to one specific container.
* Check whether access is direct or inherited.
* Remove someone's access.
* Investigate why a user can, or cannot, see a container.

---

## Prerequisites

* You have permission to modify permissions on this container.
* The user or group already exists.
* You know which [role](../02-Datacenter/Permissions/Roles.md) grants the access needed.

---

# Procedure

## Step 1: Open the Permissions Tab

1. Log in to the VM2Cloud web interface.
2. Expand the node in the resource tree.
3. Select the container.
4. Click **Permissions**.

---

### Screenshot 1

**Container Permissions Tab**

```text
[ Place Screenshot Here ]
```

> **Capture:** A container → Permissions, showing both a direct grant and an inherited
> one, with the **Add** and **Remove** buttons visible.

---

## Step 2: Read the Permission List

| Column | Meaning |
|---|---|
| **Path** | Where the permission was granted. `/vms/101` is direct; `/` or `/pool/name` is inherited. |
| **User / Group** | Who it applies to. |
| **Role** | What they can do. |
| **Propagate** | Whether it extends to objects below the path. |

Read the **Path** column first. A permission granted at a broader path cannot be removed from this tab — go to the path that granted it.

---

## Step 3: Grant Access to This Container

1. Click **Add**.
2. Choose **Group Permission** or **User Permission**.
3. Select the group or user.
4. Select the **Role**.
5. Confirm.

Prefer granting to a **group**. Membership changes are then a single edit rather than a sweep across containers.

---

### Screenshot 2

**Adding a Permission**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Add Permission dialog opened from a container's Permissions tab.

---

## Step 4: Consider Console Access

Granting access to a container is a stronger decision than it looks.

A role that permits console access gives a shell **inside** the container. For an unprivileged container that shell is confined to the container; for a **privileged** container the isolation from the host is substantially weaker.

Before granting console access to a privileged container, decide whether you would give the same person a shell on the node. See [Container Options](CT-Options.md) for the privilege model.

---

## Step 5: Verify the Access

1. Ask the user to log in, or use a test account.
2. Confirm they can see the container.
3. Confirm they can perform the intended actions.
4. Confirm they cannot perform unintended ones.

---

## Step 6: Remove Access

1. Select the permission.
2. Click **Remove**.
3. Confirm.

Only permissions granted at this path can be removed here.

> **Warning:** Removal takes effect immediately. If it was the only grant, the user loses access without warning.

---

# Configuration / Options

| Option | Description |
|---|---|
| **Path** | Fixed to this container when adding from this tab. |
| **User** or **Group** | Who the permission applies to. Prefer groups. |
| **Role** | The privileges granted. See [Roles](../02-Datacenter/Permissions/Roles.md). |
| **Propagate** | Whether the permission extends beneath the path. |

> **Verify:** Capture the Add Permission dialog from a container Permissions tab and
> confirm the exact field labels.

---

# Verification

Verify the following:

* The permission appears with the correct path, user or group, and role.
* The user can see the container.
* The user can perform the intended actions.
* The user cannot perform unintended ones.
* Console access is granted only where intended, particularly for privileged containers.
* Inherited permissions are as expected.
* Removal actually revokes access.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| User cannot see the container | Confirm the account is enabled and the realm is correct. |
| User can see more than intended | A broader path grants it. Check the Path column and datacenter-level permissions. |
| Cannot remove a permission | It was granted at a broader path. Remove it there. |
| User still has access after removal | They also hold it via a group, a pool, or a broader path. |
| User has console access unexpectedly | Check what the assigned role actually grants. |
| Permission applies to the wrong guest | Containers and VMs share the `/vms/<vmid>` namespace. Confirm the ID. |
| Per-container grants have become unmanageable | Use [pools](../02-Datacenter/Permissions/Pools.md). |
| Permission missing after a restore | Permissions are stored separately from the container. Re-apply after restoring to a new ID. |

---

# Best Practices

- Grant to **groups**, not individual users.
- Use [pools](../02-Datacenter/Permissions/Pools.md) for anything affecting more than one container.
- Grant the least privileged role that does the job.
- Treat console access to a **privileged** container as equivalent to node access, and grant it accordingly.
- Read the Path column before attempting changes.
- Verify by testing what a user cannot do.
- Review permissions when people change role or leave.
- Document why any per-container exception exists.

---

# Related Documentation

- [Assign Permissions](../02-Datacenter/Permissions/Assign-Permissions.md)
- [Permissions Overview](../02-Datacenter/Permissions/Permissions-Overview.md)
- [Users](../02-Datacenter/Permissions/Users.md)
- [Groups](../02-Datacenter/Permissions/Groups.md)
- [Roles](../02-Datacenter/Permissions/Roles.md)
- [Pools](../02-Datacenter/Permissions/Pools.md)
- [Container Options](CT-Options.md)
- [Container Console](Container-Console.md)
- [VM Permissions](../04-Virtual-Machines/VM-Permissions.md)

---

# Summary

The Permissions tab shows who can access a container and lets you grant access directly, listing both grants made here and those inherited from broader paths. The Path column distinguishes them, and an inherited permission must be removed where it was granted.

One consideration is specific to containers: a role permitting console access gives a shell inside the container, and for a **privileged** container the separation from the host is weak. Treat that grant as close to giving node access. For anything beyond a one-off exception, use pools rather than per-container grants.
