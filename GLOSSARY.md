# Glossary

Terminology used throughout the VM2Cloud VE documentation.

---

## Infrastructure

| Term | Definition |
|---|---|
| **Datacenter** | The top level of the resource tree. Holds cluster-wide configuration that applies to every node. |
| **Node** | A single physical server running VM2Cloud VE. Provides the CPU, memory, storage, and networking that guests consume. |
| **Cluster** | Two or more nodes managed as a single environment, enabling shared configuration, migration, and High Availability. |
| **Guest** | A virtual machine or a container. Used when a statement applies to both. |
| **Resource tree** | The navigation panel on the left of the interface, showing Datacenter, nodes, and guests. |
| **Workspace** | The main area of the interface, showing the panel for whatever is selected in the resource tree. |

---

## Guests

| Term | Definition |
|---|---|
| **Virtual Machine (VM)** | A fully virtualized guest running its own kernel and operating system. |
| **Container (CT)** | A lightweight guest sharing the host kernel. Uses fewer resources than a VM but is limited to Linux. |
| **VMID** | The unique numeric identifier assigned to each guest. |
| **Template** | A guest converted into a read-only base image used to create new guests. |
| **Container template** | A downloadable base image used to create a new container. |
| **Snapshot** | A point-in-time capture of a guest's state that can be rolled back to. |
| **Clone** | A copy of a guest. A *full clone* is independent; a *linked clone* shares base data with its source. |
| **Migration** | Moving a guest from one node to another. *Live* migration keeps the guest running. |

---

## Cluster and High Availability

| Term | Definition |
|---|---|
| **Corosync** | The service that carries cluster communication between nodes. |
| **Quorum** | The majority of cluster votes required for the cluster to make configuration changes safely. |
| **Quorate** | The state of a cluster that has quorum. |
| **Expected votes** | The total number of votes the cluster expects from all configured nodes. |
| **Split-brain** | A failure where two groups of nodes both believe they are the active cluster. Quorum and fencing prevent it. |
| **High Availability (HA)** | The framework that automatically recovers guests when the node running them fails. |
| **HA resource** | A guest registered with the HA system so its state is managed automatically. |
| **Fencing** | Guaranteeing that an unreachable node is no longer running its HA resources before recovering them elsewhere. |
| **Watchdog** | A timer that resets a node if the HA services stop refreshing it. The mechanism behind fencing. |
| **Node affinity** | A rule defining which nodes an HA resource should prefer or be restricted to. |
| **Resource affinity** | A rule keeping HA resources on the same node or deliberately apart. |
| **QDevice** | An external vote provider used to maintain quorum in two-node or even-node clusters. |

---

## Storage

| Term | Definition |
|---|---|
| **Storage** | A configured location where VM2Cloud VE keeps disks, ISO images, templates, or backups. |
| **Content type** | What a storage is allowed to hold — disk images, ISO images, container templates, backups, or snippets. |
| **Local storage** | Storage available on a single node only. |
| **Shared storage** | Storage reachable from every node, required for live migration without disk transfer. |
| **LVM** | Logical Volume Manager. Block storage carved from a volume group. |
| **LVM-Thin** | LVM using a thin pool, allowing over-provisioning and snapshots. |
| **ZFS** | A combined filesystem and volume manager supporting snapshots, clones, and replication. |
| **Directory** | Storage backed by an ordinary filesystem path on the node. |
| **Replication** | Periodic synchronization of a guest's local storage to another node. |

---

## Access Control

| Term | Definition |
|---|---|
| **Realm** | An authentication source, such as the local Linux accounts or an Active Directory domain. |
| **User** | An account that authenticates through a realm. Written as `name@realm`. |
| **Group** | A collection of users that permissions can be assigned to collectively. |
| **Role** | A named set of privileges. |
| **Permission** | A role granted to a user or group on a specific path in the resource hierarchy. |
| **Pool** | A logical grouping of guests and storage that permissions can be applied to as a unit. |
| **API token** | A credential allowing programmatic access without a password. |
| **Two-Factor Authentication (2FA)** | A second authentication step in addition to the password. |

---

## Networking

| Term | Definition |
|---|---|
| **Linux bridge** | A virtual switch connecting guests to a physical network interface. |
| **Bond** | Two or more physical interfaces combined for redundancy or increased throughput. |
| **VLAN** | A tagged virtual network allowing traffic separation over shared physical links. |
| **Management network** | The network carrying the web interface and cluster communication. |

---

## Operations

| Term | Definition |
|---|---|
| **Task** | A background operation such as creating a VM or running a backup. |
| **Task log** | The panel at the bottom of the interface listing running and completed tasks. |
| **Cluster log** | The cluster-wide record of events across all nodes. |
| **Backup** | An independent, restorable copy of a guest. Distinct from replication. |
| **Restore** | Recreating a guest from a backup. |
| **Repository** | A configured package source used when updating a node. |
