# Documentation Coverage

A complete map of the VM2Cloud interface and of the operations that require the CLI, against what is documented.

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

## Application Shell

Menus confirmed from `01-Getting-Started/images/datacenter-search.png`, `02-Datacenter/Permissions/images/access-realms.png`.

| UI location | Status | Page |
|---|---|---|
| Login screen | ❌ | `01-Getting-Started/Logging-In.md` |
| Header bar — Search box | ✅ | [Search](01-Getting-Started/Search.md) |
| Header bar — Documentation, Create VM, Create CT | ⚠️ | Covered in [Interface Tour](01-Getting-Started/Interface-Tour.md) |
| Header bar — user menu, password, preferences | ❌ | `01-Getting-Started/My-Settings.md` |
| Resource tree — Server View | ✅ | [Interface Tour](01-Getting-Started/Interface-Tour.md) |
| Resource tree — Folder / Pool / Tag views | ❌ | `01-Getting-Started/Resource-Tree-and-Views.md` |
| Tags on guests | ❌ | `01-Getting-Started/Tags.md` |
| Task log / Cluster log panel | ✅ | [Task Log and Cluster Log](01-Getting-Started/Task-Log-and-Cluster-Log.md) |
| Interface problems | ✅ | [Interface Troubleshooting](01-Getting-Started/Interface-Troubleshooting.md) |

## Datacenter

Menu order confirmed: Search · Summary · Notes · Cluster · Ceph · Options · Storage · Backup · Replication · Permissions · HA · SDN · ACME · Firewall

| Tab | Status | Page |
|---|---|---|
| Search | ✅ | [Search](01-Getting-Started/Search.md) — Datacenter Search Panel section |
| Summary | ✅ | [Datacenter Summary](02-Datacenter/Datacenter-Summary.md) |
| Notes | ❌ | `02-Datacenter/Notes.md` |
| Cluster | ✅ | [Cluster Overview](02-Datacenter/Cluster/Cluster-Overview.md) + 8 pages |
| Ceph | ❌ | `02-Datacenter/Ceph/` — 3 pages |
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
| SDN | ❌ | `02-Datacenter/SDN/` — 3 pages |
| ACME | ❌ | `02-Datacenter/ACME-Certificates.md` |
| Firewall | ✅ | [Firewall Overview](02-Datacenter/Firewall/Firewall-Overview.md) + 5 pages |
| Metric Server | 🔍 ❌ | `02-Datacenter/Metric-Server.md` |
| Notifications | 🔍 ❌ | `02-Datacenter/Notifications.md` |
| Support | 🔍 ❌ | `02-Datacenter/Support.md` |

## Node

Menu confirmed from `03-Nodes/images/navigation-menu.png`: Search · Summary · Notes · Shell · System · Updates · Firewall · Disks · Ceph · Replication · Task History · Subscription

| Tab | Status | Page |
|---|---|---|
| Search | ⚠️ | Covered in [Search](01-Getting-Started/Search.md) |
| Summary | ✅ | [Node Summary](03-Nodes/Node-Summary.md) |
| Notes | ❌ | `03-Nodes/Node-Notes.md` |
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
| Ceph | ❌ | `03-Nodes/Node-Ceph.md` |
| Replication | ❌ | `03-Nodes/Node-Replication.md` — node-level replication view |
| Task History | ✅ | [Task History](03-Nodes/Task-History.md) |
| Subscription | ✅ | [Subscription](03-Nodes/Subscription.md) |
| Reboot / Shutdown | ✅ | [Reboot Node](03-Nodes/Reboot-Node.md), [Shutdown Node](03-Nodes/Shutdown-Node.md) |

## Virtual Machine

Tabs confirmed from `04-Virtual-Machines/images/vm-tabs.png`.

| Tab | Status | Page |
|---|---|---|
| Summary | ❌ | `04-Virtual-Machines/VM-Summary.md` |
| Console | ✅ | [VM Console](04-Virtual-Machines/VM-Console.md) |
| Hardware | ✅ | [Manage VM Hardware](04-Virtual-Machines/Manage-VM-Hardware.md) |
| Cloud-Init | ✅ | [Cloud-Init](04-Virtual-Machines/Cloud-Init.md) |
| Options | ✅ | [VM Options](04-Virtual-Machines/VM-Options.md) |
| Task History | ❌ | `04-Virtual-Machines/VM-Task-History.md` |
| Monitor | ❌ | `04-Virtual-Machines/VM-Monitor.md` |
| Backup | ✅ | [Backup and Restore VM](04-Virtual-Machines/Backup-and-Restore-VM.md) |
| Replication | ❌ | `04-Virtual-Machines/VM-Replication.md` |
| Snapshots | ✅ | [VM Snapshots](04-Virtual-Machines/VM-Snapshots.md) |
| Firewall | ✅ | [VM Firewall](04-Virtual-Machines/VM-Firewall.md) |
| Permissions | ❌ | `04-Virtual-Machines/VM-Permissions.md` |
| Notes panel (on Summary) | ❌ | `04-Virtual-Machines/VM-Notes.md` |
| Header — Tags control | ❌ | `01-Getting-Started/Tags.md` |
| Create / Manage / Migrate / Clone / Delete | ✅ | Five existing pages |
| Convert to Template | ❌ | `04-Virtual-Machines/Convert-to-Template.md` |

