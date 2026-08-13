# Firewall Lockout Recovery

---

## Overview

If a firewall rule blocks your access to the VM2Cloud VE web interface or to SSH, you cannot use the interface to fix the rule — the rule is what is stopping you reaching it.

This page is the way out. It requires console or physical access to a node, because every network path may be blocked.

The other firewall pages tell you to "use console access to disable the firewall". This page says how.

> **Warning:** Disabling the firewall removes filtering from the cluster, node, or guest concerned, leaving it exposed until you re-enable it. Do this as a recovery step, fix the rule that caused the lockout, and re-enable filtering immediately afterwards.

---

## When to Use

Use this procedure when:

* The web interface stopped responding after a firewall change.
* SSH to a node stopped working after a firewall change.
* A guest lost all network access after its firewall was enabled.
* Cluster nodes lost contact with each other after a firewall change.

If access was lost after a **network** change rather than a firewall change, see [Network Troubleshooting](../../03-Nodes/System/Network/Network-Troubleshooting.md) instead — the recovery is different.

---

## Prerequisites

Before starting, ensure that:

* You have console access to the node — out-of-band management (iDRAC, iLO, IPMI), a hypervisor console for a virtual node, or physical access with keyboard and monitor.
* You have the root password for the node.
* You know which change caused the lockout, if possible.
* You know whether the cluster currently has quorum. This determines which method works.

---

# How to Choose a Method

The firewall configuration lives on the cluster file system, which becomes **read-only when the cluster loses quorum**. That determines which recovery path is available.

```text
Can you reach a node console?
        |
        v
Does the cluster have quorum?
        |
   +----+----+
   |         |
  Yes        No
   |         |
   v         v
Method A   Method B
(edit      (stop the
config)    service)
```

**Method A — edit the firewall configuration.** Cleaner, survives a reboot, works per level. Requires a writable cluster file system.

**Method B — stop the firewall service.** Works regardless of quorum, but is local to that node and does not survive a reboot.

If quorum is also lost, recover that first — see [Recover Quorum](../Cluster/Recover-Quorum.md) — then use Method A.

---

# Procedure — Method A: Disable in the Configuration

## Step 1: Open a Console on the Node

Connect through out-of-band management, the hypervisor console, or physically. Log in as root.

---

### Screenshot 1

**Node Console Login**

```text
[ Place Screenshot Here ]
```

> **Capture:** An out-of-band or hypervisor console session showing the node login
> prompt, to illustrate the access method.

---

## Step 2: Identify Which Level Is Blocking

Firewall configuration is stored per level:

| Level | Configuration file |
|---|---|
| Datacenter | `/etc/pve/firewall/cluster.fw` |
| Node | `/etc/pve/nodes/<nodename>/host.fw` |
| Guest | `/etc/pve/firewall/<vmid>.fw` |

Start with the level you changed most recently. If the web interface is unreachable, the datacenter or node level is the usual cause.

> **Verify:** Confirm these configuration paths in this deployment.

---

## Step 3: Disable the Firewall at That Level

Edit the relevant file:

```bash
nano /etc/pve/firewall/cluster.fw
```

Find the `[OPTIONS]` section and set the enable flag off:

```text
[OPTIONS]
enable: 0
```

Save and exit.

The change applies without a restart. Filtering at that level stops.

---

### Screenshot 2

**Firewall Configuration File**

```text
[ Place Screenshot Here ]
```

> **Capture:** A console session showing `cluster.fw` open in an editor with the
> `[OPTIONS]` section and the enable line visible.

---

## Step 4: Confirm Access Is Restored

From another machine, load the web interface or connect over SSH.

If access is still blocked, another level is also filtering. Repeat Step 3 for the node file, and then the guest file if a guest is affected.

---

# Procedure — Method B: Stop the Firewall Service

Use this when the cluster file system is read-only, or when you need access back immediately.

## Step 1: Open a Console on the Node

As in Method A.

## Step 2: Stop the Firewall Service

```bash
systemctl stop pve-firewall
```

Filtering stops on that node immediately.

## Step 3: Confirm Access

Try the web interface or SSH again.

> **Warning:** This is local to the node and does not survive a reboot. The firewall will start again with the same rules on the next boot, and lock you out again, unless you fix the underlying rule. Treat this as buying time, not as a fix.

> **Verify:** Confirm the exact service name for the firewall in this deployment.

---

### Screenshot 3

**Stopping the Firewall Service**

```text
[ Place Screenshot Here ]
```

> **Capture:** A console session showing the firewall service being stopped and its
> status afterwards.

---

# After Access Is Restored

## Step 1: Find the Rule That Caused It

Open the rule list for the level you disabled and look for:

* A rule blocking TCP **8006**, the web interface port.
* A rule blocking TCP **22**, if SSH was lost.
* A broad **DROP** or **REJECT** rule positioned above your management-access rule.
* A **missing** management-access rule, combined with the default input policy of DROP.
* A rule blocking traffic between node addresses, if the cluster lost contact.

