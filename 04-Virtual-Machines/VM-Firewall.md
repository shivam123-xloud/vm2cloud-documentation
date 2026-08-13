# VM Firewall

---

## Overview

The **Firewall** tab on a virtual machine filters traffic to and from that machine only.

Guest-level rules apply in addition to the node and datacenter rules. Traffic must be permitted at every applicable level to reach the virtual machine.

For the rule model, direction, actions, and evaluation order, see [Firewall Overview](../02-Datacenter/Firewall/Firewall-Overview.md). This page covers the VM firewall panel only.

### Enabling requires two steps

A virtual machine is only filtered when **both** of these are true:

1. The firewall is enabled on the VM's **Firewall → Options** panel.
2. The firewall checkbox is ticked on the machine's **network device**, under **Hardware**.

This is the single most common reason VM firewall rules appear to do nothing. Rules exist, options say enabled, and no filtering happens — because the network device was never ticked.

The datacenter-level firewall must also be enabled. Guest rules do nothing without it.

---

## When to Use

Configure the VM firewall when:

* A virtual machine should only accept traffic on specific ports.
* A machine must be isolated from other guests on the same bridge.
* A public-facing machine needs its exposure restricted.
* A guest's own operating system firewall is not sufficient or not manageable.
* A group of machines sharing a role needs one consistent policy, applied with a [security group](../02-Datacenter/Firewall/Security-Groups.md).

---

## Prerequisites

Before configuring the VM firewall, ensure that:

* You have administrator privileges, or permissions on the virtual machine.
* The datacenter-level firewall is enabled.
* You know which ports and sources the machine must accept.
* You have console access to the machine, in case a rule blocks its network access.
* Any alias, IPSet, or security group the rules reference already exists.

---

# Procedure

## Step 1: Open the VM Firewall Panel

1. Log in to the VM2Cloud VE web interface.
2. Expand the node in the resource tree.
3. Select the virtual machine.
4. Expand **Firewall**.
5. Click **Rules**.

---

### Screenshot 1

**VM Firewall Rules Panel**

```text
[ Place Screenshot Here ]
```

> **Capture:** A virtual machine → Firewall → Rules, showing the empty or populated rule
> list with the **Add**, **Edit**, **Remove**, and **Insert: Security Group** controls.

---

## Step 2: Add the Required Rules

1. Click **Add**.
2. Set **Direction** to **in** for traffic arriving at the machine.
3. Set **Action** to **ACCEPT**.
4. Select a **Macro** for a standard service, or set **Protocol** and **Dest. port** manually.
5. Set **Source** to restrict who may connect. Leave empty only if the service should genuinely accept any source.
6. Add a **Comment** explaining the rule.
7. Click **Add**.

Repeat for each service the machine must expose.

---

### Screenshot 2

**Adding a VM Firewall Rule**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Add Rule dialog on a virtual machine, filled in for an inbound rule
> permitting a specific service port from a specific source.

---

## Step 3: Insert a Security Group (Optional)

If the machine shares a role with others, apply the shared policy rather than retyping it.

1. Click **Insert: Security Group**.
2. Select the group.
3. Confirm.

---

## Step 4: Enable the Firewall on the Network Device

This is the step that is most often missed.

1. Select the virtual machine.
2. Click **Hardware**.
3. Select the network device.
4. Click **Edit**.
5. Tick the firewall checkbox.
6. Click **OK**.

Repeat for every network device that should be filtered.

> **Verify:** Confirm the exact label of the firewall checkbox in the VM network device
> edit dialog.

---

### Screenshot 3

**Firewall Enabled on the Network Device**

```text
[ Place Screenshot Here ]
```

> **Capture:** VM → Hardware → Network Device edit dialog, showing the firewall checkbox
> ticked.

---

## Step 5: Enable the Firewall on the VM

1. Click **Options** under the machine's **Firewall**.
2. Select the firewall enable setting.
3. Click **Edit**.
4. Enable it.
5. Click **OK**.

---

### Screenshot 4

**VM Firewall Options**

```text
[ Place Screenshot Here ]
```

> **Capture:** A virtual machine → Firewall → Options, showing the full settings list
> including the enable setting and default policies.

---

## Step 6: Verify From Outside

1. From another machine, connect to a service the rules permit. It should succeed.
2. Connect to a port the rules do not permit. It should fail.
3. Confirm the machine can still reach the services it needs outbound.
4. Confirm the guest console still works — console access is independent of guest networking, so it remains available even if a rule blocks the network.

