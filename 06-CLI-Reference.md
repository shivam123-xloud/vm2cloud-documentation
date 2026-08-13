# CLI Reference

---

## Overview

A reference to the VM2Cloud command-line tools, collected in one place.

The CLI has no location in the interface, so it cannot be mirrored into the folder structure the way every other section is. It lives here as an appendix.

**The interface remains the primary method.** Per the house rule, CLI belongs in a procedure only when the UI cannot perform the operation, when recovery requires it, when it provides useful verification, or when the official workflow depends on it. This page is a lookup, not an argument for working in a shell.

Commands run as root on a node — through the [Shell](03-Nodes/Shell.md) panel, over SSH, or at the console.

---

## When to Use

Reach for the CLI when:

* The operation has no interface equivalent — dissolving a cluster, recovering quorum.
* You are recovering from a failure that has taken the interface away.
* You need to verify something the interface summarises.
* You are scripting a repetitive operation across many guests.
* Support has asked for specific output.

Otherwise use the interface. It logs actions as tasks, applies safety checks, and is visible to your colleagues.

> **Warning:** CLI operations bypass the interface's confirmations, and some do not appear in [Task History](03-Nodes/Task-History.md). An action taken in a shell can be invisible to whoever investigates later. Prefer the interface for anything routine.

---

## Prerequisites

* Root access to a node.
* Confirmation that you are on the intended node — commands act locally unless stated otherwise.
* For cluster-wide operations, the cluster has quorum.

---

# Command Families

| Command | Covers |
|---|---|
| `qm` | Virtual machines |
| `pct` | Containers |
| `pvesm` | Storage |
| `pvecm` | Cluster membership and quorum |
| `pveum` | Users, groups, roles, permissions |
| `ha-manager` | High availability resources |
| `pvesr` | Storage replication |
| `pvenode` | Node-level tasks, certificates, ACME |
| `pveceph` | Ceph deployment and management |
| `pveam` | Container template downloads |
| `vzdump` | Backups |
| `qmrestore` / `pct restore` | Restoring backups |
| `pvesubscription` | Subscription keys |

Every command supports `help`:

```bash
qm help
qm help set
```

---

# Virtual Machines — `qm`

| Task | Command |
|---|---|
| List machines | `qm list` |
| Show status | `qm status <vmid>` |
| Start / stop | `qm start <vmid>` / `qm stop <vmid>` |
| Graceful shutdown | `qm shutdown <vmid>` |
| Show configuration | `qm config <vmid>` |
| Change a setting | `qm set <vmid> --<option> <value>` |
| Migrate | `qm migrate <vmid> <target-node>` |
| Clone | `qm clone <vmid> <newid>` |
| Convert to template | `qm template <vmid>` |
| Delete | `qm destroy <vmid>` |
| Open a monitor command | `qm monitor <vmid>` |

`qm set` is the main reason to use `qm` at all — it reaches configuration options the interface does not expose. Check `qm help set` for the full list.

Migration with a different target storage is another case the interface may not offer:

```bash
qm migrate 100 node2 --targetstorage ceph-pool
```

> **Warning:** `qm destroy` deletes the machine and its disks permanently, with no confirmation prompt. There is no undo. Check the ID twice.

UI equivalents: [Manage Virtual Machine](04-Virtual-Machines/Manage-Virtual-Machine.md), [Manage VM Hardware](04-Virtual-Machines/Manage-VM-Hardware.md), [Migrate](04-Virtual-Machines/Migrate-Virtual-Machine.md), [Convert to Template](04-Virtual-Machines/Convert-to-Template.md).

---

# Containers — `pct`

| Task | Command |
|---|---|
| List containers | `pct list` |
| Show status | `pct status <vmid>` |
| Start / stop | `pct start <vmid>` / `pct stop <vmid>` |
| Graceful shutdown | `pct shutdown <vmid>` |
| Enter the container | `pct enter <vmid>` |
| Run one command inside | `pct exec <vmid> -- <command>` |
| Show configuration | `pct config <vmid>` |
| Change a setting | `pct set <vmid> --<option> <value>` |
| Migrate | `pct migrate <vmid> <target-node>` |
| Clone | `pct clone <vmid> <newid>` |
| Delete | `pct destroy <vmid>` |

`pct enter` gives a shell inside the container without needing its network or credentials — useful when a container has lost connectivity.

UI equivalents: [Manage Container](05-Containers/Manage-Container.md), [Container Console](05-Containers/Container-Console.md), [CT Options](05-Containers/CT-Options.md).

---

# Backup and Restore

## Creating a backup

```bash
vzdump <vmid>
vzdump <vmid> --storage <storage-id> --mode snapshot
vzdump --all --mode snapshot --storage backup-nfs
```

