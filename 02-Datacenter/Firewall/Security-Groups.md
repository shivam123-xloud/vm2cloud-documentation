# Security Groups

---

## Overview

A **security group** is a named, reusable set of firewall rules. You define the rules once, then insert the group into a rule list at the datacenter, node, or guest level.

Where an [alias](Aliases.md) reuses an *address* and an [IPSet](IPSets.md) reuses a *group of addresses*, a security group reuses *whole rules*.

The practical value is consistency. A `web-server` group containing the HTTP and HTTPS rules can be applied to every guest serving web traffic. When the policy changes — adding a rate limit, restricting a source — you edit the group once and every guest using it is updated.

Security groups are defined at the datacenter level and can be inserted at any level.

For the firewall model as a whole, see [Firewall Overview](Firewall-Overview.md).

---

## When to Use

Create a security group when:

* The same set of rules is needed on more than one guest or node.
* Guests fall into recognisable roles — web servers, database servers, jump hosts.
* A policy should be changed in one place and applied everywhere.
* You are repeatedly copying the same two or three rules onto new guests.

If a rule set only ever applies to one object, write the rules directly on that object instead.

---

## Prerequisites

Before creating a security group, ensure that:

* You have administrator privileges.
* You know the rules the group should contain.
* Any alias or IPSet the rules reference already exists.
* You have chosen a clear name describing the role.
* The cluster has quorum.

---

# Procedure

## Step 1: Open the Security Group Panel

1. Log in to the VM2Cloud VE web interface.
2. Select **Datacenter** in the resource tree.
3. Expand **Firewall**.
4. Click **Security Group**.

The existing groups are listed alongside the rules belonging to the selected group.

---

### Screenshot 1

**Security Group Panel**

```text
[ Place Screenshot Here ]
```

> **Capture:** Datacenter → Firewall → Security Group, showing the group list and the
> rule list for a selected group, with the **Add**, **Edit**, and **Remove** buttons
> visible.

---

## Step 2: Create the Group

1. Click **Add** in the group list.
2. In **Name**, enter a descriptive identifier such as `web-server`.
3. In **Comment**, describe what the group is for and which guests should use it.
4. Click **Add**.

The group is created empty. It has no effect until rules are added and the group is inserted somewhere.

---

### Screenshot 2

**Create Security Group Dialog**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Create Security Group dialog, showing the Name and Comment fields.

---

## Step 3: Add Rules to the Group

1. Select the new group in the group list.
2. Click **Add** in the rule list.
3. Configure the rule exactly as you would a normal firewall rule:
   - Set **Direction** to **in** or **out**.
   - Set **Action** to **ACCEPT**, **DROP**, or **REJECT**.
   - Select a **Macro**, or set **Protocol** and **Dest. port** manually.
   - Set **Source** and **Destination** as required.
   - Add a **Comment**.
4. Click **Add**.
5. Repeat for each rule the group should contain.

The field reference is identical to a standalone rule — see [Firewall Rules](Firewall-Rules.md).

---

### Screenshot 3

**Adding a Rule to a Security Group**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Add Rule dialog opened from within a security group, filled in for an
> inbound rule.

---

### Screenshot 4

**Security Group With Rules**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Security Group panel with a group selected and two or more rules
> listed beneath it.

---

## Step 4: Order the Rules Within the Group

Rules inside a group are evaluated top to bottom, the same as anywhere else.

1. Select a rule.
2. Use the move controls to reposition it.
3. Place narrower rules above broader ones.

---

## Step 5: Insert the Group Into a Rule List

A security group does nothing until it is inserted somewhere.

1. Open the rule list where the group should apply — Datacenter → Firewall → Rules, the node's Firewall panel, or a guest's Firewall panel.
2. Click **Insert: Security Group**.
3. Select the group.
4. Confirm.

The group's rules now apply at that point in the list.

> **Verify:** Confirm the exact label of the control used to insert a security group
> into a rule list, and whether it appears as a separate button or as an option under
> **Add**.

