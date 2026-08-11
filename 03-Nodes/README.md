# Nodes

Administration of an individual server in the VM2Cloud environment.

Corresponds to selecting a node under **Datacenter** in the resource tree. Settings here are node-specific — applying them to one node does not apply them to the others.

---

## Node Pages

| Page | Covers |
|---|---|
| [Node Summary](Node-Summary.md) | Health, CPU, memory, storage, network, and system information |
| [Shell](Shell.md) | Browser-based console access to the node |
| [Reboot Node](Reboot-Node.md) | Controlled restart |
| [Shutdown Node](Shutdown-Node.md) | Controlled power-off |
| [Task History](Task-History.md) | Operations previously run on this node |
| [Subscription](Subscription.md) | Licence status |
| [Node Firewall](Node-Firewall.md) | Host filtering — management access, SSH, cluster traffic |
| [Node Troubleshooting](Node-Troubleshooting.md) | Node offline, unreachable, or misbehaving |

---

## System

Node-level operating system configuration.

[System Overview](System/System-Overview.md) · [Certificates](System/Certificates.md) · [DNS](System/DNS.md) · [Hosts](System/Hosts.md) · [Time and NTP](System/Time-and-NTP.md) · [Syslog](System/Syslog.md) · [Boot Mode](System/Boot-Mode.md) · [Kernel](System/Kernel.md) · [Services](System/Services.md) · [System Troubleshooting](System/System-Troubleshooting.md)

### Network

Bridges, bonds, and VLANs. Reached through **System → Network**.

[Network Overview](System/Network/Network-Overview.md) · [Manage Linux Bridge](System/Network/Manage-Linux-Bridge.md) · [Manage Bond](System/Network/Manage-Bond.md) · [Manage VLAN](System/Network/Manage-VLAN.md) · [Apply Network Configuration](System/Network/Apply-Network-Configuration.md) · [Network Troubleshooting](System/Network/Network-Troubleshooting.md)

---

## Updates

Keeping the node patched.

[Update Node](Updates/Update-Node.md) · [Repositories](Updates/Repositories.md)

---

## Disks

Physical disks and the storage backends built on them.

[Disks Overview](Disks/Disks-Overview.md) · [View Disk Information](Disks/View-Disk-Information.md) · [Disk Management](Disks/Disk-Management.md) · [LVM](Disks/LVM.md) · [LVM-Thin](Disks/LVM-Thin.md) · [Directory](Disks/Directory.md) · [ZFS](Disks/ZFS.md) · [Disk Troubleshooting](Disks/Disk-Troubleshooting.md)

> Disks here are the physical devices attached to this node. To make storage available to guests, see [Datacenter → Storage](../02-Datacenter/Storage/).

---

## Planned Pages

Not yet written: `Node-Notes.md`, `Node-Ceph.md`, `Node-Replication.md`.
