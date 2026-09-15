---
type: Concept
title: "Tables, Items & Attributes"
description: "Cars, tags, and why the garage doesn't care what's written on any given tag — the schemaless core of a DynamoDB table."
tags: [aws, dynamodb, tables, items, attributes, data-types]
sources:
  - id: aws-ddb-core-components
    resource: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.CoreComponents.html
    title: Amazon DynamoDB Developer Guide — Core components
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Tables, Items & Attributes — Cars, Tags, and Tickets

## 🚗🧾 The mnemonic

A **table** is the whole garage. An **item** is one parked car. **Attributes** are whatever's written on that car's tag — make, color, owner's name — and critically, **no two cars need the same tags.** One car's tag might list a license plate and a color; another's might additionally list a parking-fee balance and a valet's note. The garage doesn't demand every car carry identical information — it only insists every car has a ticket.

## The one rule: every item needs a primary key

A table has no fixed set of columns. What it **does** enforce, for every single item, is the **primary key** — the ticket number (see [Primary Keys & Partitions](primary-keys-and-partitions.md)). Beyond that one required field (or pair of fields), an item can carry any attributes you like, and different items in the same table are free to carry entirely different ones.

**Mnemonic:** *"Every car needs a ticket. Nothing says every car needs the same tags."*

## Attribute data types

Attributes aren't untyped free text — DynamoDB tracks a type for every value:

| Category | Types |
|---|---|
| **Scalar** | String, Number, Binary, Boolean, Null |
| **Document** | List (an ordered array of values), Map (a nested set of key-value pairs — a JSON-like object) |
| **Set** | String Set, Number Set, Binary Set — a collection of *unique* values, all of one type |

**Mnemonic:** *"A car's tag can hold a single word (Scalar), a whole glovebox of grouped items (Document), or a set of unique keychain fobs, no duplicates allowed (Set)."*

## Example: two items in the same table, two different shapes

```json
{
  "CustomerId": "C-1001",
  "OrderId": "2026-01-15#O-9001",
  "Status": "SHIPPED",
  "TotalAmount": 42.50
}
```

```json
{
  "CustomerId": "C-1001",
  "OrderId": "2026-02-03#O-9002",
  "Status": "PENDING",
  "TotalAmount": 88.00,
  "GiftMessage": "Happy birthday!",
  "Items": ["SKU-101", "SKU-204"]
}
```

Both items share the same primary key structure (`CustomerId` + `OrderId`) — but the second one carries a `GiftMessage` and an `Items` list that the first doesn't have at all. Neither item is "wrong"; this is exactly how DynamoDB is meant to be used.

**Mnemonic:** *"Two cars, two different tags — the garage doesn't reject the second car for having an extra note stapled to it."*

## Item size limit

A single item — every attribute combined — is capped at **400 KB**. This is a hard ceiling worth designing around early: if a "car's tag" is at risk of growing unbounded (an ever-appending list, for instance), that's usually a sign the data belongs in a separate item, or a separate table, rather than piled onto one.

**Mnemonic:** *"A car's tag only has so much room — if you're stapling on page after page, it's time to give that information its own ticket."*

## Next up

Now the single most important design decision in DynamoDB — the ticket number itself: [Primary Keys & Partitions](primary-keys-and-partitions.md).

[^aws-ddb-core-components]: Amazon DynamoDB Developer Guide, "Core components of Amazon DynamoDB."
