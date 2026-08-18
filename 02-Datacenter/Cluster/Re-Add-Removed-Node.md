# Re-Add a Removed Node

---

## Overview

Removing a node from a cluster leaves the two sides in **different** states, and understanding that asymmetry is the whole of this page.

* **The cluster** forgets the node completely. Its entry, its keys, and its directory under `/etc/pve/nodes/` are purged from every remaining member.
* **The removed node** is not touched at all. It still holds `/etc/pve/corosync.conf`, still has `/etc/corosync/`, still runs Corosync, and still believes it belongs to a cluster that no longer answers it. Because it cannot reach quorum alone, its cluster file system goes **read-only**.

So a removed node does not become standalone on its own. It becomes an orphan holding a stale cluster identity, which is why leaving one powered on inside the cluster network is discouraged — Corosync on that node keeps trying to talk to members that have already forgotten it.

Turning the orphan into a genuine standalone node takes a deliberate cleanup, documented as Step 5 of [Remove Node from Cluster](Remove-Node-from-Cluster.md). Once that cleanup has been done, the node is standalone in every meaningful sense and can join a cluster again.

---

## Two Paths

| Path | Use when | Cost |
|---|---|---|
| **Clean, then rejoin** | The cleanup was performed as part of the removal, the node has no guests left, and it has not been sitting powered on in the cluster network since. | Minutes. |
| **Reinstall, then join** | The cleanup was never done, the node was left running with the old cluster configuration, or you cannot account for what state it is in. | A full installation. |

The condition that decides this is **whether the cleanup was performed** — not how much time has passed. A node cleaned properly is as good as a freshly installed one for joining purposes.

Reinstalling is the safer default for production, because it is the only path whose result is independent of the node's history. It is not, however, a technical requirement, and treating it as one wastes a great deal of time on nodes that were removed cleanly.

> **Note:** Two pieces of state survive the cleanup — the node's SSH host keys and its local `pve-cluster` database. Neither normally prevents a join, but a host-key mismatch is the usual reason a rejoin fails on the cleanup path. See [Common Issues](#common-issues).

---

## When to Use

Use this procedure when:

* A node was removed and now needs to return to the cluster.
* Hardware was repaired after a failure and the node had been removed during the outage.
* A node was removed in error.
* Hardware is being repurposed back into the cluster.

If the node is still a cluster member and merely offline, none of this applies — bring it back online and it rejoins on its own. Confirm with `pvecm nodes` before doing anything else.

---

## Prerequisites

Before re-adding a node, ensure that:

* The node was genuinely removed — confirm with `pvecm nodes` on a working member.
* No stale directory for it remains under `/etc/pve/nodes/` on any member.
* The cluster is healthy and has quorum.
* The node has no guests configured. A join is refused if it does.
* Time is synchronized on both sides. See [Time and NTP](../../03-Nodes/System/Time-and-NTP.md).
* Names resolve in both directions between the node and the cluster. See [Hosts](../../03-Nodes/System/Hosts.md).
* You have the root password for an existing cluster node.
* For the reinstall path: installation media, console access, and confirmation that **anything on the node is backed up or expendable**.

---

# Procedure

## Step 1: Confirm the Cluster Forgot the Node

On any working cluster member:

```bash
pvecm nodes
ls /etc/pve/nodes/
```

The node must be absent from **both**. The membership list is the obvious check; the directory listing is the one people skip, and a leftover directory is what causes a join to fail later with a name conflict.

If a directory remains, remove it before continuing:

```bash
rm -rf /etc/pve/nodes/<nodename>
```

If the node still appears in `pvecm nodes`, it was never fully removed — finish [Remove Node from Cluster](Remove-Node-from-Cluster.md) first.

---

### Screenshot 1

**Cluster After Removal**

```text
[ Place Screenshot Here ]
```

> **Capture:** A shell on a **remaining** cluster node showing `pvecm nodes` and
> `ls /etc/pve/nodes/` in one frame, after a node has been removed. The removed node must
> be absent from both.

---

## Step 2: Check the State of the Removed Node

Now look at the node itself. This determines which path you take.

Log in to it — by console if the interface is unreachable — and check:

```bash
ls /etc/corosync/
cat /etc/pve/corosync.conf
pvecm status
```

| What you find | What it means |
|---|---|
| Corosync configuration present, `pvecm status` reports no quorum | The node is an **orphan**. Clean it — Step 3. |
| No Corosync configuration, cluster file system writable | The node is already **standalone**. Skip to Step 4. |
| Cannot determine, or the node has been running in the network since removal | **Reinstall** — Step 5. |

An orphan's web interface still shows the old cluster name under **Datacenter → Cluster**, which is the quickest visual check if the interface is reachable.

---

### Screenshot 2

**Removed Node Still Holding Cluster Configuration**

```text
[ Place Screenshot Here ]
```

> **Capture:** The removed node's own interface at Datacenter → Cluster, still showing the
> old cluster name, or a shell showing `pvecm status` reporting no quorum. This is the
> state the cleanup exists to resolve.

