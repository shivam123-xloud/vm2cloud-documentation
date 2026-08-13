# Reset Root Password

---

## Overview

This page covers regaining access when a password is lost.

Which procedure you need depends on **which account** is locked out, and that depends on the realm:

| Account type | What the password is | Recovery |
|---|---|---|
| `root@pam` | The node's Linux root password | UI if another administrator can log in; otherwise console recovery |
| Other `@pam` users | That Linux user's password | Same as above |
| `@pve` users | Stored in the VM2Cloud VE user database | Any administrator can reset it in the interface |
| LDAP / Active Directory users | Held by the directory service | Reset in the directory, not in VM2Cloud VE |

The `root@pam` account is the one that causes real trouble, because its password is the node's system root password. If no other administrator account can log in, recovery requires console access to the node.

For how realms work, see [Authentication Realms](../02-Datacenter/Permissions/Authentication-Realms.md).

---

## When to Use

Use this page when:

* The `root@pam` password is unknown and nobody can log in.
* A `@pve` user has forgotten their password.
* An administrator has left and their account must be secured.
* A password must be rotated after a suspected compromise.

If you can still log in as *any* account with administrator privileges, start with Method 1 — it does not require downtime.

---

## Prerequisites

**For Method 1 (interface):**

* An account that can still log in, with permission to change user passwords.

**For Method 2 (console recovery):**

* Console access to the node — out-of-band management (iDRAC, iLO, IPMI), a hypervisor console for a virtual node, or physical access with keyboard and monitor.
* A maintenance window. **The node must be rebooted, which stops every guest running on it.**
* Knowledge of which guests will be affected.

---

# Method 1 — Reset From the Interface

Works when at least one administrator account can still log in. No downtime.

## Step 1: Open the Users Panel

1. Log in as an account with administrator privileges.
2. Select **Datacenter**.
3. Click **Permissions**.
4. Click **Users**.

---

### Screenshot 1

**Users Panel**

```text
[ Place Screenshot Here ]
```

> **Capture:** Datacenter → Permissions → Users, showing the user list with the
> **Password** button visible in the toolbar.

---

## Step 2: Set the New Password

1. Select the user whose password must be reset.
2. Click **Password**.
3. Enter the new password and confirm it.
4. Click **OK**.

For a `@pam` user, including `root@pam`, this changes the underlying Linux password on the node. For a `@pve` user it updates the VM2Cloud VE user database.

---

### Screenshot 2

**Set Password Dialog**

```text
[ Place Screenshot Here ]
```

> **Capture:** The password change dialog for a selected user, showing the password and
> confirmation fields.

---

## Step 3: Verify

1. Log out, or use a private browser window.
2. Log in with the new credentials.
3. Confirm the expected permissions are present.

> **Verify:** Confirm that changing a `@pam` user's password through this dialog updates
> the node's Linux password in this deployment, and whether the change applies to one
> node or across the cluster.

---

# Method 2 — Console Recovery

Use only when no account can log in. This requires a reboot.

> **Warning:** This procedure reboots the node. Every guest running on it stops. On a cluster, plan for the node leaving and rejoining, and for any HA resources it holds being recovered elsewhere. Do this in a maintenance window.

## Step 1: Open a Console

Connect through out-of-band management, the hypervisor console, or physically at the machine. You need to see the boot process, so a network SSH session is not sufficient.

---

### Screenshot 3

**Console Access to the Node**

```text
[ Place Screenshot Here ]
```

> **Capture:** An out-of-band or hypervisor console session attached to a node, before
> reboot.

---

## Step 2: Reboot Into the Boot Menu

1. Reboot the node.
2. When the boot menu appears, interrupt it before it continues — usually by pressing an arrow key.
3. Highlight the normal boot entry.
4. Press **e** to edit it.

The boot menu appears only briefly. If the node boots normally, reboot and try again.

---

## Step 3: Boot Into a Root Shell

1. Find the line beginning with `linux`.
2. Move to the end of that line.
3. Append:

```text
init=/bin/bash
```

4. Boot the edited entry — usually **Ctrl-X** or **F10**.

The node boots to a root shell instead of starting normally. This edit is not saved; a later normal reboot is unaffected.

> **Verify:** Confirm the boot loader in use and the exact keys for editing and booting
> an entry in this deployment.

---

### Screenshot 4

**Editing the Boot Entry**

```text
[ Place Screenshot Here ]
```

> **Capture:** The boot menu entry open for editing, with the `linux` line visible and
> `init=/bin/bash` appended.

---

## Step 4: Remount the Filesystem as Writable

The root filesystem is mounted read-only at this point.