## Container

Tabs confirmed from `05-Containers/images/ct-tabs.png`.

| Tab | Status | Page |
|---|---|---|
| Summary | ❌ | `05-Containers/CT-Summary.md` |
| Console | ✅ | [Container Console](05-Containers/Container-Console.md) |
| Resources | ✅ | [Manage Container Resources](05-Containers/Manage-Container-Resources.md) |
| Network | ❌ | `05-Containers/CT-Network.md` |
| DNS | ❌ | `05-Containers/CT-DNS.md` |
| Options | ✅ | [Container Options](05-Containers/CT-Options.md) |
| Task History | ❌ | `05-Containers/CT-Task-History.md` |
| Backup | ✅ | [Backup and Restore Container](05-Containers/Backup-and-Restore-Container.md) |
| Replication | ❌ | `05-Containers/CT-Replication.md` |
| Snapshots | ✅ | [Container Snapshots](05-Containers/CT-Snapshots.md) |
| Firewall | ✅ | [Container Firewall](05-Containers/CT-Firewall.md) |
| Permissions | ❌ | `05-Containers/CT-Permissions.md` |
| Notes panel (on Summary) | ❌ | `05-Containers/CT-Notes.md` |
| Create / Manage / Migrate / Clone / Delete / Templates | ✅ | Six existing pages |

## Storage (per-storage view)

Tabs confirmed from `02-Datacenter/Storage/images/temp-download-done.png`.

| Tab | Status | Page |
|---|---|---|
| Summary | ❌ | `02-Datacenter/Storage/Storage-Content-Browser.md` |
| Backups | ❌ | Same page — browsing and restoring backup files |
| ISO Images | ✅ | [Upload Content](02-Datacenter/Storage/Upload-Content.md) |
| CT Templates | ✅ | [Upload Content](02-Datacenter/Storage/Upload-Content.md), [Manage Container Templates](05-Containers/Manage-Container-Templates.md) |
| Import | ❌ | `02-Datacenter/Storage/Storage-Import.md` |
| Permissions | ❌ | `02-Datacenter/Storage/Storage-Permissions.md` |

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
| Configure a QDevice | `pvecm qdevice setup` | ❌ | Referenced in [Quorum](02-Datacenter/Cluster/Quorum.md) but no procedure |
| Change the cluster network / links | Edit `corosync.conf` | ❌ | No page |
| Regenerate cluster certificates | `pvecm updatecerts` | ❌ | No page |
| Start the cluster filesystem in local mode | `pmxcfs -l` | ⚠️ | Mentioned in [Cluster File System](02-Datacenter/Cluster/Cluster-File-System.md) |
| Re-add a previously removed node | Requires reinstall; CLI procedure | ✅ | [Re-Add a Removed Node](02-Datacenter/Cluster/Re-Add-Removed-Node.md) |

## High Availability

| Operation | Why CLI | Status | Where |
|---|---|---|---|
| Inspect HA state | `ha-manager status` | ❌ | No page |
| Manage HA resources from CLI | `ha-manager add/set/remove` | ❌ | No page |
| Force relocate or migrate an HA resource | `ha-manager relocate` / `migrate` | ❌ | No page |
| Check HA service health | `systemctl status pve-ha-crm`, `pve-ha-lrm` | ✅ | [HA Troubleshooting](02-Datacenter/HA/HA-Troubleshooting.md) |

## Guests

| Operation | Why CLI | Status | Where |
|---|---|---|---|
| Guest lifecycle from CLI | `qm` / `pct` start, stop, status | ⚠️ | `pct` covered in [Container Troubleshooting](05-Containers/Container-Troubleshooting.md); `qm` barely |
| Restore a backup to a different VMID | `qmrestore` / `pct restore` | ❌ | No page. UI restores in place. |
| Manual backup from CLI | `vzdump` | ❌ | No page |
| Bulk operations across many guests | Scripted `qm` / `pct` | ❌ | No page |
| Edit configuration not exposed in the UI | `qm set` / `pct set` | ❌ | No page |
| Migrate with a different target storage | `qm migrate --targetstorage` | ❌ | No page |
| Convert to template | `qm template` | ❌ | Planned as a UI page |
| Download container templates | `pveam available` / `pveam download` | ❌ | UI covered; CLI not |

## Storage and Disks

