---
type: Concept
title: "What is Amazon S3?"
description: "S3 is a global chain of self-storage warehouses for data — rent a unit, drop in boxes, and never think about the building it's stored in."
tags: [aws, s3, storage, fundamentals]
sources:
  - id: aws-s3-intro
    resource: https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html
    title: Amazon S3 User Guide — What is Amazon S3?
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-12T00:00:00Z
---

# What is Amazon S3?

**Amazon S3 (Simple Storage Service)** is object storage: you hand it a file, it hands you back a durable, globally-reachable place to fetch that file from — forever, without you ever managing a disk, a server, or a filesystem.

## 🏭 The mnemonic: three S's, twice

AWS's own name gives you the mnemonic for free: **S**imple **S**torage **S**ervice — three S's. Picture it instead as your own **S**elf-**S**torage **S**ervice — same three S's, and now you have a physical building to hang every concept on.

![The S3 Self-Storage Facility](assets/images/s3-warehouse-overview.svg)

S3 is a **chain of self-storage warehouses**, one (or more) per AWS Region. You rent a **unit** (a bucket), you fill it with **boxes** (objects), each box has a **label** (a key), and you can pick which **shelf** each box sits on — from the front-door shelf you visit daily to a sealed mountain vault you open once a decade.

## Why object storage, and not a hard drive or a database?

| | Traditional disk / filesystem | Amazon S3 |
|---|---|---|
| Capacity | You provision it, and it runs out | Effectively unlimited — never provision capacity |
| Structure | Folders and files, nested | A **flat** namespace of key → object pairs (folders are simulated, see [Buckets & Objects](buckets-and-objects.md)) |
| Access | Attached to one server | Reachable over HTTPS from anywhere with a permission to ask |
| Durability | You manage backups yourself | **11 nines (99.999999999%)** durability, built in |

## The numbers worth memorizing

| Fact | Value |
|---|---|
| Durability | **99.999999999%** ("11 nines") per object, per year |
| Availability | 99.9%–99.99% depending on storage class |
| Max single object size | **5 TB** |
| Max size for a single PUT (no multipart) | 5 GB — above that, [multipart upload](performance-and-transfer.md) is required |
| Bucket scope | Global name, but the bucket itself **lives in one Region** |
| Objects per bucket | Unlimited |
| Consistency | **Strong read-after-write** for all operations, since December 2020 (see [Consistency Model](consistency-model.md)) |

**Mnemonic for durability:** *"11 nines" means if you stored 10,000,000 objects in S3, you'd statistically expect to lose one object every 10,000 years.* That's not a typo — it's what "eleven nines" actually buys you.

## What makes an object

Every object in the warehouse is really three things bundled together:

1. **The key** — its full name, e.g. `photos/2026/vacation.jpg`
2. **The value** — the actual bytes of data (the contents of the box)
3. **Metadata** — a set of name/value pairs describing the object (content-type, custom tags, etc.)

Full detail on this in [Buckets & Objects](buckets-and-objects.md).

## What you'll use S3 for, in practice

- Hosting static websites and single-page apps ([Static Website Hosting](static-website-hosting.md))
- Data lakes for analytics (queried directly with Athena, Redshift Spectrum, EMR)
- Backup and disaster recovery targets, with lifecycle rules pushing old data into cold storage ([Lifecycle Management](lifecycle-management.md))
- Application asset storage (images, videos, logs, backups)
- Big data staging for ML training pipelines

## Next up

Start with the two nouns everything else depends on: [Buckets & Objects](buckets-and-objects.md).

[^aws-s3-intro]: Amazon S3 User Guide, "What is Amazon S3?"
