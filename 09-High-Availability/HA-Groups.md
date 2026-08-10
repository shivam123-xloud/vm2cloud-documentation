# HA Groups

---

## Overview

HA Groups are a legacy mechanism for controlling the placement of High Availability resources across cluster nodes.

HA Groups allow administrators to define a group of nodes and assign priorities to those nodes. HA resources can then be associated with the group to influence where the resource should run.

> **Important:** HA Groups are deprecated in current VM2Cloud versions based on recent Proxmox VE releases. New HA configurations should use **Node Affinity** rules instead. This document is retained to help administrators understand and manage existing legacy HA Group configurations.

HA Groups do not provide storage replication. They only define placement preferences and restrictions for HA resources.

---

## When to Use

Use this document when:

- An existing cluster contains legacy HA Groups.
- You need to understand an existing HA Group configuration.
- You need to maintain or troubleshoot a legacy HA deployment.
- You are migrating an older HA configuration to current node-affinity rules.
- You need to understand how existing HA resources are associated with legacy groups.

For new HA deployments, use:

```text
09-High-Availability/Node-Affinity.md
