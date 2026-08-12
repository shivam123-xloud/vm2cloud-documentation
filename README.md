# VM2Cloud Documentation

Administrator documentation for the **VM2Cloud Virtual Environment** web interface — creating and running clusters, nodes, storage, networking, virtual machines, and containers.

---

## How This Documentation Is Organized

**The folder structure mirrors the VM2Cloud interface.** A folder corresponds to a panel in the UI, and a path corresponds to a click path.

If you are looking at a screen and want its documentation, follow the same route you took in the interface:

| In the interface | In this repository |
|---|---|
| Datacenter → Permissions → Users | [02-Datacenter/Permissions/Users.md](02-Datacenter/Permissions/Users.md) |
| Datacenter → HA → Fencing | [02-Datacenter/HA/Fencing.md](02-Datacenter/HA/Fencing.md) |
| Node → System → Network | [03-Nodes/System/Network/](03-Nodes/System/Network/) |
| Node → Disks → ZFS | [03-Nodes/Disks/ZFS.md](03-Nodes/Disks/ZFS.md) |

Every page follows the same template: **Overview → When to Use → Prerequisites → Procedure → Verification → Common Issues → Summary**. See [TEMPLATE.md](TEMPLATE.md).

---

## Sections

### [01-Getting-Started](01-Getting-Started/) — the application shell

| Page | Description |
|---|---|
| [Interface Tour](01-Getting-Started/Interface-Tour.md) | Navigation panel, resource tree, workspace, header bar, task viewer |
| [Search](01-Getting-Started/Search.md) | Header search box and the Datacenter Search panel |
| [Task Log and Cluster Log](01-Getting-Started/Task-Log-and-Cluster-Log.md) | Monitoring running and completed operations |
| [Interface Troubleshooting](01-Getting-Started/Interface-Troubleshooting.md) | Problems reaching or using the web interface |

### [02-Datacenter](02-Datacenter/) — cluster-wide configuration

| Page | Description |
|---|---|
| [Datacenter Summary](02-Datacenter/Datacenter-Summary.md) | Health, guest counts, and resource widgets |
| [Datacenter Notes](02-Datacenter/Notes.md) | Environment-wide documentation, visible to every administrator |
| [Datacenter Options](02-Datacenter/Options.md) | Cluster-wide defaults and settings |

**[Cluster](02-Datacenter/Cluster/)** — [Overview](02-Datacenter/Cluster/Cluster-Overview.md) · [Create](02-Datacenter/Cluster/Create-Cluster.md) · [Join Node](02-Datacenter/Cluster/Join-Node-to-Cluster.md) · [Remove Node](02-Datacenter/Cluster/Remove-Node-from-Cluster.md) · [Re-Add Removed Node](02-Datacenter/Cluster/Re-Add-Removed-Node.md) · [Delete](02-Datacenter/Cluster/Delete-Cluster.md) · [File System](02-Datacenter/Cluster/Cluster-File-System.md) · [Certificates](02-Datacenter/Cluster/Cluster-Certificates.md) · [Quorum](02-Datacenter/Cluster/Quorum.md) · [Recover Quorum](02-Datacenter/Cluster/Recover-Quorum.md) · [Troubleshooting](02-Datacenter/Cluster/Cluster-Troubleshooting.md)

**[Storage](02-Datacenter/Storage/)** — [Overview](02-Datacenter/Storage/Storage-Overview.md) · [Types](02-Datacenter/Storage/Storage-Types.md) · [Add](02-Datacenter/Storage/Add-Storage.md) · [Manage](02-Datacenter/Storage/Manage-Storage.md) · [Upload Content](02-Datacenter/Storage/Upload-Content.md) · [Content Browser](02-Datacenter/Storage/Storage-Content-Browser.md) · [Import](02-Datacenter/Storage/Storage-Import.md) · [Permissions](02-Datacenter/Storage/Storage-Permissions.md) · [Troubleshooting](02-Datacenter/Storage/Storage-Troubleshooting.md)

