# Screenshot Capture Plan

The order to work through when capturing screenshots, and what is already done.

**440 placeholders remain across 103 pages.** They are grouped below into ten phases that follow how a lab is actually built — install, storage, users, guests, cluster, and so on — so you never have to revisit a panel or rebuild to reach a state you have already passed.

---

## The One Rule

**Take a VMware snapshot of all three nodes at every phase boundary.**

Every "empty state" screenshot — no users, no pools, no backup jobs, no HA resources, no replication jobs — exists only *before* you create the first one. Once the environment is populated, the only way back is a rebuild.

A snapshot turns "I missed the empty Roles panel" from an afternoon into two minutes.

---

## Progress

| | Pages | Placeholders |
|---|---:|---:|
| ✅ Complete | 51 | 0 |
| ⬜ No screenshots needed | 21 | 0 |
| ⏳ Awaiting capture | 98 | 402 |
| **Total** | **170** | **402** |

---

# ✅ Already Complete — 44 pages

These need nothing. Do not recapture them.

## Installation — 29 screenshots

Captured 22 July 2026 during a real installation.

| Page | Images |
|---|---:|
| [Mount the Installation ISO](00-Installation/Mount-Installation-Media.md) | 11 |
| [Configure Installation Storage](00-Installation/Configure-Installation-Storage.md) | 6 |
| [Configure Management Network](00-Installation/Configure-Management-Network.md) | 5 |
| [Verify Installation](00-Installation/Verify-Installation.md) | 3 |
| [Complete Installation](00-Installation/Complete-Installation.md) | 2 |
| [Configure Location and Administrator](00-Installation/Configure-Location-and-Administrator.md) | 2 |

## Cluster — 28

[Delete Cluster](02-Datacenter/Cluster/Delete-Cluster.md) 9 · [Join Node to Cluster](02-Datacenter/Cluster/Join-Node-to-Cluster.md) 7 · [Remove Node from Cluster](02-Datacenter/Cluster/Remove-Node-from-Cluster.md) 7 · [Create Cluster](02-Datacenter/Cluster/Create-Cluster.md) 5

## Storage — 31

[Manage Storage](02-Datacenter/Storage/Manage-Storage.md) 10 · [Upload Content](02-Datacenter/Storage/Upload-Content.md) 7 · [Add Storage](02-Datacenter/Storage/Add-Storage.md) 5 · [Storage Overview](02-Datacenter/Storage/Storage-Overview.md) 5 · [Storage Types](02-Datacenter/Storage/Storage-Types.md) 1 · [Permissions Overview](02-Datacenter/Permissions/Permissions-Overview.md) 3

## Node and network — 52

[Manage Linux Bridge](03-Nodes/System/Network/Manage-Linux-Bridge.md) 10 · [Manage Bond](03-Nodes/System/Network/Manage-Bond.md) 9 · [Manage VLAN](03-Nodes/System/Network/Manage-VLAN.md) 9 · [Node Summary](03-Nodes/Node-Summary.md) 9 · [Apply Network Configuration](03-Nodes/System/Network/Apply-Network-Configuration.md) 7 · [Network Overview](03-Nodes/System/Network/Network-Overview.md) 7 · [Reboot Node](03-Nodes/Reboot-Node.md) 5 · [Shutdown Node](03-Nodes/Shutdown-Node.md) 5

## Virtual machines — 85

[Manage VM Hardware](04-Virtual-Machines/Manage-VM-Hardware.md) 18 · [Manage Virtual Machine](04-Virtual-Machines/Manage-Virtual-Machine.md) 11 · [Backup and Restore VM](04-Virtual-Machines/Backup-and-Restore-VM.md) 10 · [Create Virtual Machine](04-Virtual-Machines/Create-Virtual-Machine.md) 9 · [VM Snapshots](04-Virtual-Machines/VM-Snapshots.md) 9 · [Delete Virtual Machine](04-Virtual-Machines/Delete-Virtual-Machine.md) 6 · [VM Console](04-Virtual-Machines/VM-Console.md) 6 · [Virtual Machine Overview](04-Virtual-Machines/Virtual-Machine-Overview.md) 6 · [Clone Virtual Machine](04-Virtual-Machines/Clone-Virtual-Machine.md) 5 · [Migrate Virtual Machine](04-Virtual-Machines/Migrate-Virtual-Machine.md) 5

## Containers — 74

