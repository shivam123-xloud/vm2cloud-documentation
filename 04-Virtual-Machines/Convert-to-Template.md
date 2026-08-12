# Convert to Template

---

## Overview

Converting a virtual machine to a **template** turns it into a read-only base image used to create new machines. Instead of installing an operating system for every new machine, you build one, convert it, and clone from it.

A template cannot be started or edited. That is the point: it stays fixed, so every clone begins from the same known state.

> **Warning:** Conversion is **permanent and one-way**. There is no "convert back to VM" action. Once converted, the machine can never be started again — only cloned. If you might still need it as a running machine, clone it first and convert the clone.

For creating machines from a template afterwards, see [Clone Virtual Machine](Clone-Virtual-Machine.md).

---

## When to Use

Convert to a template when:

* You have prepared a base image others will deploy from.
* The same operating system and configuration is needed repeatedly.
* You want deployments to start from a consistent, known state.
* You are building a Cloud-Init base image. See [Cloud-Init](Cloud-Init.md).
* A golden image needs protecting from accidental modification.

Do **not** convert:

* A production machine still in use.
* A machine you may need to start again.
* A machine that has not been prepared — see the preparation steps below.

---

## Prerequisites

Before converting, ensure that:

* You have permission to modify the machine.
* The machine is **stopped**.
* The guest has been prepared, following the steps below.
* You are certain you will not need it as a running machine.
* You have a backup, if there is any doubt.

---

# Preparing the Guest First

This is the part that determines whether the template is actually usable. A template made from an unprepared machine produces clones that conflict with each other.

Before converting, boot the machine one last time and, inside the guest:

1. **Remove the SSH host keys.** Otherwise every clone presents the same host identity, and SSH clients will warn about mismatches. Most systems regenerate them on next boot once removed.
2. **Clear the machine ID**, if the guest uses one. Duplicate machine IDs cause DHCP to hand every clone the same address on some networks.
3. **Remove persistent network interface rules**, if the distribution creates them. They can bind the clone's interface to the template's MAC address.
4. **Clear logs, shell history, and temporary files.** They travel into every clone otherwise.
5. **Remove any credentials or keys** that should not be shared.
6. **Install and enable the guest agent**, so clones get IP reporting and clean shutdowns. See [VM Options](VM-Options.md).
7. **Apply updates**, so clones do not each start with a backlog.

Then shut the machine down cleanly.

> **Verify:** Confirm which preparation steps apply to the guest operating systems used
> in this deployment. The specifics differ between distributions.

---

# Procedure

## Step 1: Stop the Machine

1. Select the virtual machine.
2. Click **Shutdown** and wait for it to stop cleanly.

A template cannot be created from a running machine.

---

### Screenshot 1

**Machine Stopped**

```text
[ Place Screenshot Here ]
```

> **Capture:** A virtual machine's Summary tab showing status `stopped`, ready for
> conversion.

---

## Step 2: Convert

1. Right-click the machine in the resource tree, or open the **More** menu.
2. Select **Convert to template**.
3. Read the confirmation carefully.
4. Confirm.

---

### Screenshot 2

**Convert to Template Action**

```text
[ Place Screenshot Here ]
```

> **Capture:** The menu showing the **Convert to template** action for a stopped virtual
> machine.

---

### Screenshot 3

**Conversion Confirmation**

```text
[ Place Screenshot Here ]
```

> **Capture:** The confirmation dialog for converting to a template, showing the warning
> text presented.

---

## Step 3: Verify the Template

After conversion:

* The machine's icon changes in the resource tree to indicate a template.
* **Start** is no longer available.
* Hardware can no longer be edited.
* **Clone** is available.

---

### Screenshot 4

**Template in the Resource Tree**

```text
[ Place Screenshot Here ]
```

> **Capture:** The resource tree showing a template alongside normal virtual machines,
> so the icon difference is visible.

---

## Step 4: Create a Machine From It

