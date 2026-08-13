# High Availability Overview

---

## Overview

VM2Cloud VE High Availability (HA) is designed to automatically recover supported virtual machines and containers when the node running them becomes unavailable.

HA is a **cluster-level feature**. It uses the cluster's communication, quorum, fencing, and HA management components to determine whether a node has failed and where an HA-managed guest can be recovered.

The primary purpose of HA is to reduce service downtime caused by node failures.

HA does **not** mean that a VM runs simultaneously on multiple nodes. Normally, an HA-managed VM runs on one node at a time. If that node fails, HA can recover the VM on another eligible node when the required resources are available.

---

## When to Use

Use HA when:

* A VM or container is an important production workload.
* Automatic recovery is required after a node failure.
* Multiple cluster nodes are available.
* The guest's storage is accessible from the nodes where it may be recovered.
* Manual intervention after a node failure should be minimized.
* Planned maintenance requires controlled workload movement.

HA is especially useful for workloads where extended downtime after a physical-node failure is unacceptable.

---

# How HA Works

At a high level, HA works as follows:

```text
                VM / Container
                      |
                      v
               HA Resource
                      |
                      v
              Running on Node 1
                      |
              Node 1 fails
                      |
                      v
             Cluster detects failure
                      |
                      v
                Quorum check
                      |
                      v
                 Fencing
                      |
                      v
            HA selects eligible node
                      |
                      v
             Guest is recovered
                      |
                      v
             Running on Node 2
```

The exact recovery behavior depends on cluster state, resource configuration, node availability, storage availability, and HA placement rules.

---

# HA Components

VM2Cloud VE HA depends on several cluster components.

## HA Resource

An HA resource is a VM or container that is managed by the HA framework.

The resource configuration tells HA that the guest should be managed automatically.

Examples:

```text
VM 100
VM 101
CT 200
```

Once a guest is added as an HA resource, its state is managed by the HA system.

---

## HA Manager

The HA manager coordinates HA resources and determines what operations need to be performed.

It handles operations such as:

* Starting resources.
* Stopping resources.
* Recovering resources.
* Relocating resources.
* Following HA placement rules.
* Reacting to node failures.

The underlying platform stores HA resource information in the cluster configuration and maintains HA status information across the cluster.

---

## Cluster Communication

HA depends on reliable cluster communication.

The cluster uses Corosync for communication between nodes.

This communication allows nodes to determine:

* Which nodes are available.
* Which nodes are communicating.
* Whether the cluster has quorum.
* Whether a node has become unreachable.

A reliable, low-latency cluster network is therefore critical for HA.

---

## Quorum

Quorum determines whether the cluster has enough votes to make consistent cluster decisions.

Each node normally contributes one vote.

If the cluster loses quorum, important cluster configuration operations are restricted because the cluster must avoid conflicting state changes.

HA should therefore always be deployed with a healthy cluster quorum.

---

## Fencing

Fencing ensures that a failed or isolated node cannot continue running HA resources while another node attempts to recover them.

This prevents two nodes from believing that they own and run the same guest.

In other words:

```text
Node 1 becomes unavailable
        |
        v
Cluster cannot safely assume
Node 1 stopped running the VM
        |
        v
Node 1 is fenced
        |
        v
Another node can safely recover VM
```

Fencing is an important part of preventing split-brain situations.

---

# HA Resource States

An HA-managed resource can have different operational states.

> **Verify:** Confirm the exact set of HA resource states shown on the Datacenter → HA panel.

Common concepts include:

* Started.
* Stopped.
* Starting.
* Stopping.
* Recovering.
* Migrating.
* Failed.
* Disabled.

The state indicates what HA currently expects or is doing with the resource.

---

# Adding a VM to HA

Adding a VM to HA tells VM2Cloud VE that the guest should be managed by the HA framework.

The general workflow is:

```text
Select VM
   ↓
Open HA management
   ↓
Add VM as HA resource
   ↓
Configure desired HA behavior
   ↓
Confirm
   ↓
HA begins managing the VM
```

> **Verify:** Confirm the exact button and menu labels for adding a guest to HA on the
> Datacenter → HA panel.

---

# What Happens During Normal Operation