[Manage Container Resources](05-Containers/Manage-Container-Resources.md) 16 · [Create Container](05-Containers/Create-Container.md) 9 · [Manage Container](05-Containers/Manage-Container.md) 9 · [Backup and Restore Container](05-Containers/Backup-and-Restore-Container.md) 8 · [Container Overview](05-Containers/Container-Overview.md) 7 · [Manage Container Templates](05-Containers/Manage-Container-Templates.md) 6 · [Clone Container](05-Containers/Clone-Container.md) 5 · [Delete Container](05-Containers/Delete-Container.md) 5 · [Container Console](05-Containers/Container-Console.md) 4

---

# ⬜ No Screenshots Needed — 16 pages

Conceptual, reference, or symptom-based. They carry no placeholders by design.

[What Is VM2Cloud VE](01-Getting-Started/What-Is-VM2Cloud-VE.md) · [Firewall Overview](02-Datacenter/Firewall/Firewall-Overview.md) · [HA Overview](02-Datacenter/HA/HA-Overview.md) · [Cluster Overview](02-Datacenter/Cluster/Cluster-Overview.md) · [System Overview](03-Nodes/System/System-Overview.md) · [Services](03-Nodes/System/Services.md) · [CLI Reference](06-CLI-Reference.md)

Plus the nine troubleshooting pages, which are organised by symptom rather than by screen.

---

# ⏳ Capture Phases

Work top to bottom. **Snapshot at every boundary.**

---

## Phase 1 — Fresh single node · 123 shots · 29 pages

**Before creating anything.** This is the largest phase and the only chance at most empty-state panels.

Node: `v2c1` only. Do not cluster yet.

### Getting Started — 31
| Page | Shots |
|---|---:|
| [Task Log and Cluster Log](01-Getting-Started/Task-Log-and-Cluster-Log.md) | 10 |
| [Interface Tour](01-Getting-Started/Interface-Tour.md) | 7 |
| [My Settings](01-Getting-Started/My-Settings.md) | 4 |
| [Resource Tree and Views](01-Getting-Started/Resource-Tree-and-Views.md) | 4 |
| [Logging In](01-Getting-Started/Logging-In.md) | 3 |
| [Search](01-Getting-Started/Search.md) | 3 |

> Resource Tree and Views needs Pool and Tag views, which are empty now. Capture Server and Folder views here, and return in Phase 3 for the other two.

### Datacenter panels — 33
| Page | Shots |
|---|---:|
| [Datacenter Summary](02-Datacenter/Datacenter-Summary.md) | 6 |
| [ACME Certificates](02-Datacenter/ACME-Certificates.md) | 4 |
| [Notifications](02-Datacenter/Notifications.md) | 4 |
| [Datacenter Options](02-Datacenter/Options.md) | 3 |
| [Metric Server](02-Datacenter/Metric-Server.md) | 3 |
| [Resource Mappings](02-Datacenter/Resource-Mappings.md) | 3 |
| [Directory Mappings](02-Datacenter/Directory-Mappings.md) | 3 |
| [Custom CPU Models](02-Datacenter/Custom-CPU-Models.md) | 3 |
| [Datacenter Notes](02-Datacenter/Notes.md) | 2 |
| [Support](02-Datacenter/Support.md) | 2 |

> The three mapping panels are worth capturing even though the lab has nothing mapped — the empty state and the Add dialog are what the pages need most, and the Add dialog shows the field labels the `Verify:` markers are waiting on.
>
> **Still unconfirmed:** node System → Options. It was written from the standard platform layout because no available screenshot showed it. If the panel is absent, tell me and I will remove or rework the page.

### Node System — 42
| Page | Shots |
|---|---:|
| [Time and NTP](03-Nodes/System/Time-and-NTP.md) | 9 |
| [Hosts](03-Nodes/System/Hosts.md) | 8 |
| [Kernel](03-Nodes/System/Kernel.md) | 6 |
| [Certificates](03-Nodes/System/Certificates.md) | 5 |
| [DNS](03-Nodes/System/DNS.md) | 4 |
| [Syslog](03-Nodes/System/Syslog.md) | 3 |
| [Boot Mode](03-Nodes/System/Boot-Mode.md) | 2 |

### Node other — 17
| Page | Shots |
|---|---:|
| [Update Node](03-Nodes/Updates/Update-Node.md) | 5 |
| [Repositories](03-Nodes/Updates/Repositories.md) | 5 |
| [Task History](03-Nodes/Task-History.md) | 4 |
| [Shell](03-Nodes/Shell.md) | 3 |
| [Subscription](03-Nodes/Subscription.md) | 3 |
| [Node Notes](03-Nodes/Node-Notes.md) | 2 |

