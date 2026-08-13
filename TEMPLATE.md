# Feature Name

<!--
Copy this file to start a new page.

Rules:
- The H1 must match the UI label of the panel being documented.
- Place the file in the folder matching its location in the interface.
- Keep the section order below.
- "Configuration / Options" and "Best Practices" are standard. Omit one only when the
  page genuinely has nothing real to put in it. Never leave an empty heading.
- Separate every block with a horizontal rule (---).
- The product is "VM2Cloud VE". Never write "Proxmox", and never "VM2Cloud" alone.
- Never guess a UI label. If you cannot verify it, use a > **Verify:** marker.
-->

---

## Overview

What this feature is, and what it does. Explain the concept, not the clicks.

Where the feature matters technically, explain how it actually works — normal
operation, what happens on failure, what it depends on, and what it explicitly does
*not* do. See `02-Datacenter/HA/HA-Overview.md` for the reference standard.

---

## When to Use

Use *[feature]* when:

* A situation where this page applies.
* Another situation.

State when **not** to use it if that prevents a mistake.

---

## Prerequisites

Before *[performing this task]*, ensure that:

* You have administrator privileges.
* The node is online.
* Any other condition that must be true before starting.

---

# Procedure

Every workflow walks the complete path:

```
Open location → Select resource → Click button → Configure fields
→ Review options → Confirm → Verify result
```

## Step 1: Open the Panel

1. Log in to the VM2Cloud VE web interface.
2. Select **Datacenter**.
3. Click **[UI element]**.

---

**Caption Describing the Screenshot**

![Caption Describing the Screenshot](images/descriptive-file-name.png)

---

## Step 2: Configure the Settings

Name every field and say what it does. Never write "Configure the required options."

1. In **[Field Name]**, enter […]. This controls […].
2. Select **[Option]**. Use this when […].

> **Note:** Use a note for information that prevents a mistake.

> **Warning:** Use a warning for any operation that may permanently affect or delete
> data. State exactly what is lost.

> **Verify:** Use this marker when a UI label, field, or option could not be confirmed
> against the real interface. Say precisely what needs checking. Never guess instead.

---

### Screenshot 2

**Short Caption**

```text
[ Place Screenshot Here ]
```

> **Capture:** Datacenter → Firewall → Rules, showing the rule list with the **Add**,
> **Edit**, and **Remove** buttons visible.

Every placeholder carries a **Capture:** line naming the exact screen and state to
photograph, so whoever takes the screenshots can work straight down the page without
guessing what is wanted.

---

## Step 3: Confirm and Verify

1. Review the configuration.
2. Click **[Confirm button]**.
3. Wait for the task to complete.

---

# Configuration / Options

Every field on the panel, and what each one does.

| Option | Description |
|--------|-------------|
| **Field name** | What it controls, valid values, and the default. |
| **Another field** | What it controls, and when to change it. |

---

# Verification

Verify the following:

* An observable result that confirms success.
* Another observable result.
* The task completed without errors.

Include a CLI verification command only where it genuinely helps:

```bash
pvecm status
```

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Something the reader might hit | What to check or do about it. |
| Another likely failure | The fix. |

---

# Best Practices

- Guidance that prevents problems rather than fixing them.
- Production considerations.
- What to avoid, and why.

---

# Related Documentation

- [Another Page](Another-Page.md)
- [A Page in a Sibling Folder](../Folder/Page.md)

---

# Summary

One paragraph restating what the reader accomplished and what they can do next. Do not
introduce new instructions here.
