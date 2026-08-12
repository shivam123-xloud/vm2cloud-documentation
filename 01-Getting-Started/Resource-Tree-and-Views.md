# Resource Tree and Views

---

## Overview

The **resource tree** on the left of the interface lists everything in the environment. The **view selector** above it changes how that list is organised.

Most people never change the view, and on a small environment that is fine. On a larger one, switching view is the difference between scrolling through two hundred guests and seeing only the twelve that belong to the project you are working on.

The tree contents are the same in every view. Only the grouping changes.

---

## When to Use

Change the view when:

* The tree is long enough that finding things is slow.
* You want to see only one team's or project's guests.
* You are working across nodes rather than within one.
* You want to group guests by a label rather than by location.

---

## Prerequisites

* You are logged in.
* You have permission to view the resources concerned. The tree only shows what your account can see.

---

# The Views

| View | Groups by | Best for |
|---|---|---|
| **Server View** | Node, then the guests and storage on it | Default. Node-centric work — maintenance, hardware, storage |
| **Folder View** | Resource type — all nodes, all VMs, all containers, all storage | Working across nodes on one kind of thing |
| **Pool View** | [Pool](../02-Datacenter/Permissions/Pools.md) membership | Multi-tenant environments; per-team or per-project work |
| **Tag View** | [Tag](Tags.md) applied to guests | Cross-cutting groupings — environment, role, owner |

---

## Server View

The default. Guests appear beneath the node currently running them.

```text
Datacenter
├── node1
│   ├── 100 (web-01)
│   ├── 101 (db-01)
│   └── local-lvm
└── node2
    └── 102 (app-01)
```

This shows *where things are*, which is what you want for maintenance, hardware, and storage work.

Its limitation: guests move. A machine that migrates appears somewhere else in the tree, which is accurate but unhelpful when you are tracking a workload rather than a server.

---

### Screenshot 1

**Server View**

```text
[ Place Screenshot Here ]
```

> **Capture:** The resource tree in Server View, showing nodes with their guests and
> storage nested beneath, and the view selector at the top.

---

## Folder View

Groups by resource type instead of by node.

```text
Datacenter
├── Nodes
│   ├── node1
│   └── node2
├── Virtual Machines
│   ├── 100 (web-01)
│   └── 102 (app-01)
├── Containers
│   └── 101 (db-01)
└── Storage
```

Useful when the question is "show me every virtual machine" rather than "show me what is on node1" — reviewing all guests, or comparing storage across the cluster.

---

### Screenshot 2

**Folder View**

```text
[ Place Screenshot Here ]
```

> **Capture:** The resource tree in Folder View, showing the type-based grouping.

---

## Pool View

Groups by [pool](../02-Datacenter/Permissions/Pools.md) membership.

```text
Datacenter
├── finance
│   ├── 100 (web-01)
│   └── 101 (db-01)
└── customer-acme
    └── 102 (app-01)
```

This is the view that makes large multi-tenant environments manageable. It also matches how permissions are usually granted, so a user with access to one pool sees a tree containing only their own resources.

Guests not in any pool do not appear grouped here, which is itself useful — it shows what has been missed.

---

### Screenshot 3

**Pool View**

```text
[ Place Screenshot Here ]
```

> **Capture:** The resource tree in Pool View, showing guests grouped under two or more
> pools.

---

## Tag View

Groups by [tag](Tags.md).

```text
Datacenter
├── production
│   ├── 100 (web-01)
│   └── 101 (db-01)
└── staging
    └── 102 (app-01)
```

Tags are more flexible than pools: a guest belongs to one pool but can carry several tags. That makes Tag View good for cross-cutting groupings — environment, application role, owner — that do not line up with the permission structure.

---

### Screenshot 4

**Tag View**

```text
[ Place Screenshot Here ]
```

> **Capture:** The resource tree in Tag View, showing guests grouped by tag.

---

# Procedure

## Step 1: Change the View

1. Click the view selector at the top of the resource tree.
2. Select the view.

The tree regroups immediately. The selection persists for your session.

---

## Step 2: Read Guest Status From the Icons

Whatever the view, each guest's icon indicates its state — running, stopped, or in a transitional state. Nodes show whether they are online.

This is the fastest way to scan an environment. A stopped guest that should be running is visible without opening anything.

> **Verify:** Capture the resource tree showing guests in running, stopped, and
> transitional states, so the icon differences are documented.

---

## Step 3: Use Search for Direct Lookup

When you know what you want, searching is faster than navigating any view. See [Search](Search.md).

Views are for browsing and grouping. Search is for going directly to something.

---

# Configuration / Options

| Control | Purpose |
|---|---|
| **View selector** | Switches between Server, Folder, Pool, and Tag views. |
| **Expand / collapse** | Shows or hides children of a tree entry. |
| **Search box** | Filters to matching resources. See [Search](Search.md). |

> **Verify:** Confirm the exact view names in the selector and whether the choice
> persists between sessions.

---

# Verification

Verify the following:

* The view selector offers all four views.
* Server View groups guests under their current node.
* Folder View groups by resource type.
* Pool View reflects current pool membership.
* Tag View reflects the tags applied to guests.
* Guest state icons match actual state.
* The tree shows only resources your account may see.
* A migrated guest appears under its new node in Server View.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Pool View is empty | No pools exist, or no guests are in them. See [Pools](../02-Datacenter/Permissions/Pools.md). |
| Tag View is empty | No tags have been applied. See [Tags](Tags.md). |
| A guest is missing from Pool View | It is not in any pool. Add it. |
| The tree shows less than expected | Your account lacks permission on the missing resources. |
| A guest appears under the wrong node | It was migrated, or HA recovered it elsewhere. The tree is correct. |
| Tree not refreshing | Reload the interface. |
| Cannot find a guest in a long tree | Use [Search](Search.md) rather than scrolling. |

---

# Best Practices

- Use **Server View** for node and hardware work, **Pool View** for tenant work.
- Adopt pools and tags early. Both views are useless until the underlying grouping exists.
- Use tags for cross-cutting labels — `production`, `database`, `owner-finance`.
- Use pools where the grouping should also control permissions.
- Check Pool View occasionally for guests missing from any pool. Those are the ones that fall outside pool-based backup jobs.
- Use Search rather than scrolling once the tree is long.
- Scan guest icons as a quick health check.

---

# Related Documentation

- [Interface Tour](Interface-Tour.md)
- [Search](Search.md)
- [Tags](Tags.md)
- [Pools](../02-Datacenter/Permissions/Pools.md)
- [Logging In](Logging-In.md)
- [What Is VM2Cloud](What-Is-VM2Cloud.md)
- [Assign Permissions](../02-Datacenter/Permissions/Assign-Permissions.md)

---

# Summary

The resource tree lists the whole environment, and the view selector changes how it is grouped: **Server View** by node, **Folder View** by resource type, **Pool View** by pool membership, and **Tag View** by tag.

Server View answers "where is this running". Pool View answers "what belongs to this team", and is what makes a large multi-tenant environment navigable. Both Pool and Tag views depend on the underlying grouping existing, so assigning pools and tags as guests are created is what makes them useful later. For finding one known resource, Search beats every view.
