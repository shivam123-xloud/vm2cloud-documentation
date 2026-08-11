# Node Affinity

---

## Overview

Node affinity controls where High Availability (HA) resources should run within the VM2Cloud cluster.

Node-affinity rules allow administrators to define preferred or restricted nodes for HA-managed virtual machines and containers.

Node affinity is the current placement mechanism for HA resources in recent VM2Cloud versions based on the underlying Proxmox VE HA implementation.

Node affinity controls resource placement only. It does not provide:

- Storage replication.
- VM or container backup.
- Network failover by itself.
- Guest operating system recovery.
- Hardware replication.

The HA resource must still have access to all required storage, networking, CPU, memory, and hardware resources on the target node.

---

## When to Use

Use node affinity when:

- An HA resource should prefer a specific node.
- An HA resource should prefer a defined order of nodes.
- A workload should run only on selected nodes.
- Different workloads should be distributed across cluster nodes.
- A workload has node-specific requirements.
- You need controlled HA recovery placement.
- You are replacing an older HA Group placement configuration.

---

## Prerequisites

Before configuring node affinity:

- VM2Cloud must be configured as a cluster.
- HA must be available on the cluster.
- The HA resource must already exist.
- The cluster should have quorum.
- Required nodes must be online.
- Cluster communication must be healthy.
- Target nodes must have sufficient CPU and memory.
- Required storage must be available on possible target nodes.
- Required network configuration must exist on possible target nodes.
- Required hardware must be available on possible target nodes.
- The administrator must have sufficient permissions to modify HA configuration.

---

# Procedure

## Step 1: Open the HA Configuration

1. Log in to the VM2Cloud web interface.
2. Select **Datacenter** from the left navigation tree.
3. Select **HA**.
4. Open the HA placement configuration available in the installed VM2Cloud version.
5. Review the existing node-affinity rules.

### Screenshot 1

```text
[ Place Screenshot Here ]
