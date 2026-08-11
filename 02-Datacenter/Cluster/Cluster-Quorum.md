# Cluster Quorum

---

## Overview

**Quorum** is a mechanism that ensures a VM2Cloud cluster operates safely and consistently. It determines whether enough cluster members are available to safely perform cluster-wide operations.

A cluster has quorum when more than half of the expected votes are available. Maintaining quorum prevents multiple groups of nodes from operating independently, which could lead to inconsistent cluster configurations.

Without quorum, cluster-wide configuration changes are restricted to protect the integrity of the cluster.

---

## Why Quorum is Important

Quorum helps:

- Maintain cluster consistency.
- Prevent split-brain situations.
- Protect shared cluster configuration.
- Ensure only a healthy cluster can perform administrative operations.
- Maintain reliable communication between cluster nodes.

---

## How Quorum Works

Each node in a cluster contributes one vote by default.

The cluster compares the number of available votes with the expected number of votes.

If the available votes are greater than half of the expected votes, the cluster has quorum.

Example:

| Total Nodes | Expected Votes | Votes Required for Quorum |
|-------------|---------------:|--------------------------:|
| 1 | 1 | 1 |
| 2 | 2 | 2 |
| 3 | 3 | 2 |
| 4 | 4 | 3 |
| 5 | 5 | 3 |

---

## Expected Votes

Expected votes represent the total number of votes that the cluster expects from all configured nodes.

Each node normally contributes one vote.

The expected vote count is automatically updated when nodes are added to or removed from the cluster.

---

## Cluster Status

The cluster can operate in one of the following states:

### Quorate

The cluster has enough active nodes to safely perform cluster-wide operations.

Configuration changes and management tasks are permitted.

### Not Quorate

The cluster does not have enough active nodes.

To protect the cluster configuration, VM2Cloud restricts cluster-wide changes until quorum is restored.

---

## Split-Brain Protection

Split-brain occurs when two groups of cluster nodes lose communication with each other and both believe they are the active cluster.

Quorum prevents this condition by allowing only the group with sufficient votes to continue making cluster-wide changes.

---

## View Cluster Quorum

## Step 1: Open the Cluster

1. Log in to the VM2Cloud web interface.
2. Select **Datacenter**.
3. Select **Cluster**.

---

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

## Step 2: Review Cluster Status

Review the cluster information.

Typical information includes:

- Cluster Name
- Cluster Nodes
- Quorum Status
- Expected Votes
- Active Nodes
- Cluster Communication Status

---

### Screenshot 2

```text
[ Place Screenshot Here ]
```

---

## Verify Quorum Using the CLI

The VM2Cloud web interface displays the cluster status. For additional troubleshooting, the following CLI command can be used.

```bash
pvecm status
```

Typical information includes:

- Quorate status
- Expected votes
- Total votes
- Cluster nodes
- Cluster ID
- Transport

---

### Screenshot 3

```text
[ Place Screenshot Here ]
```

---

## Best Practices

- Deploy clusters with an odd number of voting nodes whenever possible.
- Ensure reliable network connectivity between cluster nodes.
- Synchronize the system time on all nodes.
- Investigate communication issues immediately if quorum is lost.
- Avoid forcing quorum except during controlled recovery procedures.

---

# Verification

Verify the following:

- The cluster reports **Quorate**.
- All expected nodes are online.
- Expected votes match the number of configured nodes.
- Cluster communication is healthy.
- Administrative operations complete successfully.

---

# Common Issues

| Issue | Resolution |
|--------|------------|
| Cluster is not quorate | Verify that enough cluster nodes are online and communicating. |
| Expected votes do not match the cluster size | Confirm that all cluster nodes are correctly configured and that removed nodes have been cleaned up. |
| Node appears offline | Verify network connectivity and the Corosync service on the affected node. |
| Unable to perform cluster operations | Check whether the cluster has quorum before attempting administrative tasks. |

---

# Summary

Quorum is a fundamental component of every VM2Cloud cluster. It ensures that cluster-wide operations are performed only when a healthy majority of nodes is available, protecting the cluster from configuration conflicts and maintaining consistent operation.