```bash
mount -o remount,rw /
```

If this fails, the password cannot be changed. Check the message before continuing.

---

## Step 5: Set the New Password

```bash
passwd root
```

Enter the new password twice.

Choose something you can type accurately on a console keyboard, which may have a different layout from the one you normally use.

---

## Step 6: Flush and Reboot

```bash
sync
```

Then reboot. Because the normal init system is not running, a clean shutdown command may not be available:

```bash
exec /sbin/init
```

If that does not work, force a reboot from the out-of-band management interface or by power-cycling the machine.

> **Warning:** Always run `sync` before rebooting. Skipping it can leave the password change unwritten, and you will be locked out again after the reboot.

---

## Step 7: Verify

1. Let the node boot normally.
2. Log in to the web interface as `root@pam` with the new password.
3. Confirm the node reports online in the cluster.
4. Confirm guests that should have started are running.
5. Confirm HA resources have settled.

---

### Screenshot 5

**Successful Login After Recovery**

```text
[ Place Screenshot Here ]
```

> **Capture:** The VM2Cloud VE login screen and the resulting dashboard after logging in
> with the reset password.

---

# Configuration / Options

| Command | Purpose |
|---|---|
| `mount -o remount,rw /` | Make the root filesystem writable in the recovery shell. |
| `passwd root` | Set a new root password. |
| `passwd <username>` | Set a new password for another Linux user. |
| `sync` | Flush pending writes to disk before rebooting. |

---

# Verification

Verify the following:

* Login succeeds with the new password.
* The node reports **Online** in the cluster.
* The cluster is quorate.
* Guests on the node started as expected.
* HA resources are in their expected state.
* Any other administrator accounts still work.
* No unexpected authentication failures appear in the log.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Boot menu does not appear | It may be configured to pass quickly. Reboot and press an arrow key immediately and repeatedly. |
| `mount -o remount,rw /` fails | Read the error. An encrypted or unusual root layout may need a rescue image instead. |
| Password change appears not to have applied | `sync` was not run before rebooting. Repeat the procedure. |
| Locked out again after reboot | As above, or the change was made to a different installation. Confirm you edited the correct boot entry. |
| Cannot type the password correctly on the console | The console keyboard layout may differ. Choose a password using unambiguous characters. |
| Node did not rejoin the cluster | See [Cluster Troubleshooting](../02-Datacenter/Cluster/Cluster-Troubleshooting.md). |
| Guests did not start after reboot | **Start at boot** may not be enabled. See [VM Options](../04-Virtual-Machines/VM-Options.md) and [Container Options](../05-Containers/CT-Options.md). |
| A `@pve` user still cannot log in | Confirm the account is enabled and not expired. See [Users](../02-Datacenter/Permissions/Users.md). |
| An LDAP or AD user cannot log in | The password lives in the directory service. Reset it there. |

---

# Best Practices

- **Keep more than one administrator account**, so a lost password never requires a reboot. This single measure avoids Method 2 entirely.
- Store the root password in a password manager the whole team can reach.
- Configure and test out-of-band management before you need it.
- Rotate the root password when an administrator leaves.
- Prefer SSH keys over passwords for shell access.
- Enable [two-factor authentication](../02-Datacenter/Permissions/Two-Factor-Authentication.md) on administrator accounts.
- Use `@pve` accounts for day-to-day administration and keep `root@pam` for recovery, so routine work never depends on the account that is hard to reset.
- Record any console recovery — an unexplained reboot should not be a mystery later.

---

# Related Documentation

- [Users](../02-Datacenter/Permissions/Users.md)
- [Authentication Realms](../02-Datacenter/Permissions/Authentication-Realms.md)
- [Two-Factor Authentication](../02-Datacenter/Permissions/Two-Factor-Authentication.md)
- [Permissions Troubleshooting](../02-Datacenter/Permissions/Permissions-Troubleshooting.md)
- [Shell](Shell.md)
- [Reboot Node](Reboot-Node.md)
- [Node Troubleshooting](Node-Troubleshooting.md)
- [Cluster Troubleshooting](../02-Datacenter/Cluster/Cluster-Troubleshooting.md)

---

# Summary

How you recover a lost password depends on the realm. A `@pve` user can be reset by any administrator in the interface. A `@pam` user, including `root@pam`, is a Linux account on the node — it can also be reset from the interface, but only while some administrator can still log in.

When no account can log in, recovery means console access, editing the boot entry to reach a root shell, remounting the filesystem writable, and running `passwd`. That reboots the node and stops its guests, so it needs a maintenance window.

The way to never need Method 2 is to keep a second administrator account and store the root password somewhere the team can reach.
