# Node Affinity

---

## Overview

Node affinity controls where High Availability (HA) resources should run within the VM2Cloud cluster.

Node-affinity rules allow administrators to define preferred or restricted nodes for HA-managed virtual machines and containers.

Node affinity is the current placement mechanism for HA resources in recent VM2Cloud versions based on the underlying Proxmox VE HA implementation.

Node affinity controls resource placement only. It does not provide:

- Storage replication.
- VM or container backup.
- Network failover by itself.
- Guest operating system recovery.
- Hardware replication.

The HA resource must still have access to all required storage, networking, CPU, memory, and hardware resources on the target node.

---

## When to Use

Use node affinity when:

- An HA resource should prefer a specific node.
- An HA resource should prefer a defined order of nodes.
- A workload should run only on selected nodes.
- Different workloads should be distributed across cluster nodes.
- A workload has node-specific requirements.
- You need controlled HA recovery placement.
- You are replacing an older HA Group placement configuration.

---

## Prerequisites

Before configuring node affinity:

- VM2Cloud must be configured as a cluster.
- HA must be available on the cluster.
- The HA resource must already exist.
- The cluster should have quorum.
- Required nodes must be online.
- Cluster communication must be healthy.
- Target nodes must have sufficient CPU and memory.
- Required storage must be available on possible target nodes.
- Required network configuration must exist on possible target nodes.
- Required hardware must be available on possible target nodes.
- The administrator must have sufficient permissions to modify HA configuration.

---

# Procedure

## Step 1: Open the HA Configuration

1. Log in to the VM2Cloud web interface.
2. Select **Datacenter** from the left navigation tree.
3. Select **HA**.
4. Open the HA placement configuration available in the installed VM2Cloud version.
5. Review the existing node-affinity rules.

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

## Step 2: Create a Node-Affinity Rule

1. Click **Add** in the placement configuration.
2. Enter a name for the rule.
3. Select the HA resources the rule applies to.
4. Select the nodes the resources should use.
5. Assign a priority to each node if the version supports it.

Higher-priority nodes are preferred. Lower-priority nodes act as fallback targets.

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

## Step 3: Choose the Enforcement Behaviour

Node-affinity rules can be preferred or strict.

| Behaviour | Effect |
|-----------|--------|
| Preferred | HA tries the listed nodes first but may use another node if none are available. |
| Strict | HA uses only the listed nodes. If none are available, the resource is not started elsewhere. |

Use strict placement only when the workload genuinely cannot run on other nodes, for example when it depends on node-specific hardware.

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

## Step 4: Save and Verify the Rule

1. Review the configuration.
2. Save the rule.
3. Confirm the rule appears in the placement list.
4. Confirm the affected resources show the expected node assignment.

### Screenshot 4

```text
[ Place Screenshot Here ]
```

---

# Verification

Verify the following:

- The rule appears in the HA placement configuration.
- The correct resources are associated with the rule.
- The listed nodes are correct.
- HA resources run on the expected nodes.
- Recovery selects a node permitted by the rule.
- The cluster reports quorum.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Resource does not run on the preferred node | Verify the node is online and has sufficient CPU, memory, storage, and network resources. |
| Resource does not start at all | A strict rule may list only unavailable nodes. Verify node availability or relax the rule. |
| Rule appears to be ignored | Confirm the resource is managed by HA and that the rule references the correct resource. |
| Resource does not move back after a node returns | Node affinity does not automatically relocate a running resource; migrate it manually if required. |
| Conflicting placement behaviour | Review resource-affinity rules, which may compete with node affinity. |

---

# Related Documentation

- [HA Overview](HA-Overview.md)
- [HA Resources](HA-Resources.md)
- [Resource Affinity](Resource-Affinity.md)
- [Fencing](Fencing.md)
- [HA Troubleshooting](HA-Troubleshooting.md)

---

# Summary

Node affinity controls which cluster nodes an HA resource should prefer or be restricted to. Preferred rules give HA a placement order while still allowing recovery elsewhere; strict rules confine a resource to a defined set of nodes. Node affinity governs placement only — the target node must still provide the storage, networking, and hardware the guest requires.
