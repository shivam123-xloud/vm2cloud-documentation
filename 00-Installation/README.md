# Installation

Installing VM2Cloud VE on a physical server, from mounting the ISO through to a verified first login.

This section covers what happens **before** the interface exists. Everything else in this documentation assumes a running node; this is how you get one.

---

## The Route

Follow these in order. Each page ends by pointing at the next.

| Step | Page | Covers |
|---|---|---|
| 1 | [Mount the Installation ISO](Mount-Installation-Media.md) | BMC Virtual Media, controlled restart, booting the graphical installer |
| 2 | [Configure Installation Storage](Configure-Installation-Storage.md) | Target disks, filesystem and RAID layout, ZFS options |
| 3 | [Configure Location and Administrator Access](Configure-Location-and-Administrator.md) | Time zone, keyboard, root password, notification email |
| 4 | [Configure the Management Network](Configure-Management-Network.md) | Management NIC, hostname, addressing, VLAN, bonding, extra networks |
| 5 | [Complete the Installation](Complete-Installation.md) | Reviewing the summary, installing, automatic reboot |
| 6 | [Verify the Installation](Verify-Installation.md) | Console login, web login, dashboard, and what to do next |

---

## Before You Start

Have these ready. Gathering them mid-installation is where mistakes happen.

**Access**

* Authorized access to the server's BMC web interface, with permission to use Virtual Media and restart the server.
* An approved maintenance window.

**Media**

* `vm2cloud-ve-9.2-amd64-v10.iso` on the computer you will run the console from.

**Storage plan**

* Which disks to use, identified by device name, capacity, **and** model.
* The intended filesystem or RAID layout.
* For ZFS: disks presented directly, not through hardware RAID.

**Identity**

* A unique fully qualified hostname.
* A strong root password, and a shared password manager to record it in.
* A monitored operational email address.

**Network plan**

* Static management IP with prefix, gateway, and DNS resolver.
* The management VLAN ID, and whether the switch presents it tagged or untagged.
* For bonding: confirmed switch-side configuration.
* Any additional networks — storage, Corosync, custom.

---

## Environment

| | |
|---|---|
| **Software version** | VM2Cloud VE 9.2, ISO build v10 |
| **Environment** | Physical server with a browser-based BMC remote console |
| **Total time** | 40–70 minutes |
| **Data impact** | Selected disks are erased |

> **Warning:** Installation erases every selected disk. Confirm disk identity before proceeding, and confirm the maintenance window before restarting the server.

---

## Things That Cost the Most

Four points account for most failed installations. Each is trivial to get right at the time and expensive afterwards.

**Disk identity.** Device names such as `/dev/sda` are assigned per boot. Confirm capacity and model too. Erasing the wrong disk has no recovery.

**Switch-side network configuration.** A VLAN entered when the switch sends untagged traffic, or LACP configured when no port channel exists, produces a server that installs perfectly and cannot be reached. Verify the switch first, and prefer **active-backup** over LACP when uncertain.

**Keyboard layout.** You will type the root password at a console using the layout selected during installation. A mismatch makes a correct password appear wrong.

**The still-mapped ISO.** If the installer reappears after the reboot, the ISO was never unmapped. Harmless, but alarming if unexpected.

---

## Scope

This section documents installation on a **physical server via BMC remote console**, which is the verified path.

Not currently covered: USB media installation, nested or virtual installation, automated or unattended installation, and in-place upgrades.

---

## After Installation

[Verify the Installation](Verify-Installation.md) ends with an ordered list of post-installation tasks — updates, time synchronization, individual user accounts, certificates, storage, clustering, and backups.

New to the interface? Start with [What Is VM2Cloud VE](../01-Getting-Started/What-Is-VM2Cloud-VE.md) and the [Interface Tour](../01-Getting-Started/Interface-Tour.md).

---

## Source

These pages were converted from the standalone VM2Cloud VE installation site, verified against the screenshots captured during a real installation on 22 July 2026. Each page carries its own change history.
