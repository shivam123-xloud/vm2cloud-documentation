# Resource Affinity

---

## Overview

Resource Affinity controls the placement relationship between HA-managed resources in VM2Cloud.

It is used when administrators need to define how multiple HA resources should be placed relative to each other.

Resource-affinity rules are useful when workloads have a dependency or placement relationship, for example when related services should remain together or when workloads should be distributed across different nodes.

Resource affinity controls HA placement. It does not provide:

- Storage replication.
- Backup.
- Network redundancy.
- Application-level clustering.
- Guest operating system failover.

VM2Cloud places HA resources using HA rules.

---

## When to Use

Use resource affinity when:

- Two or more HA resources have a placement relationship.
- Related workloads should preferably run together.
- Workloads should preferably be separated.
- Application components have node-placement requirements.
- HA resources need coordinated placement.
- You need to control placement without manually migrating individual resources.

Examples include:

- Keeping an application server close to a required service.
- Separating resource-intensive workloads.
- Preventing two HA resources from competing for the same node.
- Keeping related workloads on the same node when appropriate.

---

## Prerequisites

Before configuring resource affinity:

- VM2Cloud must be configured as a cluster.
- HA must be available.
- The affected VMs or containers must exist.
- The affected resources should be managed by HA.
- Cluster quorum should be healthy.
- Cluster communication should be working.
- Target nodes should have sufficient CPU and memory.
- Required storage must be available.
- Required networking must be available.
- You must have sufficient permissions to modify HA configuration.

---

# Procedure

## Step 1: Open HA Configuration

1. Log in to the VM2Cloud web interface.
2. Select **Datacenter** from the left navigation tree.
3. Select **HA**.
4. Open the available HA rules or placement configuration.
5. Review the existing rules.

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

## Step 2: Choose the Affinity Type

Resource affinity describes the relationship between two or more HA resources.

| Type | Effect |
|------|--------|
| Positive (keep together) | The selected resources should run on the same node. |
| Negative (keep separate) | The selected resources should run on different nodes. |

Use positive affinity for workloads that communicate heavily or depend on each other. Use negative affinity to keep redundant instances of a service apart so a single node failure cannot take out both.

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

## Step 3: Create the Rule

1. Click **Add**.
2. Enter a name for the rule.
3. Select the affinity type.
4. Select the HA resources the rule applies to.
5. Save the rule.

A rule must reference at least two HA resources to be meaningful.

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

## Step 4: Verify the Rule

1. Confirm the rule appears in the rules list.
2. Confirm the correct resources are listed.
3. Confirm the resources are placed according to the rule.

### Screenshot 4

```text
[ Place Screenshot Here ]
```

---

# Verification

Verify the following:

- The rule appears in the HA configuration.
- The correct resources are associated with the rule.
- Resources with positive affinity run on the same node.
- Resources with negative affinity run on different nodes.
- Recovery after a node failure respects the rule.
- The cluster reports quorum.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Resources are not kept together | Verify that a single node has enough CPU, memory, and storage for all resources in the rule. |
| Resources are not kept apart | Verify the cluster has enough eligible nodes to separate them. |
| A resource fails to start | Conflicting affinity rules can leave no valid node; review all rules affecting the resource. |
| Rule has no effect | Confirm every referenced guest is managed by HA. |
| Unexpected placement after recovery | Review node-affinity rules, which are evaluated alongside resource affinity. |

---

# Related Documentation

- [HA Overview](HA-Overview.md)
- [HA Resources](HA-Resources.md)
- [Node Affinity](Node-Affinity.md)
- [Fencing](Fencing.md)
- [HA Troubleshooting](HA-Troubleshooting.md)

---

# Summary

Resource affinity defines relationships between HA resources so that related workloads run together or redundant workloads stay apart. Positive rules keep resources on the same node; negative rules distribute them across nodes. Because affinity rules constrain placement, confirm the cluster has enough eligible nodes and capacity to satisfy every rule, and review node-affinity rules alongside them to avoid conflicts.
