# Aliases

---

## Overview

An **alias** gives a name to a single IP address or network, so firewall rules can refer to the name instead of the raw address.

Instead of writing `10.0.5.0/24` into a dozen rules, define an alias called `office-network` and use that. When the office moves to a different subnet, you change the alias once and every rule that references it follows.

Aliases are defined at the datacenter level and can be referenced by rules at any level.

An alias names **one** address or network. To group several addresses together, use an [IPSet](IPSets.md).

For the firewall model as a whole, see [Firewall Overview](Firewall-Overview.md).

---

## When to Use

Create an alias when:

* The same address or network appears in more than one rule.
* An address is likely to change and you want a single place to change it.
* A raw address would be unclear to the next administrator reading the rule.
* You want rule lists to read meaningfully — `office-network` rather than `10.0.5.0/24`.

A named rule list is far easier to audit than one full of bare addresses.

---

## Prerequisites

Before creating an alias, ensure that:

* You have administrator privileges.
* You know the exact address or network, including the CIDR prefix.
* You have chosen a clear, descriptive name.
* The cluster has quorum.

---

# Procedure

## Step 1: Open the Alias Panel

1. Log in to the VM2Cloud web interface.
2. Select **Datacenter** in the resource tree.
3. Expand **Firewall**.
4. Click **Alias**.

The existing aliases are listed.

---

### Screenshot 1

**Alias Panel**

```text
[ Place Screenshot Here ]
```

> **Capture:** Datacenter → Firewall → Alias, showing the alias list with the **Add**,
> **Edit**, and **Remove** buttons visible.

---

## Step 2: Open the Add Alias Dialog

1. Click **Add**.

The alias creation dialog opens.

---

### Screenshot 2

**Add Alias Dialog**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Add Alias dialog, empty, showing the Name, IP/CIDR, and Comment
> fields.

---

## Step 3: Configure the Alias

1. In **Name**, enter a descriptive identifier such as `office-network` or `backup-server`.
2. In **IP/CIDR**, enter the address or network:
   - A single host as `192.168.1.50`.
   - A network as `10.0.5.0/24`.
3. In **Comment**, describe what the alias represents and who owns it.

Use names that describe the *role* of the address rather than its location in the rack. `monitoring-server` survives a hardware replacement; `server-in-rack-3` does not.

---

## Step 4: Create the Alias

1. Review the values.
2. Click **Add**.

The alias appears in the list and becomes available in the Source and Destination fields of firewall rules.

---

### Screenshot 3

**Alias Created**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Alias panel showing the newly created alias with its name, address,
> and comment.

---

## Step 5: Use the Alias in a Rule

1. Open **Datacenter → Firewall → Rules**.
2. Click **Add**, or edit an existing rule.
3. In **Source** or **Destination**, enter or select the alias name.
4. Complete the rule and click **Add**.

---

### Screenshot 4

**Alias Referenced in a Rule**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Add Rule dialog with an alias name entered in the **Source** field.

---

## Step 6: Edit or Remove an Alias

**To edit:**

1. Select the alias.
2. Click **Edit**.
3. Change the address or comment.
4. Click **OK**.

The change applies to every rule referencing the alias, immediately.

> **Warning:** Editing an alias silently changes the behaviour of every rule that uses it. Check which rules reference an alias before changing its address.

**To remove:**

1. Select the alias.
2. Click **Remove**.
3. Confirm.

> **Warning:** Removing an alias that is still referenced by a rule may leave that rule invalid or cause it to stop matching as intended. Update the rules first.

---

# Configuration / Options

| Option | Description |
|---|---|
| **Name** | Identifier used to reference the alias in rules. Choose something descriptive and stable. |
| **IP/CIDR** | A single address (`192.168.1.50`) or a network in CIDR notation (`10.0.5.0/24`). |
| **Comment** | Description of what the alias represents. |

> **Verify:** Confirm the exact field labels in the Add Alias dialog and whether the
> name field enforces any character restrictions.

---

# Verification

Verify the following:

* The alias appears in the Alias list with the correct address.
* The alias can be selected or entered in a rule's Source and Destination fields.
* Rules referencing the alias behave as expected against the intended addresses.
* Traffic from addresses outside the alias is not matched.
* After editing an alias, rules using it reflect the new address.

Test a rule that uses the alias before relying on it.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Alias not available in a rule | Confirm the alias was created at datacenter level and the name is spelled exactly as defined. |
| Rule matches more traffic than expected | The CIDR prefix is broader than intended. `10.0.0.0/8` is far larger than `10.0.5.0/24`. |
| Rule matches nothing | The address or prefix is wrong, or the traffic does not originate from that network. |
| Cannot remove an alias | It is still referenced by a rule. Update or remove those rules first. |
| Changing an alias broke unrelated rules | The alias was used more widely than expected. Review all rules before editing an alias. |
| Duplicate name rejected | Alias names must be unique. Choose a different name. |

---

# Best Practices

- Name aliases by role, not by location or hardware.
- Define an alias the moment an address appears in a second rule.
- Comment every alias with what it represents and who is responsible for it.
- Check which rules reference an alias before editing it.
- Use consistent naming, for example `net-` for networks and `host-` for single addresses.
- Prefer an [IPSet](IPSets.md) when you need to group several addresses under one name.
- Review aliases periodically and remove ones no longer referenced.

---

# Related Documentation

- [Firewall Overview](Firewall-Overview.md)
- [Firewall Rules](Firewall-Rules.md)
- [IPSets](IPSets.md)
- [Security Groups](Security-Groups.md)
- [Firewall Options](Firewall-Options.md)

---

# Summary

An alias gives a single IP address or network a meaningful name that firewall rules can reference. It makes rule lists readable and means an address change is a single edit rather than a hunt through every rule. Aliases are defined once at the datacenter level and usable at every level. Because an edit immediately changes every rule that references it, always check where an alias is used before changing its address.
