# Quorum

---

## Overview

Quorum is the mechanism used by the VM2Cloud cluster to determine whether enough cluster members are available to safely make cluster-wide decisions.

Each cluster node normally contributes one vote. The cluster needs a majority of the configured votes to be quorate.

Quorum is important for:

* Cluster configuration.
* Distributed configuration management.
* HA operation.
* Resource management.
* Preventing conflicting cluster states.
* Protecting against split-brain conditions.

VM2Cloud uses the underlying Proxmox VE cluster architecture, where cluster communication and quorum are provided through Corosync.

The VM2Cloud cluster filesystem becomes read-only when a node loses quorum. This protects cluster configuration from unsafe changes while the cluster does not have a valid majority.

---

## Split-Brain Protection

Split-brain occurs when two groups of cluster nodes lose communication with each other and both believe they are the active cluster.

Quorum prevents this condition by allowing only the group with sufficient votes to continue making cluster-wide changes. The group without a majority is placed into a read-only state and cannot modify cluster configuration.

Quorum therefore helps:

* Maintain cluster consistency.
* Prevent split-brain situations.
* Protect shared cluster configuration.
* Ensure only a healthy cluster can perform administrative operations.
* Maintain reliable communication between cluster nodes.

---

## When to Use

Use quorum information when:

* Checking cluster health.
* Troubleshooting HA.
* Investigating unavailable cluster operations.
* Investigating a node that cannot modify cluster configuration.
* Troubleshooting a cluster after a node failure.
* Investigating split-brain conditions.
* Checking whether enough nodes are available.
* Configuring or troubleshooting a QDevice.
* Verifying cluster recovery after network problems.

---

## Prerequisites

Before troubleshooting or modifying quorum:

* You must have administrative access to VM2Cloud.
* The cluster should be configured correctly.
* Cluster nodes should have stable network connectivity.
* Corosync should be running.
* Node names and IP addresses should be correctly configured.
* The cluster should have a valid configuration.
* For HA environments, sufficient cluster nodes should be available for recovery.

For production clusters:

* Use a reliable and low-latency cluster network.
* Maintain enough nodes to provide quorum after a node failure.
* Consider a QDevice for supported two-node or even-node cluster designs where additional availability is required.

---

# Procedure

## Step 1: Open the Cluster Status

1. Log in to the VM2Cloud web interface.
2. Select **Datacenter** from the left navigation tree.
3. Open the cluster status or cluster information view available in the installed VM2Cloud version.
4. Review the cluster members.
5. Review the cluster health.
6. Check whether the cluster is quorate.

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

## Step 2: Identify Cluster Nodes

1. Review the list of cluster nodes.
2. Confirm that all expected nodes are present.
3. Check the status of each node.
4. Identify nodes that are offline or unavailable.
5. Compare the number of available nodes with the expected cluster membership.

### Screenshot 2

```text
[ Place Screenshot Here ]
```

Each normal cluster node contributes one vote to the cluster.

---

## Step 3: Check Quorum State

The cluster status should indicate whether quorum is available.

A healthy cluster should show a quorate state.

Review:

* Expected votes.
* Total votes.
* Quorum requirement.
* Current participating nodes.
* QDevice status, if configured.

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

## Step 4: Understand Expected Votes

**Expected votes** represents the number of votes the cluster expects from its configured members and voting devices.

For example, a three-node cluster normally has:

```text
Node 1 = 1 vote
Node 2 = 1 vote
Node 3 = 1 vote

Expected votes = 3
```

The cluster requires a majority to remain quorate.

For three votes:

```text
Required majority = 2
```

Therefore:

```text
3 available → Quorate
2 available → Quorate
1 available → Not quorate
```

The exact vote count can also be affected by configured quorum devices.

---

## Step 5: Check Total Votes

**Total votes** represents the votes currently available to the cluster.

For a healthy three-node cluster:

```text
Expected votes = 3
Total votes    = 3
```

If one node becomes unavailable:

```text
Expected votes = 3
Total votes    = 2
```

