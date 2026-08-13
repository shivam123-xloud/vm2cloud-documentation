# Fencing

---

## Overview

Fencing is a High Availability (HA) safety mechanism used by VM2Cloud VE to protect cluster resources when a node is no longer reliably reachable.

When a cluster node loses communication with the HA system, VM2Cloud VE must determine whether that node is still running. If the node may still be active, starting its HA resources on another node could result in the same resource running in two places at the same time.

This condition is known as a split-brain situation.

Fencing prevents this by ensuring that a failed or unreachable node is no longer able to run its HA resources before those resources are recovered elsewhere.

VM2Cloud VE provides this protection through watchdog-based fencing. The HA stack combines cluster-wide locking with watchdog functionality so that HA-managed guests are recovered correctly from a fenced node.

---

## When to Use

Fencing is relevant when:

- Configuring High Availability.
- Troubleshooting HA recovery.
- Investigating unexpected node fencing.
- Investigating HA resources that fail to recover.
- Troubleshooting cluster communication failures.
- Troubleshooting quorum-related HA behavior.
- Designing reliable HA infrastructure.
- Verifying watchdog functionality.

Administrators normally do not manually fence a node as part of routine VM or container management. Fencing is primarily an HA safety mechanism.

---

## Prerequisites

Before relying on HA fencing:

- VM2Cloud VE must be configured as a cluster.
- HA must be configured where required.
- Cluster communication must be healthy.
- Corosync must be functioning.
- Cluster quorum must be available.
- The node must have a working watchdog mechanism.
- HA-managed resources should be configured correctly.
- Nodes should have reliable power and network infrastructure.
- The cluster should have enough nodes for the required quorum and recovery design.

For production HA deployments:

- Use reliable cluster networking.
- Avoid unstable network paths for cluster communication.
- Avoid asymmetric network connectivity.
- Verify watchdog functionality before relying on automatic recovery.
- Maintain adequate cluster redundancy.

---

# Procedure

## Step 1: Understand the Fencing Process

Fencing is normally performed automatically by the HA system.

A simplified failure sequence is:

```text
Node loses reliable cluster communication
                ↓
Cluster membership/quorum state changes
                ↓
HA determines that the node is no longer reliable
                ↓
Fencing mechanism is triggered
                ↓
Failed node is prevented from running HA resources
                ↓
HA can recover resources on another eligible node
```

The cluster does not need administrator interaction for this sequence. Fencing is triggered by the HA stack itself.

---

## Step 2: Understand the Watchdog Mechanism

VM2Cloud VE uses a watchdog to fence an unreachable node.

A watchdog is a timer that must be reset regularly by the HA services on the node.

1. While the node is healthy, the HA services reset the watchdog timer.
2. If the node loses quorum or the HA services stop functioning, the timer is no longer reset.
3. When the timer expires, the watchdog resets the node.
4. The node is then guaranteed not to be running its HA resources.
5. HA can safely recover those resources elsewhere.

This design means fencing works even when the node can no longer communicate with the rest of the cluster.

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

## Step 3: Verify Fencing Readiness

Fencing depends on a healthy cluster, so verify the following before relying on automatic recovery:

1. Confirm the cluster reports quorum.
2. Confirm all nodes are communicating.
3. Confirm the HA services are running on each node.
4. Confirm the watchdog device is available on each node.
5. Review the cluster log for previous fencing events.

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

## Step 4: Recovery After Fencing

After a node has been fenced:

1. The node restarts.
2. It rejoins the cluster if the underlying problem is resolved.
3. HA resources that were recovered elsewhere remain on their new node.
4. Resources are not automatically moved back unless a placement rule requires it.

Investigate the original cause before returning the node to production.

---

# Verification

Verify the following:

- The cluster reports quorum.
- All expected nodes are online and communicating.
- HA services are running on every node.
- HA resources recover on an eligible node after a node failure.
- The cluster log records the fencing event.
- The fenced node rejoins the cluster after restarting.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| A node is fenced unexpectedly | Investigate cluster network stability and confirm the node is not losing quorum intermittently. |
| Nodes are fenced during network maintenance | Plan maintenance windows and consider stopping HA services before disruptive network work. |
| HA resources do not recover after fencing | Verify the remaining cluster retained quorum and that an eligible node can reach the guest's storage. |
| Repeated fencing on the same node | Check the cluster network path, NIC health, and system load on the affected node. |
| Fencing does not occur | Verify that the watchdog is available and that the HA services are running. |

---

# Related Documentation

- [HA Overview](HA-Overview.md)
- [HA Resources](HA-Resources.md)
- [Quorum](../Cluster/Quorum.md)
- [HA Troubleshooting](HA-Troubleshooting.md)
- [Cluster Troubleshooting](../Cluster/Cluster-Troubleshooting.md)

---

# Summary

Fencing is the safety mechanism that allows VM2Cloud VE High Availability to recover resources without risking a split-brain condition. By using a watchdog to guarantee that an unreachable node is no longer running its HA resources, the cluster can safely start those resources on another node. Reliable cluster networking, healthy quorum, and a working watchdog are all required for fencing to behave correctly.
