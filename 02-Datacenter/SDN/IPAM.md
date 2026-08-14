# IPAM

---

## Overview

The **IPAM** panel shows which IP addresses have been allocated to which guests on SDN networks.

It is a read-only view of the address assignments SDN is tracking. Allocation happens elsewhere — when a guest attaches to a [VNet](VNets.md) whose subnet has a DHCP range, the address is recorded here.

Each entry shows the guest, its address, MAC, and gateway.

The IPAM **backend** — where these records are stored — is configured on [SDN Options](SDN-Options.md). The built-in backend is used by default and needs no setup.

> **Note:** DHCP integration for SDN is a newer capability. Confirm its status in your deployment before depending on it for production addressing.

---

## When to Use

Open the IPAM panel when you need to:

* Find which address a guest was given.
* Check whether an address is already allocated.
* Confirm DHCP is issuing addresses on a VNet.
* Investigate a duplicate-address problem.
* See which guests hold addresses in a subnet.

---

## Prerequisites

* SDN is configured with at least one [zone](Zones.md) and [VNet](VNets.md).
* The VNet has a subnet with a DHCP range, if you expect automatic allocation.
* For automatic leasing, the DHCP service is installed on every node — see below.

---

# Procedure

## Step 1: Open the IPAM Panel

1. Log in to the VM2Cloud VE web interface.
2. Select **Datacenter**.
3. Expand **SDN**.
4. Click **IPAM**.

---

### Screenshot 1

**IPAM Panel**

```text
[ Place Screenshot Here ]
```

> **Capture:** Datacenter → SDN → IPAM with at least one guest holding an address,
> showing the Name/VMID, IP Address, MAC, Gateway, and Actions columns plus the
> **Reload** button.

---

## Step 2: Read the Entries

| Column | Meaning |
|---|---|
| **Name / VMID** | The guest holding the address. |
| **IP Address** | The allocated address. |
| **MAC** | The guest interface the address is bound to. |
| **Gateway** | The gateway for that subnet. |
| **Actions** | Per-entry operations. |

An empty panel means no addresses have been allocated — either no subnet has a DHCP range, or no guest has requested one yet.

Click **Reload** to refresh after starting a guest.

---

## Step 3: Enable DHCP Leasing

Addresses are only issued automatically when the DHCP service is available on each node.

1. Install the DHCP service on **every** node.
2. Disable its default standalone instance, so SDN controls it.
3. Configure a DHCP range on the [subnet](VNets.md).
4. **Apply** the SDN configuration.
5. Start a guest on the VNet and confirm it receives an address.

> **Verify:** Confirm the exact package name and the steps to disable the default
> instance in this deployment, then record them here.

Miss the per-node installation and DHCP works on some nodes and not others — which presents as a guest that gets an address until it migrates.

---

## Step 4: Investigate an Address

1. Find the guest by name or VMID.
2. Confirm the address matches what the guest reports internally — see [CT Network](../../05-Containers/CT-Network.md) or the guest console.
3. If they differ, the guest is configured statically and is not using the allocation recorded here.

A mismatch between this panel and the guest is the usual sign of a static address overlapping a DHCP range.

---

# Configuration / Options

The panel itself is read-only. What it displays is governed elsewhere:

| Setting | Where |
|---|---|
| IPAM backend | [SDN Options](SDN-Options.md) → IPAM |
| Which backend a zone uses | [Zones](Zones.md) |
| Subnet, gateway, DHCP range | [VNets](VNets.md) → Subnets |

> **Verify:** Confirm which actions are available in the Actions column.

---

# Verification

Verify the following:

* The panel lists the guests you expect.
* Addresses fall inside the configured subnet.
* No address appears twice.
* Gateways match the subnet configuration.
* A newly started guest appears after **Reload**.
* A migrated guest keeps its address.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Panel is empty | No subnet has a DHCP range, or no guest has requested an address. |
| Guest gets no address | The DHCP service may not be installed on that node. Check every node, not just the first. |
| Guest loses its address after migrating | The DHCP service is missing on the target node. |
| Address here differs from the guest | The guest is statically configured. Reconcile, or remove the static address. |
| Duplicate address | A static address overlaps the DHCP range, or an external system is also allocating. |
| Entries not updating | Click **Reload**. |
| External backend shows nothing | Check its URL and token on [SDN Options](SDN-Options.md). |

---

# Best Practices

- Install the DHCP service on **every** node at the same time. Partial installation produces failures that only appear after a migration.
- Keep static addresses outside any DHCP range.
- Use one source of truth for each range — either SDN or an external system, never both.
- Check this panel before assigning a static address, to confirm it is free.
- Reload before concluding an address is missing.
- Treat the built-in backend as sufficient unless an external system already owns your addressing.

---

# Related Documentation

- [SDN Overview](SDN-Overview.md)
- [SDN Options](SDN-Options.md)
- [VNets](VNets.md)
- [Zones](Zones.md)
- [CT Network](../../05-Containers/CT-Network.md)
- [Manage VM Hardware](../../04-Virtual-Machines/Manage-VM-Hardware.md)
- [Network Troubleshooting](../../03-Nodes/System/Network/Network-Troubleshooting.md)

---

# Summary

The IPAM panel is a read-only view of the addresses SDN has allocated to guests, showing the guest, address, MAC, and gateway for each. Allocation is driven by a DHCP range on a VNet's subnet, and the records are held in whichever IPAM backend the zone uses — the built-in one by default.

The failure worth anticipating is partial DHCP deployment. The leasing service must be present on **every** node, and when it is missing from one, guests get addresses normally until they migrate to that node and then do not.
