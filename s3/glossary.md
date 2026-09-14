---
type: Concept
title: "S3 Glossary"
description: "Every S3 term used in this folder, defined in one line, with its warehouse-analogy equivalent."
tags: [aws, s3, glossary]
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-13T00:00:00Z
---

# S3 Glossary

| Term | Definition | Warehouse analogy |
|---|---|---|
| **Bucket** | A globally-named top-level container for objects, created in one Region | A rented storage unit |
| **Object** | The actual data stored, plus its key and metadata | A box on a shelf |
| **Key** | An object's full name/path within a bucket | The label on the box |
| **Prefix** | The shared leading portion of many keys, used to simulate folders | Boxes grouped by the first part of their label |
| **Storage Class** | The pricing/performance tier an object is stored in | Which shelf the box sits on |
| **S3 Standard** | General-purpose, frequently-accessed storage class | The front shelf by the door |
| **S3 Intelligent-Tiering** | A storage class that auto-moves objects between tiers based on access | A robot managing the shelf for you |
| **S3 Glacier (all tiers)** | Archive storage classes trading retrieval speed for lower cost | Vaults, from a fast one to a sealed mountain vault |
| **Versioning** | Keeps every past edition of an object instead of overwriting it | Stacking new editions instead of discarding the old one |
| **Delete marker** | A marker placed on top of a version stack by a DELETE, when versioning is on | A sticky note reading "not here" |
| **MFA Delete** | Requires an MFA code to permanently delete a version or change versioning state | A second lock on the delete button |
| **Lifecycle rule** | An automated rule that transitions or expires objects over time | The scheduled warehouse robot |
| **Replication (CRR/SRR)** | Automatically copying new objects to another bucket, same or different Region | Shipping a duplicate box to a sister warehouse |
| **Bucket policy** | A resource-based JSON policy attached to a bucket | The sign taped to the unit's door |
| **ACL (Access Control List)** | A legacy, coarse-grained access mechanism predating IAM/bucket policies | An old-fashioned guest list |
| **Block Public Access (BPA)** | Account/bucket-level settings that override any policy allowing public access | The master switch welding every door shut |
| **Access Point** | A named entry point into a bucket with its own policy and network restrictions | A private entrance for one department |
| **Multi-Region Access Point (MRAP)** | A single global endpoint routing to the nearest of several regional bucket copies | One phone number reaching the nearest sister warehouse |
| **SSE-S3 / SSE-KMS / SSE-C** | Server-side encryption options, differing in who manages the key | Different locks on the box |
| **Client-side encryption** | Encrypting data before it's ever sent to S3 | Locking the box before it leaves your building |
| **Presigned URL** | A time-limited, signed URL granting one action on one object | A one-time guest pass |
| **Requester Pays** | A setting shifting data transfer costs to the downloader instead of the bucket owner | The courier's company pays for shipping |
| **Static website hosting** | Serving HTML/CSS/JS directly from a bucket via a special endpoint | Turning a unit into a storefront window |
| **Multipart upload** | Uploading a large object as independently-uploaded parts | Moving a couch through the door in pieces |
| **Transfer Acceleration** | Routing uploads through CloudFront edge locations onto AWS's backbone | Taking the highway instead of back roads |
| **Consistency model** | The guarantee about when a write becomes visible to subsequent reads | The front desk never giving a stale answer |
| **Event Notifications** | Automatic triggers (to SNS/SQS/Lambda/EventBridge) on object changes | The doorbell ringing when a box changes |
| **S3 Select** | Running a SQL-like query against an object to retrieve only matching data | Reading one paragraph instead of hauling out the whole book |
| **S3 Batch Operations** | Applying one operation to millions of objects listed in a manifest | A crew processing a thousand boxes at once |
| **Object Lock** | WORM protection preventing a version from being altered or deleted before a set date | A time-locked vault |
| **Governance mode** | Object Lock mode allowing an authorized override | A fire axe behind glass |
| **Compliance mode** | Object Lock mode allowing no override, by anyone, until expiry | A vault with no axe at all |
| **Legal Hold** | An indefinite lock independent of any retention date, removed only explicitly | A sticky note with no expiry |
| **S3 Storage Lens** | An organization-wide dashboard of usage, activity, and cost metrics | Head office's central dashboard |
| **S3 Inventory** | A scheduled report listing every object and its metadata | A mailed clipboard audit |
| **Server Access Logging** | Per-request logs of who accessed what, when | Security camera footage at the loading dock |

Back to the [folder index](index.md) · [mnemonics cheat sheet](mnemonics-cheatsheet.md).
