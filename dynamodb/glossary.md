---
type: Concept
title: "DynamoDB Glossary"
description: "Every DynamoDB term used in this folder, defined in one line, with its valet-garage analogy equivalent."
tags: [aws, dynamodb, glossary]
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# DynamoDB Glossary

| Term | Definition | Valet-garage analogy |
|---|---|---|
| **Table** | A collection of items, each identified by a primary key | The garage itself |
| **Item** | A single record in a table | A parked car |
| **Attribute** | A field on an item; can differ from item to item | What's written on a car's tag |
| **Primary key** | The required, unique identifier for every item — Partition Key alone, or Partition Key + Sort Key | The ticket number |
| **Partition Key** | The attribute DynamoDB hashes to decide an item's physical partition | The ticket number that decides the section |
| **Sort Key** | An optional second key attribute; orders and groups items sharing a Partition Key | The spot letter within a shared ticket number |
| **Simple primary key** | A primary key made of a Partition Key alone | One unique ticket number per car |
| **Composite primary key** | A primary key made of a Partition Key + Sort Key | A shared ticket number plus a unique spot letter |
| **Partition** | A physical storage/throughput unit that a hashed Partition Key value maps to | A numbered section of the garage |
| **Hot partition** | A partition receiving disproportionate traffic because too many items share one Partition Key value | One section jammed because every ticket says "VIP" |
| **GetItem** | Fetch exactly one item by its full primary key | Hand over one exact ticket, get one car |
| **Query** | Fetch every item sharing a Partition Key, optionally narrowed by a Sort Key condition | Hand over a ticket number, get every car in that section |
| **Scan** | Read every item in a table, then optionally filter | Check every car in the garage by hand |
| **Eventually consistent read** | A read that may briefly reflect slightly stale data; cheaper | The nearest valet, possibly a few seconds behind |
| **Strongly consistent read** | A read guaranteed to reflect every prior completed write; costs more | The one valet certain the newest arrival is logged |
| **Global Secondary Index (GSI)** | An index with its own independent Partition/Sort Key and capacity | A second ticket board, organized by license plate |
| **Local Secondary Index (LSI)** | An index sharing the base table's Partition Key, with a different Sort Key; must be created with the table | A second board, same ticket number, sorted differently |
| **Read Capacity Unit (RCU)** | One strongly consistent read per second for an item up to 4 KB (provisioned mode) | One valet fetching one small car, once a second |
| **Write Capacity Unit (WCU)** | One write per second for an item up to 1 KB (provisioned mode) | One valet parking one small car, once a second |
| **On-Demand capacity mode** | Pay-per-request throughput; scales automatically, no pre-planning | Calling in exactly as many valets as show up |
| **Provisioned capacity mode** | Reserved, fixed throughput, optionally paired with Auto Scaling | A fixed valet crew, hired in advance |
| **Throttling** | A request rejected because provisioned capacity was exceeded | A customer turned away because every valet is busy |
| **DynamoDB Streams** | A time-ordered log of item-level changes, consumable by services like Lambda | A security camera logging every arrival and departure |
| **Time to Live (TTL)** | An attribute marking when DynamoDB should automatically delete an item | The automatic tow truck |
| **Global Tables** | A table replicated across multiple Regions with multi-active writes | The same valet network, franchised across cities |
| **DAX (DynamoDB Accelerator)** | An optional in-memory cache in front of a table for microsecond reads | A fast-lane counter that remembers recent tickets |
| **Item size limit** | The maximum size of a single item — 400 KB | How much a car's tag can physically hold |

Back to the [folder index](index.md) · [mnemonics cheat sheet](mnemonics-cheatsheet.md).