---

# Configuration / Options

The VM **Firewall → Options** panel controls filtering for this machine.

| Option | Description |
|---|---|
| **Firewall** | Enables filtering for this machine. Requires the network device checkbox as well. |
| **Input Policy** | Action for inbound traffic no rule matches. Normally **DROP**. |
| **Output Policy** | Action for outbound traffic no rule matches. Normally **ACCEPT**. |
| **DHCP** | Permits DHCP so the guest can obtain an address. Required if the machine uses DHCP. |
| **NDP** | Neighbor Discovery Protocol, used by IPv6. |
| **Router Advertisement** | Controls whether the machine may send router advertisements. |
| **MAC filter** | Restricts the machine to its configured MAC address, preventing spoofing. |
| **IP filter** | Restricts the machine to its configured addresses. |
| **Log level (in)** | Logging detail for inbound traffic. |
| **Log level (out)** | Logging detail for outbound traffic. |

Rule fields are identical to the datacenter level — see [Firewall Rules](../02-Datacenter/Firewall/Firewall-Rules.md).

> **Verify:** Capture the complete VM Firewall → Options list and confirm the exact
> setting names, defaults, and which of the above are present in this deployment.

---

# Verification

Verify the following:

* The firewall is enabled on the VM's Options panel.
* The firewall checkbox is ticked on every relevant network device.
* Permitted services accept connections from permitted sources.
* Connections from other sources are refused.
* Ports with no rule are not reachable.
* The machine can still reach DNS, updates, and any outbound services it needs.
* The machine obtains an address if it uses DHCP.
* The console still works.
* Matching traffic appears in the firewall log if logging is enabled.

Test from a machine on a different network from your administrative workstation where possible.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Rules have no effect | The firewall checkbox on the network device is not ticked. This is the most common cause. |
| Rules still have no effect | The datacenter-level firewall is disabled, or the VM Options enable setting is off. All three are required. |
| Machine loses all network access | The input policy is **DROP** and no rule permits the required traffic. Use the console to investigate. |
| Machine cannot get an IP address | DHCP is being blocked. Enable the DHCP option. |
| IPv6 stops working | NDP is being blocked. Enable the NDP option. |
| Outbound traffic fails | The output policy was set to DROP without matching outbound rules. |
| A rule is ignored | An earlier rule matched first. Reorder so narrower rules sit above broader ones. |
| Rules lost after cloning | Firewall configuration is per machine. Check the clone's own firewall settings. |
| Connections hang rather than fail | The action is **DROP**. Use **REJECT** on internal networks. |

---

# Best Practices

- Tick the network device checkbox and enable the Options setting together, so filtering is never half-configured.
- Restrict inbound rules by source rather than accepting from anywhere.
- Use a [security group](../02-Datacenter/Firewall/Security-Groups.md) for any policy shared by more than one machine.
- Keep the guest operating system firewall configured as well. Defence in depth.
- Enable **MAC filter** and **IP filter** for guests you do not fully control.
- Verify console access works before enabling, so a mistake is recoverable.
- Test on a non-production machine first.
- Comment every rule with why it exists.
- Review firewall settings after cloning or migrating a machine.

---

# Related Documentation

- [Firewall Overview](../02-Datacenter/Firewall/Firewall-Overview.md)
- [Firewall Rules](../02-Datacenter/Firewall/Firewall-Rules.md)
- [Security Groups](../02-Datacenter/Firewall/Security-Groups.md)
- [Aliases](../02-Datacenter/Firewall/Aliases.md)
- [IPSets](../02-Datacenter/Firewall/IPSets.md)
- [Manage VM Hardware](Manage-VM-Hardware.md)
- [VM Console](VM-Console.md)
- [VM Troubleshooting](VM-Troubleshooting.md)
- [Container Firewall](../05-Containers/CT-Firewall.md)

---

# Summary

The VM firewall filters traffic for one virtual machine, on top of the node and datacenter rules. Enabling it takes two steps that are easy to get half right: the firewall must be enabled on the machine's Firewall → Options panel **and** the firewall checkbox must be ticked on its network device under Hardware. Rules that appear configured but have no effect are almost always missing that second step. Restrict inbound rules by source, use security groups for shared policy, and verify console access before enabling so a mistake stays recoverable.
