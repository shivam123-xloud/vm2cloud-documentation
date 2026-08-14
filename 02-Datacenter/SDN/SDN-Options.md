# SDN Options

---

## Overview

The **Options** panel holds the backend services SDN depends on. Nothing here creates a network — these are the components that zones and VNets consume.

Three sections:

| Section | Provides |
|---|---|
| **Controllers** | The routing control plane for EVPN and BGP zones |
| **IPAM** | Where IP address assignments are recorded |
| **DNS** | Automatic DNS registration for guests on SDN networks |

A simple or VLAN zone needs none of these. They matter once you move to EVPN, or want addresses and DNS records managed automatically.

For the SDN model as a whole, see [SDN Overview](SDN-Overview.md).

---

## When to Use

Open Options when you need to:

* Add a controller before creating an EVPN zone.
* Point IPAM at an external address-management system.
* Register guest addresses in DNS automatically.
* Check which IPAM backend a zone is using.

---

## Prerequisites

* You have administrator privileges.
* The cluster has quorum.
* For an external IPAM or DNS backend: its URL and an API token.
* For a controller: the BGP design, including AS numbers and peers.

---

# Controllers

A controller runs the routing protocol that makes an EVPN zone work. Without one, an EVPN zone has no control plane and cannot distribute routes.

Simple, VLAN, QinQ, and VXLAN zones do **not** use a controller.

## Add a controller

1. Click **Add** in the Controllers section.
2. Select the controller type.
3. Enter the required fields — typically an AS number and peer addresses.
4. Confirm.
5. Return to the SDN panel and **Apply**.

> **Verify:** Capture the Add controller dropdown and confirm which controller types are
> offered in this deployment, and the fields each one requires.

---

### Screenshot 1

**SDN Options Panel**

```text
[ Place Screenshot Here ]
```

> **Capture:** Datacenter → SDN → Options, showing all three sections — Controllers,
> IPAM, and DNS — with their **Add**, **Remove**, and **Edit** controls.

---

# IPAM

**IP Address Management** records which addresses are assigned to which guests on SDN networks. When a subnet has a DHCP range, this is what tracks the allocations.

A built-in backend named `pve` exists by default and needs no configuration. Its type shows as **VM2Cloud**.

| Backend | Use |
|---|---|
| **Built-in** (`pve`) | Default. Addresses tracked inside the cluster. Sufficient for most deployments. |
| **phpIPAM** | Where phpIPAM is the organisation's source of truth for addressing. |
| **NetBox** | Where NetBox is the source of truth. |

Point SDN at an external system only when that system already owns your addressing. Running two sources of truth produces conflicts that surface as duplicate addresses.

## Add an external IPAM backend

1. Click **Add** in the IPAM section.
2. Select the backend type.
3. Enter its URL and API token.
4. Confirm and **Apply**.

The backend then becomes selectable when creating a [zone](Zones.md).

> **Verify:** Confirm the external IPAM backends offered in this deployment and the
> exact fields each requires.

---

### Screenshot 2

**IPAM Backends**

```text
[ Place Screenshot Here ]
```

> **Capture:** The IPAM section of SDN → Options, showing the default `pve` entry with
> its type, and the Add dropdown open.

---

# DNS

A DNS backend lets SDN register guest addresses automatically as they are assigned, so a guest becomes resolvable by name without anyone editing a zone file.

This is optional and unconfigured by default.

## Add a DNS backend

1. Click **Add** in the DNS section.
2. Select the backend type.
3. Enter its URL, API key, and the zone to register into.
4. Confirm and **Apply**.
5. On a [subnet](VNets.md), set the DNS prefix so records are created under it.

Registration only happens for subnets configured to use it — adding the backend alone changes nothing.

> **Verify:** Confirm which DNS backends are available and the fields each requires.

---

### Screenshot 3

**DNS Backends**

```text
[ Place Screenshot Here ]
```

> **Capture:** The DNS section of SDN → Options with the Add dropdown open, showing the
> available backend types.

---

# Configuration / Options

| Section | Field | Description |
|---|---|---|
| **Controllers** | Type | Routing protocol implementation for EVPN zones. |
| | AS number | Autonomous system number for BGP. |
| | Peers | Addresses of the BGP peers. |
| **IPAM** | ID | Identifier referenced when creating a zone. |
| | Type | Built-in or an external system. |
| | URL / Token | Connection details for an external backend. |
| **DNS** | ID | Identifier referenced by subnets. |
| | Type | DNS backend implementation. |
| | URL / Key | Connection details. |

> **Verify:** Capture each Add dialog and confirm the exact field labels.

---

# Verification

Verify the following:

* The default `pve` IPAM entry is present.
* Any external backend shows no connection error.
* A controller exists before creating an EVPN zone.
* **Apply** completed on every node.
* Addresses assigned from a DHCP-enabled subnet appear in [IPAM](IPAM.md).
* DNS records are created, if a DNS backend is configured.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| EVPN zone will not work | No controller is configured, or its AS number and peers are wrong. |
| External IPAM unreachable | Check the URL, the API token, and outbound connectivity from the nodes. |
| Addresses not recorded | The subnet may have no DHCP range. See [VNets](VNets.md). |
| Duplicate addresses | Two systems are assigning from the same range. Decide which one owns it. |
| DNS records not created | No DNS prefix is set on the subnet, or the backend credentials are wrong. |
| Changes have no effect | **Apply** was not clicked. SDN configuration is staged. |
| Cannot remove an IPAM entry | A zone still references it. |

---

# Best Practices

- Keep the built-in IPAM unless an external system genuinely owns your addressing.
- Never run two sources of truth for the same address range.
- Add the controller **before** the EVPN zone that needs it.
- Use a scoped API token for external backends, not a full-access credential.
- Record which zones use which backend, so a backend change is not a surprise.
- Apply and verify on every node after any change here.

---

# Related Documentation

- [SDN Overview](SDN-Overview.md)
- [Zones](Zones.md)
- [VNets](VNets.md)
- [IPAM](IPAM.md)
- [Network Overview](../../03-Nodes/System/Network/Network-Overview.md)
- [DNS](../../03-Nodes/System/DNS.md)

---

# Summary

The SDN Options panel configures the backends SDN uses rather than the networks themselves: controllers for EVPN routing, IPAM for recording address assignments, and DNS for automatic name registration.

Most deployments need none of it. The built-in `pve` IPAM works without configuration, and simple, VLAN, and VXLAN zones need no controller. Reach for this panel when you deploy EVPN, or when an existing address-management or DNS system should own what SDN assigns — and in that case, make sure only one system owns each range.
