# Documentation Coverage

A complete map of the VM2Cloud VE interface and of the operations that require the CLI, against what is documented.

Use this to answer one question: **is there anything an administrator can do that we have not written down?**

---

## Legend

| Mark | Meaning |
|---|---|
| ✅ | Documented |
| ⚠️ | Partially documented — covered inside another page, but has no page of its own |
| ❌ | Not documented |
| 🔍 | UI presence not yet visually confirmed against a live environment |

Menus marked 🔍 are drawn from the platform's standard layout rather than from a screenshot in this repository. Confirm them during screenshot capture.

---

# Part 1 — Interface Coverage

## Installation (pre-interface)

Installing VM2Cloud VE on a physical server, before the web interface exists.
Ported from the standalone installation site on 13 August 2026.

| Stage | Status | Page |
|---|---|---|
| Mount ISO via BMC Virtual Media | ✅ | [Mount the Installation ISO](00-Installation/Mount-Installation-Media.md) |
| Installation storage and RAID layout | ✅ | [Configure Installation Storage](00-Installation/Configure-Installation-Storage.md) |
| Location, time zone, root credentials | ✅ | [Configure Location and Administrator Access](00-Installation/Configure-Location-and-Administrator.md) |
| Management network, VLAN, bonding | ✅ | [Configure the Management Network](00-Installation/Configure-Management-Network.md) |
| Summary review, install, reboot | ✅ | [Complete the Installation](00-Installation/Complete-Installation.md) |
| Console login, web login, dashboard | ✅ | [Verify the Installation](00-Installation/Verify-Installation.md) |
| USB media installation | ❌ | Not documented |
| Nested / virtual installation | ❌ | Not documented |
| Automated / unattended installation | ❌ | Not documented |
| In-place upgrade | ❌ | Not documented |

All 29 installation screenshots are placed. This is the only section with no
outstanding screenshot placeholders.

## Application Shell

Menus confirmed from `01-Getting-Started/images/datacenter-search.png`, `02-Datacenter/Permissions/images/access-realms.png`.

| UI location | Status | Page |
|---|---|---|
| Login screen | ✅ | [Logging In](01-Getting-Started/Logging-In.md) |
| Header bar — Search box | ✅ | [Search](01-Getting-Started/Search.md) |
| Header bar — Documentation, Create VM, Create CT | ⚠️ | Covered in [Interface Tour](01-Getting-Started/Interface-Tour.md) |
| Header bar — user menu, password, preferences | ✅ | [My Settings](01-Getting-Started/My-Settings.md) |
| Resource tree — Server View | ✅ | [Interface Tour](01-Getting-Started/Interface-Tour.md) |
| Resource tree — Folder / Pool / Tag views | ✅ | [Resource Tree and Views](01-Getting-Started/Resource-Tree-and-Views.md) |
| Tags on guests | ✅ | [Tags](01-Getting-Started/Tags.md) |
| Task log / Cluster log panel | ✅ | [Task Log and Cluster Log](01-Getting-Started/Task-Log-and-Cluster-Log.md) |
| Interface problems | ✅ | [Interface Troubleshooting](01-Getting-Started/Interface-Troubleshooting.md) |

## Datacenter

Menu order confirmed: Search · Summary · Notes · Cluster · Ceph · Options · Storage · Backup · Replication · Permissions · HA · SDN · ACME · Firewall · Metric Server · Resource Mappings · Directory Mappings · Custom CPU models · Notifications · Support

