# HA Troubleshooting

---

## Overview

High Availability (HA) allows VM2Cloud to manage selected virtual machines and containers across cluster nodes and recover workloads when a node becomes unavailable.

HA problems can be caused by:

* Loss of cluster quorum.
* Corosync communication problems.
* Node failure.
* Network failure.
* Storage problems.
* Insufficient CPU or memory.
* HA resource configuration.
* HA placement rules.
* Watchdog or fencing problems.
* Cluster configuration problems.
* VM or container configuration problems.

VM2Cloud uses the underlying Proxmox VE HA architecture. HA depends on a healthy cluster, reliable Corosync communication, and quorum. The cluster switches to read-only mode when quorum is lost.

---

## When to Use

Use this document when:

* An HA resource does not start.
* An HA resource does not migrate or recover.
* A VM or container is stuck in an HA state.
* A node unexpectedly becomes unavailable.
* A node is repeatedly fenced.
* HA resources move unexpectedly.
* The cluster loses quorum.
* HA operations remain pending.
* HA configuration cannot be changed.
* A recovered node does not behave normally.
* HA reports errors after a network failure.

---

## Prerequisites

Before troubleshooting HA:

* Log in to VM2Cloud with an account that has sufficient permissions.
* Identify the affected VM, container, or node.
* Confirm the cluster name.
* Know which node normally hosts the affected resource.
* Check whether the problem is currently affecting production workloads.
* Have a backup or recovery plan before performing destructive operations.
* Do not manually start an HA resource on another node if the original node may still be running it.

> **Warning:** Never bypass HA fencing or quorum protections simply to make a resource start. First determine the actual state of the affected node.

---

# Procedure

## Step 1: Identify the Affected HA Resource

1. Log in to VM2Cloud.
2. Select **Datacenter**.
3. Open **HA**.
4. Open the HA resource view.
5. Locate the affected VM or container.
6. Record its resource ID.
7. Record its current state.
8. Record the node currently associated with the resource.
9. Check whether another HA rule affects the resource.

### Screenshot 1

```text
[ Place Screenshot Here ]
```

---

## Step 2: Check Cluster Status

1. Open the cluster status information.
2. Check the cluster name.
3. Check all cluster nodes.
4. Verify that expected nodes are visible.
5. Check whether nodes are online.
6. Check quorum.
7. Check whether any node is disconnected.
8. Check for cluster warnings.

### Screenshot 2

```text
[ Place Screenshot Here ]
```

A healthy HA environment requires a functioning cluster and quorum.

---

## Step 3: Check Quorum

1. Review the cluster quorum state.
2. Check **Expected Votes**.
3. Check **Total Votes**.
4. Determine whether the cluster is quorate.
5. Identify missing or unavailable voting members.
6. Check whether the problem is caused by a node failure or network partition.

If quorum is lost, restore reliable cluster communication or restore the required cluster members before making additional HA changes.

### Screenshot 3

```text
[ Place Screenshot Here ]
```

The underlying cluster uses a majority-based quorum model. When quorum is lost, cluster configuration becomes read-only to protect the distributed state.

---

## Step 4: Check the Affected Node

1. Select the node associated with the HA resource.
2. Check whether the node is online.
3. Check CPU usage.
4. Check memory usage.
5. Check system status.
6. Check network interfaces.
7. Check storage status.
8. Review recent tasks.
9. Determine whether the node is responsive.

### Screenshot 4

```text
[ Place Screenshot Here ]
```

If the node is unreachable, determine whether it is:

* Powered off.
* Operating but disconnected from the cluster.
* Experiencing a network problem.
* Experiencing a hardware problem.
* Fenced by HA.

---

## Step 5: Check HA Resource State

1. Return to **Datacenter → HA**.
2. Locate the affected resource.
3. Review its current state.
4. Check whether it is running.
5. Check whether it is stopped.
6. Check whether it is recovering.
7. Check whether it is pending.
8. Check whether the resource has entered an error state.
9. Review recent HA tasks.

### Screenshot 5