The cluster can still remain quorate because two of three votes form a majority.

### Screenshot 4

```text
[ Place Screenshot Here ]
```

---

## Step 6: Check the Quorum Requirement

The quorum requirement is based on the configured voting structure.

For a standard cluster:

```text
Quorum = majority of available voting members
```

Examples:

| Cluster Votes | Quorum Required |
| ------------: | --------------: |
|             1 |               1 |
|             2 |               2 |
|             3 |               2 |
|             4 |               3 |
|             5 |               3 |

A cluster must retain enough votes to satisfy the majority requirement.

---

## Step 7: Check the Effect of a Node Failure

When a node becomes unavailable:

1. Check the cluster status.
2. Identify the missing node.
3. Check the current total votes.
4. Compare total votes with the quorum requirement.
5. Confirm whether the cluster remains quorate.
6. Check HA resource status if HA is enabled.
7. Review recent cluster tasks.
8. Check the cluster network.

### Screenshot 5

```text
[ Place Screenshot Here ]
```

Do not immediately change quorum configuration simply because one node is offline.

First determine whether the remaining nodes already have quorum.

---

## Step 8: Troubleshoot Loss of Quorum

If the cluster is not quorate:

1. Identify the unavailable nodes.
2. Check whether the nodes are actually powered off.
3. Check network connectivity between the remaining nodes.
4. Check Corosync status.
5. Check the cluster configuration.
6. Check whether the remaining nodes can communicate with each other.
7. Determine whether the issue is a node failure or a network partition.
8. Restore the failed node or cluster communication when possible.
9. Recheck quorum.
10. Verify that the cluster returns to a healthy state.

### Screenshot 6

```text
[ Place Screenshot Here ]
```

---

# Configuration / Options

## Expected Votes

Expected votes represents the number of configured voting members the cluster expects.

Do not manually change expected votes simply to make a cluster appear quorate.

Changing quorum-related values without understanding the cluster state can remove important safety protections.

For temporary emergency recovery procedures, use the official VM2Cloud/underlying platform procedure and verify the cluster state before making changes.

---

## Total Votes

Total votes represents the number of votes currently participating.

For example:

```text
Expected votes: 3
Total votes:    2
```

The cluster can still be quorate because two votes are a majority of three.

---

## Quorate

A **quorate** cluster has enough votes to safely perform cluster operations.

A quorate cluster can normally:

* Update cluster configuration.
* Coordinate HA operations.
* Maintain distributed cluster state.
* Perform normal cluster management operations.

---

## Not Quorate

A cluster that is **not quorate** does not have enough votes to safely make cluster-wide decisions.

The cluster filesystem is designed to become read-only when quorum is lost.

This behavior protects against inconsistent cluster configuration.

---

## QDevice

A QDevice is an external voting mechanism that can provide an additional vote to the cluster.

It is particularly useful for supported even-node cluster configurations and is recommended by the underlying platform documentation for two-node clusters that require higher availability.

A QDevice consists of:

* A QDevice daemon on cluster nodes.
* An external vote daemon on an independent system.

The external system participates in the quorum decision without being a normal cluster node.

---

# QDevice Considerations

## Two-Node Cluster

A two-node cluster has an inherent quorum limitation.

Normally:

```text
Node 1 = 1 vote
Node 2 = 1 vote

Total = 2
Majority = 2
```

If one node fails:

```text
Available votes = 1
Required votes   = 2
```

The remaining node therefore does not have quorum.

A QDevice can provide an additional voting mechanism for supported configurations.

The underlying documentation recommends QDevice for two-node clusters when higher availability is required.

---

## Even-Node Clusters

The underlying VM2Cloud platform documentation supports QDevices for clusters with an even number of nodes.

For an even-node cluster, the QDevice can provide an additional vote and improve availability without reducing the safety properties of quorum.

---

## Odd-Node Clusters

The underlying documentation discourages using QDevices with odd-node cluster sizes because the voting behavior provides little benefit and introduces additional considerations.

Use the official QDevice documentation before deploying one in an odd-node cluster.