---

## Step 3: Make the Node Standalone

This is the cleanup documented in [Remove Node from Cluster](Remove-Node-from-Cluster.md) — Step 5 there covers it in full, with screenshots. In summary, on the removed node:

```bash
systemctl stop pve-cluster
systemctl stop corosync
pmxcfs -l
rm -f /etc/pve/corosync.conf
rm -rf /etc/corosync/*
killall pmxcfs
systemctl start pve-cluster
systemctl restart pvedaemon pveproxy pvestatd
```

Then confirm the result:

```bash
pvecm status
```

It should report that the node is not part of a cluster, and `/etc/pve` should be writable again. In the interface, **Datacenter → Cluster** now reads **Standalone node - no cluster defined**.

> **Warning:** Run this on the **removed** node, never on a cluster member. Deleting the Corosync configuration on a node that is still a member takes that node out of the cluster and, if enough members are affected, costs the cluster its quorum.

---

### Screenshot 3

**Standalone After Cleanup**

```text
[ Place Screenshot Here ]
```

> **Capture:** The cleaned node's Datacenter → Cluster panel reading **Standalone node -
> no cluster defined**, with an empty Cluster Nodes table.

---

## Step 4: Decide on the Hostname and Address

You may reuse the previous hostname and address or choose new ones.

| Choice | Consideration |
|---|---|
| **Reuse** | Simpler for documentation, monitoring, and firewall rules. Safe once Step 1 confirmed the old entry is fully purged. |
| **New** | Removes any chance of a stale reference. Means updating anything that referred to the old name. |

If Step 1 turned up any leftover entry that you could not fully clear, use a new name.

---

## Step 5: Reinstall Instead (When Required)

Take this path when Step 2 pointed to it.

1. Confirm anything on the node is backed up or genuinely expendable.
2. Boot the hardware from the installation media.
3. Install VM2Cloud VE.
4. Set the hostname and network configuration decided in Step 4.
5. Complete the installation and let the node boot.

> **Warning:** Reinstalling destroys all local data on the node, including any guest still stored there. This cannot be undone. Verify before proceeding.

The node comes up standalone with no cluster configuration — the same end state Step 3 produces, reached the long way.

---

## Step 6: Prepare the Node

Whichever path brought you here, confirm the basics before joining. A node that joins with the wrong time or an unresolvable name creates problems that are much harder to diagnose afterwards.

1. Confirm the node is reachable on the network.
2. Confirm its name resolves from the cluster members, and theirs from it.
3. Confirm time is synchronized.
4. Confirm it has no guests configured — `qm list` and `pct list` should both be empty.

Time synchronization matters more than it appears. Cluster communication and certificate validation both depend on it, and a skewed clock produces a join failure that reads like a certificate problem.

---

## Step 7: Join the Cluster

The node is new as far as the cluster is concerned, so this is the ordinary join.

1. On an existing member: **Datacenter → Cluster → Join Information → Copy Information**.
2. On the returning node: **Datacenter → Cluster → Join Cluster**.
3. Paste the information. The peer address and fingerprint fill themselves in.
4. Enter the root password **of the existing cluster node**, not of the joining one.
5. Confirm.

The joining node's interface disconnects during the join and its certificate changes, so expect a browser warning. Continue from an existing member's interface rather than the returning node's.

See [Join Node to Cluster](Join-Node-to-Cluster.md) for the full workflow.

---

### Screenshot 4

**Join Cluster Dialog**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Join Cluster dialog on the returning node with the join information
> pasted, showing the peer address and fingerprint populated.

---

## Step 8: Verify the Node Rejoined

On any cluster member:

```bash
pvecm nodes
pvecm status
```

The node must appear in the membership list, **Total votes** must match the full node count, and the cluster must be quorate. That restored vote count is the real confirmation — the interface can show a node before it is fully participating.

In the interface, confirm the node appears in the resource tree and reports **Online**.

---

### Screenshot 5

**Votes Restored**

```text
[ Place Screenshot Here ]
```

> **Capture:** `pvecm nodes` and `pvecm status` on an existing member after the rejoin, in
> one frame. The membership list must include the returned node, and **Total votes** must
> match the full node count.

---

### Screenshot 6

**Node Online in the Interface**

```text
[ Place Screenshot Here ]
```

> **Capture:** The resource tree showing the returned node online alongside the existing
> members.

---

## Step 9: Restore the Node to Service

A node that took the reinstall path keeps nothing. A node that took the cleanup path keeps its local configuration, but joining replaces the cluster-wide portion of `/etc/pve` with the cluster's own. Either way, work through this list rather than assuming.

1. Reconfigure storage that should be available on this node. See [Add Storage](../Storage/Add-Storage.md).
2. Reconfigure networking — bridges, bonds, VLANs. See [Network Overview](../../03-Nodes/System/Network/Network-Overview.md).
3. Re-apply the node firewall configuration if used. See [Node Firewall](../../03-Nodes/Node-Firewall.md).
4. Migrate guests back, or restore them from backup.
5. Add the node to HA placement rules if it should host HA resources. See [Node Affinity](../HA/Node-Affinity.md).
6. Confirm backup jobs covering this node still resolve correctly. See [Manage Backup Job](../Backup/Manage-Backup-Job.md).

