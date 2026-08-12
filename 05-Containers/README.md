# Containers

Lightweight Linux guests that share the host kernel. Containers use fewer resources than virtual machines but can only run Linux.

Corresponds to selecting a container in the resource tree. For guests that need their own kernel or a non-Linux operating system, see [04-Virtual-Machines](../04-Virtual-Machines/).

---

## Pages

| Page | Covers |
|---|---|
| [Container Overview](Container-Overview.md) | What a container is, the tabs available, and how containers differ from VMs |
| [CT Summary](CT-Summary.md) | The default view — status, CPU, memory, swap, Notes |
| [Create Container](Create-Container.md) | The creation wizard: General, Template, Disks, CPU, Memory, Network, DNS, Confirm |
| [Manage Container](Manage-Container.md) | Start, stop, shutdown, and reboot |
| [Container Console](Container-Console.md) | Connecting to the container shell |
| [Manage Container Resources](Manage-Container-Resources.md) | CPU, memory, swap, root disk resizing, and mount points |
| [Manage Container Templates](Manage-Container-Templates.md) | Downloading and managing the base images used to create containers |
| [Container Options](CT-Options.md) | Start at boot, sequencing, protection, privilege model, features |
| [Container Firewall](CT-Firewall.md) | Per-container traffic filtering, enforced by the host |
| [Container Snapshots](CT-Snapshots.md) | Creating, rolling back, and removing snapshots |
| [Backup and Restore Container](Backup-and-Restore-Container.md) | Creating backups and restoring from them |
| [Migrate Container](Migrate-Container.md) | Moving a container to another node |
| [Clone Container](Clone-Container.md) | Copying a container |
| [Delete Container](Delete-Container.md) | Permanently removing a container |
| [CT Network](CT-Network.md) | Interfaces, bridges, VLANs, addressing |
| [CT DNS](CT-DNS.md) | Search domain and nameservers |
| [CT Task History](CT-Task-History.md) | Operations performed on this container |
| [CT Replication](CT-Replication.md) | Replication jobs for this container |
| [CT Permissions](CT-Permissions.md) | Who can access this container |
| [Container Troubleshooting](Container-Troubleshooting.md) | Container will not start, console, network, mount, and resource problems |

---

## Related

- Storage that holds container disks and templates → [Datacenter → Storage](../02-Datacenter/Storage/)
- Automatic recovery after a node failure → [Datacenter → HA](../02-Datacenter/HA/)
- Keeping a synchronized copy on another node → [Datacenter → Replication](../02-Datacenter/Replication/)

---

## Planned Pages

Every container tab is now documented. The Notes panel is covered by [CT Summary](CT-Summary.md).