```text
[ Place Screenshot Here ]
```

Do not repeatedly start or migrate the resource while HA is already processing a recovery operation.

---

## Step 6: Check Task History

1. Open **Task History**.
2. Locate the most recent HA-related task.
3. Identify the affected resource.
4. Identify the affected node.
5. Check the task status.
6. Open failed tasks.
7. Read the task output.
8. Record the error message.
9. Check the time of the failure.
10. Compare the failure time with any node or network event.

### Screenshot 6

```text
[ Place Screenshot Here ]
```

Task history is often the first place to identify whether an HA operation failed because of storage, networking, node availability, or resource configuration.

---

## Step 7: Check Node Connectivity

If the HA resource is not recovering:

1. Check whether the source node is reachable.
2. Check whether the target node is reachable.
3. Check cluster network connectivity.
4. Check network interface status.
5. Check VLAN configuration.
6. Check bonding configuration if used.
7. Check switch connectivity.
8. Check for packet loss.
9. Check for recent network failures.

### Screenshot 7

```text
[ Place Screenshot Here ]
```

The underlying cluster requires reliable communication between nodes. Corosync carries cluster communication and is critical to cluster stability.

---

## Step 8: Check Corosync

If the UI indicates a cluster communication problem, use CLI verification.

Run:

```bash
systemctl status corosync
```

Check:

* Service state.
* Recent failures.
* Restart events.
* Network errors.
* Communication errors.

### Screenshot 8

```text
[ Place Screenshot Here ]
```

If Corosync is not running, investigate the underlying service and network configuration before attempting HA recovery.

---

## Step 9: Check Cluster Status Through CLI

CLI is secondary and should be used for verification when the UI does not provide enough information.

Run:

```bash
pvecm status
```

Review:

```text
Cluster information
-------------------
Name:
Config Version:
Transport:
Secure auth:

Quorum information
------------------
Expected votes:
Total votes:
Quorum:
Flags:
```

Also review the node list shown by the command.

### Screenshot 9

```text
[ Place Screenshot Here ]
```

Do not modify quorum values simply because the cluster reports that it is not quorate.

---

## Step 10: Check HA Services

When HA appears stuck, verify the underlying HA services.

Run:

```bash
systemctl status pve-ha-lrm
```

Then:

```bash
systemctl status pve-ha-crm
```

Review:

* Service state.
* Failed state.
* Recent restart events.
* Error messages.

### Screenshot 10

```text
[ Place Screenshot Here ]
```

The HA services are responsible for local resource management and cluster-wide HA management.

---

## Step 11: Check HA Resource Configuration

If the resource cannot start or recover:

1. Open **Datacenter → HA**.
2. Locate the affected resource.
3. Review its HA configuration.
4. Check its configured state.
5. Check placement rules.
6. Check node-affinity rules.
7. Check resource-affinity rules.
8. Check whether the resource has restrictions preventing placement.
9. Verify that an eligible node exists.

### Screenshot 11

```text
[ Place Screenshot Here ]
```

---

## Step 12: Check Node Capacity

An HA resource may fail to start if the target node does not have sufficient resources.

Check:

* CPU availability.
* Memory availability.
* Storage availability.
* Network availability.
* Required hardware.
* PCI devices if applicable.

### Screenshot 12

```text
[ Place Screenshot Here ]
```

If the target node cannot satisfy the resource requirements, identify another eligible node or correct the resource constraints.

---

## Step 13: Check Storage Availability

If the VM or container cannot start:

1. Open the affected VM or container.
2. Review its hardware or storage configuration.
3. Identify the storage used by the resource.
4. Open **Datacenter → Storage**.
5. Check whether the storage is available.
6. Check storage content.
7. Check whether the required disk or volume exists.
8. Check whether the storage is accessible from the target node.
9. Review storage errors.

### Screenshot 13

```text
[ Place Screenshot Here ]
```

An HA resource cannot recover successfully if required storage is unavailable.

---

## Step 14: Check Network Requirements

If the resource starts but cannot provide network connectivity:

1. Open the affected VM or container.
2. Review its network configuration.
3. Identify the bridge or network used.
4. Check the target node.
5. Verify that the required network exists there.
6. Check VLAN configuration.
7. Check bridge configuration.
8. Check physical interface status.
9. Check network connectivity.

### Screenshot 14

```text
[ Place Screenshot Here ]
```

---

## Step 15: Check HA Placement Rules

If the resource cannot be placed on a node:

1. Open **Datacenter → HA**.
2. Review HA placement rules.
3. Check node-affinity rules.
4. Check resource-affinity rules.
5. Check whether the target node is excluded.
6. Check whether another resource creates a placement conflict.
7. Verify that at least one eligible node remains.

### Screenshot 15

```text
[ Place Screenshot Here ]
```

> **Warning:** Overly restrictive placement rules can prevent HA resources from finding an eligible recovery node.

---

## Step 16: Check for Fencing

If a node suddenly became unavailable:

1. Determine whether the node is actually powered off.
2. Check whether the node is reachable.
3. Review HA task history.
4. Check for fencing-related events.
5. Check watchdog availability when required.
6. Check cluster communication.
7. Check quorum.
8. Determine whether the node was safely fenced.

### Screenshot 16

```text
[ Place Screenshot Here ]
```

> **Warning:** Do not manually start a resource elsewhere unless you are certain that the original node cannot still run that resource.

---

## Step 17: Check Watchdog Availability

When fencing or HA recovery is suspected, CLI verification may be required.

Run:

```bash
ls -l /dev/watchdog*
```

Review whether a watchdog device exists.

### Screenshot 17

```text
[ Place Screenshot Here ]
```

If the watchdog device is unavailable, investigate the node's hardware and watchdog configuration before relying on HA fencing.

---

## Step 18: Review System Logs

When the UI and task history do not provide enough information, review system logs.

Use:

```bash
journalctl -b
```

For a specific service, use:

```bash
journalctl -u pve-ha-crm
```

and:

```bash
journalctl -u pve-ha-lrm
```

For Corosync:

```bash
journalctl -u corosync
```

Look for:

* HA errors.
* Corosync errors.
* Watchdog messages.
* Storage failures.
* Network failures.
* Resource start failures.
* Node communication problems.

### Screenshot 18

```text
[ Place Screenshot Here ]
```

---

# Common HA Problems

## HA Resource Does Not Start

### Possible Causes

* Target node is unavailable.
* Cluster has no quorum.
* Storage is unavailable.
* Network is unavailable.
* Insufficient CPU.
* Insufficient memory.
* Placement rule prevents the resource from starting.
* VM or container configuration is invalid.
* Required hardware is unavailable.

### Resolution

1. Check cluster quorum.
2. Check node status.
3. Check HA resource state.
4. Check task history.
5. Check storage.
6. Check networking.
7. Check CPU and memory.
8. Check placement rules.
9. Review the resource configuration.
10. Retry only after the underlying problem is corrected.

---

## HA Resource Is Stuck

### Possible Causes

* HA manager is waiting for another operation.
* Node communication failure.
* Resource lock.
* Storage operation is still running.
* Cluster quorum problem.
* Target node is unavailable.

### Resolution

1. Check the resource state.
2. Check Task History.
3. Check cluster quorum.
4. Check node connectivity.
5. Check HA services.
6. Check storage.
7. Review system logs.
8. Wait for an active recovery operation to complete when appropriate.
9. Do not repeatedly issue start/stop operations.

---

## HA Resource Moves to Another Node Unexpectedly

### Possible Causes

* Original node failed.
* Node lost cluster connectivity.
* HA recovery occurred.
* Placement rules changed.
* Node became unavailable.
* HA detected a failure condition.

### Resolution

1. Check HA task history.
2. Check the original node.
3. Check cluster quorum.
4. Check Corosync.
5. Check network connectivity.
6. Review HA placement rules.
7. Determine whether the migration was an expected HA recovery.

---

## Node Is Fenced

### Possible Causes