The last is worth checking even if the web interface was your only symptom — a rule that breaks cluster communication causes quorum loss, which causes its own set of problems.

## Step 2: Add or Correct the Management-Access Rule

Create an inbound ACCEPT rule for TCP 8006 from your administrative network, and confirm it sits **above** any broader deny rule. See [Firewall Rules](Firewall-Rules.md).

## Step 3: Re-Enable the Firewall

**If you used Method A**, set `enable: 1` in the configuration file, or re-enable through the interface now that it is reachable.

**If you used Method B**, restart the service:

```bash
systemctl start pve-firewall
```

## Step 4: Verify From a Second Machine

1. Open a second browser session **before** testing, and keep it logged in.
2. From a different machine, confirm the web interface responds.
3. Confirm SSH works if you rely on it.
4. Confirm cluster nodes are online and quorate.
5. Confirm guests have the network access they need.

---

# Configuration / Options

| Action | Command or file |
|---|---|
| Disable datacenter firewall | `enable: 0` in `/etc/pve/firewall/cluster.fw` |
| Disable node firewall | `enable: 0` in `/etc/pve/nodes/<nodename>/host.fw` |
| Disable guest firewall | `enable: 0` in `/etc/pve/firewall/<vmid>.fw` |
| Stop filtering on a node | `systemctl stop pve-firewall` |
| Start filtering on a node | `systemctl start pve-firewall` |
| Check the firewall service | `systemctl status pve-firewall` |

> **Verify:** Confirm the configuration paths, the service name, and the exact enable
> flag syntax in this deployment.

---

# Verification

Verify the following:

* The web interface responds from a machine other than the one you are working on.
* SSH works if required.
* All cluster nodes report online and the cluster is quorate.
* Guests have their expected network access.
* The rule that caused the lockout has been corrected, not merely bypassed.
* Filtering has been re-enabled at every level you disabled.
* A management-access rule exists and sits above any broader deny rule.

The sixth check is the one that gets forgotten. A cluster left with the firewall disabled after a recovery is a lasting exposure created by a temporary fix.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Cannot edit the configuration file | The cluster file system is read-only, meaning quorum is lost. Use Method B, or see [Recover Quorum](../Cluster/Recover-Quorum.md). |
| Access still blocked after disabling one level | Another level is also filtering. Disable the node and guest levels in turn. |
| Locked out again after a reboot | Method B does not persist. Fix the rule and re-enable properly. |
| No console access available | There is no remote recovery from a full lockout. Physical or out-of-band access is required. This is why it must be arranged in advance. |
| Cluster nodes still cannot see each other | A rule is blocking cluster traffic. Check the node-level rules for anything affecting traffic between node addresses. |
| A guest is still isolated | Guest filtering is separate. Disable it in the guest's own configuration file. |
| Unsure which rule caused it | Review the most recent firewall changes, then check for a broad deny above your management rule. |

---

# Best Practices

- **Arrange console access before you need it.** Out-of-band management configured and tested is the difference between a five-minute recovery and a trip to the data centre.
- Keep a second browser session open during any firewall change, so a lockout is visible before you lose your working session.
- Add and verify the management-access rule *before* enabling the firewall — see [Firewall Options](Firewall-Options.md).
- Make firewall changes during a maintenance window.
- Change one thing at a time, so the cause of a lockout is obvious.
- Prefer Method A when quorum allows — it is per level and survives a reboot.
- Re-enable filtering immediately after fixing the rule, and verify it.
- Record what was disabled during the incident, so nothing is left off.
- Test firewall changes on a non-production node first.

---

# Related Documentation

- [Firewall Overview](Firewall-Overview.md)
- [Firewall Options](Firewall-Options.md)
- [Firewall Rules](Firewall-Rules.md)
- [Node Firewall](../../03-Nodes/Node-Firewall.md)
- [VM Firewall](../../04-Virtual-Machines/VM-Firewall.md)
- [Container Firewall](../../05-Containers/CT-Firewall.md)
- [Recover Quorum](../Cluster/Recover-Quorum.md)
- [Network Troubleshooting](../../03-Nodes/System/Network/Network-Troubleshooting.md)
- [Shell](../../03-Nodes/Shell.md)

---

# Summary

A firewall rule that blocks the web interface cannot be fixed from the web interface. Recovery needs console or out-of-band access to a node, then one of two methods: edit the firewall configuration file to set `enable: 0` at the affected level, which is clean and persistent but needs a writable cluster file system; or stop the firewall service on the node, which always works but is local and does not survive a reboot.

Either way, the recovery is not finished when access returns. Find the rule that caused the lockout, correct it, re-enable filtering at every level you disabled, and verify from a second machine. The most common lasting damage from a lockout is a cluster left running with its firewall switched off.