Consider a three-node cluster:

```text
Cluster
├── node1
├── node2
└── node3
```

A VM is running on `node1`.

```text
VM 100
   |
   v
node1
```

The HA system continuously participates in cluster monitoring and resource management.

As long as:

* `node1` is available,
* cluster communication is healthy,
* quorum exists,
* and the VM is operating normally,

the VM continues running on `node1`.

HA does not continuously move the VM between nodes.

---

# What Happens When the Node Loses Power

Consider:

```text
node1
  |
  +--- VM 100
  |
  +--- Power failure
```

The VM immediately stops because the physical host has lost power.

The remaining cluster nodes detect that `node1` is no longer communicating.

The process can be represented as:

```text
Node 1 loses power
       ↓
Corosync communication is lost
       ↓
Cluster detects node failure
       ↓
Cluster evaluates quorum
       ↓
Failed node is fenced / confirmed stopped
       ↓
HA determines recovery location
       ↓
VM 100 is started on another eligible node
```

The exact recovery time depends on the failure detection and recovery process.

HA nodes continuously report their presence to the cluster. When a failed node does not return, HA recovers its guests on the remaining cluster nodes.

---

# HA Recovery Is Not Live Migration

This distinction is important.

## Live Migration

Live migration normally occurs while the source node is still operational.

```text
Node 1
  |
  | VM running
  |
  +---------> Node 2
                 |
              VM continues
              with minimal
              interruption
```

The VM is transferred from one operational node to another.

---

## HA Recovery

If the source node has suddenly lost power, there is no running source VM from which to perform a live migration.

Instead:

```text
Node 1
  |
  X Power failure
  |
  VM stops
       ↓
HA detects failure
       ↓
Node 2
  |
  +--- VM is started
```

Therefore, **HA recovery is normally a restart/recovery operation, not a live migration of the failed VM.**

---

# Storage Requirement

HA recovery requires the VM or container's required resources to be available on the node where HA attempts recovery.

For VM disks, this commonly means using shared storage that is accessible from multiple nodes.

For example:

```text
             Shared Storage
              /     |     \
             /      |      \
          node1   node2   node3
            |
          VM 100
```

If `node1` fails, another node can access the VM's storage and recover the VM.

The official documentation specifically notes that HA recovery requires the guest's resources to be available on the remaining nodes, primarily meaning that disk images should be available on shared storage.

---

# HA With Local Storage

Local storage requires additional planning.

If a VM's disk exists only on:

```text
node1
```

and `node1` completely fails:

```text
node1
  |
  +--- VM disk
  |
  X node failure
```

another node cannot simply access that local disk.

Therefore, automatic HA recovery requires an appropriate storage strategy.

Possible approaches include:

* Shared storage.
* Storage replication where appropriate.
* Other storage architectures that make the required guest data available on the recovery node.

Storage replication is asynchronous, so recovery can involve some data loss depending on the most recent successful replication.

---

# HA Placement

HA must determine where a resource should run.

For example:

```text
Cluster
├── node1
├── node2
└── node3
```

A VM may be configured to prefer:

```text
node1
```

If `node1` fails, HA can use another eligible node according to the configured HA rules and available resources.

Placement can be influenced by:

* HA groups.
* Node preferences.
* Node priorities.
* Resource-affinity rules.
* Node availability.
* Resource requirements.

See:

- [Node Affinity](Node-Affinity.md)
- [Resource Affinity](Resource-Affinity.md)
- [HA Resources](HA-Resources.md)

---

# Example: Node Preference

Suppose:

```text
VM 100
Preferred:
node1

Fallback:
node2
node3
```

Normal operation:

```text
VM 100
   ↓
node1
```

If `node1` fails:

```text
node1
  X
  |
  v
HA recovery
  |
  +---- node2
  |
  +---- node3
```

HA selects an eligible node according to the configured placement rules and current cluster state.

The purpose of node-affinity configuration is therefore not simply to "move a VM to a node." It defines placement preferences or restrictions that HA uses when deciding where a resource should run.

---

# Example: Node Failure

Consider a three-node cluster:

```text
             Cluster
        ┌──────┼──────┐
        │      │      │
      node1  node2  node3
        |
      VM 100
```

