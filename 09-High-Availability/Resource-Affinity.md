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

VM2Cloud uses the underlying Proxmox VE HA rule mechanism for resource placement.

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
