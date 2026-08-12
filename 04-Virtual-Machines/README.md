# Virtual Machines

Fully virtualized guests, each running its own kernel and operating system.

Corresponds to selecting a VM in the resource tree. For lightweight Linux guests that share the host kernel, see [05-Containers](../05-Containers/) instead.

---

## Pages

| Page | Covers |
|---|---|
| [Virtual Machine Overview](Virtual-Machine-Overview.md) | What a VM is, the tabs available, and how VMs fit into the environment |
| [VM Summary](VM-Summary.md) | The default view — status, usage, HA state, IPs, Notes |
| [Create Virtual Machine](Create-Virtual-Machine.md) | The full creation wizard: General, OS, System, Disks, CPU, Memory, Network, Confirm |
| [Manage Virtual Machine](Manage-Virtual-Machine.md) | Start, stop, shutdown, reboot, pause, and resume |
| [VM Console](VM-Console.md) | Connecting to the guest display, toolbar, and special keys |
| [Manage VM Hardware](Manage-VM-Hardware.md) | Disks, CPU, memory, network devices, ISO images, USB, PCI, and TPM |
| [Cloud-Init](Cloud-Init.md) | Automatic first-boot configuration — user, SSH key, network, DNS |
| [VM Options](VM-Options.md) | Boot order, start at boot, start/shutdown sequencing, protection, guest agent |
| [VM Firewall](VM-Firewall.md) | Per-machine traffic filtering |
| [VM Snapshots](VM-Snapshots.md) | Creating, viewing, restoring, and deleting snapshots |
| [Backup and Restore VM](Backup-and-Restore-VM.md) | Creating backups and restoring from them |
| [Migrate Virtual Machine](Migrate-Virtual-Machine.md) | Moving a VM to another node |
| [Clone Virtual Machine](Clone-Virtual-Machine.md) | Full and linked clones |
| [Delete Virtual Machine](Delete-Virtual-Machine.md) | Permanently removing a VM |
| [VM Task History](VM-Task-History.md) | Operations performed on this machine, and their output |
| [VM Monitor](VM-Monitor.md) | Low-level hypervisor command interface, for diagnostics |
| [VM Replication](VM-Replication.md) | Replication jobs for this machine |
| [Convert to Template](Convert-to-Template.md) | Turning a machine into a read-only base image |
| [VM Permissions](VM-Permissions.md) | Who can access this machine |
| [VM Troubleshooting](VM-Troubleshooting.md) | VM will not start, console problems, performance, disk and network issues |

---

## Related

- Storage that holds VM disks and ISO images → [Datacenter → Storage](../02-Datacenter/Storage/)
- Automatic recovery after a node failure → [Datacenter → HA](../02-Datacenter/HA/)
- Keeping a synchronized copy on another node → [Datacenter → Replication](../02-Datacenter/Replication/)

---

## Planned Pages

Every virtual machine tab is now documented. The Notes panel is covered by [VM Summary](VM-Summary.md).
