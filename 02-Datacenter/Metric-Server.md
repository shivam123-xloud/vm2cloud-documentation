# Metric Server

---

## Overview

A **metric server** is an external system VM2Cloud VE sends performance data to — CPU, memory, storage, and network figures for nodes and guests — so that data can be stored long-term and graphed alongside everything else you monitor.

The built-in graphs on the Summary tabs are useful for a quick look, but they are limited: retention is short, there is no alerting on thresholds, and you cannot correlate VM2Cloud VE data against the rest of your infrastructure. Sending metrics out solves all three.

> **Verify:** The Datacenter menu was not fully visible in the screenshots available when
> this page was written, so **confirm the Metric Server panel exists in this deployment**
> and capture it before relying on this page.

---

## When to Use

Configure a metric server when:

* You need performance history longer than the built-in graphs retain.
* Alerting on thresholds is required — a node above 90% memory, a storage filling.
* VM2Cloud VE data should sit alongside other infrastructure in one dashboard.
* Capacity planning needs trends over months.
* An existing monitoring stack should cover the virtualization layer.

If you have no monitoring system and no plans for one, this adds nothing — the built-in graphs are the tool for that situation.

---

## Prerequisites

* You have administrator privileges.
* A metrics database is running and reachable from every node.
* You know its address, port, and protocol.
* You have any credentials or tokens it requires.
* Firewall rules permit outbound traffic from the nodes to it.
* The cluster has quorum.

---

# Procedure

## Step 1: Open the Metric Server Panel

1. Log in to the VM2Cloud VE web interface.
2. Select **Datacenter** in the resource tree.
3. Click **Metric Server**.

---

### Screenshot 1

**Metric Server Panel**

```text
[ Place Screenshot Here ]
```

> **Capture:** Datacenter → Metric Server, showing the server list and the **Add**,
> **Edit**, and **Remove** controls. If this panel does not exist, capture where metric
> export is configured instead.

---

## Step 2: Add a Metric Server

1. Click **Add**.
2. Select the server **type** matching your metrics database.
3. Enter a **Name**.
4. Enter the **Server** address and **Port**.
5. Configure the type-specific fields — protocol, database or bucket name, authentication.
6. Set the **Update interval** — how often metrics are sent.
7. Confirm **Enabled** is set.
8. Confirm.

> **Verify:** Capture the Add metric server dialog and confirm which server types are
> available in this deployment and their required fields.

---

### Screenshot 2

**Add Metric Server Dialog**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Add metric server dialog with a type selected, showing address, port,
> authentication, and interval fields.

---

## Step 3: Choose the Update Interval

The interval trades resolution against volume.

| Interval | Resolution | Consideration |
|---|---|---|
| 10 seconds | High | Large data volume; useful for short-lived spikes |
| 30 seconds | Good | A reasonable default for most environments |
| 60 seconds | Adequate | Lower volume; misses brief spikes |

A shorter interval multiplies storage consumption in the metrics database and network traffic from every node. Start at 30 seconds and shorten only if you find you are missing detail that matters.

---

## Step 4: Verify Metrics Are Arriving

1. Wait a few intervals.
2. Check the metrics database for incoming data.
3. Confirm data appears from **every** node, not just one.
4. Confirm guest metrics appear as well as node metrics.

Every node sends independently. A firewall rule blocking one node produces a monitoring gap that looks like that node being quiet rather than an obvious error.

---

### Screenshot 3

**Metrics Arriving**

```text
[ Place Screenshot Here ]
```

> **Capture:** The metrics database or dashboard showing incoming VM2Cloud VE data from
> multiple nodes.

---

## Step 5: Build Dashboards and Alerts

Once data is flowing, the useful work happens in the monitoring system rather than here.

Worth alerting on:

* Node memory and CPU sustained above a threshold.
* Storage approaching capacity — particularly backup storage.
* A node no longer reporting, which usually means it is down.
* Guest resource usage against configured limits.

That third one is the reason to alert on absence as well as on values. A node that stops sending metrics has stopped doing something, and a dashboard that only shows what is present will not highlight it.

---

# Configuration / Options

| Option | Description |
|---|---|
| **Type** | The metrics database protocol. |
| **Name** | Local identifier for this server. |
| **Server** | Hostname or IP of the metrics database. |
| **Port** | Port it listens on. |
| **Protocol** | Transport, where the type offers a choice. |
| **Database / Bucket** | Destination within the metrics database. |
| **Authentication** | Token or credentials, where required. |
| **Update interval** | How often metrics are sent. |
| **Enabled** | Whether export is active. |
| **MTU** | Packet size, where the transport is datagram-based. |

> **Verify:** Capture the complete dialog and confirm the exact field labels and
> available types.

---

# Verification

Verify the following:

* The metric server appears in the list and is enabled.
* Data is arriving in the metrics database.
* **Every** node is reporting, not just some.
* Guest metrics appear as well as node metrics.
* The update interval matches what was configured.
* Data volume is sustainable for the metrics database.
* Alerts fire when thresholds are crossed.
* An alert exists for a node that stops reporting.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| No data arriving | Check the address and port, and that nodes can reach the server. Check firewall rules. |
| Data from some nodes only | A per-node firewall or routing problem. Check the missing nodes specifically. |
| Data arrives then stops | The server may be full or rejecting writes. Check its own logs. |
| Metrics database filling quickly | The interval is too short. Lengthen it, or shorten retention in the database. |
| Guest metrics missing | Confirm the type exports guest data and the guests are running. |
| Authentication failures | Credentials or token may have expired. |
| Gaps in the data | Intermittent connectivity between nodes and the server. |
| Panel not present | Metric export may not be available in this deployment. See the Verify note above. |

---

# Best Practices

- Start at a 30-second interval and shorten only if you have a demonstrated need.
- Confirm **every** node is reporting, not just the first one you check.
- Alert on absence as well as on thresholds — a node that stops reporting is telling you something.
- Alert on backup storage capacity. Backup failure from a full storage is common and preventable.
- Keep retention in the metrics database long enough for capacity planning, which usually means months.
- Permit the outbound traffic explicitly if the node firewall is enabled. See [Node Firewall](../03-Nodes/Node-Firewall.md).
- Re-check after adding a node to the cluster — a new node needs no configuration here, but confirm it is actually reporting.
- Document what is being monitored and who receives the alerts.

---

# Related Documentation

- [Notifications](Notifications.md)
- [Datacenter Summary](Datacenter-Summary.md)
- [Node Summary](../03-Nodes/Node-Summary.md)
- [VM Summary](../04-Virtual-Machines/VM-Summary.md)
- [CT Summary](../05-Containers/CT-Summary.md)
- [Node Firewall](../03-Nodes/Node-Firewall.md)
- [Storage Content Browser](Storage/Storage-Content-Browser.md)
- [Datacenter Options](Options.md)

---

# Summary

A metric server receives performance data from VM2Cloud VE so it can be retained long-term, alerted on, and correlated with the rest of your infrastructure — the three things the built-in Summary graphs cannot do.

Configuration is a single entry naming the metrics database, its credentials, and how often to send. Two things are worth care afterwards: confirm **every** node is reporting rather than assuming, since a firewall rule blocking one node creates a silent gap; and alert on a node **ceasing** to report, not only on threshold values, because absence is often the more important signal.