**[Backup](02-Datacenter/Backup/)** — [Backup Jobs Overview](02-Datacenter/Backup/Backup-Jobs-Overview.md) · [Create Backup Job](02-Datacenter/Backup/Create-Backup-Job.md) · [Manage Backup Job](02-Datacenter/Backup/Manage-Backup-Job.md) · [Backup Retention](02-Datacenter/Backup/Backup-Retention.md)

**[Replication](02-Datacenter/Replication/)** — [Overview](02-Datacenter/Replication/Replication-Overview.md) · [Create Job](02-Datacenter/Replication/Create-Replication-Job.md) · [Edit Job](02-Datacenter/Replication/Edit-Replication-Job.md) · [Delete Job](02-Datacenter/Replication/Delete-Replication-Job.md) · [Scheduling](02-Datacenter/Replication/Replication-Scheduling.md) · [Status](02-Datacenter/Replication/Replication-Status.md) · [Troubleshooting](02-Datacenter/Replication/Replication-Troubleshooting.md)

**[Permissions](02-Datacenter/Permissions/)** — [Overview](02-Datacenter/Permissions/Permissions-Overview.md) · [Users](02-Datacenter/Permissions/Users.md) · [Groups](02-Datacenter/Permissions/Groups.md) · [Roles](02-Datacenter/Permissions/Roles.md) · [Pools](02-Datacenter/Permissions/Pools.md) · [API Tokens](02-Datacenter/Permissions/API-Tokens.md) · [Two-Factor Authentication](02-Datacenter/Permissions/Two-Factor-Authentication.md) · [Authentication Realms](02-Datacenter/Permissions/Authentication-Realms.md) · [Assign Permissions](02-Datacenter/Permissions/Assign-Permissions.md) · [Troubleshooting](02-Datacenter/Permissions/Permissions-Troubleshooting.md)

**[HA](02-Datacenter/HA/)** — [Overview](02-Datacenter/HA/HA-Overview.md) · [Resources](02-Datacenter/HA/HA-Resources.md) · [Node Affinity](02-Datacenter/HA/Node-Affinity.md) · [Resource Affinity](02-Datacenter/HA/Resource-Affinity.md) · [Fencing](02-Datacenter/HA/Fencing.md) · [Troubleshooting](02-Datacenter/HA/HA-Troubleshooting.md)

**[Firewall](02-Datacenter/Firewall/)** — [Overview](02-Datacenter/Firewall/Firewall-Overview.md) · [Options](02-Datacenter/Firewall/Firewall-Options.md) · [Rules](02-Datacenter/Firewall/Firewall-Rules.md) · [Security Groups](02-Datacenter/Firewall/Security-Groups.md) · [Aliases](02-Datacenter/Firewall/Aliases.md) · [IPSets](02-Datacenter/Firewall/IPSets.md) · [Lockout Recovery](02-Datacenter/Firewall/Firewall-Lockout-Recovery.md)

### [03-Nodes](03-Nodes/) — per-server administration

| Page | Description |
|---|---|
| [Node Summary](03-Nodes/Node-Summary.md) | Health, CPU, memory, storage, network, and system information |
| [Shell](03-Nodes/Shell.md) | Browser-based console access to the node |
| [Reboot Node](03-Nodes/Reboot-Node.md) · [Shutdown Node](03-Nodes/Shutdown-Node.md) | Controlled restart and power-off |
| [Task History](03-Nodes/Task-History.md) | Past operations on the node |
| [Subscription](03-Nodes/Subscription.md) | Licence status |
| [Node Notes](03-Nodes/Node-Notes.md) | Per-server documentation — location, hardware, out-of-band access |
| [Node Replication](03-Nodes/Node-Replication.md) | Every replication job involving this node |
| [Node Firewall](03-Nodes/Node-Firewall.md) | Host filtering, management access, cluster traffic |
| [Reset Root Password](03-Nodes/Reset-Root-Password.md) | Recovering a lost password, from the UI or the console |
| [Node Troubleshooting](03-Nodes/Node-Troubleshooting.md) | Node-level problems |