| Tab | Status | Page |
|---|---|---|
| Search | ✅ | [Search](01-Getting-Started/Search.md) — Datacenter Search Panel section |
| Summary | ✅ | [Datacenter Summary](02-Datacenter/Datacenter-Summary.md) |
| Notes | ✅ | [Datacenter Notes](02-Datacenter/Notes.md) |
| Cluster | ✅ | [Cluster Overview](02-Datacenter/Cluster/Cluster-Overview.md) + 8 pages |
| Ceph | ✅ | [Ceph Overview](02-Datacenter/Ceph/Ceph-Overview.md) + 2 pages |
| Options | ✅ | [Datacenter Options](02-Datacenter/Options.md) |
| Storage | ✅ | [Storage Overview](02-Datacenter/Storage/Storage-Overview.md) + 5 pages |
| Backup | ✅ | [Backup Jobs Overview](02-Datacenter/Backup/Backup-Jobs-Overview.md) + 3 pages |
| Replication | ✅ | [Replication Overview](02-Datacenter/Replication/Replication-Overview.md) + 6 pages |
| Permissions → Users | ✅ | [Users](02-Datacenter/Permissions/Users.md) |
| Permissions → API Tokens | ✅ | [API Tokens](02-Datacenter/Permissions/API-Tokens.md) |
| Permissions → Two Factor | ✅ | [Two-Factor Authentication](02-Datacenter/Permissions/Two-Factor-Authentication.md) |
| Permissions → Groups | ✅ | [Groups](02-Datacenter/Permissions/Groups.md) |
| Permissions → Pools | ✅ | [Pools](02-Datacenter/Permissions/Pools.md) |
| Permissions → Roles | ✅ | [Roles](02-Datacenter/Permissions/Roles.md) |
| Permissions → Realms | ✅ | [Authentication Realms](02-Datacenter/Permissions/Authentication-Realms.md) |
| HA | ✅ | [HA Overview](02-Datacenter/HA/HA-Overview.md) + 5 pages |
| SDN → Zones | ✅ | [Zones](02-Datacenter/SDN/Zones.md) |
| SDN → VNets | ✅ | [VNets](02-Datacenter/SDN/VNets.md) |
| SDN → Options | ✅ | [SDN Options](02-Datacenter/SDN/SDN-Options.md) |
| SDN → IPAM | ✅ | [IPAM](02-Datacenter/SDN/IPAM.md) |
| SDN → VNet Firewall | ✅ | [VNet Firewall](02-Datacenter/SDN/VNet-Firewall.md) |
| SDN → Fabrics | ✅ | [Fabrics](02-Datacenter/SDN/Fabrics.md) |
| SDN → Route Maps | ✅ | [Route Maps](02-Datacenter/SDN/Route-Maps.md) |
| SDN → Prefix Lists | ✅ | [Prefix Lists](02-Datacenter/SDN/Prefix-Lists.md) |
| ACME | ✅ | [ACME Certificates](02-Datacenter/ACME-Certificates.md) |
| Firewall | ✅ | [Firewall Overview](02-Datacenter/Firewall/Firewall-Overview.md) + 5 pages |
| Metric Server | ✅ | [Metric Server](02-Datacenter/Metric-Server.md) |
| Resource Mappings | ✅ | [Resource Mappings](02-Datacenter/Resource-Mappings.md) |
| Directory Mappings | ✅ | [Directory Mappings](02-Datacenter/Directory-Mappings.md) |
| Custom CPU models | ✅ | [Custom CPU Models](02-Datacenter/Custom-CPU-Models.md) |
| Notifications | ✅ | [Notifications](02-Datacenter/Notifications.md) |
| Support | ✅ | [Support](02-Datacenter/Support.md) |

## Node

Menu confirmed from `03-Nodes/images/navigation-menu.png`: Search · Summary · Notes · Shell · System · Updates · Firewall · Disks · Ceph · Replication · Task History · Subscription

