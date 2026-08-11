# Virtual Machines

Fully virtualized guests, each running its own kernel and operating system.

Corresponds to selecting a VM in the resource tree. For lightweight Linux guests that share the host kernel, see [05-Containers](../05-Containers/) instead.

---

## Pages

| Page | Covers |
|---|---|
| [Virtual Machine Overview](Virtual-Machine-Overview.md) | What a VM is, the tabs available, and how VMs fit into the environment |
| [Create Virtual Machine](Create-Virtual-Machine.md) | The full creation wizard: General, OS, System, Disks, CPU, Memory, Network, Confirm |
| [Manage Virtual Machine](Manage-Virtual-Machine.md) | Start, stop, shutdown, reboot, pause, and resume |
| [VM Console](VM-Console.md) | Connecting to the guest display, toolbar, and special keys |
| [Manage VM Hardware](Manage-VM-Hardware.md) | Disks, CPU, memory, network devices, ISO images, USB, PCI, and TPM |
| [VM Snapshots](VM-Snapshots.md) | Creating, viewing, restoring, and deleting snapshots |
| [Backup and Restore VM](Backup-and-Restore-VM.md) | Creating backups and restoring from them |
| [Migrate Virtual Machine](Migrate-Virtual-Machine.md) | Moving a VM to another node |
| [Clone Virtual Machine](Clone-Virtual-Machine.md) | Full and linked clones |
| [Delete Virtual Machine](Delete-Virtual-Machine.md) | Permanently removing a VM |
| [VM Troubleshooting](VM-Troubleshooting.md) | VM will not start, console problems, performance, disk and network issues |

---

## Related

- Storage that holds VM disks and ISO images → [Datacenter → Storage](../02-Datacenter/Storage/)
- Automatic recovery after a node failure → [Datacenter → HA](../02-Datacenter/HA/)
- Keeping a synchronized copy on another node → [Datacenter → Replication](../02-Datacenter/Replication/)

---

## Planned Pages

Not yet written: `VM-Summary.md`, `Cloud-Init.md`, `VM-Options.md`, `VM-Notes.md`, `VM-Task-History.md`, `VM-Monitor.md`, `Convert-to-Template.md`, `VM-Firewall.md`, `VM-Permissions.md`.
