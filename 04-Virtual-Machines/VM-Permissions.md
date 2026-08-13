# VM Permissions

---

## Overview

The **Permissions** tab shows who has access to this virtual machine, and lets you grant access to it directly.

Permissions in VM2Cloud VE are assigned on a **path** in the resource hierarchy. This tab works on the path for this machine — `/vms/<vmid>` — so anything granted here applies to this machine alone.

It also shows permissions **inherited** from higher up: a role granted on `/` or on a pool containing this machine appears here too, because those grants apply to it.

For the permission model itself — users, groups, roles, and how paths work — see [Assign Permissions](../02-Datacenter/Permissions/Assign-Permissions.md). This page covers the per-machine panel.

> **Note:** Granting access here is convenient but does not scale. Ten machines needing the same access means ten grants to maintain. Put those machines in a [pool](../02-Datacenter/Permissions/Pools.md) and grant once instead.

---

## When to Use

Use this tab when you need to:

* See who currently has access to a machine.
* Give one person or group access to one specific machine.
* Check whether access comes from this machine or is inherited.
* Remove someone's access to a machine.
* Investigate why a user can, or cannot, see a machine.

---

## Prerequisites

* You have permission to modify permissions on this machine.
* The user or group already exists. See [Users](../02-Datacenter/Permissions/Users.md) and [Groups](../02-Datacenter/Permissions/Groups.md).
* You know which [role](../02-Datacenter/Permissions/Roles.md) grants the access needed.

---

# Procedure

## Step 1: Open the Permissions Tab

1. Log in to the VM2Cloud VE web interface.
2. Expand the node in the resource tree.
3. Select the virtual machine.
4. Click **Permissions**.

Existing permissions affecting this machine are listed.

---

### Screenshot 1

**VM Permissions Tab**

```text
[ Place Screenshot Here ]
```

> **Capture:** A virtual machine → Permissions, showing both a direct grant and an
> inherited one, with the **Add** and **Remove** buttons visible.

---

## Step 2: Read the Permission List

| Column | Meaning |
|---|---|
| **Path** | Where the permission was granted. `/vms/100` is direct; `/` or `/pool/name` is inherited. |
| **User / Group** | Who it applies to. |
| **Role** | What they can do. |
| **Propagate** | Whether it extends to objects below the path. |

The **Path** column is the one to read carefully. It tells you whether a permission was set here or somewhere broader — and a permission granted at a broader path cannot be removed from this tab. Go to the path that granted it.

---

## Step 3: Grant Access to This Machine

1. Click **Add**.
2. Choose **Group Permission** or **User Permission**.
3. Select the group or user.
4. Select the **Role**.
5. Confirm.

The path is fixed to this machine, so there is nothing to choose there.

Prefer granting to a **group** rather than a user. When someone joins or leaves, you change group membership rather than hunting through machines.

---

### Screenshot 2

**Adding a Permission**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Add Permission dialog opened from a VM's Permissions tab, showing the
> group or user field and the role selector.

---

## Step 4: Verify the Access

1. Ask the user to log in, or use a test account.
2. Confirm they can see the machine.
3. Confirm they can perform the actions the role allows.
4. Confirm they **cannot** perform actions it does not allow.

That last check is the one that matters. A role granting more than intended is only discovered when someone uses it.

---

## Step 5: Remove Access

1. Select the permission.
2. Click **Remove**.
3. Confirm.

Only permissions granted **at this path** can be removed here. If the entry shows a broader path, remove it there instead.

> **Warning:** Removing a permission takes effect immediately. If it was the only grant giving someone access, they lose it without warning while working.

---

# Configuration / Options

| Option | Description |
|---|---|
| **Path** | Fixed to this machine when adding from this tab. |
| **User** or **Group** | Who the permission applies to. Prefer groups. |
| **Role** | The set of privileges granted. See [Roles](../02-Datacenter/Permissions/Roles.md). |
| **Propagate** | Whether the permission extends to objects beneath the path. |

> **Verify:** Capture the Add Permission dialog from a guest Permissions tab and confirm
> the exact field labels and whether Propagate is offered at this level.

---

# Verification

Verify the following:

* The permission appears in the list with the correct path, user or group, and role.
* The user can see the machine.
* The user can perform the intended actions.
* The user cannot perform unintended ones.
* Inherited permissions are as expected.
* Removing a permission actually revokes the access.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| User still cannot see the machine | Confirm the account is enabled and the realm is correct. See [Users](../02-Datacenter/Permissions/Users.md). |
| User can see more than intended | Another permission at a broader path grants it. Check the Path column and the datacenter-level permissions. |
| Cannot remove a permission | It was granted at a broader path. Remove it there. |
| User still has access after removal | They also hold it through a group, a pool, or a broader path. |
| Role does not allow an expected action | Check what the role actually grants. See [Roles](../02-Datacenter/Permissions/Roles.md). |
| Managing permissions machine by machine is unmanageable | That is the expected outcome. Use [pools](../02-Datacenter/Permissions/Pools.md). |
| Permission missing after a restore | Permissions are stored separately from the machine's disk. Re-apply after a restore to a new ID. |

---

# Best Practices

- Grant to **groups**, not individual users.
- Use [pools](../02-Datacenter/Permissions/Pools.md) whenever more than one machine needs the same access. Per-machine grants do not scale.
- Grant the least privileged role that lets the person do their work.
- Read the Path column before trying to change anything — most confusion comes from inherited permissions.
- Verify by testing what a user **cannot** do, not only what they can.
- Review permissions when someone changes role or leaves.
- Keep per-machine grants for genuine exceptions, and document why each one exists.

---

# Related Documentation

- [Assign Permissions](../02-Datacenter/Permissions/Assign-Permissions.md)
- [Permissions Overview](../02-Datacenter/Permissions/Permissions-Overview.md)
- [Users](../02-Datacenter/Permissions/Users.md)
- [Groups](../02-Datacenter/Permissions/Groups.md)
- [Roles](../02-Datacenter/Permissions/Roles.md)
- [Pools](../02-Datacenter/Permissions/Pools.md)
- [Permissions Troubleshooting](../02-Datacenter/Permissions/Permissions-Troubleshooting.md)
- [CT Permissions](../05-Containers/CT-Permissions.md)

---

# Summary

The Permissions tab shows who can access a virtual machine and lets you grant access to it directly. It lists both permissions set on the machine itself and those inherited from broader paths such as `/` or a pool — and the Path column is what distinguishes them, since an inherited permission cannot be removed from here.

Direct grants are useful for genuine exceptions. For anything repeated across machines, put them in a pool and grant once; managing access machine by machine stops being practical quickly.
