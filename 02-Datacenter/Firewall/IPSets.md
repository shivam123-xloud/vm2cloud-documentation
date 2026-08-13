# IPSets

---

## Overview

An **IPSet** is a named collection of several IP addresses, networks, or aliases, treated as a single unit inside a firewall rule.

Where an [alias](Aliases.md) names one address, an IPSet groups many. One rule referencing an IPSet does the work of one rule per member, and adding a member later updates every rule that references the set.

Typical uses:

* A `management-hosts` set listing every workstation permitted to reach the web interface.
* A `blocklist` set of addresses to deny.
* A `web-servers` set grouping all guests serving public traffic.

IPSets are defined at the datacenter level and can be referenced by rules at any level.

For the firewall model as a whole, see [Firewall Overview](Firewall-Overview.md).

---

## When to Use

Create an IPSet when:

* Several addresses need the same treatment in a rule.
* Membership changes over time and you do not want to edit rules each time.
* You would otherwise write near-identical rules differing only by address.
* You need a blocklist or allowlist maintained in one place.

Use an alias instead when there is only one address, and a [security group](Security-Groups.md) when you need to reuse whole *rules* rather than a set of addresses.

---

## Prerequisites

Before creating an IPSet, ensure that:

* You have administrator privileges.
* You know the addresses or networks to include.
* Any aliases you plan to add already exist.
* You have chosen a clear, descriptive name.
* The cluster has quorum.

---

# Procedure

## Step 1: Open the IPSet Panel

1. Log in to the VM2Cloud VE web interface.
2. Select **Datacenter** in the resource tree.
3. Expand **Firewall**.
4. Click **IPSet**.

The existing IPSets are listed.

---

### Screenshot 1

**IPSet Panel**

```text
[ Place Screenshot Here ]
```

> **Capture:** Datacenter → Firewall → IPSet, showing the IPSet list on one side and the
> member list for a selected set on the other, with the **Add**, **Edit**, and
> **Remove** buttons visible.

---

## Step 2: Create the IPSet

1. Click **Add** in the IPSet list.
2. In **Name**, enter a descriptive identifier such as `management-hosts`.
3. In **Comment**, describe the purpose of the set.
4. Click **Add**.

The set is created empty. It has no effect until members are added.

---

### Screenshot 2

**Create IPSet Dialog**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Create IPSet dialog, showing the Name and Comment fields.

---

## Step 3: Add Members

1. Select the new IPSet in the list.
2. Click **Add** in the member list.
3. Enter the address, network, or alias:
   - A single host as `192.168.1.50`.
   - A network as `10.0.5.0/24`.
   - An existing alias by name.
4. Add a comment identifying the member.
5. Click **Add**.
6. Repeat for each member.

---

### Screenshot 3

**Adding a Member to an IPSet**

```text
[ Place Screenshot Here ]
```

> **Capture:** The dialog for adding an entry to an IPSet, showing the address field and
> comment field.

---

### Screenshot 4

**IPSet With Members**

```text
[ Place Screenshot Here ]
```

> **Capture:** Datacenter → Firewall → IPSet with a set selected and several members
> listed.

---

## Step 4: Use the IPSet in a Rule

1. Open **Datacenter → Firewall → Rules**.
2. Click **Add**, or edit an existing rule.
3. In **Source** or **Destination**, reference the IPSet.
4. Complete the rule and click **Add**.

The rule now matches traffic involving any member of the set.

> **Verify:** Confirm how an IPSet is referenced in the Source and Destination fields —
> whether it is selected from a list or entered with a prefix such as `+setname`.

---

### Screenshot 5

**IPSet Referenced in a Rule**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Add Rule dialog with an IPSet selected in the **Source** field.

---

## Step 5: Maintain the Set

**To add a member later:**

1. Select the IPSet.
2. Click **Add** in the member list.
3. Enter the address and confirm.

Every rule referencing the set immediately includes the new member. No rule is edited.

**To remove a member:**

1. Select the IPSet.
2. Select the member.
3. Click **Remove** and confirm.

**To delete the whole set:**

1. Select the IPSet.
2. Click **Remove** and confirm.

> **Warning:** Adding or removing a member changes the behaviour of every rule referencing the set, immediately. Removing a member from a `management-hosts` set can remove someone's access to the web interface.

---

# Configuration / Options

### IPSet

| Option | Description |
|---|---|
| **Name** | Identifier used to reference the set in rules. Must be unique. |
| **Comment** | Description of the set's purpose. |

### Member entry

| Option | Description |
|---|---|
| **IP/CIDR** | A single address, a network in CIDR notation, or the name of an existing alias. |
| **Comment** | Description identifying this member. |
| **nomatch** | Excludes an address or range that would otherwise be covered by a broader member of the same set. |

The `nomatch` option lets you express "this whole network except these hosts" — add the network as one member, then add the exceptions with `nomatch` set.

> **Verify:** Confirm the exact field labels in the IPSet dialogs and the behaviour and
> label of the `nomatch` option.

---

# Verification

Verify the following:

* The IPSet appears in the list with the intended members.
* Each member shows the correct address or network.
* The set can be referenced in a rule's Source and Destination fields.
* Traffic from every member is matched by rules using the set.
* Traffic from addresses outside the set is not matched.
* Any `nomatch` entries are correctly excluded.
* Adding a member takes effect without editing the rule.

Test with at least two members before relying on the set.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| IPSet has no effect | The set is empty. An IPSet with no members matches nothing. |
| Only some members are matched | Check each member's address and prefix, and check whether a `nomatch` entry is excluding them. |
| IPSet not available in a rule | Confirm it was created at datacenter level and referenced with the correct syntax. |
| Cannot delete the set | It is still referenced by a rule. Update those rules first. |
| Someone lost access unexpectedly | A member was removed from a set used by a management-access rule. Check the set's membership. |
| An alias member is not resolving | The alias must exist before being added. Create the alias first. |
| Set matches an unexpectedly wide range | A member's CIDR prefix is broader than intended. |

---

# Best Practices

- Name IPSets by purpose — `management-hosts`, `blocklist`, `web-servers`.
- Comment every member so the list stays auditable as people change.
- Prefer one IPSet over several near-identical rules.
- Use aliases as members when the underlying addresses may change.
- Review membership of access-granting sets regularly, and remove entries when staff or systems change.
- Use `nomatch` for exceptions rather than restructuring a whole rule set.
- Check which rules reference a set before changing its membership.
- Keep blocklists and allowlists in separate, clearly named sets.

---

# Related Documentation

- [Firewall Overview](Firewall-Overview.md)
- [Firewall Rules](Firewall-Rules.md)
- [Aliases](Aliases.md)
- [Security Groups](Security-Groups.md)
- [Firewall Options](Firewall-Options.md)

---

# Summary

An IPSet groups several addresses, networks, or aliases under one name so a single firewall rule can act on all of them. It replaces repetitive near-identical rules and makes membership changes a one-place edit that every referencing rule picks up immediately. Use aliases for single addresses, IPSets for groups, and security groups when it is whole rules you need to reuse. Because membership changes take effect instantly, treat any set used for management access with particular care.
