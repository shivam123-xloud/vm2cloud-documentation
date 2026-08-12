# Firewall Overview

---

## Overview

The VM2Cloud firewall filters network traffic for the cluster, for individual nodes, and for individual virtual machines and containers.

It is configured at **three independent levels**, each with its own enable switch and its own rule list:

| Level | Where it is configured | What it protects |
|---|---|---|
| **Datacenter** | Datacenter → Firewall | The whole cluster. Rules here apply to every node and can be inherited by guests. |
| **Node** | Node → Firewall | Traffic to and from the node itself, including the management interface. |
| **Guest** | VM or Container → Firewall | Traffic to and from one virtual machine or container. |

A packet is only accepted if it is permitted at every level that applies to it. Enabling the firewall at the datacenter level does not by itself filter guest traffic, and enabling it on a guest does nothing until the firewall is enabled at the datacenter level as well.

This page explains the concepts shared by all three levels — the rule model, direction, actions, evaluation order, and the reusable objects. The level-specific pages document only their own panel.

> **Warning:** The firewall can lock you out of the VM2Cloud web interface. The default input policy is **DROP**, and the web interface listens on TCP port **8006**. If you enable the firewall without a rule permitting your management access, you will lose access to the interface and will need console or physical access to recover. Always add and verify a management-access rule *before* enabling the firewall.

---

## When to Use

Use the firewall when:

* Guests must be isolated from each other.
* A guest should only accept traffic on specific ports.
* Management access should be restricted to an administrative network.
* A group of guests needs the same rule set applied consistently.
* Traffic between network segments must be controlled.
* A compliance requirement calls for host-level or guest-level filtering.

Do not rely on the VM2Cloud firewall as your only network control. It complements, rather than replaces, upstream network segmentation and the guest operating system's own firewall.

---

## Prerequisites

Before configuring the firewall, ensure that:

* You have administrator privileges, or a role granting firewall permissions.
* You know which management network and addresses must retain access.
* You have console access to the node in case a rule locks out the web interface.
* You understand which guests will be affected before enabling anything.
* For cluster-wide rules, the cluster has quorum.

---

# How the Firewall Works

## The Three Levels

Rules are evaluated per level. Traffic reaching a guest passes the datacenter rules, then the node rules, then the guest rules.

```text
                     Incoming packet
                            |
                            v
                 Datacenter firewall rules
                            |
                            v
                    Node firewall rules
                            |
                            v
                   Guest firewall rules
                            |
                            v
                     Guest operating system
```

Each level can drop the packet. A packet must survive all applicable levels to be delivered.

## Enabling Is Two Steps for Guests

A guest is only filtered when **both** of these are true:

1. The firewall is enabled on the guest, on its **Firewall → Options** panel.
2. The firewall is enabled on the guest's **network device**, using the firewall checkbox on that virtual NIC.

This catches people out regularly: rules appear configured and enabled, but no filtering happens because the network device checkbox was never ticked.

> **Verify:** Confirm the exact label of the firewall checkbox on the guest network device
> (VM → Hardware → Network Device, and Container → Network).

## Direction

Every rule has a direction, interpreted from the point of view of the object being protected.

| Direction | Meaning |
|---|---|
| **in** | Traffic arriving at the node or guest. |
| **out** | Traffic leaving the node or guest. |

A rule permitting SSH **in** to a guest does not permit that guest to make SSH connections **out**.

## Actions

| Action | Behaviour |
|---|---|
| **ACCEPT** | The packet is allowed. |
| **DROP** | The packet is discarded silently. The sender receives nothing and waits for a timeout. |
| **REJECT** | The packet is discarded and an error is returned to the sender, which fails immediately. |

Use **DROP** for untrusted networks, where silence gives an attacker less information. Use **REJECT** on internal networks, where a fast, clear failure is easier to troubleshoot.

## Default Policies

Each level has a default input policy and a default output policy, applied to any traffic no rule matches.

The usual defaults are **input: DROP** and **output: ACCEPT** — nothing may come in unless a rule allows it, and anything may go out.

> **Verify:** Confirm the default input and output policy values shown on
> Datacenter → Firewall → Options in this deployment.

## Rule Evaluation Order

Within a level, rules are evaluated **top to bottom**, and the **first match wins**. Once a rule matches, no later rule is considered.

This means order is significant:

```text
Rule 1: ACCEPT  in  tcp  dport 22   from 10.0.0.0/24
Rule 2: DROP    in  tcp  dport 22
```

Reversed, the DROP would match first and the ACCEPT would never be reached.

Use the panel's move controls to reorder rules, and put narrower rules above broader ones.

---

# Reusable Objects

Three object types let you write a definition once and use it in many rules. All three are defined at the datacenter level.

## Security Groups

A **security group** is a named, reusable set of rules. Define it once, then insert it into a rule list at any level.

Typical use: a `web-server` group containing HTTP and HTTPS rules, applied to every guest serving web traffic. Editing the group updates every place it is used.

See [Security Groups](Security-Groups.md).

## Aliases

An **alias** is a name for a single IP address or network.

Instead of repeating `10.0.5.0/24` across many rules, define an alias named `office-network` and use the name. When the network changes, you edit one alias.

See [Aliases](Aliases.md).

## IPSets

