---
type: Directory Index
title: "Amazon DynamoDB — Knowledge Folder"
description: "Index of the Amazon DynamoDB concept docs — tables, keys, capacity, indexes, and a hands-on tutorial — taught through the Valet Parking Garage analogy."
tags: [aws, dynamodb, nosql, database, index]
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Amazon DynamoDB — The Valet Parking Garage

![The DynamoDB Valet Parking Garage](assets/images/valet-garage-overview.svg)

**DynamoDB is a valet parking garage: hand over the right ticket, and the valet runs straight to your car — instantly, whether the garage holds a hundred cars or a hundred billion.**

Every car (an **item**) sits in the garage (a **table**), identified by the ticket number stamped on it (the **primary key**). The garage doesn't organize cars alphabetically or by search — it uses your ticket number to compute exactly which numbered section (a **partition**) the car is parked in, so a valet with the right ticket never has to look at another car. Ask for a car by its ticket, and you get it in the time it takes to walk to one spot — a **Query**. Wander the garage without a ticket, checking every car one by one, and that's a **Scan** — technically possible, but exactly the kind of thing you avoid if you can help it.

| Valet-garage piece | DynamoDB concept |
|---|---|
| 🅿️ The garage itself | A **Table** |
| 🎫 The ticket you're handed | The **Primary Key** — a Partition Key alone, or Partition Key + Sort Key |
| 🚗 A parked car | An **Item** — one record |
| 🧾 What's written on the car's tag | **Attributes** — an item's fields, which can vary item to item |
| 🏢 The numbered section the ticket maps to | A **Partition** — DynamoDB hashes your key to pick it automatically |
| 👉 Handing over your ticket to fetch one car (or one row of cars) fast | A **Query** / **GetItem** |
| 🔦 Walking every row checking every car, no ticket | A **Scan** |
| 📇 A second ticket board, organized by license plate instead | A **Global Secondary Index (GSI)** |
| 📋 A second board for the same ticket, sorted a different way | A **Local Secondary Index (LSI)** |
| 👷 How many valets you've staffed to fetch cars per second | **Capacity** — Read/Write Capacity Units |
| 💳 Pay per car fetched vs. keep a fixed valet crew on payroll | **On-Demand** vs. **Provisioned** capacity mode |
| 📸 A camera logging every car that arrives or leaves, live | **DynamoDB Streams** |
| ⏰ Cars towed automatically if left too long | **Time to Live (TTL)** |
| 🌎 The same valet network, running in multiple cities, synced | **Global Tables** |
| ⚡ A private fast-lane counter that remembers recent tickets | **DAX (DynamoDB Accelerator)** |

## Read in this order

1. [What is DynamoDB?](what-is-dynamodb.md) — the garage, the pun, and why "know your key" is the whole idea
2. [Tables, Items & Attributes](tables-items-and-attributes.md) — cars, tags, and why the garage doesn't care what's on them
3. [Primary Keys & Partitions](primary-keys-and-partitions.md) — how a ticket number decides exactly where your car sits
4. [Querying vs. Scanning](querying-vs-scanning.md) — handing over a ticket vs. checking every car
5. [Secondary Indexes](secondary-indexes.md) — a second ticket board, organized a different way
6. [Capacity Modes](capacity-modes.md) — staffing valets: on-demand vs. a fixed crew
7. [Streams, TTL & Global Tables](streams-ttl-and-global-tables.md) — the camera, the tow truck, and running in multiple cities
8. [Hands-On: Building a Customer Orders Table](hands-on-customer-orders-table.md) — create a real table, insert items, and query them, step by step
9. [Best Practices](best-practices.md) — how to actually run a garage
10. [Mnemonics Cheat Sheet](mnemonics-cheatsheet.md) — the one page to review before an exam or interview
11. [Glossary](glossary.md) — every term, one line each

## Official AWS references

* [Amazon DynamoDB Developer Guide](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html)
* [Core components of Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.CoreComponents.html)
* [Working with queries](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Query.html)
* [Read/write capacity modes](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ReadWriteCapacityMode.html)

See [log.md](log.md) for this folder's update history.