📸 **Snapshot: `phase1-fresh`**

---

## Phase 2 — Disks and storage · 56 shots · 11 pages

Uses the spare disks on `v2c1`. Create each storage type, capture, then leave it — later phases need them.

| Page | Shots | Needs |
|---|---:|---|
| [ZFS](03-Nodes/Disks/ZFS.md) | 11 | Disk 2 free |
| [Directory](03-Nodes/Disks/Directory.md) | 7 | Disk 6 free |
| [LVM-Thin](03-Nodes/Disks/LVM-Thin.md) | 6 | Disk 5 free |
| [Disk Management](03-Nodes/Disks/Disk-Management.md) | 5 | An unused disk |
| [LVM](03-Nodes/Disks/LVM.md) | 5 | Disk 4 free |
| [Storage Content Browser](02-Datacenter/Storage/Storage-Content-Browser.md) | 5 | |
| [Disks Overview](03-Nodes/Disks/Disks-Overview.md) | 4 | |
| [View Disk Information](03-Nodes/Disks/View-Disk-Information.md) | 4 | |
| [Storage Import](02-Datacenter/Storage/Storage-Import.md) | 4 | A disk image to import |
| [Disk Troubleshooting](03-Nodes/Disks/Disk-Troubleshooting.md) | 3 | |
| [Storage Permissions](02-Datacenter/Storage/Storage-Permissions.md) | 2 | |

> Capture **Disks Overview** and **View Disk Information** *before* creating anything, so the disks still show as unused.

📸 **Snapshot: `phase2-storage`**

---

## Phase 3 — Users and permissions · 46 shots · 9 pages

Capture each panel **empty first**, then populated. Both states appear in the pages.

| Page | Shots |
|---|---:|
| [Groups](02-Datacenter/Permissions/Groups.md) | 9 |
| [API Tokens](02-Datacenter/Permissions/API-Tokens.md) | 7 |
| [Roles](02-Datacenter/Permissions/Roles.md) | 7 |
| [Two-Factor Authentication](02-Datacenter/Permissions/Two-Factor-Authentication.md) | 6 |
| [Assign Permissions](02-Datacenter/Permissions/Assign-Permissions.md) | 5 |
| [Pools](02-Datacenter/Permissions/Pools.md) | 5 |
| [Authentication Realms](02-Datacenter/Permissions/Authentication-Realms.md) | 3 |
| [Tags](01-Getting-Started/Tags.md) | 3 |
| [Users](02-Datacenter/Permissions/Users.md) | 1 |

> Once pools and tags exist, go back and finish **Resource Tree and Views** — the Pool View and Tag View shots.

📸 **Snapshot: `phase3-access`**

---

## Phase 4 — Guests on a single node · 54 shots · 14 pages

Create one VM and one container. Nested virtualization must be working.

### Virtual machine — 27
[Cloud-Init](04-Virtual-Machines/Cloud-Init.md) 6 · [Convert to Template](04-Virtual-Machines/Convert-to-Template.md) 5 · [VM Options](04-Virtual-Machines/VM-Options.md) 5 · [VM Summary](04-Virtual-Machines/VM-Summary.md) 4 · [VM Monitor](04-Virtual-Machines/VM-Monitor.md) 3 · [VM Permissions](04-Virtual-Machines/VM-Permissions.md) 2 · [VM Task History](04-Virtual-Machines/VM-Task-History.md) 2

### Container — 27
[CT Snapshots](05-Containers/CT-Snapshots.md) 6 · [CT Network](05-Containers/CT-Network.md) 5 · [CT DNS](05-Containers/CT-DNS.md) 4 · [CT Options](05-Containers/CT-Options.md) 4 · [CT Summary](05-Containers/CT-Summary.md) 4 · [CT Permissions](05-Containers/CT-Permissions.md) 2 · [CT Task History](05-Containers/CT-Task-History.md) 2

> Convert to Template is **one-way**. Clone a throwaway VM and convert the clone.

📸 **Snapshot: `phase4-guests`**

---

## Phase 5 — Cluster · 4 shots · 3 pages

Join `v2c2` and `v2c3`. Create/Join/Remove/Delete Cluster are already captured.