| Option | Purpose |
|---|---|
| `--storage` | Target storage |
| `--mode` | `snapshot`, `suspend`, or `stop` |
| `--compress` | Compression algorithm |
| `--all` | Every guest on the node |

## Restoring

```bash
qmrestore <backup-file> <new-vmid>
pct restore <new-vmid> <backup-file>
```

**Restoring to a different ID is the main reason to use these.** It is how you test a backup without touching the original:

```bash
qmrestore /mnt/backup/dump/vzdump-qemu-100-2026_08_12.vma.zst 999
```

> **Warning:** Restoring to an **existing** ID overwrites that guest completely. Use an unused ID for test restores.

UI equivalents: [Backup Jobs Overview](02-Datacenter/Backup/Backup-Jobs-Overview.md), [Storage Content Browser](02-Datacenter/Storage/Storage-Content-Browser.md).

---

# Cluster — `pvecm`

| Task | Command |
|---|---|
| Show cluster status | `pvecm status` |
| List nodes | `pvecm nodes` |
| Create a cluster | `pvecm create <name>` |
| Join a cluster | `pvecm add <existing-node-ip>` |
| Remove a node | `pvecm delnode <name>` |
| Set expected votes | `pvecm expected <n>` |
| Regenerate certificates | `pvecm updatecerts` |
| Configure a QDevice | `pvecm qdevice setup <ip>` |

`pvecm delnode` and `pvecm expected` have **no interface equivalent**.

> **Warning:** `pvecm expected` disables split-brain protection. Read [Recover Quorum](02-Datacenter/Cluster/Recover-Quorum.md) before using it — running it while the "missing" nodes are actually alive corrupts shared storage.

UI equivalents: [Cluster Overview](02-Datacenter/Cluster/Cluster-Overview.md), [Remove Node](02-Datacenter/Cluster/Remove-Node-from-Cluster.md), [Recover Quorum](02-Datacenter/Cluster/Recover-Quorum.md).

---

# High Availability — `ha-manager`

| Task | Command |
|---|---|
| Show HA status | `ha-manager status` |
| Add a resource | `ha-manager add vm:<vmid>` |
| Change requested state | `ha-manager set vm:<vmid> --state <state>` |
| Migrate an HA resource | `ha-manager migrate vm:<vmid> <node>` |
| Relocate an HA resource | `ha-manager relocate vm:<vmid> <node>` |
| Remove a resource | `ha-manager remove vm:<vmid>` |

`ha-manager status` is the fastest way to see what HA currently believes about every resource — more direct than reading it from the interface.

Use `migrate` for a running guest and `relocate` when it must be stopped and started elsewhere.

UI equivalents: [HA Resources](02-Datacenter/HA/HA-Resources.md), [HA Overview](02-Datacenter/HA/HA-Overview.md).

---

# Storage — `pvesm`

| Task | Command |
|---|---|
| List storages and usage | `pvesm status` |
| List content on a storage | `pvesm list <storage-id>` |
| Add a storage | `pvesm add <type> <id> <options>` |
| Enable / disable | `pvesm set <id> --disable 0` / `--disable 1` |
| Remove a storage | `pvesm remove <id>` |
| Scan for ZFS pools | `pvesm zfsscan` |

`pvesm status` is the quickest check when a guest will not start for storage reasons — it shows availability and free space for everything at once.

UI equivalents: [Storage Overview](02-Datacenter/Storage/Storage-Overview.md), [Manage Storage](02-Datacenter/Storage/Manage-Storage.md).

---

# Users and Permissions — `pveum`

| Task | Command |
|---|---|
| List users | `pveum user list` |
| Add a user | `pveum user add <user>@<realm>` |
| Change password | `pveum passwd <user>@<realm>` |
| Delete a user | `pveum user delete <user>@<realm>` |
| List groups | `pveum group list` |
| Add a group | `pveum group add <group>` |
| List roles | `pveum role list` |
| Grant a permission | `pveum acl modify <path> --users <user> --roles <role>` |
| Revoke a permission | `pveum acl delete <path> --users <user> --roles <role>` |

Useful mainly for scripted onboarding, and for restoring administrative access when the interface is unreachable but the node is not.

UI equivalents: [Users](02-Datacenter/Permissions/Users.md), [Assign Permissions](02-Datacenter/Permissions/Assign-Permissions.md).

---

# Replication — `pvesr`

| Task | Command |
|---|---|
| Show job status | `pvesr status` |
| List jobs | `pvesr list` |
| Run a job now | `pvesr run --id <jobid>` |
| Create a job | `pvesr create-local-job <vmid>-<n> <target-node>` |
| Delete a job | `pvesr delete <jobid>` |

`pvesr status` shows the last sync time for every job in one view — the fastest way to check replication health across a node.

