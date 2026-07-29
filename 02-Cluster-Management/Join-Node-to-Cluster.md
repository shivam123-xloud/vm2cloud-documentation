# Join Node to Cluster

---

## Overview

After creating a VM2Cloud cluster, additional nodes can be added to expand the environment. Once a node joins the cluster, it becomes part of the same management domain and can be managed from the VM2Cloud interface.

---

## When to Use

Join a node to an existing cluster when you need to:

* Expand your VM2Cloud infrastructure.
* Increase available CPU, memory, or storage resources.
* Prepare for High Availability.
* Distribute workloads across multiple physical servers.

---

## Prerequisites

Before joining a node to the cluster, ensure that:

* VM2Cloud is installed on the new node.
* The node is not already a member of another cluster.
* All nodes are running the same VM2Cloud version.
* Hostnames are correctly configured.
* DNS resolution or hostname resolution is working.
* Network connectivity exists between all cluster nodes.
* Date and time are synchronized on every node.
* You have administrator privileges on both the existing cluster and the new node.

---

# Procedure

## Step 1: Open Cluster Management

1. Log in to the VM2Cloud web interface of the existing cluster.
2. Select **Datacenter**.
3. Click **Cluster**.

---


**Cluster Management Page**

![Cluster Management Page](images/cluster-overview.png)


---

## Step 2: Prepare the Join Information

1. On the Cluster page, click **Join Information**.
2. Copy the following details:

   * Cluster Join Information
   * Fingerprint
   * Cluster IP Address (if required)

These details will be required when joining the new node.

---

### Screenshot 2

**Cluster Join Information**

```text
[ Place Screenshot Here ]
```

---

## Step 3: Log in to the New Node

1. Open the VM2Cloud web interface of the new server.
2. Log in using an administrator account.
3. Select **Datacenter**.
4. Open **Cluster**.

---

### Screenshot 3

**New Node Cluster Page**

```text
[ Place Screenshot Here ]
```

---

## Step 4: Start the Join Process

1. Click **Join Cluster**.
2. The **Join Cluster** window opens.

---

### Screenshot 4

**Join Cluster Window**

```text
[ Place Screenshot Here ]
```

---

## Step 5: Enter Cluster Information

1. Paste the copied Join Information.
2. Verify that the cluster details are displayed correctly.
3. Enter the administrator password for the existing cluster.
4. Review the displayed fingerprint.
5. Click **Join Cluster**.

---

### Screenshot 5

**Cluster Join Configuration**

```text
[ Place Screenshot Here ]
```

---

## Step 6: Wait for the Process to Complete

1. Wait for the join operation to finish.
2. The node may restart cluster services automatically.
3. Allow the task to complete before closing the window.

---

### Screenshot 6

**Joining Cluster**

```text
[ Place Screenshot Here ]
```

---

## Step 7: Verify the Node

1. Return to the VM2Cloud web interface.
2. Open **Datacenter → Cluster**.
3. Verify that the newly added node appears in the cluster.

---

### Screenshot 7

**Node Successfully Joined**

```text
[ Place Screenshot Here ]
```

---

# Verification

Confirm the following:

* The new node appears in the Cluster view.
* The node status is **Online**.
* No warning or error icons are displayed.
* Resources of the new node are visible.
* Recent Tasks shows the join operation completed successfully.

---

# Common Issues

| Issue                                   | Resolution                                                                 |
| --------------------------------------- | -------------------------------------------------------------------------- |
| Unable to connect to the cluster        | Verify network connectivity between nodes.                                 |
| Authentication failed                   | Confirm the administrator password is correct.                             |
| Fingerprint mismatch                    | Verify that the Join Information was copied from the correct cluster.      |
| Node already belongs to another cluster | Remove the existing cluster configuration before attempting to join again. |
| Version mismatch                        | Ensure all nodes are running the same VM2Cloud version.                    |

---

# Summary

The node has now been successfully added to the VM2Cloud cluster. Once the join process is complete, the new node becomes part of the shared environment and can be managed from the same VM2Cloud interface. You can now deploy workloads on the new node or continue expanding the cluster by adding additional nodes.
