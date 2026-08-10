# HA Resources

---

## Overview

HA resources are virtual machines and containers that are managed by the VM2Cloud High Availability subsystem.

When a virtual machine or container is added as an HA resource, VM2Cloud's HA manager monitors and manages the requested state of that resource.

HA can:

- Keep a resource running when its requested state is started.
- Restart a resource after a failure.
- Relocate a resource to another suitable cluster node when required.
- Apply HA placement rules.
- Manage the resource during node failures and recovery.

HA resources are managed from:

**Datacenter → HA → Resources**

VM2Cloud uses the underlying Proxmox VE HA architecture for resource management.

> **Important:** Adding a VM or container to HA does not replicate its disks. The required storage, network configuration, and other resources must be available on a node where the guest may be started.

---

## When to Use

Use HA resources when:

- A virtual machine requires automatic recovery after a node failure.
- A container requires automatic recovery after a node failure.
- A critical workload must be managed by the HA subsystem.
- The workload must remain in a defined HA state.
- The workload requires controlled placement across cluster nodes.
- Automatic restart or relocation is required.

Do not add every VM or container to HA without evaluating the workload and cluster capacity.

---

## Prerequisites

Before adding a VM or container as an HA resource, verify:

- VM2Cloud is configured as a cluster.
- The cluster has quorum.
- Required cluster nodes are online.
- Cluster communication is working correctly.
- The VM or container already exists.
- Required storage is available to possible target nodes.
- Required network configuration is available on possible target nodes.
- The administrator has permission to manage HA.
- The cluster has sufficient CPU and memory capacity for recovery.
- Required hardware is available on possible target nodes.
- Any required PCI passthrough or host-specific configuration is compatible with HA placement.

---

# Procedure

## Step 1: Open the HA Resources Page

1. Log in to the VM2Cloud web interface.
2. Select **Datacenter** from the left navigation tree.
3. Select **HA**.
4. Open the **Resources** section.
5. Review the resources currently managed by HA.

### Screenshot 1

```text
[ Place Screenshot Here ]