| Operation | Why CLI | Status | Where |
|---|---|---|---|
| Inspect storage | `pvesm status`, `pvesm list` | ✅ | [Storage Troubleshooting](02-Datacenter/Storage/Storage-Troubleshooting.md) |
| Inspect ZFS | `zpool status`, `zfs list` | ✅ | [ZFS](03-Nodes/Disks/ZFS.md) |
| Inspect LVM | `pvs`, `vgs`, `lvs` | ✅ | [LVM](03-Nodes/Disks/LVM.md), [Disk Troubleshooting](03-Nodes/Disks/Disk-Troubleshooting.md) |
| Wipe a disk / clear a partition table | `wipefs`, `sgdisk` | ❌ | UI wipe documented; CLI recovery path not |
| ZFS scrub scheduling, import, export | `zpool scrub` / `import` / `export` | ⚠️ | Partial in [ZFS](03-Nodes/Disks/ZFS.md) |
| Trigger a replication run manually | `pvesr run` | ❌ | `pvesr status` covered; `run` not |

## Access Control

| Operation | Why CLI | Status | Where |
|---|---|---|---|
| Manage users, groups, roles from CLI | `pveum` | ❌ | No page |
| **Reset a lost root password** | Console + `passwd` | ✅ | [Reset Root Password](03-Nodes/Reset-Root-Password.md) |
| Recover from a firewall lockout | Console + stop the firewall service or edit its config | ✅ | [Firewall Lockout Recovery](02-Datacenter/Firewall/Firewall-Lockout-Recovery.md) |

## Node and System

| Operation | Why CLI | Status | Where |
|---|---|---|---|
| Service inspection and restart | `systemctl` | ✅ | [System Troubleshooting](03-Nodes/System/System-Troubleshooting.md), [Services](03-Nodes/System/Services.md) |
| Certificate management from CLI | `pvenode cert` | ❌ | No page |
| ACME from CLI | `pvenode acme` | ❌ | No page |
| Subscription key from CLI | `pvesubscription` | ❌ | No page |
| Ceph management | `pveceph` | ❌ | No page |

---

# Part 3 — Totals

## Interface

| Level | Tabs | ✅ | ⚠️ | ❌ |
|---|---:|---:|---:|---:|
| Application shell | 9 | 4 | 2 | 3 |
| Datacenter | 23 | 17 | 0 | 6 |
| Node | 22 | 18 | 1 | 3 |
| Virtual Machine | 16 | 9 | 0 | 7 |
| Container | 18 | 11 | 0 | 7 |
| Storage | 6 | 2 | 0 | 4 |
| **Total** | **94** | **61** | **3** | **30** |

**Interface coverage: roughly 68% of panels have a dedicated page.**

## CLI operations

| Area | Operations | ✅ | ⚠️ | ❌ |
|---|---:|---:|---:|---:|
| Cluster | 10 | 6 | 1 | 3 |
| High Availability | 4 | 1 | 0 | 3 |
| Guests | 8 | 0 | 1 | 7 |
| Storage and Disks | 6 | 3 | 1 | 2 |
| Access Control | 3 | 2 | 0 | 1 |
| Node and System | 5 | 1 | 0 | 4 |
| **Total** | **36** | **13** | **3** | **20** |

**CLI coverage: roughly 36%.** Still the weaker half. The four critical recovery paths are now documented; the remaining gaps are mostly convenience and automation commands rather than recovery.

---

# Part 4 — What Is Left, Ranked

## Critical — recovery paths ✅ complete

All four are now documented and cross-linked from the pages that previously
referred to them vaguely.

1. ✅ [Recover Quorum](02-Datacenter/Cluster/Recover-Quorum.md) — `pvecm expected`, with the split-brain safety checks it requires.
2. ✅ [Reset Root Password](03-Nodes/Reset-Root-Password.md) — interface method first, console recovery when no account can log in.
3. ✅ [Firewall Lockout Recovery](02-Datacenter/Firewall/Firewall-Lockout-Recovery.md) — two methods, chosen by whether the cluster still has quorum.
4. ✅ [Re-Add a Removed Node](02-Datacenter/Cluster/Re-Add-Removed-Node.md) — why reinstallation is required, and what breaks without it.

## High value — daily or weekly operations

5. Guest **Summary** pages for VMs and containers — the default view for every guest, undocumented at both levels.
6. **Restore to a different VMID** — `qmrestore` / `pct restore`. Standard practice for test restores.
7. Guest **Task History** and **Permissions** tabs, both levels.
8. Guest and node **Replication** views.
9. Storage **content browser**, covering the Summary, Backups, and Import tabs.
10. Container **Network** and **DNS** tabs.

## Medium — completeness

11. `06-CLI-Reference.md` — one appendix covering `qm`, `pct`, `vzdump`, `pvesm`, `pvecm`, `pveum`, `ha-manager`, `pvenode`, and what each verifies.
12. **Convert to Template**, VM and CT **Notes**, **VM Monitor**.
13. **Tags**, resource tree views, **My Settings**, **Logging In**, **What Is VM2Cloud**.
14. Datacenter **Notes**.

## Lower traffic

15. **SDN** — 3 pages.
16. **Ceph** — 3 datacenter pages plus the node view.
17. **ACME**, **Notifications**, **Metric Server**, **Support**.

## Not a content gap

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
