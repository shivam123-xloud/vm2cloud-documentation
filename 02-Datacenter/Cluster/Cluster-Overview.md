# Cluster Overview

---

## Overview

A **cluster** is a group of two or more VM2Cloud nodes that work together as a single management domain. After joining a cluster, administrators can manage all nodes from a single web interface while maintaining centralized configuration and resource management.

Each node continues to use its own CPU, memory, and local storage, but cluster members share configuration information and communicate to coordinate management operations.

Clusters simplify administration by allowing administrators to manage multiple nodes from a single interface instead of logging in to each node individually.

---

## Benefits of a Cluster

A VM2Cloud cluster provides several advantages, including:

- Centralized management of multiple nodes.
- Single web interface for cluster administration.
- Shared cluster configuration across all nodes.
- Live migration of virtual machines between nodes.
- High Availability (HA) support.
- Replication support.
- Simplified resource management.
- Improved scalability by adding additional nodes.

---

## Cluster Architecture

A VM2Cloud cluster consists of one or more physical nodes connected through a reliable network.

Each node contributes its own computing resources while sharing cluster configuration with the other members.

The cluster relies on several components to operate correctly:

- Corosync for cluster communication.
- Cluster File System (pmxcfs) for synchronized configuration.
- Quorum to maintain cluster consistency.
- Cluster certificates for secure communication between nodes.

---

## Cluster Components

### Node

A node is an individual VM2Cloud server participating in the cluster.

Each node has its own:

- CPU
- Memory
- Network interfaces
- Storage
- Virtual machines
- Containers

---

### Corosync

Corosync is the communication service used by cluster members.

It is responsible for:

- Maintaining communication between nodes.
- Detecting node failures.
- Maintaining cluster membership.
- Supporting quorum calculations.

---

### Cluster File System (pmxcfs)

The Cluster File System stores the cluster configuration.

Configuration changes made on one node are automatically synchronized with the other cluster members.

Examples include:

- Virtual machine configuration
- Container configuration
- Storage configuration
- Cluster configuration
- User and permission configuration

---

### Quorum

Quorum ensures that only a healthy majority of cluster nodes can make cluster-wide changes.

Maintaining quorum helps prevent split-brain situations where multiple nodes attempt to operate independently.

---

### Cluster Certificates

Each cluster node uses certificates to establish trusted communication with other cluster members.

Certificates are automatically managed as part of the cluster configuration.

---

## Cluster Requirements

Before creating a cluster, ensure that:

- All nodes are running compatible VM2Cloud versions.
- Nodes can communicate over the management network.
- Hostnames and DNS resolution are configured correctly.
- System time is synchronized across all nodes.
- Required firewall ports are open.
- Each node has a unique hostname and IP address.

---

## Typical Cluster Workflow

A typical cluster deployment consists of the following steps:

1. Install VM2Cloud on each server.
2. Configure networking.
3. Create a cluster on the first node.
4. Join additional nodes to the cluster.
5. Verify cluster health.
6. Configure shared storage or replication (if required).
7. Configure High Availability (optional).

---

## Cluster Features

VM2Cloud clusters support features such as:

- Centralized administration
- Live Migration
- High Availability (HA)
- Replication
- Shared permissions
- Shared storage integration
- Cluster-wide monitoring

---

## Verification

Verify the following after creating or joining a cluster:

- All nodes appear in the cluster.
- Each node reports an online status.
- Cluster communication is healthy.
- Quorum is established.
- Cluster resources are accessible.

---

## Related Documentation

- Create Cluster
- Join Node to Cluster
- Remove Node from Cluster
- Cluster Quorum
- Cluster Certificates
- Cluster File System
- Cluster Troubleshooting

---

## Common Issues

| Issue | Resolution |
|-------|------------|
| Cluster page does not load | Refresh the interface and verify the cluster services are running on the node. |
| A node shows as offline | Verify network connectivity and confirm Corosync is running on the affected node. |
| Cluster is not quorate | Verify that enough nodes are online and communicating. See [Quorum](Quorum.md). |
| Configuration changes are rejected | The cluster file system becomes read-only without quorum. Restore quorum before making changes. |
| A removed node still appears | Complete the cleanup described in [Remove Node from Cluster](Remove-Node-from-Cluster.md). |

---

## Summary

A VM2Cloud cluster combines multiple nodes into a single management environment, enabling centralized administration, simplified resource management, and advanced features such as Live Migration, High Availability, and Replication. Understanding the core cluster components helps administrators deploy and maintain a stable and highly available infrastructure.