* Loss of cluster communication.
* Loss of quorum.
* Node failure.
* Network partition.
* Watchdog action.
* Hardware problem.

### Resolution

1. Determine whether the node is powered off.
2. Check cluster status.
3. Check quorum.
4. Check Corosync.
5. Check cluster networking.
6. Check watchdog state.
7. Review HA task history.
8. Restore the node only after confirming the fencing state.
9. Verify HA resources after recovery.

---

## Cluster Loses Quorum

### Possible Causes

* Multiple nodes offline.
* Network partition.
* Corosync failure.
* Switch failure.
* VLAN failure.
* Node network failure.

### Resolution

1. Check all nodes.
2. Check cluster network.
3. Check Corosync.
4. Check expected votes.
5. Check total votes.
6. Determine whether the missing nodes are actually offline.
7. Restore node connectivity.
8. Verify quorum.

> **Warning:** Do not force quorum merely to bypass the problem.

---

## HA Cannot Be Configured

### Possible Causes

* Cluster is not quorate.
* Insufficient permissions.
* Cluster configuration problem.
* Corosync problem.
* Datacenter configuration is unavailable.

### Resolution

1. Check user permissions.
2. Check cluster quorum.
3. Check cluster status.
4. Check Corosync.
5. Check whether cluster configuration is writable.
6. Correct the underlying cluster problem.
7. Retry the HA configuration.

---

## HA Resource Cannot Recover After Node Failure

### Possible Causes

* No eligible target node.
* Target node lacks resources.
* Storage unavailable.
* Network unavailable.
* Placement rules prevent recovery.
* Original node has not been safely fenced.
* Quorum unavailable.

### Resolution

1. Confirm the failed node state.
2. Verify fencing.
3. Check quorum.
4. Check HA resource state.
5. Check available nodes.
6. Check CPU and memory.
7. Check storage.
8. Check network.
9. Review placement rules.
10. Review task history.
11. Correct the blocking condition.

---

# Diagnostic Decision Flow

Use the following sequence when an HA resource fails:

```text
HA resource problem
        |
        v
Check resource state
        |
        v
Check cluster quorum
        |
   +----+----+
   |         |
No          Yes
   |         |
   v         v
Fix quorum  Check node
             |
             v
       Check connectivity
             |
             v
       Check HA services
             |
             v
       Check storage
             |
             v
       Check networking
             |
             v
       Check placement rules
             |
             v
       Check resource config
             |
             v
       Review task/log output
             |
             v
       Correct root cause
             |
             v
       Verify HA recovery
```

---

# Verification

After resolving an HA problem:

1. Open **Datacenter → HA**.
2. Confirm that the affected resource is visible.
3. Confirm that the resource has the expected state.
4. Confirm that the resource is running where expected.
5. Check cluster quorum.
6. Check node status.
7. Check storage.
8. Check networking.
9. Review Task History.
10. Confirm that no new HA errors are appearing.
11. Monitor the resource for a reasonable period.
12. Confirm that the original failure condition is no longer present.

### Screenshot 19

```text
[ Place Screenshot Here ]
```

---

## Verify Cluster Health

Confirm:

```text
Cluster
   ↓
All expected nodes visible
   ↓
Quorum available
   ↓
Corosync healthy
   ↓
HA resources healthy
```

---

## Verify HA Services

Run:

```bash
systemctl is-active pve-ha-crm
```

Then:

```bash
systemctl is-active pve-ha-lrm
```

Both services should report an active state on nodes where they are expected to operate.

---

## Verify Corosync

Run:

```bash
systemctl is-active corosync
```

The service should report an active state.

---

## Verify Cluster Status

Run:

```bash
pvecm status
```

Verify:

* Cluster membership.
* Expected votes.
* Total votes.
* Quorum.
* Cluster state.

---

# Emergency Recovery Considerations

When an HA node fails:

1. Do not immediately start the affected resource manually.
2. Determine whether the failed node is still running.
3. Determine whether the node is fenced.
4. Verify cluster quorum.
5. Check HA state.
6. Check whether HA recovery is already running.
7. Wait for the HA system to complete recovery when appropriate.
8. Only perform manual recovery when the official recovery procedure requires it and the source node is confirmed safe.

