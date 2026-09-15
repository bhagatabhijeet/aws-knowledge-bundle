---
type: Concept
title: "Designing for High Availability"
description: "Building across boroughs and cities on purpose: what Multi-AZ actually buys you, when Multi-Region is worth the cost, and the fault-isolation boundaries that make either one work."
tags: [aws, global-infrastructure, high-availability, disaster-recovery, multi-az, multi-region]
sources:
  - id: aws-ha-well-architected
    resource: https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/welcome.html
    title: AWS Well-Architected Framework — Reliability Pillar
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Designing for High Availability — Building Across Boroughs and Cities

## 🏘️👯 The mnemonic

The whole point of boroughs (AZs) and cities (Regions) being independent is that you get to **choose** how much of the map your application depends on. Depend on one borough, and that borough's blackout is your outage. Spread across every borough in the city, and only a city-wide disaster can stop you. Spread across two cities entirely, and even that won't.

## Multi-AZ: the default, cheap win

Because Availability Zones in the same Region are connected by private, high-bandwidth, low-latency links, spreading a workload across two or more AZs costs you very little in latency — but removes an entire class of failure (one building, one power grid, one fire) from your risk.

| Pattern | What it looks like |
|---|---|
| **Multi-AZ compute** | An Auto Scaling group and load balancer spanning two or more AZs, so an instance failure — or a whole AZ failure — just shifts traffic to the survivors |
| **Multi-AZ data** | A database (e.g., RDS Multi-AZ) synchronously replicating to a standby in a second AZ, promoted automatically if the primary's AZ fails |

**Mnemonic:** *"Multi-AZ is the cheapest insurance policy in AWS: same city, same low latency, one fewer way to go down."*

## Multi-Region: for when a whole city can go dark

A Region is designed to be fully isolated from every other Region — which is exactly why a Region-scale event (a natural disaster, a region-wide control-plane issue) can't spread to a second Region either. Multi-Region is the answer when:

- Your compliance requirements demand a live, geographically separate copy of your data
- Your users are spread globally enough that no single Region gives everyone acceptable latency
- Your business genuinely cannot tolerate the (rare, but non-zero) risk of losing an entire Region

Multi-Region is a much bigger commitment than Multi-AZ: cross-Region data replication has real latency, cost, and consistency trade-offs, and your operational surface — monitoring, deployment, failover testing — effectively doubles.

**Mnemonic:** *"Twin cities built continents apart never share one earthquake — but somebody still has to maintain two cities."*

## Common disaster-recovery patterns, by cost and complexity

| Pattern | What's running in the second Region, at rest | Recovery speed | Relative cost |
|---|---|---|---|
| **Backup & restore** | Nothing — just backups | Slowest | Cheapest |
| **Pilot light** | Core data replicated, minimal compute idling | Slow-to-moderate | Low |
| **Warm standby** | A scaled-down but fully functional copy running | Fast | Moderate |
| **Active-active (multi-Region)** | A full-scale copy actively serving traffic | Fastest (near-instant) | Highest |

**Mnemonic:** *"Pilot light keeps the gas on; warm standby keeps the lights dim; active-active keeps both cities fully open for business, all the time."*

## The fault-isolation boundary is the whole point

None of this works unless the boundary is real. Before you count on Multi-AZ or Multi-Region protecting you, check that every layer of your architecture actually respects the boundary you're relying on:

- A **shared NAT Gateway** placed in only one AZ quietly reintroduces a single point of failure for every "Multi-AZ" subnet that routes through it
- A **global service** (IAM, Route 53, CloudFront) is shared across all Regions by design — a Multi-Region architecture doesn't protect you from an outage in a global service, because there's only one
- **Application-level assumptions** — a hardcoded single database endpoint, a cache that isn't replicated — can silently turn a Multi-AZ deployment back into a single point of failure

**Mnemonic:** *"A fault-isolation boundary you didn't actually build into every layer isn't a boundary — it's a hope."*

## Next up

Turn all of this into concrete habits: [Best Practices](best-practices.md).

[^aws-ha-well-architected]: AWS Well-Architected Framework, "Reliability Pillar."