| Tab | Status | Page |
|---|---|---|
| Search | ⚠️ | Covered in [Search](01-Getting-Started/Search.md) |
| Summary | ✅ | [Node Summary](03-Nodes/Node-Summary.md) |
| Notes | ✅ | [Node Notes](03-Nodes/Node-Notes.md) |
| Shell | ✅ | [Shell](03-Nodes/Shell.md) |
| System → Network | ✅ | [Network Overview](03-Nodes/System/Network/Network-Overview.md) + 5 pages |
| System → Certificates | ✅ | [Certificates](03-Nodes/System/Certificates.md) |
| System → DNS | ✅ | [DNS](03-Nodes/System/DNS.md) |
| System → Hosts | ✅ | [Hosts](03-Nodes/System/Hosts.md) |
| System → Time | ✅ | [Time and NTP](03-Nodes/System/Time-and-NTP.md) |
| System → Syslog | ✅ | [Syslog](03-Nodes/System/Syslog.md) |
| System → Options | 🔍 ❌ | Node-level System Options, if present |
| Boot Mode / Kernel / Services | ✅ | [Boot Mode](03-Nodes/System/Boot-Mode.md), [Kernel](03-Nodes/System/Kernel.md), [Services](03-Nodes/System/Services.md) |
| Updates | ✅ | [Update Node](03-Nodes/Updates/Update-Node.md) |
| Updates → Repositories | ✅ | [Repositories](03-Nodes/Updates/Repositories.md) |
| Firewall | ✅ | [Node Firewall](03-Nodes/Node-Firewall.md) |
| Disks | ✅ | [Disks Overview](03-Nodes/Disks/Disks-Overview.md) + 7 pages |
| Ceph | ✅ | [Node Ceph](03-Nodes/Node-Ceph.md) |
| Replication | ✅ | [Node Replication](03-Nodes/Node-Replication.md) |
| Task History | ✅ | [Task History](03-Nodes/Task-History.md) |
| Subscription | ✅ | [Subscription](03-Nodes/Subscription.md) |
| Reboot / Shutdown | ✅ | [Reboot Node](03-Nodes/Reboot-Node.md), [Shutdown Node](03-Nodes/Shutdown-Node.md) |

## Virtual Machine

Tabs confirmed from `04-Virtual-Machines/images/vm-tabs.png`.

| Tab | Status | Page |
|---|---|---|
| Summary | ✅ | [VM Summary](04-Virtual-Machines/VM-Summary.md) |
| Console | ✅ | [VM Console](04-Virtual-Machines/VM-Console.md) |
| Hardware | ✅ | [Manage VM Hardware](04-Virtual-Machines/Manage-VM-Hardware.md) |
| Cloud-Init | ✅ | [Cloud-Init](04-Virtual-Machines/Cloud-Init.md) |
| Options | ✅ | [VM Options](04-Virtual-Machines/VM-Options.md) |
| Task History | ✅ | [VM Task History](04-Virtual-Machines/VM-Task-History.md) |
| Monitor | ✅ | [VM Monitor](04-Virtual-Machines/VM-Monitor.md) |
| Backup | ✅ | [Backup and Restore VM](04-Virtual-Machines/Backup-and-Restore-VM.md) |
| Replication | ✅ | [VM Replication](04-Virtual-Machines/VM-Replication.md) |
| Snapshots | ✅ | [VM Snapshots](04-Virtual-Machines/VM-Snapshots.md) |
| Firewall | ✅ | [VM Firewall](04-Virtual-Machines/VM-Firewall.md) |
| Permissions | ✅ | [VM Permissions](04-Virtual-Machines/VM-Permissions.md) |
| Notes panel (on Summary) | ✅ | Covered in [VM Summary](04-Virtual-Machines/VM-Summary.md) |
| Header — Tags control | ✅ | [Tags](01-Getting-Started/Tags.md) |
| Create / Manage / Migrate / Clone / Delete | ✅ | Five existing pages |
| Convert to Template | ✅ | [Convert to Template](04-Virtual-Machines/Convert-to-Template.md) |

## Container

Tabs confirmed from `05-Containers/images/ct-tabs.png`.

| Tab | Status | Page |
|---|---|---|
| Summary | ✅ | [CT Summary](05-Containers/CT-Summary.md) |
| Console | ✅ | [Container Console](05-Containers/Container-Console.md) |
| Resources | ✅ | [Manage Container Resources](05-Containers/Manage-Container-Resources.md) |
| Network | ✅ | [CT Network](05-Containers/CT-Network.md) |
| DNS | ✅ | [CT DNS](05-Containers/CT-DNS.md) |
| Options | ✅ | [Container Options](05-Containers/CT-Options.md) |
| Task History | ✅ | [CT Task History](05-Containers/CT-Task-History.md) |
| Backup | ✅ | [Backup and Restore Container](05-Containers/Backup-and-Restore-Container.md) |
| Replication | ✅ | [CT Replication](05-Containers/CT-Replication.md) |
| Snapshots | ✅ | [Container Snapshots](05-Containers/CT-Snapshots.md) |
| Firewall | ✅ | [Container Firewall](05-Containers/CT-Firewall.md) |
| Permissions | ✅ | [CT Permissions](05-Containers/CT-Permissions.md) |
| Notes panel (on Summary) | ✅ | Covered in [CT Summary](05-Containers/CT-Summary.md) |
| Create / Manage / Migrate / Clone / Delete / Templates | ✅ | Six existing pages |

