# Fencing

---

## Overview

Fencing is a High Availability (HA) safety mechanism used by VM2Cloud to protect cluster resources when a node is no longer reliably reachable.

When a cluster node loses communication with the HA system, VM2Cloud must determine whether that node is still running. If the node may still be active, starting its HA resources on another node could result in the same resource running in two places at the same time.

This condition is known as a split-brain situation.

Fencing prevents this by ensuring that a failed or unreachable node is no longer able to run its HA resources before those resources are recovered elsewhere.

VM2Cloud uses the underlying Proxmox VE HA architecture, including watchdog-based fencing, to provide this protection. The HA stack uses cluster-wide locking and watchdog functionality to ensure correct recovery of HA-managed guests from fenced nodes.

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

- VM2Cloud must be configured as a cluster.
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
