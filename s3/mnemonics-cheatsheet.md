---
type: Concept
title: "S3 Mnemonics Cheat Sheet"
description: "Every S3 mnemonic in this folder, on one page — read this the night before an exam or interview."
tags: [aws, s3, mnemonics, cheatsheet]
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-13T00:00:00Z
---

# The One-Page S3 Cheat Sheet

## The warehouse, in one picture

| Warehouse piece | S3 concept |
|---|---|
| 🏭 The warehouse chain | Amazon S3 |
| 🏠 A rented storage unit | Bucket — globally unique name, lives in one Region |
| 📦 A box inside the unit | Object — up to 5 TB, key + value + metadata |
| 🏷️ The label on the box | Key — the object's full name; "folders" are just shared label prefixes |
| 🗄️ Which shelf a box sits on | Storage Class — Standard, IA, Glacier, and more |
| 🕰️ Keeping every past edition | Versioning |
| 🤖 A robot moving boxes on a schedule | Lifecycle Rules |
| 🚚 A duplicate shipped to a sister warehouse | Replication (CRR/SRR) |
| 📜 The building's posted rules | Bucket Policy (resource-based, like IAM's) |
| 🚪 A private entrance for one department | Access Point |
| 🔐 Different locks on a box | Encryption — SSE-S3, SSE-KMS, SSE-C, client-side |
| 🎫 A one-time guest pass | Presigned URL |
| 🪟 A storefront window | Static Website Hosting |
| 📦➡️🧩 Moving a couch in pieces | Multipart Upload |
| 🛣️ The express highway | Transfer Acceleration |
| ✅ Front desk never lies | Strong read-after-write Consistency |
| 🔔 The doorbell | Event Notifications |
| 🔍 Reading one paragraph, not the whole book | S3 Select |
| 👥 A crew processing a million boxes at once | S3 Batch Operations |
| 🔒 A vault that can't be opened early | Object Lock (WORM) |
| 📊 Head office's dashboard | S3 Storage Lens |

## The five mnemonics worth memorizing word-for-word

1. **"The closer to the door, the more it costs to store — and the less it costs to grab."** — the entire storage-class trade-off.
2. **"Versioning turns 'overwrite' into 'stack a new one on top.'"** — nothing at the bottom of the version stack disappears on its own.
3. **"30, 30, 90, 90, 180 — the deeper the vault, the longer the lease."** — minimum storage durations for Standard-IA, One Zone-IA, Glacier Instant, Glacier Flexible, Deep Archive.
4. **"Getting into the room isn't the same as opening the box."** — access control (who's in the building) is a separate problem from encryption (can they read the box).
5. **"Whoever has the link has the pass."** — a presigned URL is a bearer credential; guard it like a password.

## The storage-class speed-vs-cost ladder

```
S3 Express One Zone   — fastest, single-digit ms, highest storage cost
S3 Standard           — milliseconds, general purpose
S3 Intelligent-Tiering — milliseconds, auto-managed
S3 Standard-IA        — milliseconds, 30-day minimum
S3 One Zone-IA        — milliseconds, single AZ, 30-day minimum
S3 Glacier Instant Retrieval — milliseconds, 90-day minimum
S3 Glacier Flexible Retrieval — minutes to hours, 90-day minimum
S3 Glacier Deep Archive — 12–48 hours, 180-day minimum, cheapest
```

## Speed-round definitions

| Term | One line |
|---|---|
| Bucket | Globally-named container for objects, lives in one Region |
| Object | The actual stored data: key + value + metadata |
| Key | An object's full name/path — the namespace is flat, not truly hierarchical |
| Versioning | Keeps every past edition of an object instead of overwriting |
| Lifecycle rule | Automatically transitions or expires objects on a schedule |
| Replication | Automatically copies new objects to another bucket/Region |
| Bucket policy | A resource-based policy attached to the bucket itself |
| Access point | A separate named entrance into a bucket with its own policy |
| Presigned URL | A time-limited, signed link granting one action on one object |
| Multipart upload | Uploading a large object in independently-uploaded parts |
| Transfer Acceleration | Routes uploads through CloudFront edge locations onto AWS's backbone |
| Object Lock | WORM protection — a version can't be altered/deleted until a set date |
| Storage Lens | Organization-wide usage and cost dashboard across all buckets |

For full definitions of every term, see the [Glossary](glossary.md). To go deeper on any single row, jump back to the [folder index](index.md).