| Page | Shots | Note |
|---|---:|---|
| [Quorum](02-Datacenter/Cluster/Quorum.md) | ✅ done | Captured 17 August |
| [Re-Add a Removed Node](02-Datacenter/Cluster/Re-Add-Removed-Node.md) | 2 | The orphan state and the Join dialog — the rest captured 18 August |
| [Cluster Certificates](02-Datacenter/Cluster/Cluster-Certificates.md) | 2 | |
| [Recover Quorum](02-Datacenter/Cluster/Recover-Quorum.md) | 2 | Power off 2 nodes to force loss of quorum |
| [Cluster File System](02-Datacenter/Cluster/Cluster-File-System.md) | 1 | |

> **Recover Quorum** needs a deliberately broken cluster. Snapshot before, capture, then roll back.
>
> **Re-Add a Removed Node** is the last thing to do in this phase, because it takes a node out of the cluster. The cleanup path keeps it non-destructive — the node is cleaned and rejoined rather than wiped — so a snapshot beforehand is enough and no reinstall is required.

📸 **Snapshot: `phase5-cluster`**

---

## Phase 6 — Ceph · 12 shots · 4 pages

Needs disk 3 free on all three nodes.

[Ceph Monitors and OSDs](02-Datacenter/Ceph/Ceph-Monitors-and-OSDs.md) 4 · [Ceph Pools](02-Datacenter/Ceph/Ceph-Pools.md) 4 · [Node Ceph](03-Nodes/Node-Ceph.md) 3 · [Ceph Overview](02-Datacenter/Ceph/Ceph-Overview.md) 1

> Capture the empty Ceph panel **before** `pveceph install`.

📸 **Snapshot: `phase6-ceph`**

---

## Phase 7 — HA and replication · 88 shots · 15 pages

The largest phase after Phase 1. Needs the cluster plus ZFS or Ceph.

### Replication — 47
[Create Replication Job](02-Datacenter/Replication/Create-Replication-Job.md) 11 · [Replication Status](02-Datacenter/Replication/Replication-Status.md) 9 · [Replication Scheduling](02-Datacenter/Replication/Replication-Scheduling.md) 8 · [Replication Overview](02-Datacenter/Replication/Replication-Overview.md) 6 · [Edit Replication Job](02-Datacenter/Replication/Edit-Replication-Job.md) 5 · [Delete Replication Job](02-Datacenter/Replication/Delete-Replication-Job.md) 4 · [Replication Troubleshooting](02-Datacenter/Replication/Replication-Troubleshooting.md) 4

### High availability — 33
[HA Troubleshooting](02-Datacenter/HA/HA-Troubleshooting.md) 19 · [HA Resources](02-Datacenter/HA/HA-Resources.md) 4 · [Node Affinity](02-Datacenter/HA/Node-Affinity.md) 4 · [Resource Affinity](02-Datacenter/HA/Resource-Affinity.md) 4 · [Fencing](02-Datacenter/HA/Fencing.md) 2

### Guest and node replication views — 8
[VM Replication](04-Virtual-Machines/VM-Replication.md) 3 · [CT Replication](05-Containers/CT-Replication.md) 3 · [Node Replication](03-Nodes/Node-Replication.md) 2

> **HA Troubleshooting needs failure states** — a node down, a resource in error, quorum lost. Snapshot first, break things deliberately, capture, roll back.

📸 **Snapshot: `phase7-ha`**

---

## Phase 8 — Backup jobs · 2 shots remaining · 2 pages

**Mostly done — 16 of 18 captured 14 August 2026.**

✅ [Backup Jobs Overview](02-Datacenter/Backup/Backup-Jobs-Overview.md) · ✅ [Backup Retention](02-Datacenter/Backup/Backup-Retention.md)

Still open:

| Page | Shot | What to capture |
|---|---|---|
| [Create Backup Job](02-Datacenter/Backup/Create-Backup-Job.md) | 6 | The **Notifications** tab, or the compression dropdown open on General |
| [Manage Backup Job](02-Datacenter/Backup/Manage-Backup-Job.md) | 6 | The task log at the bottom of the interface, filtered to backup tasks — the task *list* with several runs, not one task's Output window |

> For that last one, point a job at a nonexistent storage and let it run — a failed entry is what the page describes.

📸 **Snapshot: `phase8-backup`**

---

## Phase 9 — Firewall and SDN · 62 shots · 17 pages

