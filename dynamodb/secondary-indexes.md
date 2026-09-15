---
type: Concept
title: "Secondary Indexes"
description: "A second ticket board organized a different way — Global and Local Secondary Indexes, and why they exist because you can't hand the same car two different primary tickets."
tags: [aws, dynamodb, gsi, lsi, secondary-index]
sources:
  - id: aws-ddb-gsi
    resource: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GSI.html
    title: Amazon DynamoDB Developer Guide — Global Secondary Indexes
  - id: aws-ddb-lsi
    resource: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/LSI.html
    title: Amazon DynamoDB Developer Guide — Local Secondary Indexes
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Secondary Indexes — A Second Ticket Board

## 📇 The mnemonic

The garage's main ticket board is organized by ticket number — fast, if that's what you're holding. But sometimes a customer shows up **without** their ticket and says "I don't remember the number, but my license plate is ABC-123." For that, the garage keeps a **second board**, organized by license plate instead, that points back to where the car actually is. That's a **Secondary Index**: a different way to look items up, beyond the table's own primary key.

## Global Secondary Index (GSI) — an entirely new ticket system

A **GSI** defines its own Partition Key (and optionally Sort Key) — completely independent of the base table's primary key — and DynamoDB automatically keeps it in sync as items change.

| Fact | Detail |
|---|---|
| Keys | Any attribute(s) — don't have to relate to the base table's primary key at all |
| Consistency | **Eventually consistent only** — the index lags slightly behind the base table |
| Capacity | Has **its own** capacity, billed separately from the base table |
| When to add | Any time after table creation |

**Mnemonic:** *"A GSI is a whole second ticket board, built by license plate instead of ticket number — its own staff, its own capacity, always a few steps behind the master board."*

## Local Secondary Index (LSI) — the same ticket, sorted differently

An **LSI** keeps the **same Partition Key** as the base table, but defines a **different Sort Key** — a way to re-sort or filter the same group of items (the same Partition Key's items) by a different attribute.

| Fact | Detail |
|---|---|
| Keys | Same Partition Key as the base table; a different Sort Key |
| Consistency | Supports **strongly consistent** reads, unlike a GSI |
| Capacity | **Shares** the base table's provisioned capacity |
| When to add | **Only at table creation** — cannot be added later |

**Mnemonic:** *"An LSI is the same ticket number, just re-sorted on a second board next to the main one — same staff, same budget, but you had to plan for it before the garage opened."*

## Side by side

| | GSI | LSI |
|---|---|---|
| Partition Key | Independent — any attribute | Same as the base table |
| Sort Key | Independent — any attribute (optional) | Different from the base table's Sort Key |
| Consistency | Eventually consistent only | Eventually or strongly consistent |
| Capacity | Separate from the base table | Shared with the base table |
| Can be added later? | Yes | **No — table-creation time only** |

**Mnemonic:** *"Forget to plan an LSI, and you're rebuilding the garage. Forget a GSI, and you just open a second board tomorrow."*

## Why either exists at all

A DynamoDB table is fast along exactly one axis — its primary key. The moment your application needs a genuinely different access pattern (find orders by status instead of by customer, find users by email instead of by user ID), a secondary index is what lets that second pattern stay just as fast, instead of falling back to an expensive [Scan](querying-vs-scanning.md).

## Next up

Every one of these reads and writes draws from the same well: [Capacity Modes](capacity-modes.md).

[^aws-ddb-gsi]: Amazon DynamoDB Developer Guide, "Global Secondary Indexes."
[^aws-ddb-lsi]: Amazon DynamoDB Developer Guide, "Local Secondary Indexes."