---

# Verification

After checking or recovering quorum:

1. Open the cluster status.
2. Confirm that all expected nodes are visible.
3. Confirm the current total votes.
4. Confirm that the cluster is quorate.
5. Check node status.
6. Check HA resource status.
7. Check recent tasks.
8. Verify that cluster configuration is accessible.
9. Confirm that no network or Corosync errors remain.

### Screenshot 7

```text
[ Place Screenshot Here ]
```

---

## Verify Through CLI

CLI is secondary and should be used for verification or troubleshooting when the UI does not provide enough information.

Run:

```bash
pvecm status
```

Review the output.

Typical information includes:

```text
Cluster information
-------------------
Name:
Config Version:
Transport:
Secure auth:

Quorum information
------------------
Date:
Quorum provider:
Nodes:
Expected votes:
Highest expected:
Total votes:
Quorum:
Flags:
```

The exact output depends on the VM2Cloud version and cluster configuration.

---

## Verify Expected Votes

Run:

```bash
pvecm status
```

Check:

```text
Expected votes:
```

Compare this value with the number of configured cluster voting members and any configured QDevice.

---

## Verify Total Votes

Run:

```bash
pvecm status
```

Check:

```text
Total votes:
```

Compare the total available votes with the quorum requirement.

---

## Verify Quorate State

Run:

```bash
pvecm status
```

Check the quorum information and flags.

A healthy cluster should indicate that quorum is present.

---

# Common Issues

## Cluster Is Not Quorate

### Possible Causes

* Multiple nodes are offline.
* Cluster network failure.
* Corosync failure.
* Switch failure.
* VLAN configuration problem.
* Node network interface failure.
* Incorrect cluster configuration.
* Network partition.

### Resolution

1. Check all node statuses.
2. Check cluster network connectivity.
3. Check Corosync.
4. Check switch connectivity.
5. Check VLAN configuration.
6. Check node network interfaces.
7. Run `pvecm status` if CLI verification is required.
8. Identify whether the problem is a node failure or network partition.
9. Restore the failed node or network connectivity.
10. Verify quorum again.

---

## One Node Is Offline but Cluster Is Quorate

This can be normal in a cluster with enough remaining votes.

For example:

```text
3-node cluster

Node 1 = Online
Node 2 = Online
Node 3 = Offline

Available votes = 2
Required votes   = 2
```

The cluster can remain quorate.

### Recommended Action

1. Verify the offline node.
2. Determine why it is unavailable.
3. Check node hardware and networking.
4. Check Corosync.
5. Restore the node when appropriate.
6. Verify that the node rejoins correctly.

Do not modify quorum settings simply to compensate for a normal single-node failure.

---

## Two-Node Cluster Loses One Node

### Cause

A standard two-node cluster normally requires both votes for quorum.

### Resolution

1. Confirm that the second node is actually unavailable.
2. Check cluster network connectivity.
3. Determine whether the node is powered off or only unreachable.
4. If a QDevice is configured, verify its state.
5. Restore the failed node or supported quorum mechanism.
6. Verify quorum.
7. Check HA resource state.

Do not manually force quorum without first determining whether the other node is still running.

---

## Cluster Shows Incorrect Expected Votes

### Possible Causes

* Node membership changed.
* Cluster configuration is incomplete.
* A node was removed incorrectly.
* QDevice configuration changed.
* Corosync configuration is inconsistent.

### Resolution

1. Check cluster membership.
2. Check Corosync configuration.
3. Check whether all nodes have the expected configuration.
4. Check QDevice configuration if present.
5. Review recent cluster changes.
6. Correct the cluster configuration using the official procedure.
7. Recheck quorum.

---

## Cluster Configuration Becomes Read-Only

### Cause

The cluster may have lost quorum.

The cluster filesystem intentionally becomes read-only when quorum is lost.

### Resolution

1. Check cluster quorum.
2. Check node availability.
3. Check Corosync.
4. Check cluster network connectivity.
5. Restore sufficient cluster votes.
6. Verify that quorum returns.
7. Confirm that cluster configuration is writable again.

