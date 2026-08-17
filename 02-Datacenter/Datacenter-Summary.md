# Datacenter Summary

---

## Overview

The Dashboard displays several widgets that provide a quick overview of the VM2Cloud VE environment. These widgets help administrators monitor infrastructure health, resource utilization, and system status without navigating to individual resources.

The available widgets may vary depending on your VM2Cloud VE configuration and the resources available in your environment.

---

## Resource Summary

The **Resource Summary** widget provides a high-level overview of the virtualization environment.

It may display information such as:

* Number of Nodes
* Number of Virtual Machines
* Number of Containers
* Storage Resources
* Cluster Status

Use this widget to quickly understand the current state of your VM2Cloud VE environment.

---

### Screenshot 1

**Resource Summary Widget**

```text id="dw01"
[ Place Screenshot Here ]
```

> **Capture:** The top of Datacenter → Summary, showing guest counts and cluster health.

---

## CPU Usage

The **CPU Usage** widget displays the current processor utilization across the selected resource.

Depending on the selected object, the widget may show CPU usage for:

* Datacenter
* Node
* Virtual Machine
* Container

Monitoring CPU utilization helps identify overloaded systems and resource bottlenecks.

---

### Screenshot 2

**CPU Usage**

```text id="dw02"
[ Place Screenshot Here ]
```

> **Capture:** The CPU widget with real utilization rather than a flat zero line —
> capture it while a guest is doing some work.

---

## Memory Usage

The **Memory Usage** widget displays the amount of physical memory currently being used.

The widget generally includes:

* Total Memory
* Used Memory
* Available Memory
* Memory Utilization

Regularly monitoring memory usage helps prevent performance issues caused by insufficient RAM.

---

### Screenshot 3

**Memory Usage**

```text id="dw03"
[ Place Screenshot Here ]
```

> **Capture:** The memory widget showing used against total.

---

## Storage Usage

The **Storage Usage** widget provides information about the storage resources available to the selected object.

Typical information includes:

* Total Capacity
* Used Space
* Available Space
* Storage Utilization

This information helps administrators identify storage resources that are approaching capacity.

---

### Screenshot 4

**Storage Usage**

```text id="dw04"
[ Place Screenshot Here ]
```

> **Capture:** The storage widget with at least one storage part-full.

---

## Node Status

The **Node Status** widget displays the operational state of the nodes in the VM2Cloud VE environment.

Administrators can quickly determine whether a node is:

* Online
* Offline
* Available
* Experiencing issues

Monitoring node status helps identify infrastructure problems before they affect workloads.

---

### Screenshot 5

**Node Status**

```text id="dw05"
[ Place Screenshot Here ]
```

> **Capture:** The node list on the summary, with all nodes online.

---

## Subscription Information

The **Subscription Information** widget displays the subscription status for the selected node.

Depending on your deployment, this section may include:

* Subscription Status
* Repository Configuration
* License Information

If your environment does not use subscriptions, this widget may display a notification instead.

---

### Screenshot 6

**Subscription Widget**

```text id="dw06"
[ Place Screenshot Here ]
```

> **Capture:** Whatever this deployment shows here. **Blur any licence key** before
> saving.

---

# Verification

Verify the following:

* Dashboard widgets load successfully.
* Resource information is displayed correctly.
* CPU and Memory usage are updating.
* Storage utilization is visible.
* Node status is displayed correctly.
* Subscription information is available for the selected node.

---

# Common Issues

| Issue                                   | Resolution                                                                   |
| --------------------------------------- | ---------------------------------------------------------------------------- |
| Widget does not load                    | Refresh the Dashboard and verify the selected resource is available.         |
| CPU or Memory values are not updating   | Confirm the node is online and refresh the page.                             |
| Storage information is missing          | Verify that storage has been configured for the selected node or datacenter. |
| Node status is incorrect                | Check the node's connectivity and refresh the Dashboard.                     |
| Subscription information is unavailable | Verify the node's subscription configuration and permissions.                |

---

# Summary

Dashboard widgets provide administrators with a quick overview of the VM2Cloud VE environment, including resource utilization, storage capacity, node health, and subscription status. Regularly reviewing these widgets helps administrators monitor the infrastructure and identify potential issues at an early stage.
