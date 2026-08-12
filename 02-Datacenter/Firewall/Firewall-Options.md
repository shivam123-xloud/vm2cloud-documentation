# Firewall Options

---

## Overview

The **Options** panel at Datacenter → Firewall controls whether the firewall is active cluster-wide and what happens to traffic that no rule matches.

This is the master switch for the entire firewall. Node-level and guest-level rules do nothing until the firewall is enabled here.

For the rule model, direction, actions, and evaluation order, see [Firewall Overview](Firewall-Overview.md).

> **Warning:** Enabling the firewall applies the default input policy immediately across the cluster. That policy is normally **DROP**, and the web interface listens on TCP port **8006**. If no rule permits your management access, you will lose the web interface the moment you enable it, and will need console or physical access to recover. Configure and verify a management-access rule on [Firewall Rules](Firewall-Rules.md) *first*.

---

## When to Use

Open the Options panel when you need to:

* Enable or disable the firewall for the whole cluster.
* Change the default policy for unmatched inbound or outbound traffic.
* Adjust cluster-wide logging behaviour.
* Confirm whether the firewall is currently active before troubleshooting connectivity.

---

## Prerequisites

Before changing firewall options, ensure that:

* You have administrator privileges.
* A rule permitting your management access already exists and has been reviewed.
* You have console or physical access to at least one node.
* You know which guests and services will be affected.
* The cluster has quorum.

---

# Procedure

## Step 1: Open the Firewall Options Panel

1. Log in to the VM2Cloud web interface.
2. Select **Datacenter** in the resource tree.
3. Expand **Firewall**.
4. Click **Options**.

The panel lists the cluster-wide firewall settings and their current values.

---

### Screenshot 1

**Datacenter Firewall Options**

```text
[ Place Screenshot Here ]
```

> **Capture:** Datacenter → Firewall → Options, showing the full settings list with the
> current values and the **Edit** button visible.

---

## Step 2: Review the Current Settings

Before changing anything, note the present values, so you can restore them if connectivity breaks.

Record:

* Whether the firewall is enabled.
* The default input policy.
* The default output policy.
* The current log level settings.

---

## Step 3: Verify a Management-Access Rule Exists

Do not skip this step.

1. Click **Rules** under **Firewall**.
2. Confirm a rule permits inbound access to the management interface from your administrative network.
3. Confirm that rule is **enabled**.
4. Confirm it sits above any broader deny rule.

If no such rule exists, create it now using [Firewall Rules](Firewall-Rules.md) before continuing.

---

### Screenshot 2

**Management Access Rule in Place**

```text
[ Place Screenshot Here ]
```

> **Capture:** Datacenter → Firewall → Rules, showing an enabled inbound rule permitting
> management access, before the firewall is switched on.

---

## Step 4: Change a Setting

1. Select the setting to change.
2. Click **Edit**.
3. Set the new value.
4. Click **OK**.

---

### Screenshot 3

**Editing a Firewall Option**

```text
[ Place Screenshot Here ]
```

> **Capture:** The edit dialog for a single firewall option — ideally the **Firewall**
> enable setting — showing the available values.

---

## Step 5: Enable the Firewall

Only proceed once Step 3 is confirmed.

1. Open a **second browser session** to the web interface and leave it logged in. This is your safety net; if the change locks out new connections, the existing session may survive long enough to undo it.
2. Select the **Firewall** setting.
3. Click **Edit**.
4. Set it to enabled.
5. Click **OK**.

---

### Screenshot 4

**Firewall Enabled**

```text
[ Place Screenshot Here ]
```

> **Capture:** Datacenter → Firewall → Options immediately after enabling, showing the
> firewall setting in its enabled state.

---

## Step 6: Verify Access Immediately

1. From a **different** machine, load the web interface.
2. Confirm it responds.
3. Confirm your guests still reach the services they need.

If access is lost, follow [Firewall Lockout Recovery](Firewall-Lockout-Recovery.md).

---

# Configuration / Options

| Option | Description |
|---|---|
| **Firewall** | The cluster-wide master switch. When disabled, no rules at any level take effect. |
| **Input Policy** | Action applied to inbound traffic no rule matches. Normally **DROP**. |
| **Output Policy** | Action applied to outbound traffic no rule matches. Normally **ACCEPT**. |
| **Log level (in)** | Logging detail for inbound traffic. |
| **Log level (out)** | Logging detail for outbound traffic. |

> **Verify:** Capture the complete Options list from this deployment and confirm the
> exact setting names, their default values, and the available log levels. The list
> above covers the settings common to the platform but may not be exhaustive.

---

# Verification

Verify the following:

* The Options panel shows the firewall in the intended state.
* The web interface is reachable from a machine other than the one you changed it from.
* Cluster nodes remain online and communicating.
* Guests reach the networks and services they require.
* Permitted services respond; blocked services fail as intended.
* The firewall log records traffic as expected.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Web interface unreachable after enabling | Follow [Firewall Lockout Recovery](Firewall-Lockout-Recovery.md), then add a management-access rule before retrying. |
| Rules configured but nothing is filtered | The master **Firewall** setting is disabled. Nothing at any level applies until it is enabled here. |
| Guest rules still have no effect | The firewall must also be enabled on the guest and on the guest's network device. See [Firewall Overview](Firewall-Overview.md). |
| Cluster nodes lose contact after enabling | Cluster communication is being dropped. Permit traffic between node addresses on the cluster network. |
| Outbound guest traffic breaks | The output policy was changed to DROP without matching outbound rules. Restore **ACCEPT** or add the required rules. |
| Cannot change a setting | Confirm you have administrator privileges and that the cluster has quorum. |

---

# Best Practices

- Create and verify management-access rules **before** enabling the firewall, never after.
- Keep a second browser session open during any change to the master switch.
- Change one setting at a time and verify between changes.
- Leave the output policy at **ACCEPT** unless you have a specific requirement and a tested set of outbound rules.
- Make firewall changes during a maintenance window.
- Record the previous values before editing, so rollback is immediate.
- Ensure at least one administrator has working console access before you begin.

---

# Related Documentation

- [Firewall Overview](Firewall-Overview.md)
- [Firewall Rules](Firewall-Rules.md)
- [Security Groups](Security-Groups.md)
- [Node Firewall](../../03-Nodes/Node-Firewall.md)
- [VM Firewall](../../04-Virtual-Machines/VM-Firewall.md)
- [Container Firewall](../../05-Containers/CT-Firewall.md)

---

# Summary

The Datacenter → Firewall → Options panel is the master switch for firewall filtering across the cluster, together with the default policies applied to traffic no rule matches. Because the default input policy is **DROP**, enabling the firewall without a verified management-access rule will remove your access to the web interface. Confirm that rule first, keep a second session open, and verify access from another machine immediately after enabling.