---

## HA Resources Are Affected by Loss of Quorum

### Possible Causes

* Cluster does not have quorum.
* Node communication failure.
* HA cannot safely coordinate cluster-wide operations.

### Resolution

1. Check quorum.
2. Check Corosync.
3. Check HA resources.
4. Check node availability.
5. Restore cluster quorum.
6. Review HA task history.
7. Confirm resource state after quorum is restored.

---

## Quorum Is Lost Because of Network Failure

### Possible Causes

* Switch failure.
* VLAN failure.
* Incorrect MTU.
* Bond failure.
* NIC failure.
* Packet loss.
* High latency.
* Incorrect routing.

### Resolution

1. Check cluster network interfaces.
2. Check node-to-node connectivity.
3. Check switch ports.
4. Check VLAN configuration.
5. Check MTU.
6. Check bond configuration.
7. Check Corosync.
8. Restore stable connectivity.
9. Verify quorum.

The underlying documentation recommends a reliable, low-latency cluster network and recommends dedicated networking for Corosync where practical.

---

# Safety Considerations

> **Warning:** Do not manually manipulate quorum values simply to make a cluster appear healthy.

Incorrect quorum manipulation can remove safety protections and may contribute to split-brain conditions.

Before making any quorum-related change:

1. Determine the actual cluster state.
2. Determine which nodes are running.
3. Determine which nodes are reachable.
4. Check Corosync connectivity.
5. Check cluster membership.
6. Determine whether a network partition exists.
7. Confirm whether the missing node is powered off or still running.
8. Follow the official recovery procedure.
9. Verify quorum after the change.

---

# Best Practices

* Prefer odd-numbered cluster sizes for straightforward quorum management.
* Use at least three nodes for production clusters where practical.
* Consider a QDevice for supported two-node designs that require higher availability.
* Maintain reliable cluster networking.
* Keep Corosync traffic stable and low latency.
* Consider dedicated physical networking for Corosync.
* Monitor quorum regularly.
* Monitor node availability.
* Investigate unexpected quorum loss immediately.
* Do not change expected votes as a routine troubleshooting shortcut.
* Do not manually force quorum without understanding the cluster state.
* Maintain reliable power and network infrastructure.
* Document the cluster's voting design.
* Test node-failure scenarios in a controlled environment.
* Keep cluster configuration consistent across all nodes.

---

# Related Documentation

Cluster:

- [Cluster Overview](Cluster-Overview.md)
- [Create Cluster](Create-Cluster.md)
- [Join Node to Cluster](Join-Node-to-Cluster.md)
- [Remove Node from Cluster](Remove-Node-from-Cluster.md)
- [Cluster File System](Cluster-File-System.md)
- [Cluster Troubleshooting](Cluster-Troubleshooting.md)

High Availability:

- [HA Overview](../HA/HA-Overview.md)
- [Fencing](../HA/Fencing.md)
- [HA Troubleshooting](../HA/HA-Troubleshooting.md)

Other troubleshooting:

- [Network Troubleshooting](../../03-Nodes/System/Network/Network-Troubleshooting.md)
- [Node Troubleshooting](../../03-Nodes/Node-Troubleshooting.md)

---

# Summary

Quorum allows VM2Cloud to determine whether enough cluster members are available to safely perform cluster-wide operations.

The most important quorum concepts are:

* **Expected votes** — the number of votes the cluster expects.
* **Total votes** — the votes currently available.
* **Quorum** — the minimum voting majority required.
* **Quorate** — the cluster has enough votes.
* **Not quorate** — the cluster does not have enough votes.
* **QDevice** — an external voting mechanism that can improve availability for supported cluster designs.

A healthy quorum depends on:

* Reliable cluster nodes.
* Stable Corosync communication.
* Reliable network infrastructure.
* Correct cluster configuration.
* Sufficient voting members.

When quorum is lost, first identify the underlying node or network problem. Restore a valid quorum rather than bypassing the safety mechanism without understanding the consequences.