An **IPSet** is a named collection of several addresses, networks, or aliases, treated as one unit in a rule.

Typical use: a `blocklist` IPSet, or a `management-hosts` IPSet listing every workstation permitted to reach the web interface.

See [IPSets](IPSets.md).

An alias names *one* thing; an IPSet groups *many* things.

---

# Macros

A **macro** is a predefined rule template for a well-known service. Selecting a macro fills in the protocol and ports so you do not have to look them up.

Selecting the SSH macro, for example, configures the rule for TCP port 22.

When a macro is selected, the protocol and port fields are set by the macro and are not edited directly. To use a non-standard port, leave the macro unset and specify the protocol and port yourself.

> **Verify:** Capture the full macro list from the Add Rule dialog and document the
> commonly used entries.

---

# Configuration / Options

Fields available when creating a firewall rule. The same fields appear at all three levels.

| Option | Description |
|---|---|
| **Direction** | **in** or **out**, relative to the protected object. |
| **Action** | **ACCEPT**, **DROP**, or **REJECT**. |
| **Enable** | Whether the rule is active. Lets you stage a rule before turning it on, or disable one while troubleshooting without deleting it. |
| **Macro** | A predefined service template that sets protocol and ports. |
| **Protocol** | The IP protocol, such as `tcp`, `udp`, or `icmp`. Unavailable when a macro is selected. |
| **Source** | Source address, network, alias, or IPSet. Empty means any source. |
| **Destination** | Destination address, network, alias, or IPSet. Empty means any destination. |
| **Source port** | Source port or range. Rarely needed; most rules filter on destination port. |
| **Dest. port** | Destination port or range — the service being reached. |
| **Interface** | Restricts the rule to one network interface. |
| **Log level** | How matches are logged, from `nolog` through to `debug`. |
| **Comment** | A description. Use it — a rule list without comments becomes unmaintainable. |

> **Verify:** Confirm the exact field labels and the available log levels in the
> Add Rule dialog.

---

# Verification

After configuring the firewall, verify the following:

* The web interface is still reachable, from a second browser session opened **before** you enabled anything.
* Permitted services respond as expected.
* Blocked services fail as expected.
* Guests can still reach the networks they need.
* Rules appear in the intended order, with narrower rules above broader ones.
* Cluster communication between nodes is unaffected.
* No unexpected entries appear in the firewall log.

Test from a machine that is *not* your administrative workstation, so a mistake does not remove your own access.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Locked out of the web interface | Follow [Firewall Lockout Recovery](Firewall-Lockout-Recovery.md). Always keep a management-access rule in place. |
| Rules have no effect on a guest | The firewall must be enabled both on the guest's Firewall → Options panel **and** on its network device. Check the device checkbox. |
| Rules have no effect anywhere | The datacenter-level firewall is disabled. Guest and node rules do nothing until it is enabled. |
| A rule is ignored | An earlier rule matched first. Rules are evaluated top to bottom; move the specific rule above the broader one. |
| Connections hang instead of failing | The action is **DROP**. Use **REJECT** for a fast, clear failure on internal networks. |
| Cluster nodes lose contact | Cluster communication is being filtered. Permit traffic between node addresses on the cluster network. |
| A guest cannot reach DNS or updates | Outbound traffic is being blocked. Check the output policy and outbound rules. |
| A macro cannot be combined with a port | Macros set protocol and ports themselves. Clear the macro and specify protocol and port manually. |

---

# Best Practices

- **Add a management-access rule first, and verify it, before enabling the firewall.**
- Keep a second browser session open while making changes, so you can detect a lockout before closing your working session.
- Test on a non-production guest before applying rules broadly.
- Use aliases and IPSets rather than repeating raw addresses — one edit then updates every rule.
- Use security groups for any rule set applied to more than one guest.
- Comment every rule. State why it exists, not what it does.
- Order rules from most specific to most general.
- Enable logging on new rules while validating them, then reduce the log level once behaviour is confirmed.
- Document which guests are firewalled, so unexpected connectivity problems are quicker to diagnose.
- Keep the guest operating system firewall configured as well. Defence in depth.

---

# Related Documentation

- [Firewall Options](Firewall-Options.md)
- [Firewall Rules](Firewall-Rules.md)
- [Firewall Lockout Recovery](Firewall-Lockout-Recovery.md)
- [Security Groups](Security-Groups.md)
- [Aliases](Aliases.md)
- [IPSets](IPSets.md)
- [Node Firewall](../../03-Nodes/Node-Firewall.md)
- [VM Firewall](../../04-Virtual-Machines/VM-Firewall.md)
- [Container Firewall](../../05-Containers/CT-Firewall.md)
- [Network Overview](../../03-Nodes/System/Network/Network-Overview.md)
- [Network Troubleshooting](../../03-Nodes/System/Network/Network-Troubleshooting.md)

---

# Summary

The VM2Cloud firewall filters traffic at three independent levels — datacenter, node, and guest — and a packet must be permitted at every applicable level to pass. Rules are direction-aware, evaluated top to bottom with the first match winning, and built from reusable aliases, IPSets, security groups, and macros defined at the datacenter level.

The two things that cause the most trouble are the default **DROP** input policy, which will lock you out of the web interface if you enable the firewall without a management-access rule, and the requirement to enable guest filtering in two places — on the guest and on its network device.