`VM 100` is running on `node1`.

### Failure

```text
node1
  |
  X
Power failure
```

### Detection

The remaining nodes detect that `node1` is no longer communicating.

### Quorum

The cluster verifies that it still has sufficient votes to operate safely.

### Fencing

The failed node is fenced or otherwise confirmed stopped so that it cannot continue running the VM while recovery takes place.

### Recovery

HA selects an eligible remaining node.

```text
node2
  |
  +--- VM 100
```

The VM is started on `node2`.

### Result

```text
Before:

node1 → VM 100

After node1 failure:

node2 → VM 100
```

There is downtime while the failure is detected, fencing/recovery decisions are made, and the guest starts.

---

# What Happens When the Original Node Returns

Suppose:

```text
node1
  |
  X
failure
```

HA recovers the VM on:

```text
node2
```

Later, `node1` comes back online.

The VM does not simply start simultaneously on both nodes.

HA maintains cluster-wide resource ownership and manages the guest according to its configured placement rules.

Whether the VM is moved back depends on the configured HA placement behavior and settings.

This is why HA groups and placement configuration are important.

---

# HA Does Not Provide Zero Downtime

HA reduces downtime; it does not guarantee zero downtime.

During a sudden node failure:

```text
Node failure
    ↓
Failure detection
    ↓
Fencing / safety checks
    ↓
Recovery
    ↓
VM boot
    ↓
Application startup
```

The guest experiences downtime during this process.

The actual duration depends on:

* Failure type.
* Cluster health.
* Quorum.
* Fencing.
* Storage availability.
* VM configuration.
* Guest operating system boot time.
* Application startup time.

---

# HA and Application State

HA restarts the guest.

It does not guarantee that an application inside the guest can recover every in-memory operation.

For example:

```text
VM
 |
 +--- Application
 |
 +--- RAM state
 |
 +--- Active sessions
```

If the physical node suddenly loses power, RAM state is lost.

After HA recovery:

```text
VM starts again
      ↓
Operating system boots
      ↓
Application starts
      ↓
Application recovers using its own mechanisms
```

Applications requiring higher availability should therefore also use appropriate application-level redundancy.

---

# HA and Backups

HA is not a backup mechanism.

HA protects primarily against node availability failures by recovering the guest on another eligible node.

Backups protect against problems such as:

* Accidental deletion.
* Data corruption.
* Application-level damage.
* User mistakes.
* Malware or destructive changes.
* Recovery to an earlier point in time.

A production environment should normally use both:

```text
HA
+
Backups
+
Appropriate storage design
```

---

# HA and Replication

HA and replication solve different problems.

## HA

```text
Node failure
     ↓
Recover guest
on another node
```

## Replication

```text
Source storage
     ↓
Synchronize changes
     ↓
Target storage
```

Replication can make guest data available on another node, but it is asynchronous and therefore may not contain the very latest changes.

A design can combine:

```text
HA
+
Storage Replication
+
Backups
```

when shared storage is not used and the workload's recovery requirements allow for asynchronous replication.

---

# HA Prerequisites

Before enabling HA for production workloads, verify:

* A healthy cluster exists.
* Cluster communication is reliable.
* Quorum is available.
* Fencing is properly configured.
* Required storage is accessible from eligible recovery nodes.
* The guest can run on the intended recovery nodes.
* Required network configuration is available.
* Required hardware is available.
* PCI passthrough requirements are considered.
* HA placement rules are correctly configured.
* Backup and recovery procedures exist.

---

# Important Hardware Consideration

A VM using hardware that exists only on one node may not be recoverable on another node.

For example:

```text
VM 100
 |
 +--- PCI device
 |
 +--- node1 only
```

If `node1` fails:

```text
node1
  X
  |
  v
node2

PCI device unavailable
```

The VM may not be able to start correctly on `node2`.

Therefore, passthrough devices and other node-specific resources must be considered when designing HA. The official documentation also highlights passed-through devices as a consideration for HA recovery.

---

# HA Operational Flow

The complete HA process can be summarized as:

