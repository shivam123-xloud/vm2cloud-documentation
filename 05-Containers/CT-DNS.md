# Container DNS

---

## Overview

The **DNS** tab sets the search domain and nameservers a container uses to resolve names.

Containers have a dedicated DNS tab; virtual machines do not. A virtual machine resolves names using whatever its guest operating system is configured to use, set either inside the guest or through [Cloud-Init](../04-Virtual-Machines/Cloud-Init.md). A container's resolver configuration is written by the host, which is why it appears here.

By default a container inherits the host node's DNS settings. Values set on this tab override that inheritance for this container only.

---

## When to Use

Open the DNS tab when you need to:

* Point a container at specific nameservers rather than the host's.
* Set a search domain so short names resolve.
* Make a container use an internal DNS server for private zones.
* Troubleshoot a container that has network connectivity but cannot resolve names.
* Restore a container to the host's DNS settings.

---

## Prerequisites

Before configuring container DNS, ensure that:

* You have permission to modify the container.
* The container has working network configuration. See [CT Network](CT-Network.md).
* You know the nameserver addresses to use.
* Those nameservers are reachable from the container's network.
* You know the search domain, if one applies.

DNS depends on networking. A container that cannot reach its gateway will not resolve names no matter what is set here.

---

# Procedure

## Step 1: Open the DNS Tab

1. Log in to the VM2Cloud web interface.
2. Expand the node in the resource tree.
3. Select the container.
4. Click **DNS**.

The current settings are shown.

---

### Screenshot 1

**Container DNS Tab**

```text
[ Place Screenshot Here ]
```

> **Capture:** A container → DNS, showing the DNS domain and DNS servers fields with
> their current values and the **Edit** button.

---

## Step 2: Set the DNS Domain

1. Select **DNS domain**.
2. Click **Edit**.
3. Enter the search domain, for example `internal.example.com`.
4. Click **OK**.

The search domain lets short names resolve. With `internal.example.com` set, a lookup for `fileserver` is tried as `fileserver.internal.example.com`.

Leave it empty to inherit the host's setting.

---

### Screenshot 2

**Setting the DNS Domain**

```text
[ Place Screenshot Here ]
```

> **Capture:** The DNS domain edit dialog on a container.

---

## Step 3: Set the DNS Servers

1. Select **DNS servers**.
2. Click **Edit**.
3. Enter the nameserver addresses, most preferred first.
4. Click **OK**.

Provide at least two where possible. A single nameserver means every name lookup in the container fails when that server is unavailable, which presents as the application being broken rather than as a DNS problem.

Leave empty to inherit the host's nameservers.

> **Verify:** Confirm how multiple nameservers are entered in this deployment — whether
> separated by spaces, commas, or entered in separate fields.

---

### Screenshot 3

**Setting the DNS Servers**

```text
[ Place Screenshot Here ]
```

> **Capture:** The DNS servers edit dialog on a container, with more than one nameserver
> entered.

---

## Step 4: Verify Resolution Inside the Container

Open the [Console](Container-Console.md) and confirm names resolve.

Check the resolver configuration the host has written:

```bash
cat /etc/resolv.conf
```

The nameservers and search domain you set should appear there.

Then test an actual lookup for both an internal name and an external one. If internal names resolve but external ones do not, the nameserver is reachable but forwarding is not working — that is a DNS server problem rather than a container problem.

---

### Screenshot 4

**Resolver Configuration Inside the Container**

```text
[ Place Screenshot Here ]
```

> **Capture:** A container console session showing `/etc/resolv.conf` with the configured
> nameservers and search domain.

---

## Step 5: Restart if Needed

Most changes apply without a restart, because the host rewrites the container's resolver configuration.

If the container caches DNS internally, or runs its own resolver, restart it — or restart the service doing the caching — for the change to take effect.

---

# Configuration / Options

| Option | Description |
|---|---|
| **DNS domain** | Search domain appended to short names. Empty inherits the host setting. |
| **DNS servers** | Nameservers the container uses, in order of preference. Empty inherits the host setting. |

> **Verify:** Capture the container DNS tab and confirm the exact field labels and
> whether any options exist beyond these two.

---

# How Inheritance Works

```text
DNS fields empty on the container
              |
              v
   Container uses the host node's
   DNS domain and nameservers
              |
   Fields set on the container
              |
              v
   Container uses its own values,
   ignoring the host's
```

Inheritance is per field. Setting the servers while leaving the domain empty means the container uses your nameservers and the host's search domain.

For the host settings that are inherited, see [DNS](../03-Nodes/System/DNS.md).

---

# Verification

Verify the following:

* The DNS tab shows the intended values.
* `/etc/resolv.conf` inside the container matches them.
* Internal names resolve.
* External names resolve.
* Short names resolve, if a search domain is set.
* Applications inside the container reach the services they depend on by name.
* Resolution still works after a container restart.

Test both internal and external names. They fail differently and for different reasons.

---

# Common Issues

| Issue | Resolution |
|-------|------------|
| Names do not resolve at all | Check networking first. A container that cannot reach its gateway cannot reach a nameserver. See [CT Network](CT-Network.md). |
| Internal names fail, external work | The container is using public nameservers rather than the internal ones. Set the internal servers here. |
| External names fail, internal work | The internal nameserver is not forwarding. That is a DNS server problem, not a container one. |
| Short names do not resolve | No search domain is set. |
| Settings appear ignored | The container runs its own resolver or caches DNS. Restart the container or its resolver service. |
| `/etc/resolv.conf` does not match | Something inside the container is overwriting it. Check for a resolver manager running in the guest. |
| Resolution broke after a host change | The container was inheriting host settings that changed. Set explicit values here if the container should not follow the host. |
| Intermittent failures | Likely a single nameserver that is sometimes unavailable. Add a second. |
| Container reaches addresses but not names | This is the signature of a DNS-only problem. Networking is fine; resolution is not. |

---

# Best Practices

- Configure at least two nameservers. A single one turns any DNS outage into an application outage.
- Let containers inherit the host settings unless there is a reason not to — fewer places to update when nameservers change.
- Set explicit values for containers that must use internal DNS regardless of host configuration.
- Set a search domain when containers refer to each other by short name.
- Verify from the console rather than assuming the setting applied.
- Test internal and external resolution separately.
- Record which containers override host DNS, so a nameserver change does not miss them.
- Check DNS early when troubleshooting — "the application cannot connect" is frequently a resolution failure.

---

# Related Documentation

- [CT Network](CT-Network.md)
- [Container Console](Container-Console.md)
- [Container Options](CT-Options.md)
- [Create Container](Create-Container.md)
- [Container Troubleshooting](Container-Troubleshooting.md)
- [DNS](../03-Nodes/System/DNS.md)
- [Hosts](../03-Nodes/System/Hosts.md)
- [Cloud-Init](../04-Virtual-Machines/Cloud-Init.md)

---

# Summary

The DNS tab sets a container's search domain and nameservers, overriding the host node's settings for that container only. Containers have this tab because the host writes their resolver configuration; virtual machines configure DNS inside the guest or through Cloud-Init instead.

Leave the fields empty to inherit from the host, which is usually the right default. Set them explicitly when a container must use internal nameservers. Always configure more than one server, and verify from the console — a container that can reach addresses but not names has a DNS problem, not a networking one.
