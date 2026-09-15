---
type: Concept
title: "Capacity Modes"
description: "Staffing the garage: pay per car fetched, on demand, or keep a fixed valet crew on payroll around the clock — On-Demand vs. Provisioned capacity, and what a capacity unit actually measures."
tags: [aws, dynamodb, capacity, on-demand, provisioned, rcu, wcu, auto-scaling]
sources:
  - id: aws-ddb-capacity
    resource: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ReadWriteCapacityMode.html
    title: Amazon DynamoDB Developer Guide — Read/write capacity modes
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Capacity Modes — Staffing the Garage

## 👷 The mnemonic

Every request into the garage — fetching a car, parking a new one — takes a valet's time. **Capacity** is how many valets you've staffed to handle that traffic. You can either keep a **fixed crew** on payroll around the clock, sized for how busy you expect to be, or you can pay a **rotating pool** per car handled and let the garage scale its staffing automatically to match however busy it actually gets, minute to minute.

## The two modes

| | Provisioned | On-Demand |
|---|---|---|
| How you pay | Reserve a fixed throughput (Read/Write Capacity Units) ahead of time | Pay per request actually made — no pre-planning |
| Best for | Predictable, steady traffic, where reserving capacity ahead of time is cheaper | Unpredictable or spiky traffic, or when you'd rather not capacity-plan at all |
| Scaling | Can pair with **Auto Scaling** to adjust the reserved amount automatically within limits you set | Scales automatically and near-instantly to actual demand |
| Risk if under-provisioned | Requests can be **throttled** once you exceed the reserved capacity | Scales to meet demand — no capacity ceiling to misjudge |

**Mnemonic:** *"Provisioned is hiring a fixed valet crew for the shift, in advance, hoping you guessed the crowd size right. On-Demand is calling in exactly as many valets as show up, no guessing required — for a higher price per car."*

## What a capacity unit actually measures

In **Provisioned** mode, throughput is measured in:

| Unit | What one unit buys you |
|---|---|
| **Read Capacity Unit (RCU)** | One strongly consistent read per second, for an item up to 4 KB (an eventually consistent read uses **half** an RCU for the same item) |
| **Write Capacity Unit (WCU)** | One write per second, for an item up to 1 KB |

Larger items consume proportionally more units — a strongly consistent read of an 8 KB item costs 2 RCUs, not 1.

**Mnemonic:** *"One RCU is one valet fetching one small car, once a second. Hand them a bigger car (a bigger item), and it takes more than one valet's worth of effort."*

## Throttling: what happens when you run out of staff

If provisioned capacity is exhausted, further requests are **throttled** — DynamoDB returns an error rather than serving the request, until capacity frees up (or Auto Scaling adds more). This is the direct cost of guessing your crew size wrong in Provisioned mode; On-Demand mode is specifically designed to avoid this failure mode by never imposing a fixed ceiling in the first place.

**Mnemonic:** *"Run out of staffed valets, and the next customer waits at the gate — throttled, not served — until a valet frees up."*

## Choosing between them

- **On-Demand** is the sensible default for new tables, unpredictable workloads, and anything where operational simplicity matters more than squeezing out the lowest possible per-request cost.
- **Provisioned (with Auto Scaling)** becomes worth the extra planning once traffic is steady and predictable enough that reserving capacity ahead of time is measurably cheaper than paying On-Demand rates for the same volume.

## Next up

Capacity governs the base table and any table-level activity. See how a second, independent staffing budget applies to indexes and beyond in [Secondary Indexes](secondary-indexes.md) — or continue to what happens after an item is written: [Streams, TTL & Global Tables](streams-ttl-and-global-tables.md).

[^aws-ddb-capacity]: Amazon DynamoDB Developer Guide, "Read/write capacity modes."