1. Select the template.
2. Click **Clone**.
3. Set the new **VM ID** and **Name**.
4. Choose the clone mode:
   - **Full Clone** — an independent copy. Uses full disk space, and the template can later be deleted.
   - **Linked Clone** — shares the template's base disk. Fast, space-efficient, but the template can never be deleted while linked clones exist.
5. Confirm.

See [Clone Virtual Machine](Clone-Virtual-Machine.md) for the full workflow.

> **Warning:** A **linked clone** depends permanently on its template. The template cannot be deleted while any linked clone exists, and losing the template's storage loses every linked clone with it. Use full clones for anything long-lived or important.

---

### Screenshot 5

**Cloning From a Template**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Clone dialog opened from a template, showing the Mode selector with
> Full Clone and Linked Clone.

---

# Configuration / Options

Conversion itself has no options — it is a single confirmed action. The choices that matter are made before and after.

| Decision | Where | Consideration |
|---|---|---|
| Guest preparation | Inside the guest, before conversion | Determines whether clones conflict with one another. |
| Cloud-Init drive | [Cloud-Init](Cloud-Init.md), before conversion | Lets each clone configure itself on first boot. |
| Full vs linked clone | At clone time | Full is independent; linked is fast but permanently tied to the template. |

---

# Verification

Verify the following:

* The machine shows as a template in the resource tree.
* **Start** is unavailable.
* **Clone** is available.
* A test clone creates successfully.
* The clone boots.
* The clone has its own SSH host keys, not the template's.
* The clone obtains its own IP address rather than colliding with another.
* The clone's hostname is correct.

Always create one test clone and boot it before treating the template as ready. Preparation problems only show up in the clone.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Convert action unavailable | The machine is running. Stop it first. |
| Need the machine running again | Not possible. Clone the template and start the clone. |
| Clones share the same SSH host key | Host keys were not removed before conversion. Fix inside each clone, or rebuild the template. |
| Clones all get the same IP from DHCP | Duplicate machine IDs. Clear the machine ID before conversion. |
| Clone keeps the template's hostname | Set it per clone, or use [Cloud-Init](Cloud-Init.md). |
| Cannot delete the template | Linked clones depend on it. Convert them to full clones or remove them first. |
| Clone has no network | A persistent interface rule may bind it to the template's MAC. Remove those rules before converting. |
| Template appears out of date | Templates are read-only. Clone it, update the clone, and convert that into a new template. |

---

# Best Practices

- **Prepare the guest properly before converting.** Unprepared templates produce clones that conflict, and the problem surfaces only after several are deployed.
- Clone first and convert the clone, if there is any chance you will need the original running.
- Attach and configure a Cloud-Init drive before conversion, so clones self-configure. See [Cloud-Init](Cloud-Init.md).
- Prefer **full clones** for anything long-lived. Linked clones create a dependency that outlives most people's memory of it.
- Create and boot one test clone before publishing a template for others.
- Name templates so their contents and date are obvious.
- Rebuild templates periodically rather than letting clones inherit a growing update backlog.
- Record what is inside each template — base OS, installed packages, preparation performed.

---

# Related Documentation

- [Clone Virtual Machine](Clone-Virtual-Machine.md)
- [Create Virtual Machine](Create-Virtual-Machine.md)
- [Cloud-Init](Cloud-Init.md)
- [VM Options](VM-Options.md)
- [Manage VM Hardware](Manage-VM-Hardware.md)
- [Delete Virtual Machine](Delete-Virtual-Machine.md)
- [Manage Container Templates](../05-Containers/Manage-Container-Templates.md)

---

# Summary

Converting a virtual machine to a template turns it into a read-only base image for cloning. The conversion is permanent — a template can never be started again — so clone first and convert the clone if there is any doubt.

The work that determines whether a template is useful happens **before** conversion, inside the guest: removing SSH host keys, clearing the machine ID and persistent interface rules, and cleaning logs and credentials. Skip that and the clones will conflict with each other in ways that only appear once several are running. Build one test clone and boot it before trusting a template.
