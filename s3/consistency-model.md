---
type: Concept
title: "S3 Consistency Model"
description: "The moment you change what's in a box, the front desk immediately tells the truth to everyone who asks — no stale answers, no waiting."
tags: [aws, s3, consistency]
sources:
  - id: aws-s3-consistency
    resource: https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html#ConsistencyModel
    title: Amazon S3 User Guide — Amazon S3 Data Consistency Model
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-13T00:00:00Z
---

# Consistency Model — the Front Desk Never Lies

## ✅ The mnemonic

Since **December 2020**, S3 provides **strong read-after-write consistency** for all operations — PUTs of new objects, overwrites of existing objects, and DELETEs. The instant a write succeeds, **every subsequent read, from anywhere, sees the result of that write.** There's no window where you might still see old data or a "not found" for something that was just confirmed written.

**Mnemonic:** *"The moment the front desk confirms a box is on the shelf (or removed from it), every clerk in the building gives the same answer — immediately, no exceptions."*

## Why this used to be a bigger deal

Before this change, S3 offered only **eventual consistency** for overwrite PUTs and DELETEs — meaning a read immediately after an overwrite could briefly return either the old or new version. This was a classic source of subtle bugs (an application writes a file, immediately reads it back to confirm, and occasionally gets stale data) and forced workarounds like adding artificial delays or version-checking logic.

**None of that is necessary anymore.** If you find old tutorials or architecture diagrams warning about S3's "eventual consistency," that guidance is outdated for standard S3 usage today.

## What strong consistency does and doesn't cover

| Covered | Not automatically covered |
|---|---|
| GET after PUT of a new object | [Replication](replication.md) to another bucket/Region — that's asynchronous by nature, arriving with some delay |
| GET after PUT that overwrites an existing object | [Cross-Region Replication](replication.md) read consistency at the destination |
| LIST reflecting a just-completed PUT/DELETE | |
| GET after DELETE returning "not found" | |

**Mnemonic:** *"Strong consistency is a promise about one warehouse's own front desk — it says nothing about how fast a sister warehouse (a replication target) gets the memo."*

## Why this matters architecturally

Strong consistency simplifies a whole category of application design:

- Safe to build **read-modify-write** patterns without extra coordination for the "did my write take effect yet?" question
- Safe to use S3 as a **source of truth** that other systems poll immediately after writing to it
- Removes the need for older "consistency workaround" patterns (like DynamoDB-backed indexes to track "has this key definitely landed yet") that used to be common in serious S3-based systems

## Next up

Now that you know exactly *when* a change becomes visible, see what can automatically *react* to that change the instant it happens: [Data Processing & Notifications](data-processing-and-notifications.md).

[^aws-s3-consistency]: Amazon S3 User Guide, "Amazon S3 data consistency model."