---

# Configuration / Options

| Command | Purpose |
|---|---|
| `pvecm nodes` | List cluster members. Confirms whether a node is present. |
| `pvecm status` | Show quorum state, expected votes, and total votes. |
| `ls /etc/pve/nodes/` | Show per-node directories, revealing stale entries the membership list does not. |
| `ls /etc/corosync/` | On the removed node, reveals whether it still holds cluster configuration. |
| `pmxcfs -l` | Start the cluster file system in local mode, so it can be edited without quorum. |

> **Verify:** Confirm whether the interface exposes the cluster member list anywhere that
> makes Step 1 possible without the shell.

---

# Verification

Verify the following:

* `pvecm nodes` lists the node as a member.
* `pvecm status` shows total votes matching the full node count, and the cluster quorate.
* The node reports **Online** in the resource tree.
* The cluster file system is writable on the returned node.
* Storage configured for the node is accessible from it.
* Networking matches the other nodes where it should.
* Migration to and from the node works.
* HA rules referencing the node behave as intended.
* Backup jobs resolve to the expected guests.

Test a migration to the node before returning production workloads to it.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Join fails on a host key mismatch | The node kept its SSH host keys through the cleanup. Remove its entry from `/etc/pve/priv/known_hosts` on a cluster member and retry. This is the most common cleanup-path failure. |
| Join fails with a name or address conflict | The old entry was not fully purged. Check `/etc/pve/nodes/` on **every** member, not only the one you are using. |
| Join fails with a certificate error | Usually a time synchronization problem. Confirm time on both sides before retrying. |
| Join is refused because guests exist | A node must have no guest configuration to join. Move or remove them first. |
| Node appears offline after joining | Check network connectivity and cluster communication. See [Cluster Troubleshooting](Cluster-Troubleshooting.md). |
| Cleanup left the node still showing the old cluster | `pmxcfs -l` was not running when the configuration was deleted, so the write was rejected. Repeat Step 3 in order. |
| Removed node disturbs the cluster | It was left powered on holding the old Corosync configuration. Take it off the network, then clean or reinstall it. |
| Expected votes did not increase | The join did not complete. Check `pvecm status` and the join task output. |
| Repeated join failures on a cleaned node | Stop troubleshooting and reinstall. The cleanup path is a shortcut, not an obligation. |
| HA does not place resources on the node | It is not included in the relevant placement rules. See [Node Affinity](../HA/Node-Affinity.md). |

---

# Best Practices

- **Clean the node as part of the removal, not later.** Doing it immediately is what keeps the fast path available; a node left running with stale cluster configuration forfeits it.
- Never run the cleanup on a node that is still a cluster member.
- Confirm the node is genuinely absent from the cluster before doing anything to it, so you are not solving the wrong problem.
- Check `/etc/pve/nodes/` on every member, not only the one you happen to be logged into.
- Reinstall when you cannot account for the node's state. Certainty is worth the extra time in production.
- Verify backups before erasing anything on the reinstall path.
- Synchronize time before joining. It prevents the most common join failure.
- Prefer a new hostname when there is any doubt about stale entries.
- Rebuild storage and network configuration deliberately rather than assuming defaults match the other nodes.
- Test a migration before returning production workloads.
- Record why the node was removed and re-added.

---

# Related Documentation

- [Remove Node from Cluster](Remove-Node-from-Cluster.md)
- [Join Node to Cluster](Join-Node-to-Cluster.md)
- [Cluster Overview](Cluster-Overview.md)
- [Quorum](Quorum.md)
- [Recover Quorum](Recover-Quorum.md)
- [Cluster File System](Cluster-File-System.md)
- [Cluster Troubleshooting](Cluster-Troubleshooting.md)
- [Hosts](../../03-Nodes/System/Hosts.md)
- [Time and NTP](../../03-Nodes/System/Time-and-NTP.md)
- [Node Firewall](../../03-Nodes/Node-Firewall.md)
- [Add Storage](../Storage/Add-Storage.md)

---

# Summary

Removing a node leaves the cluster and the node in different states. The cluster forgets it entirely; the node keeps the old cluster configuration and, unable to reach quorum alone, drops to a read-only cluster file system. It is an orphan, not a standalone machine.

Making it standalone is a deliberate cleanup — stop the cluster services, start the cluster file system in local mode, delete the Corosync configuration, restart. That procedure is documented as Step 5 of [Remove Node from Cluster](Remove-Node-from-Cluster.md), and once it has been done the node can join a cluster again like any other.

Reinstalling produces the same end state and is the safer default in production, because its result does not depend on what the node has been doing since removal. It is a choice about certainty rather than a technical requirement. Take it when the cleanup was never performed, when the node was left running in the network with stale cluster configuration, or when a cleaned node refuses to join and you would rather rebuild than diagnose.