## Storage (per-storage view)

Tabs confirmed from `02-Datacenter/Storage/images/temp-download-done.png`.

| Tab | Status | Page |
|---|---|---|
| Summary | ✅ | [Storage Content Browser](02-Datacenter/Storage/Storage-Content-Browser.md) |
| Backups | ✅ | [Storage Content Browser](02-Datacenter/Storage/Storage-Content-Browser.md) |
| ISO Images | ✅ | [Upload Content](02-Datacenter/Storage/Upload-Content.md) |
| CT Templates | ✅ | [Upload Content](02-Datacenter/Storage/Upload-Content.md), [Manage Container Templates](05-Containers/Manage-Container-Templates.md) |
| Import | ✅ | [Storage Import](02-Datacenter/Storage/Storage-Import.md) |
| Permissions | ✅ | [Storage Permissions](02-Datacenter/Storage/Storage-Permissions.md) |

---

# Part 2 — Operations Requiring the CLI

Operations the interface cannot perform, or cannot perform completely. Documenting these matters as much as the UI: an administrator who cannot dissolve a cluster or recover quorum from the interface still has to do it.

The house rule remains **UI first, CLI second** — CLI appears where the UI genuinely cannot do the job, where recovery requires it, or where it provides useful verification.

## Cluster

| Operation | Why CLI | Status | Where |
|---|---|---|---|
| Dissolve / delete a cluster | No UI action exists | ✅ | [Delete Cluster](02-Datacenter/Cluster/Delete-Cluster.md) |
| Remove a node from the cluster | `pvecm delnode`; UI cannot | ✅ | [Remove Node from Cluster](02-Datacenter/Cluster/Remove-Node-from-Cluster.md) |
| Clean a removed node | Filesystem cleanup after removal | ✅ | [Remove Node from Cluster](02-Datacenter/Cluster/Remove-Node-from-Cluster.md) |
| Inspect cluster status | `pvecm status`, `pvecm nodes` | ✅ | [Quorum](02-Datacenter/Cluster/Quorum.md) |
| **Recover from lost quorum** | `pvecm expected <n>` | ✅ | [Recover Quorum](02-Datacenter/Cluster/Recover-Quorum.md) |
| Configure a QDevice | `pvecm qdevice setup` | ✅ | [06-CLI-Reference.md](06-CLI-Reference.md) |
| Change the cluster network / links | Edit `corosync.conf` | ✅ | [06-CLI-Reference.md](06-CLI-Reference.md) |
| Regenerate cluster certificates | `pvecm updatecerts` | ✅ | [06-CLI-Reference.md](06-CLI-Reference.md) |
| Start the cluster filesystem in local mode | `pmxcfs -l` | ⚠️ | Mentioned in [Cluster File System](02-Datacenter/Cluster/Cluster-File-System.md) |
| Re-add a previously removed node | Requires reinstall; CLI procedure | ✅ | [Re-Add a Removed Node](02-Datacenter/Cluster/Re-Add-Removed-Node.md) |

## High Availability

| Operation | Why CLI | Status | Where |
|---|---|---|---|
| Inspect HA state | `ha-manager status` | ✅ | [06-CLI-Reference.md](06-CLI-Reference.md) |
| Manage HA resources from CLI | `ha-manager add/set/remove` | ✅ | [06-CLI-Reference.md](06-CLI-Reference.md) |
| Force relocate or migrate an HA resource | `ha-manager relocate` / `migrate` | ✅ | [06-CLI-Reference.md](06-CLI-Reference.md) |
| Check HA service health | `systemctl status pve-ha-crm`, `pve-ha-lrm` | ✅ | [HA Troubleshooting](02-Datacenter/HA/HA-Troubleshooting.md) |

