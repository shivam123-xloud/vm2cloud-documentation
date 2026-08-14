# Content Audit

A line-by-line review of all 179 pages against the authoring rules in [CONTRIBUTING.md](CONTRIBUTING.md), started 14 August 2026.

This tracks **content correctness**, not screenshots. For screenshot progress see [SCREENSHOTS.md](SCREENSHOTS.md); for panel coverage see [COVERAGE.md](COVERAGE.md).

---

## Status

| # | Finding | Pages | Status |
|---|---|---:|---|
| 1 | Invented role names and duplicate sections in Permissions | 4 | ✅ Fixed |
| 2 | Destructive operations with no safety warning | 11 | ✅ Fixed |
| 3 | `Typical fields include` — fields hedged rather than named | 26 | ⏳ Open |
| 4 | Vague instructions such as "Configure the required settings" | 12 | ⏳ Open |
| 5 | Missing `Configuration / Options` section | 78 | ⏳ Open |
| 6 | Missing `Best Practices` section | 55 | ⏳ Open |

Findings 3–6 overlap heavily: a page that hedges its fields usually also lacks the Configuration / Options section that should contain them.

---

# ✅ 1. Permissions — factual errors

Fixed 14 August 2026.

**`Roles.md` listed six roles that do not exist.** Auditor, Resource Administrator, Virtual Machine Administrator, Datastore Administrator, and Backup Operator are not role names — the real ones are `PVEAuditor`, `PVEAdmin`, `PVEVMAdmin`, `PVEDatastoreAdmin`, and there is no backup role. Replaced with the real predefined roles and a privileges reference.

Also fixed: two `# Verification` sections in the same file; three missing permission paths (`/pool`, `/access`, `/sdn`); the access model described as a chain rather than a three-part binding; `root@pam` documented nowhere as unrestricted; API token ID format undocumented.

---

# ✅ 2. Destructive operations without warnings

Fixed 14 August 2026. Ten pages performed destructive operations with no warning at all.

| Page | Consequence now documented |
|---|---|
| `Cluster/Delete-Cluster.md` | Permanent; cluster features stop working |
| `Cluster/Remove-Node-from-Cluster.md` | **A removed node cannot rejoin without reinstallation** |
| `Storage/Manage-Storage.md` | Guests with disks on the storage fail to start |
| `System/Hosts.md` | Removing the node's own entry breaks cluster communication |
| `System/Kernel.md` | Removing the running or last kernel leaves the node unbootable |
| `Network/Manage-Bond.md` | Node unreachable if it carried management traffic |
| `Network/Manage-Linux-Bridge.md` | Disconnects every guest attached |
| `Network/Manage-VLAN.md` | Node unreachable if management is tagged there |
| `VM-Snapshots.md` | Restore point permanently discarded |
| `Manage-Container-Templates.md` | Note only — existing containers unaffected |

`Tags.md` was flagged and needs nothing: removing a tag genuinely changes nothing, and the page already says so.

---

# ⏳ 3. Fields hedged rather than named

**26 pages, 37 instances.** These write `Typical fields include` followed by a bare list, instead of naming each field and what it does.

This is the rule the authoring spec is most explicit about:

> Never write vague instructions such as *"Configure the required options."* Name every verified field and say what it does.

A bare list tells a reader the field exists. It does not tell them what to put in it, what the default is, or what happens if they get it wrong — which is the entire reason to open the page.

### Pages, worst first

| Page | Instances |
|---|---:|
| `System/Hosts.md` | 3 |
| `04-Virtual-Machines/Create-Virtual-Machine.md` | 3 |
| `05-Containers/Create-Container.md` | 3 |
| `System/DNS.md` | 2 |
| `Updates/Repositories.md` | 2 |
| `Permissions/Groups.md` | 2 |
| `Permissions/Users.md` | 2 |
| `04-Virtual-Machines/Backup-and-Restore-VM.md` | 2 |
| `05-Containers/Backup-and-Restore-Container.md` | 2 |
| 17 further pages | 1 each |

The two **Create** wizards matter most — they are the pages a new administrator opens first, and each has three separate hedges across the wizard steps.

---

# ⏳ 4. Vague instructions

**12 pages, 21 instances** of "Configure the required settings", "Enter the required information", "Modify the required information".

Same rule, same fix: replace with named steps. Usually resolved by the same edit as finding 3.

---

# ⏳ 5. Missing `Configuration / Options`

**78 pages.** The section that should hold the field reference. Its absence is why finding 3 exists — there was nowhere for the fields to go.

Adding it is only worthwhile where the page documents a panel with configurable fields. Reference and conceptual pages do not need one, and an empty heading is worse than none.

---

# ⏳ 6. Missing `Best Practices`

**55 pages.** Guidance that prevents problems rather than fixing them after the fact.

Lower priority than 3–5: its absence makes a page thinner, not wrong.

---

## Method

Detection sweeps are reproducible:

```bash
# 3. hedged fields
grep -rln 'Typical fields include\|Typical options include\|Typical information includes\|Typical editable fields' --include='*.md' .

# 4. vague instructions
grep -rn 'Configure the required settings\.\|Enter the required information\.\|Modify the required information\.' --include='*.md' .

# 5 / 6. missing sections
for f in $(find . -name '*.md' -not -name '*Troubleshooting*' -not -name 'README.md'); do
  grep -q '^#\+ *Best Practices' "$f" || echo "no best practices: $f"
done

# duplicate section headings
for f in $(find . -name '*.md' -not -path './.git/*'); do
  grep '^# [A-Z]' "$f" | sort | uniq -d | sed "s|^|$f: |"
done

# destructive operations with no warning
for f in $(find . -name '*.md' -not -name '*Troubleshooting*'); do
  grep -qiE '^#.*(Delete|Remove|Destroy|Wipe)' "$f" \
    && ! grep -qE '> \*\*(Warning|Note):\*\*' "$f" && echo "no warning: $f"
done
```

## What detection cannot catch

The Permissions errors were found by **reading against domain knowledge**, not by grep. Invented role names are well-formed text — no sweep flags them.

Sections not yet read line by line this way:

- 03-Nodes — Disks, System, Updates
- 04-Virtual-Machines and 05-Containers
- 02-Datacenter — Cluster, Storage, Replication, HA

Those still need the same treatment Permissions received.