```text
                 NORMAL OPERATION
                       |
                       v
                VM runs on Node 1
                       |
                       |
                 Node 1 fails
                       |
                       v
              Cluster communication
                 detects failure
                       |
                       v
                   Quorum check
                       |
                       v
                  Fencing / safety
                       |
                       v
              HA selects recovery node
                       |
                       v
              Storage/resources checked
                       |
                       v
                 VM is started
                       |
                       v
               VM runs on Node 2
                       |
                       v
                 Monitor normally
```

---

# What HA Does Not Do

HA does not:

* Run one VM simultaneously on multiple nodes.
* Guarantee zero downtime.
* Guarantee zero data loss.
* Replace backups.
* Automatically make local disks available on another node.
* Automatically provide identical hardware on every node.
* Guarantee that an application will recover correctly.
* Perform live migration after a node has suddenly lost power.

These limitations must be considered when designing a highly available environment.

---

# Verification

After configuring HA, verify the complete setup.

Check:

```text
[ ] Cluster is healthy
[ ] Cluster has quorum
[ ] All required nodes are available
[ ] Fencing is functional
[ ] VM is configured as an HA resource
[ ] VM storage is available to recovery nodes
[ ] Network configuration exists on recovery nodes
[ ] Hardware requirements are satisfied
[ ] HA placement rules are correct
[ ] Backup exists
[ ] Recovery procedure has been tested
```

A controlled HA test should be performed before relying on HA for production recovery.

---

# Best Practices

* Use a healthy multi-node cluster.
* Maintain reliable cluster networking.
* Maintain quorum.
* Configure fencing correctly.
* Use storage accessible to eligible recovery nodes.
* Plan HA placement before enabling production resources.
* Consider hardware passthrough limitations.
* Do not confuse HA recovery with live migration.
* Maintain independent backups.
* Test node-failure recovery.
* Monitor HA events and task history.
* Document the expected recovery behavior.
* Ensure administrators understand the difference between node failure, migration, and HA recovery.

---

# Related Documentation

High Availability:

- [HA Overview](HA-Overview.md)
- [HA Resources](HA-Resources.md)
- [Node Affinity](Node-Affinity.md)
- [Resource Affinity](Resource-Affinity.md)
- [Fencing](Fencing.md)
- [HA Troubleshooting](HA-Troubleshooting.md)

Cluster:

- [Cluster Overview](../Cluster/Cluster-Overview.md)
- [Quorum](../Cluster/Quorum.md)
- [Cluster File System](../Cluster/Cluster-File-System.md)

Related features:

- [Replication Overview](../Replication/Replication-Overview.md)
- [Backup and Restore Virtual Machine](../../04-Virtual-Machines/Backup-and-Restore-VM.md)
- [Backup and Restore Container](../../05-Containers/Backup-and-Restore-Container.md)

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| HA resources do not recover after a node failure | Verify the cluster retained quorum and that an eligible node can reach the guest's storage. |
| A resource remains in an error state | Review the HA task log and confirm the required storage and network exist on the node. |
| A guest starts on an unexpected node | Review the configured node-affinity and resource-affinity rules. |
| Nodes are fenced unexpectedly | Investigate cluster network stability. See [Fencing](Fencing.md). |
| HA repeatedly restarts a guest | Investigate the guest operating system; HA restarts a guest that fails to stay running. |
| HA options are unavailable | Confirm the node is part of a cluster and that you have permission to manage HA. |

---

# Summary

VM2Cloud VE High Availability automatically manages selected VMs and containers so they can be recovered when a node becomes unavailable.

The basic concept is:

```text
VM running
    ↓
Node fails
    ↓
Cluster detects failure
    ↓
Quorum is evaluated
    ↓
Failed node is fenced / confirmed stopped
    ↓
HA selects an eligible node
    ↓
Guest resources are accessed
    ↓
VM / container starts
    ↓
Service becomes available again
```

The most important point is that **HA recovery after a sudden node failure is not the same as live migration**.

Live migration moves a running guest between operational nodes.

HA recovery starts the guest on another eligible node after the original node has failed and has been safely isolated.

For HA to work reliably, the cluster, quorum, fencing, storage, networking, hardware requirements, and placement rules must all be designed together.
