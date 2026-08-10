# High Availability Overview

---

## Overview

High Availability (HA) allows VM2Cloud to automatically recover selected virtual machines and containers when the node running them becomes unavailable.

HA is managed at the cluster level and uses the VM2Cloud cluster infrastructure, including Corosync, quorum, fencing, and the HA manager.

When an HA-managed guest is running on a node and that node fails, the HA system can start the guest on another available cluster node, provided that the required resources are available.

HA does not replicate guest disks by itself. For automatic recovery, guest storage must be available on the node where the guest is restarted, typically through shared storage or another supported storage configuration.

VM2Cloud uses the underlying Proxmox VE HA architecture for cluster-wide guest recovery.

---

## When to Use

Use High Availability when:

- Virtual machines or containers must recover automatically after a node failure.
- Critical services need reduced downtime.
- Multiple cluster nodes are available for guest recovery.
- Guest storage is accessible from the nodes that may run the HA resource.
- Administrators need controlled placement of HA resources.
- Automatic recovery is required instead of manually restarting guests after a node failure.

Do not enable HA simply because a cluster exists. HA introduces additional cluster requirements and should be used for workloads that require automatic recovery.

---

## Prerequisites

Before configuring HA, verify the following:

- VM2Cloud is configured as a cluster.
- The cluster has multiple online nodes.
- Cluster communication is working correctly.
- The cluster has quorum.
- Corosync communication between nodes is healthy.
- Required guest storage is available to the nodes that may run the guest.
- The administrator has sufficient permissions to manage HA.
- The guest is suitable for HA recovery.
- Required network configuration is available on the potential target nodes.
- Any required hardware passed through to the guest is available on the target node.

For production environments, verify that the cluster has sufficient capacity to run workloads from a failed node.

---

# Procedure

## Step 1: Open High Availability

1. Log in to the VM2Cloud web interface.
2. Select the required cluster from the left navigation tree.
3. Select **Datacenter**.
4. Open **HA** from the Datacenter menu.
5. Review the HA management interface.

### Screenshot 1

```text
[ Place Screenshot Here ]
