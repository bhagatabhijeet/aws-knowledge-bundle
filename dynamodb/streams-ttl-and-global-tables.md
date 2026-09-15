---
type: Concept
title: "Streams, TTL & Global Tables"
description: "The security camera logging every car that moves, the tow truck that clears out overstayed cars automatically, and the same valet network running in multiple cities at once."
tags: [aws, dynamodb, streams, ttl, global-tables, dax]
sources:
  - id: aws-ddb-streams
    resource: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html
    title: Amazon DynamoDB Developer Guide — DynamoDB Streams
  - id: aws-ddb-ttl
    resource: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/TTL.html
    title: Amazon DynamoDB Developer Guide — Time to Live
  - id: aws-ddb-global-tables
    resource: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GlobalTables.html
    title: Amazon DynamoDB Developer Guide — Global Tables
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Streams, TTL & Global Tables — The Camera, the Tow Truck, and the Franchise

## 📸 DynamoDB Streams — the security camera

**DynamoDB Streams** records a **time-ordered log of every item-level change** in a table — every insert, update, and delete — and makes that feed available for other services (most commonly **AWS Lambda**) to react to in near real time.

**Mnemonic:** *"A camera over every parking spot, logging every car that arrives, leaves, or gets swapped — so something else can react the instant it happens, instead of periodically checking the whole garage."*

Typical uses: triggering a notification when an order's status changes, keeping a search index or cache in sync with the table, replicating changes into another system.

## ⏰ Time to Live (TTL) — the automatic tow truck

**TTL** lets you mark an attribute on an item as an expiration timestamp. Once that time passes, DynamoDB automatically deletes the item — at no additional write cost, in the background, without you having to run any cleanup job yourself.

**Mnemonic:** *"Cars left past their paid time get towed automatically — nobody has to walk the lot checking meters by hand."*

Typical uses: expiring session data, temporary tokens, old shopping-cart items, or any data with a natural, known lifespan.

## 🌎 Global Tables — the same garage, franchised across cities

A **Global Table** replicates a table across **multiple AWS Regions**, with **multi-active** writes — an application can write to the table in any participating Region, and DynamoDB propagates that change to every other Region automatically.

**Mnemonic:** *"The same valet network, running identically in several cities — hand your ticket to any city's garage, and eventually every other city's garage agrees on where your car is."*

Typical uses: a globally distributed application where users in different Regions each need fast, local reads and writes against what functions as one logical table.

## ⚡ DAX — the private fast-lane counter

**DynamoDB Accelerator (DAX)** is an optional, fully managed, in-memory cache that sits in front of a table. For read-heavy workloads, DAX can serve repeated reads at **microsecond** latency, without hitting the table itself each time.

**Mnemonic:** *"A private counter near the entrance that already remembers the last few tickets it was handed — if yours is one of them, you don't even wait for the main garage."*

Typical uses: read-heavy applications where the same items are requested repeatedly (a product catalog, a leaderboard) and shaving milliseconds off every read genuinely matters.

## Quick comparison

| Feature | Solves | Runs |
|---|---|---|
| **Streams** | "React to every change, as it happens" | Continuously, feeding a consumer like Lambda |
| **TTL** | "Delete this automatically once it's no longer needed" | In the background, per item |
| **Global Tables** | "Serve the same data, fast, from multiple Regions" | Continuously, across Regions |
| **DAX** | "Serve the same hot reads even faster" | In front of the table, as a cache |

## Next up

Everything above is a concept — now build a real table and see it work: [Hands-On: Building a Customer Orders Table](hands-on-customer-orders-table.md).

[^aws-ddb-streams]: Amazon DynamoDB Developer Guide, "DynamoDB Streams."
[^aws-ddb-ttl]: Amazon DynamoDB Developer Guide, "Time to Live."
[^aws-ddb-global-tables]: Amazon DynamoDB Developer Guide, "Global Tables."
