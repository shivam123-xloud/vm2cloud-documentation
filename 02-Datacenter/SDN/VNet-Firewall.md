# VNet Firewall

---

## Overview

The **VNet Firewall** filters traffic at the level of a virtual network, rather than at a node or an individual guest.

This is a fourth filtering level, distinct from the three described in [Firewall Overview](../Firewall/Firewall-Overview.md):

| Level | Protects |
|---|---|
| Datacenter | The whole cluster |
| Node | One server |
| Guest | One VM or container |
| **VNet** | **Everything attached to one virtual network** |

Its value is isolation *within* a network. Guests on the same VNet can normally reach each other freely — that is what being on the same network means. A VNet firewall lets you restrict that without configuring every guest individually, and it keeps applying as guests are added.

A common pattern: default **DROP**, then permit only traffic between the subnet and its gateway. Guests can reach out through the gateway, and cannot reach each other.

---

## When to Use

Use the VNet firewall when:

* Guests on the same VNet should not talk to each other.
* A tenant network needs isolation enforced centrally, not per guest.
* A rule set should apply automatically to guests added later.
* East-west traffic inside a virtual network must be controlled.

Use the [guest firewall](../../04-Virtual-Machines/VM-Firewall.md) instead for rules specific to one guest, and the [datacenter firewall](../Firewall/Firewall-Rules.md) for cluster-wide policy.

---

## Prerequisites

* SDN is configured with at least one [zone](Zones.md) and [VNet](VNets.md).
* The datacenter-level firewall is enabled — see [Firewall Options](../Firewall/Firewall-Options.md).
* You know which traffic must be permitted, including the gateway.
* The cluster has quorum.

> **Warning:** A default-DROP policy on a VNet cuts every guest attached to it off from everything you have not explicitly permitted — including DNS, updates, and each other. Establish the permit rules before changing the default policy.

---

# Procedure

## Step 1: Open the VNet Firewall Panel

1. Log in to the VM2Cloud VE web interface.
2. Select **Datacenter**.
3. Expand **SDN**.
4. Click **VNet Firewall**.

The panel lists VNets on the left. Selecting one opens its **Rules** and **Options** on the right.

---

### Screenshot 1

**VNet Firewall Panel**

```text
[ Place Screenshot Here ]
```

> **Capture:** Datacenter → SDN → VNet Firewall with a VNet selected, showing the VNet
> list on the left and the Rules and Options tabs populated on the right.

---

## Step 2: Select the VNet

Click the VNet in the list. Its rules and options load beside it.

Nothing is configured until a VNet is selected — an empty right-hand panel means no selection, not an absence of rules.

---

## Step 3: Review the Options

Open the **Options** tab. This is where the VNet's default policies live.

Set the defaults **after** the permit rules exist, not before.

> **Verify:** Capture the VNet Firewall Options tab and confirm which settings are
> present — at minimum the enable switch and the default input and output policies.

---

### Screenshot 2

**VNet Firewall Options**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Options tab for a selected VNet, showing the enable setting and
> default policies.

---

## Step 4: Add Rules

1. Open the **Rules** tab.
2. Click **Add**.
3. Configure direction, action, protocol, source, destination, and port.
4. Confirm.

The fields are the same as any other firewall rule — see [Firewall Rules](../Firewall/Firewall-Rules.md) for the full reference.

Rules are evaluated top to bottom, first match wins, as everywhere else.

---

### Screenshot 3

**Adding a VNet Firewall Rule**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Add Rule dialog opened from the VNet Firewall Rules tab.

---

## Step 5: Build an Isolation Policy

The pattern most people want:

1. Add a rule permitting traffic **from the subnet to the gateway**.
2. Add a rule permitting traffic **from the gateway to the subnet**.
3. Add rules for any service guests genuinely need from each other.
4. Only then set the default input policy to **DROP**.
5. **Apply**.

Guests can then reach the outside world through the gateway but not each other — which is usually what "isolate the tenant network" means in practice.

---

## Step 6: Apply

SDN configuration is staged. Return to the **SDN** panel and click **Apply**.

Nothing takes effect until this runs. See [SDN Overview](SDN-Overview.md).

---

## Step 7: Verify From Inside

1. From a guest on the VNet, reach the gateway. Should succeed.
2. From that guest, reach another guest on the same VNet. Should fail, if isolation was the intent.
3. Reach an external service. Should succeed.
4. Confirm DNS still resolves.

Test from an actual guest. The rule list looking correct is not evidence.

---

# Configuration / Options

Rule fields match the standard firewall — **Direction**, **Action**, **Protocol**, **Source**, **Destination**, **Dest. port**, **Log level**, **Comment**. See [Firewall Rules](../Firewall/Firewall-Rules.md).

VNet-level options control the default policies for the network.

> **Verify:** Capture both tabs and confirm the exact field labels and which options
> exist at VNet level.

---

# Verification

Verify the following:

* The VNet appears in the list.
* Rules are present in the intended order.
* **Apply** completed on every node with no pending changes.
* Guests reach the gateway.
* Guests cannot reach each other, if that was the intent.
* External connectivity and DNS still work.
* A guest added to the VNet afterwards inherits the same treatment.

That last check is the point of filtering here rather than per guest.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Rules have no effect | **Apply** was not clicked, or the datacenter firewall is disabled. |
| Guests lost all connectivity | Default DROP with no permit rules. Add the gateway rules, or revert the policy. |
| DNS stopped working inside guests | DNS traffic is not permitted. Add a rule for it. |
| Guests can still reach each other | No rule blocks east-west traffic, or the default policy still permits it. |
| Right-hand panel is empty | No VNet is selected. |
| A new guest is not filtered | Confirm it is attached to this VNet, and that the guest-level firewall is not overriding expectations. |
| Rules conflict with guest firewall rules | Both levels apply. Traffic must pass every applicable level. |

---

# Best Practices

- Add the permit rules **before** setting a default DROP policy. The reverse order cuts off every guest at once.
- Always permit subnet-to-gateway in both directions unless the network is deliberately fully isolated.
- Remember DNS. It is the most common thing forgotten in a default-DROP policy.
- Use VNet rules for policy that should apply to the whole network, and guest rules for exceptions.
- Test from inside a guest, not from the rule list.
- Keep console access available — a mistake here removes guest network access, not your interface access, but you will need the console to diagnose it.
- Comment every rule with why it exists.

---

# Related Documentation

- [SDN Overview](SDN-Overview.md)
- [VNets](VNets.md)
- [Zones](Zones.md)
- [Firewall Overview](../Firewall/Firewall-Overview.md)
- [Firewall Rules](../Firewall/Firewall-Rules.md)
- [Firewall Options](../Firewall/Firewall-Options.md)
- [VM Firewall](../../04-Virtual-Machines/VM-Firewall.md)
- [Container Firewall](../../05-Containers/CT-Firewall.md)
- [Firewall Lockout Recovery](../Firewall/Firewall-Lockout-Recovery.md)

---

# Summary

The VNet firewall filters traffic for an entire virtual network, adding a fourth level to the datacenter, node, and guest firewalls. It is the way to control traffic *between* guests on the same network without configuring each of them, and it keeps applying to guests added later.

The usual policy is default DROP with the subnet permitted to reach its gateway in both directions — guests get external connectivity and cannot reach each other. Build the permit rules first, remember DNS, and remember that SDN configuration is staged and does nothing until you Apply.
