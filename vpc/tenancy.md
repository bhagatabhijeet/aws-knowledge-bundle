---
type: Concept
title: "Tenancy"
description: "Whose building your house actually sits in: whether the physical server underneath your instance is shared with other AWS customers, reserved just for your account, or a specific building you can see the blueprints for."
tags: [aws, vpc, tenancy, dedicated-instances, dedicated-hosts, ec2]
sources:
  - id: aws-dedicated-instances
    resource: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/dedicated-instance.html
    title: Amazon EC2 User Guide — Dedicated Instances
  - id: aws-dedicated-hosts
    resource: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/how-dedicated-hosts-work.html
    title: Amazon EC2 User Guide — Dedicated Hosts
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Tenancy — Whose Building Your House Sits In

## 🏢 The mnemonic

Everything else in this folder is about the *network* around your instance — which street it's on, which gate it uses, which lock is on its door. **Tenancy** is a different question entirely: **whose physical building is your house actually built inside?**

- **Default tenancy** — your house sits in a **shared apartment building**. Other residents (other AWS accounts' instances) may live in the same building, on the same physical server — your unit is still fully locked and isolated from theirs, but you don't own the building.
- **Dedicated Instance tenancy** — you lease an entire **private building reserved only for your community's residents**. No other AWS account's instances share that physical server — but building management (AWS) still decides which room within it you get, and can move you between their dedicated buildings.
- **Dedicated Host tenancy** — you lease **one specific, named building, in full** — you can see its blueprints (the physical sockets and cores), and you decide exactly which resident goes in which room.

## The three values, compared

| | Default | Dedicated Instance | Dedicated Host |
|---|---|---|---|
| Physical server shared with other AWS accounts? | Yes, possibly | No — never | No — never |
| Visibility into the physical host (sockets, cores, host ID)? | No | No | **Yes** |
| Control over instance placement on a specific host? | No | No | **Yes** — "host affinity" pins an instance to one physical server |
| Typical use case | The default — nearly everything | Regulatory/compliance rules requiring no shared hardware | Per-socket/per-core software licensing (BYOL) tied to physical hardware |
| Relative cost | Baseline | A surcharge over default | The highest — you're paying for a whole physical server |

**Mnemonic:** *"Default: you don't know or care who else lives in the building. Dedicated Instance: you know for certain nobody else does, but you don't get a say in which room. Dedicated Host: you hold the deed to the building and assign the rooms yourself."*

## Where tenancy is actually set

Tenancy can be configured at **two** levels, and they interact:

1. **The VPC's own `instance tenancy` attribute** — every VPC has this setting, `default` or `dedicated`. It's the community's own zoning rule.
2. **Per-instance, at launch** — you can independently choose `default`, `dedicated`, or `host` for a specific instance when you launch it.

**The zoning rule wins when it's stricter:** if a VPC's instance tenancy attribute is set to `dedicated`, then **every** instance launched into that VPC must use `dedicated` or `host` tenancy — you cannot launch a `default`-tenancy instance into a VPC that's zoned `dedicated`, no matter what you request at launch. A VPC left at the (normal) `default` setting places no such restriction — instances launched into it can freely be `default`, `dedicated`, or `host`.

**Mnemonic:** *"A community zoned 'no shared buildings allowed' overrides any individual resident's request to live in one — but a normally-zoned community lets residents choose for themselves."*

## Dedicated Instance vs. Dedicated Host, in more depth

These two are easy to conflate, but they solve different problems:

- A **Dedicated Instance** guarantees isolation from other AWS accounts' hardware — nothing more. AWS still manages which physical server your instance lands on behind the scenes, and that can change (for example, across a stop/start).
- A **Dedicated Host** goes further: you're allocated an actual, identifiable physical server, with visibility into its socket and core counts and a stable host ID, and you can explicitly control (or pin) which instances run on it. This is what makes it suitable for **Bring Your Own License (BYOL)** scenarios where software licensing (some Windows Server and SQL Server licenses, for example) is tied to physical sockets or cores rather than to a virtual instance — the license needs a real, stable piece of hardware to attach to.

**Mnemonic:** *"A Dedicated Instance proves nobody else lives in the building. A Dedicated Host hands you the building's floor plan and the keys to every room."*

## Why this matters, and when to actually reach for it

For the overwhelming majority of workloads, **default tenancy is correct** — AWS's virtualization (and the Nitro hypervisor underneath modern instance types) already provides strong isolation between accounts sharing the same physical hardware, and default tenancy costs nothing extra. Reach for Dedicated Instances or Dedicated Hosts only when:

- A **compliance or regulatory requirement** explicitly mandates that your workloads never share a physical server with another customer's, or
- A **software license** is contractually or technically tied to a specific physical socket, core, or server (typically Dedicated Host territory, not just Dedicated Instance)

**Mnemonic:** *"Don't pay for a private building because it sounds more secure — pay for one because a rule or a license actually requires it."*

## Next up

Tenancy controls physical isolation between AWS accounts. For the connections between your own VPCs and the outside world, see [VPC Peering & Endpoints](vpc-peering-and-endpoints.md).

[^aws-dedicated-instances]: Amazon EC2 User Guide, "Dedicated Instances."
[^aws-dedicated-hosts]: Amazon EC2 User Guide, "Dedicated Hosts."