UI equivalents: [Replication Overview](02-Datacenter/Replication/Replication-Overview.md), [Node Replication](03-Nodes/Node-Replication.md).

---

# Node and Certificates — `pvenode`

| Task | Command |
|---|---|
| Show node config | `pvenode config get` |
| List tasks | `pvenode task list` |
| Show task log | `pvenode task log <upid>` |
| Upload a certificate | `pvenode cert set <cert> <key>` |
| Show certificate info | `pvenode cert info` |
| Order an ACME certificate | `pvenode acme cert order` |
| Renew an ACME certificate | `pvenode acme cert renew` |

UI equivalents: [Certificates](03-Nodes/System/Certificates.md), [ACME Certificates](02-Datacenter/ACME-Certificates.md), [Task History](03-Nodes/Task-History.md).

---

# Ceph — `pveceph`

| Task | Command |
|---|---|
| Install Ceph packages | `pveceph install` |
| Initialise configuration | `pveceph init --network <cidr>` |
| Create a monitor | `pveceph mon create` |
| Create a manager | `pveceph mgr create` |
| Create an OSD | `pveceph osd create /dev/<disk>` |
| Destroy an OSD | `pveceph osd destroy <id>` |
| Create a pool | `pveceph pool create <name>` |
| Show status | `pveceph status` |

`pveceph install` is normally required before Ceph appears in the interface at all.

> **Warning:** `pveceph osd create` wipes the target disk. Confirm it is unused first.

UI equivalents: [Ceph Overview](02-Datacenter/Ceph/Ceph-Overview.md), [Monitors and OSDs](02-Datacenter/Ceph/Ceph-Monitors-and-OSDs.md).

---

# Container Templates — `pveam`

| Task | Command |
|---|---|
| Update the template list | `pveam update` |
| List available templates | `pveam available` |
| Download a template | `pveam download <storage> <template>` |
| List downloaded templates | `pveam list <storage>` |

UI equivalent: [Manage Container Templates](05-Containers/Manage-Container-Templates.md).

---

# Subscription — `pvesubscription`

| Task | Command |
|---|---|
| Show status | `pvesubscription get` |
| Set a key | `pvesubscription set <key>` |
| Re-check | `pvesubscription update` |

UI equivalents: [Subscription](03-Nodes/Subscription.md), [Support](02-Datacenter/Support.md).

---

# Services and Diagnostics

| Task | Command |
|---|---|
| Service status | `systemctl status <service>` |
| Restart a service | `systemctl restart <service>` |
| Stop the firewall (lockout recovery) | `systemctl stop pve-firewall` |
| Cluster file system in local mode | `pmxcfs -l` |
| ZFS pool status | `zpool status` |
| LVM volumes | `pvs`, `vgs`, `lvs` |
| Disk layout | `lsblk` |
| Clear a disk signature | `wipefs -a /dev/<disk>` |
| Clear a partition table | `sgdisk --zap-all /dev/<disk>` |

## Clearing a disk that will not accept an OSD or storage

The interface refuses to use a disk that still carries a partition table or filesystem
signature. Its own wipe action handles most cases — see
[Disk Management](03-Nodes/Disks/Disk-Management.md) — but a disk previously used by
ZFS, LVM, or another Ceph cluster sometimes retains metadata the UI will not clear.

```bash
lsblk                        # confirm the device path first
wipefs -a /dev/sdX
sgdisk --zap-all /dev/sdX
```

> **Warning:** These destroy everything on the target device immediately, with no
> confirmation. Verify the device path with `lsblk` before running either — the cost of
> naming the wrong disk is total data loss on a disk that was in use.

## Changing the cluster network

The addresses Corosync uses are held in the cluster configuration and are **not editable
through the interface**. Changing them means editing `/etc/pve/corosync.conf`.

This is disruptive and easy to get wrong: an error in the file can break cluster
communication on every node at once, and the cluster file system becomes read-only
without quorum, which prevents fixing it the same way.

If you need to change the cluster network:

1. Confirm the cluster is healthy and quorate first.
2. Take a copy of the current file.
3. Increment the `config_version` value — nodes ignore a changed file otherwise.
4. Edit and save. The change distributes to all nodes.
5. Confirm every node is still visible with `pvecm status`.

> **Warning:** Have console access to every node before starting. If cluster
> communication breaks, the interface goes read-only and the shell on each node is the
> only way back. See [Recover Quorum](02-Datacenter/Cluster/Recover-Quorum.md).

> **Verify:** Confirm the exact `corosync.conf` structure in this deployment before
> documenting a worked example.

Common services: `pveproxy` (web interface), `pvedaemon`, `pvestatd` (statistics), `corosync` (cluster communication), `pve-cluster` (cluster file system), `pve-ha-crm` and `pve-ha-lrm` (HA), `pve-firewall`.