**[System](03-Nodes/System/)** — [Overview](03-Nodes/System/System-Overview.md) · [Certificates](03-Nodes/System/Certificates.md) · [DNS](03-Nodes/System/DNS.md) · [Hosts](03-Nodes/System/Hosts.md) · [Time and NTP](03-Nodes/System/Time-and-NTP.md) · [Syslog](03-Nodes/System/Syslog.md) · [Boot Mode](03-Nodes/System/Boot-Mode.md) · [Kernel](03-Nodes/System/Kernel.md) · [Services](03-Nodes/System/Services.md) · [Troubleshooting](03-Nodes/System/System-Troubleshooting.md)

**[System → Network](03-Nodes/System/Network/)** — [Overview](03-Nodes/System/Network/Network-Overview.md) · [Linux Bridge](03-Nodes/System/Network/Manage-Linux-Bridge.md) · [Bond](03-Nodes/System/Network/Manage-Bond.md) · [VLAN](03-Nodes/System/Network/Manage-VLAN.md) · [Apply Configuration](03-Nodes/System/Network/Apply-Network-Configuration.md) · [Troubleshooting](03-Nodes/System/Network/Network-Troubleshooting.md)

**[Updates](03-Nodes/Updates/)** — [Update Node](03-Nodes/Updates/Update-Node.md) · [Repositories](03-Nodes/Updates/Repositories.md)

**[Disks](03-Nodes/Disks/)** — [Overview](03-Nodes/Disks/Disks-Overview.md) · [View Disk Information](03-Nodes/Disks/View-Disk-Information.md) · [Disk Management](03-Nodes/Disks/Disk-Management.md) · [LVM](03-Nodes/Disks/LVM.md) · [LVM-Thin](03-Nodes/Disks/LVM-Thin.md) · [Directory](03-Nodes/Disks/Directory.md) · [ZFS](03-Nodes/Disks/ZFS.md) · [Troubleshooting](03-Nodes/Disks/Disk-Troubleshooting.md)

### [04-Virtual-Machines](04-Virtual-Machines/)

[Overview](04-Virtual-Machines/Virtual-Machine-Overview.md) · [Summary](04-Virtual-Machines/VM-Summary.md) · [Create](04-Virtual-Machines/Create-Virtual-Machine.md) · [Manage](04-Virtual-Machines/Manage-Virtual-Machine.md) · [Console](04-Virtual-Machines/VM-Console.md) · [Hardware](04-Virtual-Machines/Manage-VM-Hardware.md) · [Cloud-Init](04-Virtual-Machines/Cloud-Init.md) · [Options](04-Virtual-Machines/VM-Options.md) · [Task History](04-Virtual-Machines/VM-Task-History.md) · [Monitor](04-Virtual-Machines/VM-Monitor.md) · [Snapshots](04-Virtual-Machines/VM-Snapshots.md) · [Backup and Restore](04-Virtual-Machines/Backup-and-Restore-VM.md) · [Replication](04-Virtual-Machines/VM-Replication.md) · [Migrate](04-Virtual-Machines/Migrate-Virtual-Machine.md) · [Clone](04-Virtual-Machines/Clone-Virtual-Machine.md) · [Convert to Template](04-Virtual-Machines/Convert-to-Template.md) · [Delete](04-Virtual-Machines/Delete-Virtual-Machine.md) · [Firewall](04-Virtual-Machines/VM-Firewall.md) · [Permissions](04-Virtual-Machines/VM-Permissions.md) · [Troubleshooting](04-Virtual-Machines/VM-Troubleshooting.md)

### [05-Containers](05-Containers/)

