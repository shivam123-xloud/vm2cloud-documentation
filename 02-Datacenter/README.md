# Datacenter

Cluster-wide configuration. Everything here applies across all nodes rather than to one server.

Corresponds to selecting **Datacenter** at the top of the resource tree.

---

## Datacenter Pages

| Page | Covers |
|---|---|
| [Datacenter Summary](Datacenter-Summary.md) | Health status, guest counts, and resource widgets |
| [Datacenter Notes](Notes.md) | Environment-wide documentation |
| [Datacenter Options](Options.md) | Cluster-wide defaults and settings |

---

## Cluster

Forming and maintaining the cluster itself.

[Cluster Overview](Cluster/Cluster-Overview.md) · [Create Cluster](Cluster/Create-Cluster.md) · [Join Node to Cluster](Cluster/Join-Node-to-Cluster.md) · [Remove Node from Cluster](Cluster/Remove-Node-from-Cluster.md) · [Re-Add a Removed Node](Cluster/Re-Add-Removed-Node.md) · [Delete Cluster](Cluster/Delete-Cluster.md) · [Cluster File System](Cluster/Cluster-File-System.md) · [Cluster Certificates](Cluster/Cluster-Certificates.md) · [Quorum](Cluster/Quorum.md) · [Recover Quorum](Cluster/Recover-Quorum.md) · [Cluster Troubleshooting](Cluster/Cluster-Troubleshooting.md)

## Storage

Defining where disks, ISO images, templates, and backups are kept.

[Storage Overview](Storage/Storage-Overview.md) · [Storage Types](Storage/Storage-Types.md) · [Add Storage](Storage/Add-Storage.md) · [Manage Storage](Storage/Manage-Storage.md) · [Upload Content](Storage/Upload-Content.md) · [Content Browser](Storage/Storage-Content-Browser.md) · [Import](Storage/Storage-Import.md) · [Permissions](Storage/Storage-Permissions.md) · [Storage Troubleshooting](Storage/Storage-Troubleshooting.md)

## Backup

Scheduled, cluster-wide backup jobs with retention.

[Backup Jobs Overview](Backup/Backup-Jobs-Overview.md) · [Create Backup Job](Backup/Create-Backup-Job.md) · [Manage Backup Job](Backup/Manage-Backup-Job.md) · [Backup Retention](Backup/Backup-Retention.md)

## Replication

Synchronizing guest data to another node on a schedule.

[Replication Overview](Replication/Replication-Overview.md) · [Create Replication Job](Replication/Create-Replication-Job.md) · [Edit Replication Job](Replication/Edit-Replication-Job.md) · [Delete Replication Job](Replication/Delete-Replication-Job.md) · [Replication Scheduling](Replication/Replication-Scheduling.md) · [Replication Status](Replication/Replication-Status.md) · [Replication Troubleshooting](Replication/Replication-Troubleshooting.md)

## Permissions

Users, groups, roles, and how access is granted.

[Permissions Overview](Permissions/Permissions-Overview.md) · [Users](Permissions/Users.md) · [Groups](Permissions/Groups.md) · [Roles](Permissions/Roles.md) · [Pools](Permissions/Pools.md) · [API Tokens](Permissions/API-Tokens.md) · [Two-Factor Authentication](Permissions/Two-Factor-Authentication.md) · [Authentication Realms](Permissions/Authentication-Realms.md) · [Assign Permissions](Permissions/Assign-Permissions.md) · [Permissions Troubleshooting](Permissions/Permissions-Troubleshooting.md)

## High Availability

Automatic recovery of guests after a node failure.

[HA Overview](HA/HA-Overview.md) · [HA Resources](HA/HA-Resources.md) · [Node Affinity](HA/Node-Affinity.md) · [Resource Affinity](HA/Resource-Affinity.md) · [Fencing](HA/Fencing.md) · [HA Troubleshooting](HA/HA-Troubleshooting.md)

## Ceph

Distributed storage pooling disks across every node.

[Ceph Overview](Ceph/Ceph-Overview.md) · [Monitors and OSDs](Ceph/Ceph-Monitors-and-OSDs.md) · [Ceph Pools](Ceph/Ceph-Pools.md) · [Node Ceph](../03-Nodes/Node-Ceph.md)

## SDN

Virtual networks defined centrally and applied to every node.

[SDN Overview](SDN/SDN-Overview.md) · [Zones](SDN/Zones.md) · [VNets](SDN/VNets.md) · [Options](SDN/SDN-Options.md) · [IPAM](SDN/IPAM.md) · [VNet Firewall](SDN/VNet-Firewall.md) · [Fabrics](SDN/Fabrics.md) · [Route Maps](SDN/Route-Maps.md) · [Prefix Lists](SDN/Prefix-Lists.md)

## Services

[ACME Certificates](ACME-Certificates.md) · [Notifications](Notifications.md) · [Metric Server](Metric-Server.md) · [Support](Support.md)

## Firewall

Cluster-wide filtering and the reusable objects used at every level.

[Firewall Overview](Firewall/Firewall-Overview.md) · [Firewall Options](Firewall/Firewall-Options.md) · [Firewall Rules](Firewall/Firewall-Rules.md) · [Security Groups](Firewall/Security-Groups.md) · [Aliases](Firewall/Aliases.md) · [IPSets](Firewall/IPSets.md) · [Lockout Recovery](Firewall/Firewall-Lockout-Recovery.md)

> The firewall applies at three levels. Concepts are documented once in
> [Firewall Overview](Firewall/Firewall-Overview.md); the node and guest panels are
> covered in [Node Firewall](../03-Nodes/Node-Firewall.md),
> [VM Firewall](../04-Virtual-Machines/VM-Firewall.md), and
> [Container Firewall](../05-Containers/CT-Firewall.md).

---

## Planned Pages

Every Datacenter panel is now documented. Notifications, Metric Server, and Support carry `Verify` notes because their presence in this deployment is unconfirmed.
