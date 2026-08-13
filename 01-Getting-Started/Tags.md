# Tags

---

## Overview

**Tags** are short labels applied to virtual machines and containers. A guest can carry several, and they appear beside its name in the header and throughout the interface.

Their value is cross-cutting grouping. A guest belongs to exactly one [pool](../02-Datacenter/Permissions/Pools.md), which usually reflects ownership — but the same guest is also production, also a database, also owned by finance, also due for decommissioning. Tags express all of that at once.

They also drive [Tag View](Resource-Tree-and-Views.md) in the resource tree, which groups the environment by label rather than by node.

Tags are labels only. They grant no permissions and change no behaviour — for grouping that controls access, use [pools](../02-Datacenter/Permissions/Pools.md).

---

## When to Use

Use tags to mark:

* Environment — `production`, `staging`, `development`.
* Role — `database`, `web`, `cache`.
* Owner — `team-finance`, `customer-acme`.
* Lifecycle — `decommission-2027`, `temporary`.
* Anything you would otherwise track in a spreadsheet.

The lifecycle case is the one people underuse. A `temporary` tag applied at creation is the difference between a guest that gets cleaned up and one that runs for three years because nobody remembers what it was for.

---

## Prerequisites

* You have permission to modify the guest.
* You have agreed a naming convention. See Best Practices.

---

# Procedure

## Step 1: Open a Guest

1. Log in to the VM2Cloud VE web interface.
2. Select the virtual machine or container.

Existing tags appear in the header beside the guest name. A guest with none shows an empty tag control.

---

### Screenshot 1

**Tags in the Guest Header**

```text
[ Place Screenshot Here ]
```

> **Capture:** A guest's header showing the tag control — ideally one guest with tags
> applied and the `No Tags` state visible elsewhere for comparison.

---

## Step 2: Add a Tag

1. Click the tag control in the header.
2. Enter the tag, or select an existing one.
3. Confirm.

Existing tags are offered as you type. Take the suggestion rather than typing a near-match — `prod` and `production` are two different tags, and splitting a group in half defeats the purpose.

---

### Screenshot 2

**Adding a Tag**

```text
[ Place Screenshot Here ]
```

> **Capture:** The tag editing control open on a guest, showing existing tags being
> suggested.

---

## Step 3: Add Several Tags

Repeat for each label. A typical production database might carry:

```text
production   database   team-finance
```

Each answers a different question, and Tag View lets you group by any of them.

---

## Step 4: Remove a Tag

1. Click the tag control.
2. Remove the tag.
3. Confirm.

Removing a tag affects nothing but grouping. No permissions change and no behaviour is altered.

---

## Step 5: Browse by Tag

1. Open the view selector above the resource tree.
2. Select **Tag View**.

Guests are grouped by tag. See [Resource Tree and Views](Resource-Tree-and-Views.md).

---

### Screenshot 3

**Tag View**

```text
[ Place Screenshot Here ]
```

> **Capture:** The resource tree in Tag View, showing guests grouped under several tags.

---

## Step 6: Configure Tag Appearance (Optional)

Tag colours and behaviour can be configured cluster-wide in [Datacenter Options](../02-Datacenter/Options.md).

Colour is more useful than it sounds. Making `production` red and `development` grey means the distinction registers before you have read anything — which is exactly when it matters, right before you click something destructive.

> **Verify:** Confirm which tag settings are available in Datacenter → Options in this
> deployment — colour assignment, ordering, and who may edit tags — and capture them.

---

# Configuration / Options

| Setting | Where | Description |
|---|---|---|
| **Tags on a guest** | Guest header | The labels applied to that guest. |
| **Tag colours** | [Datacenter Options](../02-Datacenter/Options.md) | Colour per tag, cluster-wide. |
| **Tag permissions** | [Datacenter Options](../02-Datacenter/Options.md) | Who may add or change tags. |

> **Verify:** Capture the tag-related settings in Datacenter → Options and confirm the
> exact labels.

---

# Verification

Verify the following:

* Tags appear in the guest header.
* They persist after a page reload.
* Tag View groups guests correctly.
* Spelling is consistent — no `prod` alongside `production`.
* Colours render, if configured.
* Tags survive migration between nodes.
* Guests that should be tagged are.

That fourth check is worth doing periodically. Tag lists drift, and a split group is worse than no group because it looks complete.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Tag View is empty | No tags applied yet. |
| A guest missing from a tag group | Not tagged, or tagged with a spelling variant. |
| Two similar tags exist | `prod` and `production` are separate. Standardise and re-tag. |
| Cannot add a tag | Confirm you have permission to modify the guest, and check any tag permission setting. |
| Tags not visible | Confirm a guest is selected; tags appear in the guest header. |
| Colours not applied | Configure them in [Datacenter Options](../02-Datacenter/Options.md). |
| Expected tags to control access | They do not. Tags are labels only. Use [pools](../02-Datacenter/Permissions/Pools.md). |
| Tags lost after restore | Tags are part of guest configuration. Re-apply after restoring to a new ID. |

---

# Best Practices

- **Agree a convention before tagging anything.** Retro-fitting consistency across two hundred guests is far more work than deciding up front.
- Use lowercase and hyphens consistently — `team-finance`, not `Team Finance`.
- Prefer suggestions over typing, to avoid near-duplicates.
- Tag at creation, not later. Tagging is a five-second habit and a large retrospective project.
- Use tags for cross-cutting labels, pools for grouping that controls permissions.
- Colour `production` distinctly. It is a cheap safeguard against acting on the wrong guest.
- Use lifecycle tags such as `temporary` or `decommission-2027`, and review them.
- Audit the tag list occasionally and merge variants.
- Do not expect tags to enforce anything — they describe, they do not control.

---

# Related Documentation

- [Resource Tree and Views](Resource-Tree-and-Views.md)
- [Search](Search.md)
- [Interface Tour](Interface-Tour.md)
- [Pools](../02-Datacenter/Permissions/Pools.md)
- [Datacenter Options](../02-Datacenter/Options.md)
- [VM Summary](../04-Virtual-Machines/VM-Summary.md)
- [CT Summary](../05-Containers/CT-Summary.md)

---

# Summary

Tags are short labels on guests, several per guest, used for cross-cutting grouping that pools cannot express — environment, role, owner, lifecycle. They drive Tag View in the resource tree and appear in each guest's header.

They are descriptive only: they grant no permissions and change no behaviour. Their usefulness depends entirely on consistency, so agree a convention first, take the suggested tag rather than typing a variant, and apply tags when a guest is created rather than planning to do it later.
