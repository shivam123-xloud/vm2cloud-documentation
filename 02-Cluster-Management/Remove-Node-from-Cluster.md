# Remove Node from Cluster

---

## Overview

A node can be removed from a VM2Cloud cluster when it is no longer required, is being replaced, or needs to be rebuilt. Before removing a node, ensure that all virtual machines and containers have been migrated or backed up to prevent service disruption.

---

## When to Use

Remove a node from the cluster when you need to:

* Decommission a physical server.
* Replace faulty hardware.
* Reinstall the operating system.
* Reduce the size of the cluster.
* Remove an offline or failed node.

---

## Prerequisites

Before removing a node, ensure that:

* You have administrator privileges.
* The node is a member of the cluster.
* All virtual machines have been migrated or shut down.
* All containers have been migrated or stopped.
* No backup or migration tasks are running.
* The cluster is healthy.

---

# Procedure

## Step 1: Verify the Node

1. Log in to the VM2Cloud web interface.
2. Select **Datacenter**.
3. Open **Cluster**.
4. Verify the node that will be removed.

---

### Screenshot 1

**Cluster Nodes**

```text
[ Place Screenshot Here ]
```

---

## Step 2: Migrate Workloads

1. Move all virtual machines to another node.
2. Move all containers to another node.
3. Verify that no workloads remain on the node.

---

### Screenshot 2

**Node Workloads**

```text
[ Place Screenshot Here ]
```

---

## Step 3: Place the Node in Maintenance (If Applicable)

1. Ensure no users are connected to the node.
2. Stop any remaining services if required.
3. Confirm that the node is ready for removal.

---

### Screenshot 3

**Prepare Node**

```text
[ Place Screenshot Here ]
```

---

## Step 4: Remove the Node from the Cluster

> **Note:** Removing a node from a cluster cannot be completed from the VM2Cloud web interface. This operation must be performed from the command line on a cluster node.

Run the appropriate cluster removal command.

---

### Screenshot 4

**CLI Node Removal**

```text
[ Place Screenshot Here ]
```

---

## Step 5: Verify the Cluster

1. Refresh the VM2Cloud web interface.
2. Open **Datacenter → Cluster**.
3. Verify that the removed node no longer appears in the cluster.

---

### Screenshot 5

**Updated Cluster**

```text
[ Place Screenshot Here ]
```

---

# Verification

Verify the following:

* The node is no longer listed under **Datacenter → Cluster**.
* Remaining nodes are online.
* Cluster health is normal.
* Virtual machines and containers continue running on the remaining nodes.

---

# Common Issues

| Issue                                | Resolution                                                                                |
| ------------------------------------ | ----------------------------------------------------------------------------------------- |
| Node cannot be removed               | Verify that no virtual machines or containers remain on the node.                         |
| Cluster reports communication errors | Confirm that the node has been removed correctly and cluster services are healthy.        |
| Node still appears in the interface  | Refresh the web interface or verify the removal completed successfully.                   |
| Removal command fails                | Review the command output and ensure the node is not participating in cluster operations. |

---

# Summary

The node has been successfully removed from the VM2Cloud cluster. The remaining nodes continue operating as part of the cluster. If required, the removed server can be reinstalled and joined to the cluster again as a new node.
