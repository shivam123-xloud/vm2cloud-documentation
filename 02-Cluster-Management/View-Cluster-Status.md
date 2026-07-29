# View Cluster Status

---

## Overview

The **Cluster Status** page provides information about the overall health and state of your VM2Cloud cluster. It allows administrators to verify node availability, monitor cluster membership, and quickly identify communication or synchronization issues.

---

## When to Use

View the cluster status when you need to:

* Verify that all cluster nodes are online.
* Check the overall health of the cluster.
* Confirm that a newly added node has joined successfully.
* Monitor the current status of cluster members.
* Troubleshoot cluster-related issues.

---

## Prerequisites

Before viewing the cluster status, ensure that:

* You have administrator privileges.
* The VM2Cloud cluster has already been created.
* At least one node is available.
* The VM2Cloud web interface is accessible.

---

# Procedure

## Step 1: Log in to VM2Cloud

1. Open the VM2Cloud web interface.
2. Sign in using an administrator account.

---

### Screenshot 1

**VM2Cloud Dashboard**

```text
[ Place Screenshot Here ]
```

---

## Step 2: Open Cluster Management

1. In the navigation pane, select **Datacenter**.
2. Click **Cluster**.

---

### Screenshot 2

**Cluster Page**

```text
[ Place Screenshot Here ]
```

---

## Step 3: Review Cluster Information

Review the information displayed on the Cluster page, including:

* Cluster Name
* Cluster Nodes
* Node Status
* Node ID
* Online Status
* Cluster Communication Information

---

### Screenshot 3

**Cluster Information**

```text
[ Place Screenshot Here ]
```

---

## Step 4: Verify Node Status

Confirm that:

* All expected nodes are listed.
* Every node shows an **Online** status.
* No warning or error indicators are displayed.
* Cluster communication appears healthy.

---

### Screenshot 4

**Node Status**

```text
[ Place Screenshot Here ]
```

---

## Step 5: Review Recent Tasks (Optional)

1. Open **Recent Tasks**.
2. Verify that there are no failed cluster-related operations.

---

### Screenshot 5

**Recent Tasks**

```text
[ Place Screenshot Here ]
```

---

# Verification

Verify the following:

* The correct cluster name is displayed.
* All cluster nodes are visible.
* All nodes show an **Online** status.
* No communication errors are present.
* Recent cluster tasks completed successfully.

---

# Common Issues

| Issue                            | Resolution                                                          |
| -------------------------------- | ------------------------------------------------------------------- |
| Node is Offline                  | Verify that the node is powered on and connected to the network.    |
| Missing Node                     | Confirm the node successfully joined the cluster.                   |
| Communication Error              | Check network connectivity between cluster nodes.                   |
| Cluster information not updating | Refresh the web interface or verify cluster services are running.   |
| Failed cluster task              | Review the task log and resolve the reported error before retrying. |

---

# Summary

The Cluster Status page provides a quick overview of the health of your VM2Cloud cluster. Regularly monitoring this page helps ensure that all nodes remain connected, operational, and ready to host workloads. It is also the first place to check after adding or removing nodes from the cluster.