> **Warning:** Starting the same VM or container on two nodes can cause data corruption and other serious failures.

---

# CLI Troubleshooting Reference

CLI is secondary to the VM2Cloud web UI.

Use CLI primarily for verification and troubleshooting.

## Cluster Status

```bash
pvecm status
```

---

## Corosync Status

```bash
systemctl status corosync
```

---

## HA Cluster Resource Manager

```bash
systemctl status pve-ha-crm
```

---

## HA Local Resource Manager

```bash
systemctl status pve-ha-lrm
```

---

## Watchdog

```bash
ls -l /dev/watchdog*
```

---

## HA Service Logs

```bash
journalctl -u pve-ha-crm
```

```bash
journalctl -u pve-ha-lrm
```

---

## Corosync Logs

```bash
journalctl -u corosync
```

---

## Current Boot Logs

```bash
journalctl -b
```

---

# Safety Rules

> **Warning:** HA troubleshooting can affect production workloads.

Always:

* Confirm the affected resource.
* Check cluster quorum.
* Check node state.
* Check fencing state.
* Check storage.
* Check networking.
* Review task history.
* Identify the root cause before repeating an operation.

Never:

* Force quorum without understanding the cluster state.
* Start an HA resource on another node while the original node may still be running.
* Repeatedly restart HA services without investigating the problem.
* Delete HA configuration to bypass an error.
* Remove a cluster node without following the proper cluster-removal procedure.
* Assume that an unreachable node is powered off.

---

# Best Practices

* Keep cluster networking reliable and low latency.
* Monitor quorum continuously.
* Monitor Corosync health.
* Maintain sufficient node capacity.
* Avoid overly restrictive HA placement rules.
* Keep required storage available to recovery nodes.
* Keep network configuration consistent across nodes.
* Verify watchdog functionality where required.
* Review HA task history after failures.
* Document production HA placement policies.
* Test HA recovery in a controlled environment.
* Maintain current backups.
* Do not bypass quorum or fencing protections.
* Investigate repeated HA failures instead of repeatedly restarting services.

The underlying platform documentation recommends reliable cluster networking and notes that Corosync is central to cluster communication and the distributed configuration filesystem.

---

# Related Documentation

```text
09-High-Availability/
├── HA-Overview.md
├── HA-Resources.md
├── HA-Groups.md
├── Node-Affinity.md
├── Resource-Affinity.md
├── Fencing.md
├── Quorum.md
└── HA-Troubleshooting.md
```

Related cluster documentation:

```text
17-Cluster-Management/
├── Cluster-Overview.md
├── Create-Cluster.md
├── Join-Cluster.md
├── Add-Node.md
├── Remove-Node.md
├── Cluster-Options.md
├── Quorum.md
├── Expected-Votes.md
├── Corosync.md
├── Cluster-Status.md
└── Cluster-Troubleshooting.md
```

Related documentation:

```text
03-Node-Management/Node-Troubleshooting.md
06-Storage/Storage-Troubleshooting.md
07-Networking/Network-Troubleshooting.md
08-Backup-and-Restore/Backup-Troubleshooting.md
20-Troubleshooting/General-Troubleshooting.md
20-Troubleshooting/HA-Troubleshooting.md
```

---

# Summary

HA troubleshooting should begin with the cluster rather than the individual VM or container.

The recommended troubleshooting order is:

1. Check the HA resource.
2. Check cluster quorum.
3. Check node availability.
4. Check cluster networking.
5. Check Corosync.
6. Check HA services.
7. Check storage.
8. Check networking required by the resource.
9. Check placement rules.
10. Check fencing.
11. Review Task History.
12. Review system logs when required.
13. Correct the underlying problem.
14. Verify HA recovery.

The most important HA safety rule is to determine whether the original node is still running before manually starting an HA resource elsewhere.

This prevents duplicate resource execution and protects workload data.