## Guests

| Operation | Why CLI | Status | Where |
|---|---|---|---|
| Guest lifecycle from CLI | `qm` / `pct` start, stop, status | ✅ | [06-CLI-Reference.md](06-CLI-Reference.md) |
| Restore a backup to a different VMID | `qmrestore` / `pct restore` | ✅ | [Storage Content Browser](02-Datacenter/Storage/Storage-Content-Browser.md) (UI) and [06-CLI-Reference.md](06-CLI-Reference.md) |
| Manual backup from CLI | `vzdump` | ✅ | [06-CLI-Reference.md](06-CLI-Reference.md) |
| Bulk operations across many guests | Scripted `qm` / `pct` | ✅ | [06-CLI-Reference.md](06-CLI-Reference.md) |
| Edit configuration not exposed in the UI | `qm set` / `pct set` | ✅ | [06-CLI-Reference.md](06-CLI-Reference.md) |
| Migrate with a different target storage | `qm migrate --targetstorage` | ✅ | [06-CLI-Reference.md](06-CLI-Reference.md) |
| Convert to template | `qm template` | ✅ | [Convert to Template](04-Virtual-Machines/Convert-to-Template.md) (UI) and [06-CLI-Reference.md](06-CLI-Reference.md) |
| Download container templates | `pveam available` / `pveam download` | ✅ | [Manage Container Templates](05-Containers/Manage-Container-Templates.md) (UI) and [06-CLI-Reference.md](06-CLI-Reference.md) |

## Storage and Disks

| Operation | Why CLI | Status | Where |
|---|---|---|---|
| Inspect storage | `pvesm status`, `pvesm list` | ✅ | [Storage Troubleshooting](02-Datacenter/Storage/Storage-Troubleshooting.md) |
| Inspect ZFS | `zpool status`, `zfs list` | ✅ | [ZFS](03-Nodes/Disks/ZFS.md) |
| Inspect LVM | `pvs`, `vgs`, `lvs` | ✅ | [LVM](03-Nodes/Disks/LVM.md), [Disk Troubleshooting](03-Nodes/Disks/Disk-Troubleshooting.md) |
| Wipe a disk / clear a partition table | `wipefs`, `sgdisk` | ✅ | [06-CLI-Reference.md](06-CLI-Reference.md) |
| ZFS scrub scheduling, import, export | `zpool scrub` / `import` / `export` | ⚠️ | Partial in [ZFS](03-Nodes/Disks/ZFS.md) |
| Trigger a replication run manually | `pvesr run` | ✅ | [06-CLI-Reference.md](06-CLI-Reference.md) |

## Access Control

| Operation | Why CLI | Status | Where |
|---|---|---|---|
| Manage users, groups, roles from CLI | `pveum` | ✅ | [06-CLI-Reference.md](06-CLI-Reference.md) |
| **Reset a lost root password** | Console + `passwd` | ✅ | [Reset Root Password](03-Nodes/Reset-Root-Password.md) |
| Recover from a firewall lockout | Console + stop the firewall service or edit its config | ✅ | [Firewall Lockout Recovery](02-Datacenter/Firewall/Firewall-Lockout-Recovery.md) |

## Node and System

| Operation | Why CLI | Status | Where |
|---|---|---|---|
| Service inspection and restart | `systemctl` | ✅ | [System Troubleshooting](03-Nodes/System/System-Troubleshooting.md), [Services](03-Nodes/System/Services.md) |
| Certificate management from CLI | `pvenode cert` | ✅ | [06-CLI-Reference.md](06-CLI-Reference.md) |
| ACME from CLI | `pvenode acme` | ✅ | [06-CLI-Reference.md](06-CLI-Reference.md) |
| Subscription key from CLI | `pvesubscription` | ✅ | [06-CLI-Reference.md](06-CLI-Reference.md) |
| Ceph management | `pveceph` | ✅ | [06-CLI-Reference.md](06-CLI-Reference.md) |

---

# Part 3 — Totals

## Interface

