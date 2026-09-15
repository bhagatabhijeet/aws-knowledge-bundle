---
type: Concept
title: "DynamoDB Mnemonics Cheat Sheet"
description: "Every DynamoDB mnemonic in this folder, on one page — read this the night before an exam or interview."
tags: [aws, dynamodb, mnemonics, cheatsheet]
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# The One-Page DynamoDB Cheat Sheet

## The valet garage, in one picture

| Valet-garage piece | DynamoDB concept |
|---|---|
| 🅿️ The garage itself | A Table |
| 🎫 The ticket you're handed | The Primary Key — Partition Key alone, or Partition Key + Sort Key |
| 🚗 A parked car | An Item |
| 🧾 What's written on the car's tag | Attributes — can vary item to item |
| 🏢 The section a ticket number maps to | A Partition — decided by hashing the Partition Key |
| 👉 Hand over a ticket, get one car (or one row) | GetItem / Query |
| 🔦 Check every car by hand, no ticket | Scan |
| 📇 A second board, organized by license plate | Global Secondary Index (GSI) |
| 📋 A second board, same ticket, different sort | Local Secondary Index (LSI) |
| 👷 How many valets are staffed | Capacity — RCUs / WCUs |
| 💳 Pay per car vs. a fixed crew on payroll | On-Demand vs. Provisioned capacity |
| 📸 A camera logging every arrival/departure | DynamoDB Streams |
| ⏰ Cars towed automatically after their time | Time to Live (TTL) |
| 🌎 The same valet network, multiple cities | Global Tables |
| ⚡ A fast-lane counter that remembers recent tickets | DAX |

## The mnemonics worth memorizing word-for-word

1. **"A relational database lets you ask almost any question, slowly if it must. A valet garage answers one question — instantly — and makes you work harder for anything else."** — the core NoSQL trade-off.
2. **"Every car needs a ticket. Nothing says every car needs the same tags."** — only the primary key is fixed; attributes are schemaless.
3. **"A simple key is one ticket number, held by only one car. A composite key is a ticket number plus a spot letter — many cars can share the number."** — Partition Key alone vs. Partition Key + Sort Key.
4. **"If every customer's ticket says 'VIP' instead of their own account number, every car funnels into one section — and it grinds to a halt."** — the hot-partition problem.
5. **"GetItem: one ticket, one car. Query: one ticket, one section. Scan: no ticket, the whole garage — every time."** — the three ways to read.
6. **"Eventually consistent: the valet who might be a few seconds behind. Strongly consistent: insisting on the valet who's certain — worth it only when you actually need it."** — the consistency trade-off.
7. **"A GSI is a whole second ticket board, its own staff, its own capacity, always a few steps behind. An LSI is the same ticket, re-sorted, sharing the same staff and budget — but you had to plan it before opening day."** — GSI vs. LSI.
8. **"Provisioned is hiring a fixed valet crew in advance. On-Demand is calling in exactly as many valets as show up."** — the two capacity modes.
9. **"A camera over every spot, logging every arrival and departure."** — DynamoDB Streams.
10. **"Cars left past their paid time get towed automatically — nobody walks the lot checking meters by hand."** — TTL.

## Read paths, at a glance

```
Know the exact key?              → GetItem            (1 item)
Know the Partition Key only?     → Query               (that partition only)
Need a different access pattern? → Secondary Index     (GSI or LSI)
None of the above?               → Scan                (the whole table — avoid in hot paths)
```

## Speed-round definitions

| Term | One line |
|---|---|
| Table | A collection of items, the DynamoDB equivalent of the whole garage |
| Item | One record; only the primary key is guaranteed to exist on every item |
| Attribute | A field on an item; can vary from item to item |
| Partition Key | The key attribute DynamoDB hashes to decide physical placement |
| Sort Key | An optional second key attribute; groups and orders items sharing a Partition Key |
| Partition | A physical storage unit an item lands in, based on its Partition Key's hash |
| GetItem | Fetch exactly one item by its full primary key |
| Query | Fetch every item sharing a Partition Key, optionally filtered by Sort Key |
| Scan | Read every item in the table, then filter |
| GSI | An index with its own independent Partition/Sort Key |
| LSI | An index sharing the base table's Partition Key, with a different Sort Key |
| RCU / WCU | Read/Write Capacity Unit — the unit of provisioned throughput |
| On-Demand | Pay-per-request capacity mode, no pre-planning |
| Streams | A time-ordered log of item-level changes |
| TTL | An attribute marking when an item should be auto-deleted |
| Global Tables | Multi-Region, multi-active replicated tables |
| DAX | An in-memory read cache in front of a table |

For full definitions of every term, see the [Glossary](glossary.md). To go deeper on any single row, jump back to the [folder index](index.md).
