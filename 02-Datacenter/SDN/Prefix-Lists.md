# Prefix Lists

---

## Overview

A **prefix list** is a named, ordered list of IP prefixes used to match routes by destination.

On its own it does nothing. Its purpose is to be referenced from a [route map](Route-Maps.md), supplying the "which destinations" half of a match condition — so a single named list can be reused across many route map entries instead of repeating CIDRs.

Each entry can also match a **range** of subnet lengths rather than one exact prefix, which is what makes prefix lists more useful than a plain list of networks.

---

## When to Use

Create a prefix list when you need to:

* Filter routes by destination network.
* Reuse the same set of networks across several route map entries.
* Match a range of subnet sizes within a larger block.
* Keep route policy readable by naming groups of networks.

Prefix lists are only relevant where routing is involved — EVPN, BGP, or a [fabric](Fabrics.md). A simple or VLAN zone exchanges no routes.

---

## Prerequisites

* SDN is configured with a routing-capable zone or fabric.
* You know which destination networks the policy concerns.
* The cluster has quorum.

---

# How Matching Works

Each entry has a prefix, an action, and optionally a length range.

```text
Prefix:      10.0.0.0/8
Prefix >=    16          (ge)
Prefix <=    24          (le)

matches every subnet between /16 and /24 inside 10.0.0.0/8
```

Without `ge` or `le`, an entry matches **only that exact prefix**. `10.0.0.0/8` alone does not match `10.1.0.0/16` — it matches `10.0.0.0/8` and nothing else.

That distinction is the single most common source of confusion. If the intent is "anything inside this block", the length range is required.

## Evaluation

Entries are evaluated by sequence number, lowest first, and the first match wins. A route matching no entry is **denied** by an implicit deny at the end of every list.

---

# Procedure

## Step 1: Open the Prefix Lists Panel

1. Log in to the VM2Cloud VE web interface.
2. Select **Datacenter**.
3. Expand **SDN**.
4. Click **Prefix Lists**.

Lists appear on the left. Selecting one shows its entries on the right.

---

### Screenshot 1

**Prefix Lists Panel**

```text
[ Place Screenshot Here ]
```

> **Capture:** Datacenter → SDN → Prefix Lists with a list selected, showing the list on
> the left and its entries on the right with the Sequence Nr., Action, Prefix,
> Prefix <=, and Prefix >= columns.

---

## Step 2: Create the List

1. Click **Add** in the list panel.
2. Enter a **Name**.
3. Confirm.

The list is created empty. Name it after what it represents — `internal-nets`, `tenant-a`, `default-only` — since route map entries will reference it by name.

---

## Step 3: Add Entries

1. Select the list.
2. Click **Add** in the entries panel.
3. Set the **Sequence Nr.**, or leave it blank to have one assigned.
4. Set the **Action** — permit or deny.
5. Enter the **Prefix** in CIDR form.
6. Optionally set **Prefix >=** and **Prefix <=** to match a range of lengths.
7. Confirm.

Leaving the sequence number blank assigns the next value automatically, in steps, so entries can be inserted between existing ones later.

---

### Screenshot 2

**Add Prefix List Entry**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Add entry dialog for a prefix list, showing the Sequence Nr., Action,
> Prefix, Prefix >=, and Prefix <= fields.

---

## Step 4: Use the Length Range

| Goal | Entry |
|---|---|
| That exact network only | `10.0.0.0/8` |
| Anything inside it | `10.0.0.0/8` with `ge 9` |
| Subnets between /16 and /24 inside it | `10.0.0.0/8`, `ge 16`, `le 24` |
| A default route only | `0.0.0.0/0` |
| Absolutely everything | `0.0.0.0/0` with `ge 0`, `le 32` |

The last two are worth distinguishing. `0.0.0.0/0` on its own matches the default route and nothing else — a common mistake when the intent was "match all routes".

---

## Step 5: Reference It From a Route Map

The list does nothing until used.

1. Open [Route Maps](Route-Maps.md).
2. Add or edit an entry.
3. Set its **Match** to this prefix list.
4. Confirm.

---

## Step 6: Apply and Verify

1. Return to the **SDN** panel and click **Apply**.
2. Confirm the list shows as applied rather than pending.
3. Confirm the routes you expected to match are being handled as intended.
4. Confirm nothing else was caught by an over-broad entry.

---

# Configuration / Options

### List

| Field | Description |
|---|---|
| **Name** | Identifier referenced by route maps. |

### Entry

| Field | Description |
|---|---|
| **Sequence Nr.** | Evaluation order, lowest first. Assigned automatically if left blank. |
| **Action** | **permit** or **deny**. |
| **Prefix** | Destination network in CIDR form. `0.0.0.0/0` and `::/0` are valid. |
| **Prefix >=** (`ge`) | Minimum subnet length to match. |
| **Prefix <=** (`le`) | Maximum subnet length to match. |

> **Verify:** Capture both dialogs and confirm the exact field labels and the automatic
> sequence-number step.

---

# Verification

Verify the following:

* The list appears with the intended name.
* Entries are in the intended sequence.
* Length ranges match what you meant — exact prefix, or a range.
* **Apply** completed with nothing pending.
* Route maps referencing the list behave as expected.
* Routes you intended to match are matched.
* Routes you did not intend to match are not.

Check both directions. An entry that is too narrow silently fails to match; one that is too broad silently catches more than intended. Neither shows as an error.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Entry never matches | It has no length range, so it matches only that exact prefix. Add `ge`. |
| Entry matches too much | The length range is wider than intended. Narrow `ge` and `le`. |
| Everything is denied | Routes fell through to the implicit deny. Add a final permit entry if that was not intended. |
| Cannot insert between entries | Sequence numbers are consecutive. Renumber with gaps. |
| List has no effect | It is not referenced by any route map. |
| Route map cannot find the list | Name mismatch. They must match exactly. |
| `0.0.0.0/0` matched only one route | That is the default route. Add `ge 0 le 32` to match everything. |
| Changes have no effect | **Apply** was not clicked. |

---

# Best Practices

- **Decide whether you mean an exact prefix or a range**, and set `ge`/`le` accordingly. This is where most prefix list errors come from.
- Leave the sequence number blank so gaps are assigned automatically.
- Name lists after what they contain, not where they are used — a list may end up used in several places.
- Add an explicit final entry if unmatched routes should be permitted.
- Reuse one list across route map entries rather than repeating CIDRs.
- Keep lists short and purposeful. A list with twenty entries is usually two lists.
- Verify against the actual routing table, not the panel.
- Review lists when addressing changes — they hold network addresses that go stale.

---

# Related Documentation

- [Route Maps](Route-Maps.md)
- [SDN Overview](SDN-Overview.md)
- [Fabrics](Fabrics.md)
- [Zones](Zones.md)
- [SDN Options](SDN-Options.md)
- [Network Troubleshooting](../../03-Nodes/System/Network/Network-Troubleshooting.md)

---

# Summary

A prefix list is a named, ordered set of IP prefixes that route maps match against by destination. It exists so a group of networks can be defined once and reused, rather than repeating CIDRs across route map entries.

The behaviour to internalise is that an entry without `ge` or `le` matches **only that exact prefix** — `10.0.0.0/8` does not match `10.1.0.0/16`. Matching everything inside a block requires the length range, and matching all routes requires `0.0.0.0/0` with `ge 0 le 32`, since the bare prefix matches only the default route. As with route maps, anything matching no entry is dropped by an implicit deny.
