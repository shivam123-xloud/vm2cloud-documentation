# Contributing to VM2Cloud Documentation

---

## The One Rule

**The folder structure mirrors the VM2Cloud interface.** A folder is a UI panel; a path is a click path.

Before creating a page, open the interface and find the panel it documents. Put the file where that panel lives. If you cannot decide where a page goes, the answer is wherever the reader would click to reach the feature.

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
Overview → When to Use → Prerequisites → Procedure → Verification → Common Issues → Summary
```

`Related Documentation` is optional and goes immediately before `Summary`.

Troubleshooting pages are the only exception — they are organized by symptom and do not need a `Common Issues` table, because the whole page is one.

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

The product is **VM2Cloud**. Write "VM2Cloud" or "the VM2Cloud Virtual Environment".

Command names such as `pvecm`, `pvesm`, and `pvesr` are the real binaries shipped with the product and are written as-is.

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

If the screenshot has not been captured yet, leave a numbered placeholder so the gap stays visible:

```markdown
### Screenshot 1

```text
[ Place Screenshot Here ]
```
```

Never delete a placeholder without either adding the image or removing the step it belongs to.

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

All four should report nothing.

---

## Commits

One page or one coherent change per commit. Describe what changed and why:

```
Add Pools documentation under Datacenter Permissions

Pools were visible in the interface but undocumented. Covers creating
a pool, adding members, and assigning permissions to it.
```

Use `git mv` when relocating a file so its history survives the move.