[Overview](05-Containers/Container-Overview.md) · [Summary](05-Containers/CT-Summary.md) · [Create](05-Containers/Create-Container.md) · [Manage](05-Containers/Manage-Container.md) · [Console](05-Containers/Container-Console.md) · [Resources](05-Containers/Manage-Container-Resources.md) · [Network](05-Containers/CT-Network.md) · [DNS](05-Containers/CT-DNS.md) · [Templates](05-Containers/Manage-Container-Templates.md) · [Options](05-Containers/CT-Options.md) · [Task History](05-Containers/CT-Task-History.md) · [Snapshots](05-Containers/CT-Snapshots.md) · [Backup and Restore](05-Containers/Backup-and-Restore-Container.md) · [Replication](05-Containers/CT-Replication.md) · [Migrate](05-Containers/Migrate-Container.md) · [Clone](05-Containers/Clone-Container.md) · [Delete](05-Containers/Delete-Container.md) · [Firewall](05-Containers/CT-Firewall.md) · [Permissions](05-Containers/CT-Permissions.md) · [Troubleshooting](05-Containers/Container-Troubleshooting.md)

---

## Coverage Status

116 pages, 257 screenshots. Fully illustrated sections are marked ✅; the rest show how many `[ Place Screenshot Here ]` placeholders remain.

Every placeholder carries a **Capture:** line naming the exact screen and state to photograph, so screenshots can be worked through page by page without guessing what each one should show.

| Section | Pages | Screenshots | Status |
|---|---:|---:|---|
| 01-Getting-Started | 4 | 1 | 20 placeholders |
| 02-Datacenter (root) | 2 | 0 | 9 placeholders |
| 02-Datacenter / Cluster | 9 | 27 | 10 placeholders |
| 02-Datacenter / Storage | 6 | 25 | ✅ complete |
| 02-Datacenter / Backup | 4 | 0 | 18 placeholders |
| 02-Datacenter / Replication | 7 | 0 | 47 placeholders |
| 02-Datacenter / Permissions | 10 | 12 | 43 placeholders |
| 02-Datacenter / HA | 6 | 0 | 33 placeholders |
| 02-Datacenter / Firewall | 6 | 0 | 25 placeholders |
| 03-Nodes | 34 | 45 | 107 placeholders |
| 04-Virtual-Machines | 14 | 80 | 15 placeholders |
| 05-Containers | 14 | 67 | 14 placeholders |

Pages also carry **Verify:** markers where a UI label could not be confirmed without a live environment. Find them with:

```bash
grep -rn '> \*\*Verify:\*\*' --include='*.md' . --exclude=TEMPLATE.md --exclude=CONTRIBUTING.md
```

---

## Planned Pages

The following UI areas exist in VM2Cloud but are **not yet documented**. Their location in this structure is fixed; the pages themselves have not been written.

### 01-Getting-Started
- `What-Is-VM2Cloud.md` — product introduction
- `Logging-In.md` — first access and the login screen
- `Resource-Tree-and-Views.md` — Server View, Folder View, Pool View, Tag View
- `My-Settings.md` — user menu, password change, preferences
- `Tags.md` — tagging resources

### 02-Datacenter
- `SDN/` — software-defined networking: `SDN-Overview.md`, `Zones.md`, `VNets.md`
- `Ceph/` — `Ceph-Overview.md`, `Ceph-Monitors-and-OSDs.md`, `Ceph-Pools.md`
- `ACME-Certificates.md`, `Notifications.md`, `Metric-Server.md`, `Support.md`

### 03-Nodes
- `Node-Ceph.md`

### 04-Virtual-Machines
- All tabs documented. Guest Notes are covered by the Notes panel on [VM Summary](04-Virtual-Machines/VM-Summary.md).

### 05-Containers
- All tabs documented. Guest Notes are covered by the Notes panel on [CT Summary](05-Containers/CT-Summary.md).

### Appendix
- `06-CLI-Reference.md` — commands used across the set and what each verifies. The CLI has no UI location, so it cannot be mirrored into the structure.

---

## Contributing

- [COVERAGE.md](COVERAGE.md) — every UI panel and CLI operation mapped against what is documented
- [CONTRIBUTING.md](CONTRIBUTING.md) — writing conventions, screenshot rules, file placement
- [TEMPLATE.md](TEMPLATE.md) — copy this to start a new page
- [GLOSSARY.md](GLOSSARY.md) — terminology used across the documentation
