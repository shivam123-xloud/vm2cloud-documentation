# Notifications

---

## Overview

**Notifications** control how VM2Cloud tells you when something happens — a backup failing, a node going down, a replication job stopping.

The value is straightforward: an environment that only reports problems inside its own interface reports them to nobody. Backups that stopped running three weeks ago look exactly like backups that ran fine, unless something tells you otherwise.

Configuration has two parts:

| Part | Purpose |
|---|---|
| **Notification targets** | Where a notification goes — email, or another endpoint |
| **Notification matchers** | Which events go to which target |

> **Verify:** This page describes the notification system as it exists on the platform.
> The Datacenter menu was not fully visible in the screenshots available when this was
> written, so **confirm the Notifications panel exists in this deployment** and capture
> it. If notifications are configured elsewhere — for example only per backup job — this
> page should be adjusted or merged into [Create Backup Job](Backup/Create-Backup-Job.md).

---

## When to Use

Configure notifications when:

* Backup failures must reach someone.
* Node or cluster problems should raise an alert.
* Replication failures need reporting.
* Different teams should receive different alerts.
* Alerts should reach a chat system or an on-call tool rather than email.

This is not optional infrastructure for a production environment. Silent failure is the worst outcome, and it is the default without this.

---

## Prerequisites

* You have administrator privileges.
* The cluster has quorum.
* For email: the nodes can send mail, and you know the destination address.
* For other targets: you have the endpoint URL and any credentials.

---

# Procedure

## Step 1: Open the Notifications Panel

1. Log in to the VM2Cloud web interface.
2. Select **Datacenter** in the resource tree.
3. Click **Notifications**.

---

### Screenshot 1

**Notifications Panel**

```text
[ Place Screenshot Here ]
```

> **Capture:** Datacenter → Notifications, showing the target list and matcher list with
> their controls. If this panel does not exist, capture where notification settings live
> instead.

---

## Step 2: Add a Notification Target

1. In the targets section, click **Add**.
2. Select the target type.
3. Enter a **Name**.
4. Configure the type-specific fields:
   - **Email** — one or more recipient addresses.
   - **Webhook or gateway** — endpoint URL, method, and any authentication.
5. Add a **Comment** describing who receives this.
6. Confirm.

> **Verify:** Capture the Add target dialog and confirm which target types are available
> in this deployment.

---

### Screenshot 2

**Add Notification Target**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Add notification target dialog with a type selected, showing its
> configuration fields.

---

## Step 3: Send a Test Notification

Do this before relying on it.

1. Select the target.
2. Click the test action.
3. Confirm the message arrives.

A target that has never been tested is an assumption. Mail is routinely blocked, filtered, or silently dropped, and you will not discover that during an incident at a useful moment.

---

### Screenshot 3

**Test Notification**

```text
[ Place Screenshot Here ]
```

> **Capture:** The result of sending a test notification, and the received message.

---

## Step 4: Configure a Matcher

Matchers decide which events reach which target.

1. In the matchers section, click **Add**.
2. Enter a **Name**.
3. Set the conditions — for example by severity, or by event type.
4. Select the **Target**.
5. Confirm.

Start simple: one matcher sending everything important to one address. Split it later if different teams need different alerts. A complicated matcher set built up front tends to have gaps nobody notices.

---

### Screenshot 4

**Notification Matcher**

```text
[ Place Screenshot Here ]
```

> **Capture:** The Add matcher dialog, showing the condition fields and target selector.

---

## Step 5: Verify With a Real Event

Trigger something that should notify — a deliberately failing backup job against a nonexistent storage is a reliable test — and confirm the notification arrives.

Testing the target proves the transport works. Testing an actual event proves the matcher routes it.

---

# Configuration / Options

### Target

| Option | Description |
|---|---|
| **Name** | Identifier used when selecting the target in a matcher. |
| **Type** | Email, webhook, or another supported endpoint. |
| **Recipients / Endpoint** | Where notifications are delivered. |
| **Comment** | Who this target reaches. Worth filling in. |
| **Enabled** | Whether the target is active. |

### Matcher

| Option | Description |
|---|---|
| **Name** | Identifier for the rule. |
| **Match conditions** | Which events this matcher applies to. |
| **Target** | Where matching notifications are sent. |
| **Enabled** | Whether the matcher is active. |

> **Verify:** Capture both dialogs and confirm the exact field labels and available match
> conditions.

---

# Verification

Verify the following:

* Targets are configured and enabled.
* A test notification reaches each target.
* Matchers route the intended events.
* A real failure produces a notification.
* Notifications reach a monitored destination, not an unread mailbox.
* Backup job notifications arrive, since these matter most.
* Nobody is receiving so many notifications that they stop reading them.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Test notification not received | Check spam filtering, and whether the nodes can send mail at all. |
| Test works but real events do not notify | No matcher routes those events, or the matcher is disabled. |
| Notifications stopped arriving | Check the target is still enabled and the endpoint still valid. |
| Too many notifications | Narrow the matcher conditions. Alert fatigue makes the whole system useless. |
| Backup failures not reported | Check the backup job's own notification setting as well as the matcher. See [Create Backup Job](Backup/Create-Backup-Job.md). |
| Notifications only reach one person | Use a distribution list or shared mailbox, not an individual. |
| Webhook target failing | Check the endpoint URL, authentication, and outbound connectivity from the nodes. |
| Panel not present | Notification configuration may live elsewhere in this deployment. See the Verify note above. |

---

# Best Practices

- **Configure notifications before you need them.** Their entire purpose is telling you about things you were not watching.
- Test every target when you create it, and again periodically.
- Send to a shared mailbox or distribution list, never one person's address.
- Start with one broad matcher and refine, rather than building a complex set up front.
- Verify with a real failure, not only a test message.
- Keep the volume low enough that notifications are still read. Alert fatigue defeats the purpose.
- Route genuinely urgent alerts to something that interrupts — chat or paging — rather than email alone.
- Record who receives what, so the routing survives staff changes.
- Re-test after any mail or network change.

---

# Related Documentation

- [Create Backup Job](Backup/Create-Backup-Job.md)
- [Manage Backup Job](Backup/Manage-Backup-Job.md)
- [Datacenter Options](Options.md)
- [Metric Server](Metric-Server.md)
- [Task Log and Cluster Log](../01-Getting-Started/Task-Log-and-Cluster-Log.md)
- [Replication Status](Replication/Replication-Status.md)
- [HA Troubleshooting](HA/HA-Troubleshooting.md)

---

# Summary

Notifications route VM2Cloud events to somewhere people actually look. Targets define where messages go; matchers define which events reach which target.

The failure this prevents is silent: backups that stopped weeks ago look identical to backups that are working, unless something reports otherwise. Configure at least one target, test it when you create it, and then verify with a real failure — testing the target proves the transport works, but only a real event proves the routing does. Send to a shared address rather than an individual, and keep the volume low enough that the messages still get read.