---

### Screenshot 5

**Inserting a Security Group**

```text
[ Place Screenshot Here ]
```

> **Capture:** The dialog used to insert a security group into a rule list, showing the
> group selection field.

---

### Screenshot 6

**Group Applied to a Guest**

```text
[ Place Screenshot Here ]
```

> **Capture:** A guest's Firewall → Rules panel showing an inserted security group in
> the rule list.

---

## Step 6: Edit or Remove a Group

**To change the rules:**

1. Select the group.
2. Add, edit, or remove rules in its rule list.

Changes apply immediately everywhere the group is inserted.

> **Warning:** Editing a security group changes the firewall behaviour of every object it is inserted into, at once. Confirm where a group is used before changing it — a group applied to twenty guests will change all twenty.

**To remove a group:**

1. Remove the group from every rule list where it is inserted.
2. Select the group.
3. Click **Remove** and confirm.

---

# Configuration / Options

### Security group

| Option | Description |
|---|---|
| **Name** | Identifier used when inserting the group. Must be unique. |
| **Comment** | Description of the group's purpose. |

### Rules within a group

Rules inside a group use the same fields as standalone rules: **Direction**, **Action**, **Enable**, **Macro**, **Protocol**, **Source**, **Destination**, **Source port**, **Dest. port**, **Interface**, **Log level**, and **Comment**.

See [Firewall Rules](Firewall-Rules.md) for the full field reference.

> **Verify:** Confirm the exact field labels in the Create Security Group dialog.

---

# Verification

Verify the following:

* The group appears in the Security Group list.
* The intended rules are present and in the correct order.
* The group appears in the rule list of every object it was inserted into.
* Traffic permitted by the group is accepted on those objects.
* Traffic not covered by the group is handled by the default policy.
* Editing a rule in the group changes behaviour on every object using it.
* Objects that do not use the group are unaffected.

Test on one guest before inserting the group widely.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Group has no effect | The group was created but never inserted into a rule list. Creating it is not enough. |
| Group has no effect on a guest | The firewall must also be enabled on the guest and on its network device. See [Firewall Overview](Firewall-Overview.md). |
| Only some rules in the group apply | An earlier rule in the group, or in the surrounding list, matched first. Check ordering. |
| Editing the group broke other guests | The group was inserted more widely than expected. Check where a group is used before editing it. |
| Cannot remove the group | It is still inserted somewhere. Remove those insertions first. |
| Group rules conflict with local rules | The list is evaluated in order. Position the inserted group relative to local rules deliberately. |
| Duplicate name rejected | Group names must be unique. |

---

# Best Practices

- Name groups after the **role** they describe, not the guest they were first used on.
- Keep each group focused on one role. A guest can use several groups.
- Comment the group with which guests are expected to use it.
- Track where each group is inserted, so the impact of an edit is known before you make it.
- Order rules inside a group most specific first.
- Reference aliases and IPSets inside group rules rather than raw addresses.
- Test a group on one guest before applying it broadly.
- Review groups periodically and remove ones no longer inserted anywhere.
- Prefer a group over copying rules whenever the same rules would appear on a second object.

---

# Related Documentation

- [Firewall Overview](Firewall-Overview.md)
- [Firewall Rules](Firewall-Rules.md)
- [Firewall Options](Firewall-Options.md)
- [Aliases](Aliases.md)
- [IPSets](IPSets.md)
- [Node Firewall](../../03-Nodes/Node-Firewall.md)
- [VM Firewall](../../04-Virtual-Machines/VM-Firewall.md)
- [Container Firewall](../../05-Containers/CT-Firewall.md)

---

# Summary

A security group is a named set of firewall rules defined once at the datacenter level and inserted wherever it is needed. It keeps policy consistent across guests that share a role and turns a policy change into a single edit rather than a sweep through every rule list. A group has no effect until it is inserted somewhere, and because an edit propagates instantly to every object using it, always confirm where a group is applied before changing it.
