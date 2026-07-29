# Delete Cluster

---

## Overview

Deleting a cluster permanently removes the cluster configuration from the node. This operation is typically performed when decommissioning a cluster, rebuilding the environment, or converting a clustered node back to a standalone server.

> **Important:** Cluster deletion cannot be performed from the VM2Cloud web interface. This operation must be completed using the command line.

---

## When to Use

Delete a cluster when you need to:

* Decommission an existing cluster.
* Rebuild the cluster from scratch.
* Convert a clustered node into a standalone server.
* Remove an unused test or lab cluster.

---

## Prerequisites

Before deleting the cluster, ensure that:

* You have administrator privileges.
* All virtual machines and containers have been migrated or backed up.
* All additional nodes have been removed from the cluster.
* The node you are working on is the last remaining node.
* A recent backup of important data is available.

---

# Procedure

## Step 1: Verify the Cluster

1. Log in to the VM2Cloud web interface.
2. Navigate to **Datacenter → Cluster**.
3. Verify that all additional nodes have already been removed.

---

### Screenshot 1

**Cluster Overview**

```text
[ Place Screenshot Here ]
```

---

## Step 2: Connect to the Server

1. Open an SSH session or access the local console.
2. Log in using an administrator account.

---

### Screenshot 2

**SSH Session**

```text
[ Place Screenshot Here ]
```

---

## Step 3: Stop Cluster Services

1. Stop the cluster services on the node.

```bash
systemctl stop pve-cluster
systemctl stop corosync
```

---

### Screenshot 3

**Stop Cluster Services**

```text
[ Place Screenshot Here ]
```

---

## Step 4: Remove Cluster Configuration

1. Remove the cluster configuration files.
2. Start the cluster service again.

> **Note:** Use the appropriate commands for your VM2Cloud version and environment.

---

### Screenshot 4

**Remove Cluster Configuration**

```text
[ Place Screenshot Here ]
```

---

## Step 5: Verify Standalone Mode

1. Refresh the VM2Cloud web interface.
2. Confirm that the node is operating as a standalone server.
3. Verify that the **Cluster** page no longer displays cluster membership information.

---

### Screenshot 5

**Standalone Node**

```text
[ Place Screenshot Here ]
```

---

# Verification

Verify the following:

* The cluster configuration has been removed.
* The node operates as a standalone server.
* Cluster-related services are no longer active.
* No cluster information is displayed in the VM2Cloud interface.

---

# Common Issues

| Issue                               | Resolution                                                                      |
| ----------------------------------- | ------------------------------------------------------------------------------- |
| Unable to stop cluster services     | Verify you have administrator privileges and no cluster operations are running. |
| Cluster information still appears   | Refresh the interface and confirm the configuration was removed successfully.   |
| Node fails to start cluster service | Verify that all cluster configuration files were removed correctly.             |
| Command execution fails             | Review the command output and correct any reported errors before continuing.    |

---

# Summary

The cluster has been successfully removed from the node, and the server now operates as a standalone VM2Cloud host. If required, the node can later be used to create a new cluster or join an existing one.
