---
type: Tutorial
title: "Hands-On: Building a Customer Orders Table"
description: "Create a real DynamoDB table for a classic e-commerce use case — a customer's order history — then insert and query real items, using both the Console and the AWS CLI."
tags: [aws, dynamodb, tutorial, cli, hands-on]
sources:
  - id: aws-ddb-getting-started
    resource: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStarted.html
    title: Amazon DynamoDB Developer Guide — Getting started
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Hands-On: Building a Customer Orders Table

This walks through a genuinely common DynamoDB use case: an e-commerce site's **order history**, where the one thing an application constantly needs is "show me all of this customer's orders" — fast, regardless of how many customers or orders exist. Every step below has both a **Console** version (clicking through the AWS Console) and a **CLI** version (a command you can copy and run) — pick whichever fits how you work, or use both to check your understanding against each other.

**Prerequisites:** an AWS account, and (for the CLI steps) the [AWS CLI](https://aws.amazon.com/cli/) installed and configured with credentials that have DynamoDB permissions (`aws configure`).

## The design, up front

| Decision | Choice | Why |
|---|---|---|
| Table name | `CustomerOrders` | — |
| Partition Key | `CustomerId` (String) | Every order for one customer lands in the same partition — see [Primary Keys & Partitions](primary-keys-and-partitions.md) |
| Sort Key | `OrderId` (String), formatted `YYYY-MM-DD#O-nnnn` | Embedding the date at the front makes Query results come back in chronological order, and lets you filter by date range using plain string comparison |
| Capacity mode | On-Demand | No traffic to predict yet for a tutorial table — see [Capacity Modes](capacity-modes.md) |

This single design already answers the two most common questions an order history needs: *"give me one specific order"* (GetItem) and *"give me all of this customer's orders, in order"* (Query).

## Step 1 — Create the table

**Console:**

1. Open the [DynamoDB console](https://console.aws.amazon.com/dynamodbv2/) and choose **Tables** → **Create table**.
2. Table name: `CustomerOrders`.
3. Partition key: `CustomerId`, type **String**.
4. Add a Sort key: `OrderId`, type **String**.
5. Under **Table settings**, choose **Customize settings** and confirm the capacity mode is **On-demand**.
6. Choose **Create table**, and wait for the table's status to become **Active**.

**CLI:**

```bash
aws dynamodb create-table \
  --table-name CustomerOrders \
  --attribute-definitions \
      AttributeName=CustomerId,AttributeType=S \
      AttributeName=OrderId,AttributeType=S \
  --key-schema \
      AttributeName=CustomerId,KeyType=HASH \
      AttributeName=OrderId,KeyType=RANGE \
  --billing-mode PAY_PER_REQUEST \
  --region us-east-1
```

`HASH` is the CLI's name for the Partition Key; `RANGE` is its name for the Sort Key. `PAY_PER_REQUEST` is On-Demand mode (see [Capacity Modes](capacity-modes.md)).

Wait for the table to finish creating before moving on:

```bash
aws dynamodb wait table-exists --table-name CustomerOrders --region us-east-1
```

## Step 2 — Insert items (PutItem)

**Console:** Open the table, choose **Explore table items** → **Create item**, switch to **JSON view**, paste one item's JSON (below), and choose **Create item**. Repeat for each item.

**CLI:** Insert four sample orders across two customers — three for `C-1001`, one for `C-2002`:

```bash
aws dynamodb put-item --table-name CustomerOrders --region us-east-1 --item '{
  "CustomerId":  {"S": "C-1001"},
  "OrderId":     {"S": "2026-01-15#O-9001"},
  "OrderDate":   {"S": "2026-01-15"},
  "Status":      {"S": "SHIPPED"},
  "TotalAmount": {"N": "42.50"},
  "Items":       {"L": [{"S": "SKU-101"}, {"S": "SKU-204"}]}
}'

aws dynamodb put-item --table-name CustomerOrders --region us-east-1 --item '{
  "CustomerId":   {"S": "C-1001"},
  "OrderId":      {"S": "2026-02-03#O-9002"},
  "OrderDate":    {"S": "2026-02-03"},
  "Status":       {"S": "PENDING"},
  "TotalAmount":  {"N": "88.00"},
  "GiftMessage":  {"S": "Happy birthday!"}
}'

aws dynamodb put-item --table-name CustomerOrders --region us-east-1 --item '{
  "CustomerId":  {"S": "C-1001"},
  "OrderId":     {"S": "2026-03-10#O-9003"},
  "OrderDate":   {"S": "2026-03-10"},
  "Status":      {"S": "DELIVERED"},
  "TotalAmount": {"N": "15.00"}
}'

aws dynamodb put-item --table-name CustomerOrders --region us-east-1 --item '{
  "CustomerId":  {"S": "C-2002"},
  "OrderId":     {"S": "2026-01-20#O-8001"},
  "OrderDate":   {"S": "2026-01-20"},
  "Status":      {"S": "SHIPPED"},
  "TotalAmount": {"N": "120.00"}
}'
```

Notice the second and third items for `C-1001` don't carry the same attributes as the first (no `Items` list; the second has a `GiftMessage` the others don't) — exactly the schemaless flexibility described in [Tables, Items & Attributes](tables-items-and-attributes.md). Each value's type is spelled out explicitly (`S` for String, `N` for Number, `L` for List) because the CLI's `put-item` speaks DynamoDB's low-level JSON format directly.

## Step 3 — Fetch one exact order (GetItem)

**Console:** **Explore table items** → **Query** → enter the Partition Key `C-1001` and Sort Key `2026-01-15#O-9001` → **Run**.

**CLI:**

```bash
aws dynamodb get-item --table-name CustomerOrders --region us-east-1 --key '{
  "CustomerId": {"S": "C-1001"},
  "OrderId":    {"S": "2026-01-15#O-9001"}
}'
```

## Step 4 — Get all of one customer's orders (Query)

This is the access pattern the whole table was designed around — see [Querying vs. Scanning](querying-vs-scanning.md).

**Console:** **Explore table items** → **Query** → enter only the Partition Key `C-1001`, leave the Sort Key blank → **Run**. All three of `C-1001`'s orders come back, in chronological order.

**CLI:**

```bash
aws dynamodb query --table-name CustomerOrders --region us-east-1 \
  --key-condition-expression "CustomerId = :cid" \
  --expression-attribute-values '{":cid": {"S": "C-1001"}}'
```

**Narrow it further** — only orders from February 2026 onward, using a Sort Key condition (this works because the date prefix in `OrderId` sorts correctly as a plain string):

```bash
aws dynamodb query --table-name CustomerOrders --region us-east-1 \
  --key-condition-expression "CustomerId = :cid AND OrderId >= :start" \
  --expression-attribute-values '{
    ":cid":   {"S": "C-1001"},
    ":start": {"S": "2026-02-01"}
  }'
```

This returns only the `2026-02-03` and `2026-03-10` orders — the `2026-01-15` one is excluded, and DynamoDB never had to look at `C-2002`'s partition at all.

## Step 5 — See why a Scan is different (and costlier)

For comparison, find every `SHIPPED` order **across every customer** — something the primary key doesn't support directly:

```bash
aws dynamodb scan --table-name CustomerOrders --region us-east-1 \
  --filter-expression "#s = :status" \
  --expression-attribute-names '{"#s": "Status"}' \
  --expression-attribute-values '{":status": {"S": "SHIPPED"}}'
```

This works, but as the table grows, this request reads **every item in the table** to find the two that match — exactly the trade-off described in [Querying vs. Scanning](querying-vs-scanning.md). (`#s` is an **expression attribute name placeholder**, needed here because `Status` collides with a reserved word in DynamoDB's expression syntax.)

## Step 6 — Clean up

A tutorial table left running still counts as a real resource in your account. When you're done experimenting:

**Console:** Select the table → **Delete** → confirm.

**CLI:**

```bash
aws dynamodb delete-table --table-name CustomerOrders --region us-east-1
```

## What this exercise actually demonstrated

- A **composite primary key** (`CustomerId` + `OrderId`) that makes "all of one customer's orders" a fast, single-partition Query
- **Schemaless items** — real items in the same table carrying different attributes
- The concrete difference in *behavior*, not just theory, between **GetItem**, **Query**, and **Scan**
- A practical trick — embedding a sortable date at the front of a Sort Key — for getting range queries "for free" out of plain string comparison

## Next up

Turn what you just built into habits for real tables: [Best Practices](best-practices.md).

[^aws-ddb-getting-started]: Amazon DynamoDB Developer Guide, "Getting started with DynamoDB."