UI equivalents: [Services](03-Nodes/System/Services.md), [System Troubleshooting](03-Nodes/System/System-Troubleshooting.md), [Firewall Lockout Recovery](02-Datacenter/Firewall/Firewall-Lockout-Recovery.md).

---

# Operations With No Interface Equivalent

The cases where the CLI is not optional:

| Operation | Command | Documented in |
|---|---|---|
| Dissolve a cluster | Manual procedure | [Delete Cluster](02-Datacenter/Cluster/Delete-Cluster.md) |
| Remove a node from a cluster | `pvecm delnode` | [Remove Node from Cluster](02-Datacenter/Cluster/Remove-Node-from-Cluster.md) |
| Recover from lost quorum | `pvecm expected` | [Recover Quorum](02-Datacenter/Cluster/Recover-Quorum.md) |
| Reset a lost root password | Console + `passwd` | [Reset Root Password](03-Nodes/Reset-Root-Password.md) |
| Recover from a firewall lockout | `systemctl stop pve-firewall` | [Firewall Lockout Recovery](02-Datacenter/Firewall/Firewall-Lockout-Recovery.md) |
| Configure a QDevice | `pvecm qdevice setup` | [Quorum](02-Datacenter/Cluster/Quorum.md) |
| Install Ceph packages | `pveceph install` | [Ceph Overview](02-Datacenter/Ceph/Ceph-Overview.md) |
| Restore to a different guest ID | `qmrestore` / `pct restore` | [Storage Content Browser](02-Datacenter/Storage/Storage-Content-Browser.md) |

---

# Verification

After any CLI operation:

* Confirm the result in the interface, so state agrees in both places.
* Check [Task History](03-Nodes/Task-History.md) — some CLI actions appear, some do not.
* For cluster operations, confirm `pvecm status` reports quorate.
* For guest operations, confirm the expected state on the guest's Summary tab.
* For storage operations, confirm with `pvesm status`.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Command not found | Confirm you are on a VM2Cloud node and running as root. |
| Operation affected the wrong guest | IDs are shared between VMs and containers. Check with `qm list` and `pct list`. |
| Command fails with a lock error | Another operation is running on that guest. Wait for it to finish. |
| Cluster commands fail | The cluster may lack quorum, making the configuration read-only. |
| Change not visible in the interface | Refresh. If it persists, check the change was made on the right node. |
| Action missing from Task History | Some CLI operations are not recorded as tasks. Prefer the interface for auditable work. |
| Unsure of an option | Use `<command> help <subcommand>`. |

---

# Best Practices

- **Use the interface unless the CLI is genuinely required.** It logs, confirms, and is visible to colleagues.
- Confirm which node you are on before running anything.
- Check `qm list` and `pct list` before acting on an ID — the namespace is shared.
- Use `help` rather than guessing options.
- Test destructive commands on a throwaway guest first.
- Record CLI work somewhere, since it may not appear in Task History.
- Prefer `qmrestore` to a spare ID for restore testing.
- Never run `pvecm expected` without reading [Recover Quorum](02-Datacenter/Cluster/Recover-Quorum.md).
- Keep console access available before doing anything that could remove network access.

---

# Related Documentation

- [Shell](03-Nodes/Shell.md)
- [Task History](03-Nodes/Task-History.md)
- [Recover Quorum](02-Datacenter/Cluster/Recover-Quorum.md)
- [Reset Root Password](03-Nodes/Reset-Root-Password.md)
- [Firewall Lockout Recovery](02-Datacenter/Firewall/Firewall-Lockout-Recovery.md)
- [Remove Node from Cluster](02-Datacenter/Cluster/Remove-Node-from-Cluster.md)
- [Delete Cluster](02-Datacenter/Cluster/Delete-Cluster.md)
- [Storage Content Browser](02-Datacenter/Storage/Storage-Content-Browser.md)
- [System Troubleshooting](03-Nodes/System/System-Troubleshooting.md)
- [COVERAGE.md](COVERAGE.md)

---

# Summary

This appendix collects the VM2Cloud command-line tools in one place, because the CLI has no interface location to be filed under. Each family — `qm`, `pct`, `pvecm`, `pvesm`, `pveum`, `ha-manager`, `pvesr`, `pvenode`, `pveceph`, `pveam`, `vzdump` — is listed with its common operations and a link to the interface equivalent.

The interface remains the primary method: it logs actions as tasks, applies confirmations, and leaves a trail colleagues can follow. Use the CLI for the handful of operations that genuinely have no interface equivalent — listed in the table above — for recovery when the interface is unreachable, and for scripting. Everything else belongs in the UI.
