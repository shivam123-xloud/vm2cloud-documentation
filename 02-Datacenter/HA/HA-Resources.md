# HA Resources

## Overview

An HA resource is a virtual machine or container that is managed by the VM2Cloud High Availability system.

When a VM or container is added as an HA resource, VM2Cloud monitors its state and can perform HA operations according to the configured resource state, cluster health, node availability, and HA placement rules.

HA resources are normally used for workloads that should automatically recover when the node running them becomes unavailable.

---

## When to Use

Use HA resources when:

- A VM requires automatic recovery after a node failure.
- A container requires automatic recovery after a node failure.
- A production workload should be managed by HA.
- You want HA to control the start/stop state of a guest.
- You want a guest to participate in HA placement and recovery rules.

Do not add every VM or container to HA automatically. HA should be enabled only for workloads that have been designed and tested for HA recovery.

---

## Prerequisites

Before adding a resource to HA:

- The VM2Cloud cluster must be configured.
- The cluster should have quorum.
- The required nodes must be available.
- The guest must already exist.
- Required guest storage must be available on eligible recovery nodes.
- Required networking must be available on eligible nodes.
- Any required hardware must be available on the nodes where the guest may run.
- Appropriate permissions are required to manage HA resources.

For production workloads, verify that backups and recovery procedures are also available.

---

# What Is an HA Resource?

An HA resource is a guest that has been registered with the HA system.

For example:

```text
Cluster
├── node1
│   └── VM 100
├── node2
└── node3
