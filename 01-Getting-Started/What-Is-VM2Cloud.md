# What Is VM2Cloud

---

## Overview

**VM2Cloud** is a virtualization platform for running virtual machines and containers on your own hardware, managed through a single web interface.

One installation on one server gives you a working virtualization host. Join several servers into a cluster and they are managed as one environment, with guests able to move between them, storage shared across them, and workloads recovered automatically when a server fails.

It combines in one product what is often assembled from several: the hypervisor, the management interface, storage integration, networking, backup, and high availability.

---

## What It Runs

VM2Cloud runs two kinds of guest, and choosing between them is the first decision for any workload.

| | Virtual machine | Container |
|---|---|---|
| Runs | Its own full operating system and kernel | Shares the host kernel |
| Guest operating systems | Any — Linux, Windows, BSD | Linux only |
| Resource overhead | Higher | Lower |
| Isolation | Stronger | Weaker, especially when privileged |
| Boot time | Seconds to minutes | Near-instant |
| Best for | Windows, unusual kernels, strong isolation | Linux services, density |

A container is lighter and starts faster; a virtual machine is more isolated and runs anything. Where both would work, containers use markedly fewer resources.

See [Virtual Machine Overview](../04-Virtual-Machines/Virtual-Machine-Overview.md) and [Container Overview](../05-Containers/Container-Overview.md).

---

## How It Is Organised

The interface reflects a hierarchy, and understanding it makes everything else easier to find.

```text
Datacenter          cluster-wide configuration
   └── Node         one physical server
         └── Guest  a virtual machine or container
         └── Storage
```

| Level | Scope | Examples |
|---|---|---|
| **Datacenter** | Everything, across all nodes | Users, storage definitions, backup jobs, HA rules, firewall |
| **Node** | One physical server | Network interfaces, disks, updates, services |
| **Guest** | One VM or container | Hardware, console, snapshots, its own firewall |

Settings live at the level they apply to. A user account is defined once at datacenter level; a network bridge is configured per node, because it belongs to that server's physical interfaces.

This documentation follows the same structure — see the [repository index](../README.md).

---

## What It Provides

**Clustering.** Several nodes managed as one. Shared configuration, a single interface, and guests that can move between nodes. See [Cluster Overview](../02-Datacenter/Cluster/Cluster-Overview.md).

**Storage flexibility.** Local disks with LVM, ZFS, or plain directories; shared storage over the network; or [Ceph](../02-Datacenter/Ceph/Ceph-Overview.md) pooling disks across nodes into one distributed layer. See [Storage Types](../02-Datacenter/Storage/Storage-Types.md).

**Live migration.** Move a running guest between nodes without stopping it. With shared storage, only memory transfers, so the move takes seconds.

**Backup and replication.** Scheduled [backup jobs](../02-Datacenter/Backup/Backup-Jobs-Overview.md) producing independent restore points, and [replication](../02-Datacenter/Replication/Replication-Overview.md) keeping a current copy on another node. They solve different problems, and production workloads generally want both.

**High availability.** Automatic recovery of guests when a node fails. See [HA Overview](../02-Datacenter/HA/HA-Overview.md).

**Access control.** Users, groups, roles, and pools, with permissions granted on paths in the hierarchy. See [Permissions Overview](../02-Datacenter/Permissions/Permissions-Overview.md).

**Firewall.** Filtering at datacenter, node, and guest level. See [Firewall Overview](../02-Datacenter/Firewall/Firewall-Overview.md).

---

## What It Does Not Do

Worth stating plainly, because assumptions here cause real damage.

**It is not a backup system by itself.** Snapshots and replication protect against hardware failure. Neither protects against deletion, corruption, or a mistake made last week — replication faithfully copies the deletion. Configure [backup jobs](../02-Datacenter/Backup/Backup-Jobs-Overview.md).

**High availability is not zero downtime.** HA restarts a guest on another node after a failure. The guest still stops, boots, and needs its application to recover. Expect minutes, not zero.

**It does not manage the guest operating system.** Patching, configuration, and monitoring inside a guest remain your responsibility.

**Clustering is not automatic redundancy.** A cluster without shared or replicated storage still loses access to a failed node's guests.

---

## Getting Started

If this is a new environment:

1. Read the [Interface Tour](Interface-Tour.md) to learn the layout.
2. [Log in](Logging-In.md) and change the default password.
3. Create additional [users](../02-Datacenter/Permissions/Users.md) rather than sharing the root account.
4. Configure [storage](../02-Datacenter/Storage/Storage-Overview.md).
5. Create your first [virtual machine](../04-Virtual-Machines/Create-Virtual-Machine.md) or [container](../05-Containers/Create-Container.md).
6. Set up [backup jobs](../02-Datacenter/Backup/Backup-Jobs-Overview.md) before the workload matters.
7. Consider [clustering](../02-Datacenter/Cluster/Create-Cluster.md) if you have more than one server.

Step 6 is the one people defer. Configure backups while the environment is empty and the habit is cheap.

---

# Verification

You have a working environment when:

* The web interface loads and you can log in.
* At least one node shows **Online**.
* Storage is configured with capacity available.
* A test guest can be created, started, and reached.
* A backup job exists and has run successfully.
* A backup has been restored to a spare guest ID at least once.

That last item is the only real proof the backups work.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Cannot reach the interface | See [Logging In](Logging-In.md) and [Interface Troubleshooting](Interface-Troubleshooting.md). |
| Unsure whether to use a VM or container | Windows or unusual kernel means a VM. A Linux service where density matters means a container. |
| Cannot find a setting | It is probably at a different level. Datacenter for cluster-wide, node for server-specific, guest for one workload. |
| Guest will not start | Usually storage or resource limits. See [VM Troubleshooting](../04-Virtual-Machines/VM-Troubleshooting.md). |
| Unclear whether backup or replication is needed | Both. They protect against different failures. |

---

# Best Practices

- Create individual user accounts rather than sharing root. See [Users](../02-Datacenter/Permissions/Users.md).
- Configure backups before the workload becomes important.
- Test a restore. An untested backup is an assumption.
- Use containers for Linux workloads where density matters, VMs where isolation does.
- Keep at least two administrator accounts, so a lost password never requires a console.
- Document the environment in [Datacenter Notes](../02-Datacenter/Notes.md).
- Deploy an odd number of nodes if clustering, so quorum survives a failure.

---

# Related Documentation

- [Interface Tour](Interface-Tour.md)
- [Logging In](Logging-In.md)
- [Resource Tree and Views](Resource-Tree-and-Views.md)
- [Virtual Machine Overview](../04-Virtual-Machines/Virtual-Machine-Overview.md)
- [Container Overview](../05-Containers/Container-Overview.md)
- [Cluster Overview](../02-Datacenter/Cluster/Cluster-Overview.md)
- [Storage Overview](../02-Datacenter/Storage/Storage-Overview.md)
- [Backup Jobs Overview](../02-Datacenter/Backup/Backup-Jobs-Overview.md)
- [HA Overview](../02-Datacenter/HA/HA-Overview.md)
- [Glossary](../GLOSSARY.md)

---

# Summary

VM2Cloud runs virtual machines and containers on your own hardware, managed from one web interface, with clustering, storage, networking, backup, and high availability included rather than assembled separately.

Its structure — Datacenter, Node, Guest — determines where every setting lives, and knowing that makes the interface predictable. The things it does *not* do are worth remembering too: replication and snapshots are not backups, HA is not zero downtime, and nothing here manages what happens inside a guest.
