# Contributing to VM2Cloud VE Documentation

---

## The One Rule

**The folder structure mirrors the VM2Cloud VE interface.** A folder is a UI panel; a path is a click path.

Before creating a page, open the interface and find the panel it documents. Put the file where that panel lives. If you cannot decide where a page goes, the answer is wherever the reader would click to reach the feature.

### One page per UI location

A feature that appears at several levels gets a page at **each** level, and each page documents **only its own panel**. Shared concepts are written once in the primary page and linked from the others — never copied.

The firewall is the worked example:

| Page | Contains |
|---|---|
| `02-Datacenter/Firewall/Firewall-Overview.md` | The rule model, direction, actions, macros — written once |
| `03-Nodes/Node-Firewall.md` | Only what the node firewall panel does |
| `04-Virtual-Machines/VM-Firewall.md` | Only what the VM firewall panel does |
| `05-Containers/CT-Firewall.md` | Only what the container firewall panel does |

If you find yourself explaining the same concept twice, move it to the primary page and link.

---

## What Every Page Must Answer

A reader should finish a page knowing:

1. What the feature is.
2. When and why to use it.
3. Where it lives in the VM2Cloud VE UI.
4. The complete UI workflow.
5. What every configuration field does.
6. How to verify the result.
7. How to troubleshoot it.
8. How it actually works technically.

---

## Never Guess

Do not assume a menu location, button name, field name, available option, workflow, or technical behaviour. Verify against the real VM2Cloud VE UI.

Where you cannot verify a detail, **mark it** — do not guess and do not hedge vaguely:

```markdown
> **Verify:** Confirm the exact label of the **Add** button on the Firewall → Rules panel.
```

Every marker is a tracked item. Find outstanding ones with:

```bash
grep -rn '> \*\*Verify:\*\*' --include='*.md' .
```

Clear them as screenshots are captured. Do **not** write vague hedges such as *"depends on the installed VM2Cloud VE version"* — either state the verified fact or leave a `Verify` marker naming exactly what to check.

Statements about genuine runtime variation are fine and are not hedges — for example, *"The available actions depend on the guest status and your user permissions."*

---

## UI First, CLI Second

Document the GUI procedure first. It is the primary method.

Include CLI only when:

- The UI cannot perform the operation.
- CLI is required for troubleshooting.
- CLI provides useful verification.
- The official workflow requires it.

---

## Starting a New Page

1. Copy [TEMPLATE.md](TEMPLATE.md) to the correct folder.
2. Name the file after the UI label, in `Title-Case-With-Hyphens.md`.
3. Make the H1 match the UI label.
4. Add the page to the parent folder's `README.md` and to the root [README.md](README.md).
5. If the page was listed under **Planned Pages** in the root README, remove it from that list.

---

## Page Structure

Every page follows the same order:

```
# Feature Name
## Overview
## When to Use
## Prerequisites
# Procedure
## Step 1 … Step N
# Configuration / Options
# Verification
# Common Issues
# Best Practices
# Related Documentation
# Summary
```

Add further sections where a feature needs them.

`Configuration / Options` and `Best Practices` are standard. Omit one only when the page genuinely has nothing real to put in it — **never leave an empty heading**.

Troubleshooting pages are the exception: they are organized by symptom and do not need a `Common Issues` table, because the whole page is one.

---

## Complete Procedures

Every workflow walks the full path:

```
Open location → Select resource → Click button → Configure fields
→ Review options → Confirm → Verify result
```

Never write vague instructions such as *"Configure the required options."* Name every verified field and say what it does:

```markdown
1. In **Name**, enter a unique identifier for the rule.
2. Set **Direction** to **in** for traffic arriving at the guest.
3. Set **Action** to **ACCEPT**, **DROP**, or **REJECT**.
```

Deliver each page complete. No partial sections, no "add this later", no omitted steps.

---

## Explain How It Works

Do not only explain which button to press. Where the mechanism matters, explain it — normal operation, failure behaviour, dependencies, limitations, and what the feature explicitly does *not* do.

[02-Datacenter/HA/HA-Overview.md](02-Datacenter/HA/HA-Overview.md) is the reference standard: it covers what HA is, what an HA resource is, normal operation, node power loss, quorum, fencing, recovery, why HA recovery is not live migration, storage and network requirements, and why HA is neither zero-downtime nor a backup.

---

## Safety

Any operation that may permanently affect or delete data carries an explicit warning stating exactly what is lost:

```markdown
> **Warning:** Deleting a group removes the group, its memberships, and any permissions
> assigned to it. Users who were members will lose the permissions inherited from it.
```

---

## Writing Style

- Address the reader as **you**. Use the imperative for actions: "Click **Create**", not "The user should click Create".
- Bold every UI element exactly as it appears on screen: **Datacenter**, **Create Cluster**, **OK**.
- Number sequential actions. Use bullets for non-sequential lists.
- Keep paragraphs short. One idea per paragraph.
- Separate blocks with `---`.
- Use `> **Note:**` for information that prevents a mistake.
- Use `> **Warning:**` for anything destructive or irreversible.

### Branding

The product is **VM2Cloud VE**. Use that name throughout — "the VM2Cloud VE web interface", "a VM2Cloud VE node", "the VM2Cloud VE cluster".

Three exceptions:

| Form | When |
|---|---|
| **VM2Cloud Virtual Environment** | The expanded form. Keep it where it appears as a verbatim UI label, such as the authentication realm name. |
| `vm2cloud` (lowercase) | Filesystem paths, URLs, and filenames — `/mnt/vm2cloud-storage`, `vm2cloud-ve-9.2-amd64-v10.iso`. Never change these. |
| **VM2Cloud VE-managed**, **VM2Cloud VE-specific** | Hyphenated compound adjectives. Correct as written. |

Do not write "VM2Cloud" on its own as the product name.

**Never write "Proxmox."** No upstream attribution appears anywhere in this documentation. Where a page previously said *"VM2Cloud VE uses the underlying Proxmox VE HA architecture, including watchdog-based fencing"*, it now says *"VM2Cloud VE provides this protection through watchdog-based fencing"* — the technical statement is kept, the attribution is dropped.

Command names such as `pvecm`, `pvesm`, `pvesr`, and `pveproxy` are the real binaries shipped with the product and are written as-is.

---

## Screenshots

Screenshots live in an `images/` folder beside the Markdown that uses them.

```
02-Datacenter/Permissions/
├── Users.md
└── images/
    └── user-page.png
```

- Reference them with a relative path: `![Caption](images/user-page.png)`.
- Name files in lowercase with hyphens: `add-user.png`, not `Add User.png`. **No spaces.**
- Give every screenshot a bold caption line above it, matching the alt text:

```markdown
**Users Page**

![Users Page](images/user-page.png)
```

### Placeholders

If the screenshot has not been captured yet, leave a numbered placeholder so the gap stays visible, **and say what to capture**:

```markdown
### Screenshot 1

**Firewall Rules Panel**

​```text
[ Place Screenshot Here ]
​```

> **Capture:** Datacenter → Firewall → Rules, showing the rule list with the **Add**,
> **Edit**, and **Remove** buttons visible.
```

The `Capture:` line names the exact screen and state to photograph. Whoever takes the screenshots should be able to work straight down a page without guessing what each one is meant to show.

Find every outstanding capture with:

```bash
grep -rn '> \*\*Capture:\*\*' --include='*.md' . --exclude=TEMPLATE.md --exclude=CONTRIBUTING.md
```

Never delete a placeholder without either adding the image or removing the step it belongs to. When you add the image, delete the `Capture:` line with it.

### Capturing

- Capture the panel being documented, with enough surrounding interface for the reader to orient.
- Use a clean demo environment. Do not capture real hostnames, IP addresses, licence keys, or user data.

---

## Links

Use relative links between pages:

```markdown
[Quorum](../Cluster/Quorum.md)
```

Never link to a file or folder that does not exist. If you want to reference a page that has not been written, add it to **Planned Pages** in the root README instead of linking to it.

---

## Before You Commit

Run these checks from the repository root.

**No broken links or images:**

```bash
python3 - <<'PY'
import re, pathlib
pat = re.compile(r'\[([^\]]*)\]\(([^)#]+?)(?:#[^)]*)?\)')
SKIP = {"TEMPLATE.md", "CONTRIBUTING.md"}   # contain example links by design
bad = []
for md in sorted(pathlib.Path(".").rglob("*.md")):
    if ".git" in md.parts or md.name in SKIP: continue
    fenced = False
    for i, line in enumerate(md.read_text().splitlines(), 1):
        if line.lstrip().startswith("```"):
            fenced = not fenced
            continue
        if fenced: continue          # code samples are not live links
        for m in pat.finditer(line):
            t = m.group(2).strip()
            if t.startswith(("http://", "https://", "mailto:")): continue
            if not (md.parent / t).exists():
                bad.append(f"{md}:{i} -> {t}")
print(f"broken: {len(bad)}")
for b in bad: print(" ", b)
PY
```

**All code fences closed** (catches a page truncated mid-diagram):

```bash
for f in $(find . -name '*.md' -not -path './.git/*'); do
  n=$(grep -c '^```' "$f"); [ $((n % 2)) -ne 0 ] && echo "UNBALANCED: $f"
done
```

**Template sections present:**

```bash
for f in $(find . -name '*.md' -not -path './.git/*' -not -name '*Troubleshooting*' \
           -not -name 'README.md' -not -name 'TEMPLATE.md' -not -name 'CONTRIBUTING.md' \
           -not -name 'GLOSSARY.md'); do
  for s in Overview Summary; do
    grep -q "^#\+ *$s" "$f" || echo "$f missing $s"
  done
done
```

**No orphan screenshots** (committed but never referenced):

```bash
find . -name '*.png' -not -path './.git/*' | while read -r p; do
  b=$(basename "$p"); d=$(dirname "$(dirname "$p")")
  grep -rqF "$b" "$d" --include='*.md' 2>/dev/null || echo "ORPHAN: $p"
done
```

**No brand leaks:**

```bash
grep -rni 'proxmox' --include='*.md' . \
  --exclude=TEMPLATE.md --exclude=CONTRIBUTING.md && echo "BRAND LEAK" || echo "clean"
```

*(This file and `TEMPLATE.md` are excluded because they state the rule itself.)*

All five should report nothing.

### Tracking outstanding verification

Not a failure condition, but review it before publishing a section:

```bash
grep -rn '> \*\*Verify:\*\*' --include='*.md' . \
  --exclude=TEMPLATE.md --exclude=CONTRIBUTING.md
```

Each marker is a UI detail awaiting confirmation against the real interface.

---

## Commits

One page or one coherent change per commit. Describe what changed and why:

```
Add Pools documentation under Datacenter Permissions

Pools were visible in the interface but undocumented. Covers creating
a pool, adding members, and assigning permissions to it.
```

Use `git mv` when relocating a file so its history survives the move.