### Firewall — 38
[Firewall Rules](02-Datacenter/Firewall/Firewall-Rules.md) 6 · [Security Groups](02-Datacenter/Firewall/Security-Groups.md) 6 · [IPSets](02-Datacenter/Firewall/IPSets.md) 5 · [Node Firewall](03-Nodes/Node-Firewall.md) 5 · [Aliases](02-Datacenter/Firewall/Aliases.md) 4 · [Firewall Options](02-Datacenter/Firewall/Firewall-Options.md) 4 · [VM Firewall](04-Virtual-Machines/VM-Firewall.md) 4 · [CT Firewall](05-Containers/CT-Firewall.md) 4

### SDN — 24
[VNets](02-Datacenter/SDN/VNets.md) 4 · [Zones](02-Datacenter/SDN/Zones.md) 3 · [SDN Options](02-Datacenter/SDN/SDN-Options.md) 3 · [IPAM](02-Datacenter/SDN/IPAM.md) 1 · [VNet Firewall](02-Datacenter/SDN/VNet-Firewall.md) 3 · [Fabrics](02-Datacenter/SDN/Fabrics.md) 4 · [Route Maps](02-Datacenter/SDN/Route-Maps.md) 2 · [Prefix Lists](02-Datacenter/SDN/Prefix-Lists.md) 2 · [SDN Overview](02-Datacenter/SDN/SDN-Overview.md) 1

> SDN has eight panels, not two. The six beyond Zones and VNets were written on
> 14 August 2026 after the submenu was seen for the first time.

> Capture firewall panels **before enabling** the firewall. Enabling it without a management rule locks you out — which is Phase 10's problem, not this one's.

📸 **Snapshot: `phase9-firewall`**

---

## Phase 10 — Recovery scenarios · 8 shots · 2 pages

**Destructive. Do these last, after snapshotting everything.**

| Page | Shots | What to break |
|---|---:|---|
| [Reset Root Password](03-Nodes/Reset-Root-Password.md) | 5 | Boot to the GRUB entry, reach a root shell |
| [Firewall Lockout Recovery](02-Datacenter/Firewall/Firewall-Lockout-Recovery.md) | 3 | Enable the firewall with no management rule |

Roll back to `phase9-firewall` afterwards.

---

# How to Capture

Follow [CONTRIBUTING.md](CONTRIBUTING.md). In short:

- Save to the page's own `images/` folder
- Lowercase-hyphen filenames, no spaces — `add-user.png`
- Replace the whole placeholder block:

```markdown
**Users Page**

![Users Page](images/user-page.png)
```

- Delete the `Capture:` line along with the placeholder
- No real hostnames, addresses, licence keys, or credentials

**Every one of the 402 placeholders carries a `Capture:` line** naming the exact screen and state to shoot. You should never have to read the surrounding prose to work out what was wanted.

Some carry an instruction beyond the framing — blur a licence key or a token secret, snapshot before causing a failure, or skip the shot entirely rather than staging a fault. Those are worth reading before you press the shutter.

---

# While You Are There

**Clear the `Verify:` markers.** 126 of them flag UI labels, field lists, and option sets that could not be confirmed without a live environment. You are about to be looking at every one of those screens.

Where a screenshot settles one, its `Capture:` line says so — look for **"this clears a `Verify` marker."** Those are the highest-value shots in the set, because they fix correctness rather than only illustrating it. Thirteen are called out that way:

Hosts · Roles privilege picker · HA requested-state list · 2FA method list · Realms list · Node-affinity label and enforcement control · Resource-affinity types · Kernel default control · Certificates upload control · Repositories panel · Datacenter Options location · Quorum display · Header bar

```bash
grep -rn '> \*\*Verify:\*\*' --include='*.md' . \
  --exclude=TEMPLATE.md --exclude=CONTRIBUTING.md
```

**Confirm the panel list.** [COVERAGE.md](COVERAGE.md) was built from six screenshots plus the standard platform layout. If you find a tab that is not in it, that tab is undocumented — tell me and I will write the page.

---

# Tracking

```bash
# remaining
grep -rho 'Place Screenshot Here' --include='*.md' . \
  --exclude=TEMPLATE.md --exclude=CONTRIBUTING.md --exclude=SCREENSHOTS.md | wc -l

# by section
for d in 0*/; do
  printf "%-24s %s\n" "$d" \
    "$(grep -rho 'Place Screenshot Here' "$d" --include='*.md' | wc -l)"
done

# every image resolves
grep -rno '](images/[^)]*)' --include='*.md' . | while IFS= read -r l; do
  f="${l%%:*}"; img=$(echo "$l" | sed -n 's/.*](\(images\/[^)]*\)).*/\1/p')
  [ -f "$(dirname "$f")/$img" ] || echo "BROKEN: $l"
done
```