| Level | Tabs | ✅ | ⚠️ | ❌ |
|---|---:|---:|---:|---:|
| Application shell | 9 | 8 | 1 | 0 |
| Datacenter | 32 | 32 | 0 | 0 |
| Node | 22 | 21 | 1 | 0 |
| Virtual Machine | 16 | 16 | 0 | 0 |
| Container | 18 | 18 | 0 | 0 |
| Storage | 6 | 6 | 0 | 0 |
| **Total** | **103** | **101** | **2** | **0** |

**Interface coverage: 101 of 103 panels**, the remaining two being partial by design.

The Datacenter menu was confirmed in full on 14 August 2026. It runs three panels
longer than had been inferred — Resource Mappings, Directory Mappings, and Custom CPU
models — and all three were written the same day. The same screenshot confirmed Metric
Server, Notifications, and Support, which had been carrying 🔍 markers.

The SDN submenu was confirmed the same day and turned out to have eight panels where
two had been inferred; all six missing pages were written then. Seven other submenus —
node System, Updates, Disks and Ceph, datacenter Ceph, HA and Firewall, and the guest
Firewall — are still inferred rather than seen, and may hold the same kind of surprise.
Two entries are marked partial rather than missing — the header bar buttons and
node-level Search — because they are documented inside other pages rather than having
pages of their own, which is correct for what they are.

One entry still carries 🔍: node-level System Options. A page does not exist for it,
and its presence in this deployment was never confirmed from a screenshot. Verify
during capture.

## CLI operations

| Area | Operations | ✅ | ⚠️ | ❌ |
|---|---:|---:|---:|---:|
| Cluster | 10 | 9 | 1 | 0 |
| High Availability | 4 | 4 | 0 | 0 |
| Guests | 8 | 8 | 0 | 0 |
| Storage and Disks | 6 | 5 | 1 | 0 |
| Access Control | 3 | 3 | 0 | 0 |
| Node and System | 5 | 5 | 0 | 0 |
| **Total** | **36** | **34** | **2** | **0** |

**CLI coverage: complete.** Every operation is covered, either in its own page or in [06-CLI-Reference.md](06-CLI-Reference.md), which collects the command families and lists the operations with no interface equivalent.

---

# Part 4 — What Is Left, Ranked

## Critical — recovery paths ✅ complete

All four are now documented and cross-linked from the pages that previously
referred to them vaguely.

1. ✅ [Recover Quorum](02-Datacenter/Cluster/Recover-Quorum.md) — `pvecm expected`, with the split-brain safety checks it requires.
2. ✅ [Reset Root Password](03-Nodes/Reset-Root-Password.md) — interface method first, console recovery when no account can log in.
3. ✅ [Firewall Lockout Recovery](02-Datacenter/Firewall/Firewall-Lockout-Recovery.md) — two methods, chosen by whether the cluster still has quorum.
4. ✅ [Re-Add a Removed Node](02-Datacenter/Cluster/Re-Add-Removed-Node.md) — why reinstallation is required, and what breaks without it.

## High value ✅ complete

Guest Summary, Task History, Permissions, Monitor, and Replication views;
container Network and DNS; Convert to Template; the storage content browser,
Import, and Permissions; and Datacenter and Node Notes are all written.

Restoring to a different guest ID is now covered in
[Storage Content Browser](02-Datacenter/Storage/Storage-Content-Browser.md).

## Everything above ✅ complete

The CLI reference, onboarding pages, SDN, Ceph, ACME, Notifications, Metric Server,
Support, Resource Mappings, Directory Mappings, and Custom CPU Models are all written.

## Remaining work

18. **260 screenshot placeholders** on pre-existing pages, and 82 on the new Tier 1 pages. The Tier 1 placeholders carry `Capture:` lines; the older ones do not yet.
19. **43 `Verify:` markers** on UI labels awaiting confirmation.

---

## Keeping This Current

Update this file whenever a page is added or a UI area is confirmed. Regenerate the totals with:

```bash
find . -name '*.md' -not -path './.git/*' -not -name 'README.md' \
  -not -name 'TEMPLATE.md' -not -name 'CONTRIBUTING.md' \
  -not -name 'GLOSSARY.md' -not -name 'COVERAGE.md' | wc -l
```

Confirm the 🔍 entries during screenshot capture and update their status.
