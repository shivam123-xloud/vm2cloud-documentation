# Datacenter

Cluster-wide configuration. Everything here applies across all nodes rather than to one server.

Corresponds to selecting **Datacenter** at the top of the resource tree.

---

## Datacenter Pages

| Page | Covers |
|---|---|
| [Datacenter Summary](Datacenter-Summary.md) | Health status, guest counts, and resource widgets |
| [Datacenter Options](Options.md) | Cluster-wide defaults and settings |

---

## Cluster

Forming and maintaining the cluster itself.

[Cluster Overview](Cluster/Cluster-Overview.md) · [Create Cluster](Cluster/Create-Cluster.md) · [Join Node to Cluster](Cluster/Join-Node-to-Cluster.md) · [Remove Node from Cluster](Cluster/Remove-Node-from-Cluster.md) · [Delete Cluster](Cluster/Delete-Cluster.md) · [Cluster File System](Cluster/Cluster-File-System.md) · [Cluster Certificates](Cluster/Cluster-Certificates.md) · [Quorum](Cluster/Quorum.md) · [Cluster Troubleshooting](Cluster/Cluster-Troubleshooting.md)

## Storage

Defining where disks, ISO images, templates, and backups are kept.

[Storage Overview](Storage/Storage-Overview.md) · [Storage Types](Storage/Storage-Types.md) · [Add Storage](Storage/Add-Storage.md) · [Manage Storage](Storage/Manage-Storage.md) · [Upload Content](Storage/Upload-Content.md) · [Storage Troubleshooting](Storage/Storage-Troubleshooting.md)

## Replication

Synchronizing guest data to another node on a schedule.

[Replication Overview](Replication/Replication-Overview.md) · [Create Replication Job](Replication/Create-Replication-Job.md) · [Edit Replication Job](Replication/Edit-Replication-Job.md) · [Delete Replication Job](Replication/Delete-Replication-Job.md) · [Replication Scheduling](Replication/Replication-Scheduling.md) · [Replication Status](Replication/Replication-Status.md) · [Replication Troubleshooting](Replication/Replication-Troubleshooting.md)

## Permissions

Users, groups, roles, and how access is granted.

[Permissions Overview](Permissions/Permissions-Overview.md) · [Users](Permissions/Users.md) · [Groups](Permissions/Groups.md) · [Roles](Permissions/Roles.md) · [API Tokens](Permissions/API-Tokens.md) · [Two-Factor Authentication](Permissions/Two-Factor-Authentication.md) · [Authentication Realms](Permissions/Authentication-Realms.md) · [Assign Permissions](Permissions/Assign-Permissions.md) · [Permissions Troubleshooting](Permissions/Permissions-Troubleshooting.md)

## High Availability

Automatic recovery of guests after a node failure.

[HA Overview](HA/HA-Overview.md) · [HA Resources](HA/HA-Resources.md) · [Node Affinity](HA/Node-Affinity.md) · [Resource Affinity](HA/Resource-Affinity.md) · [Fencing](HA/Fencing.md) · [HA Troubleshooting](HA/HA-Troubleshooting.md)

---

## Planned Pages

Not yet written:

- `Notes.md`
- `Backup/` — `Backup-Jobs-Overview.md`, `Create-Backup-Job.md`, `Manage-Backup-Job.md`, `Backup-Retention.md`
- `Permissions/Pools.md`
- `Firewall/` — `Firewall-Overview.md`, `Firewall-Options.md`, `Firewall-Rules.md`, `Security-Groups.md`, `Aliases.md`, `IPSets.md`
- `SDN/` — `SDN-Overview.md`, `Zones.md`, `VNets.md`
- `Ceph/Ceph-Overview.md`
- `ACME-Certificates.md`, `Notifications.md`, `Metric-Server.md`, `Support.md`
