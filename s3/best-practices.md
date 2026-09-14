---
type: Concept
title: "S3 Best Practices"
description: "How a well-run warehouse chain actually operates day to day — the checklist that ties every concept in this folder together."
tags: [aws, s3, best-practices]
sources:
  - id: aws-s3-best-practices
    resource: https://docs.aws.amazon.com/AmazonS3/latest/userguide/security-best-practices.html
    title: Amazon S3 User Guide — Security Best Practices for Amazon S3
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-13T00:00:00Z
---

# Best Practices — Running a Well-Managed Warehouse

Every row below maps back to a piece of the warehouse you've already met. Read this last — it's the "put it all together" checklist.

## ✅ The warehouse-wide checklist

| ✅ Practice | Warehouse analogy | Why |
|---|---|---|
| Leave Block Public Access **on** unless a bucket truly needs to be public | The master switch stays welded shut by default | Prevents the single most common cause of real-world data leaks ([Security & Access Control](security-and-access-control.md)) |
| Use bucket policies and IAM policies; avoid ACLs | Post a clear sign, retire the old guest list | Signs are auditable and expressive; guest lists are coarse and legacy ([Security & Access Control](security-and-access-control.md)) |
| Enable versioning on important buckets, paired with a lifecycle rule to expire old versions | Keep the version stack, but don't let it grow forever | Protects against accidental overwrite/delete without an unbounded storage bill ([Versioning](versioning.md), [Lifecycle Management](lifecycle-management.md)) |
| Default to **Intelligent-Tiering** when access patterns are unknown | Let the robot decide the shelf | Removes guesswork, no retrieval fees ([Storage Classes](storage-classes.md)) |
| Use **Object Lock in Compliance mode** for regulated, must-not-alter data | The vault nobody can open early | Provable immutability for audits ([Object Lock & Compliance](object-lock-and-compliance.md)) |
| Encrypt everything (it's now the default) and use SSE-KMS for sensitive data | Lock every box; use the keymaster's office for the important ones | Confidentiality plus an audit trail ([Encryption](encryption.md)) |
| Use presigned URLs instead of making objects public for one-off sharing | Hand out a guest pass, not a spare building key | Time-boxed, revocable, scoped to one object ([Sharing & Presigned URLs](sharing-and-presigned-urls.md)) |
| Use multipart upload for anything over ~100 MB | Move the couch through the door in pieces | Faster, resumable, and required past 5 GB ([Performance & Transfer](performance-and-transfer.md)) |
| Turn on Server Access Logging or CloudTrail data events for sensitive buckets | Keep the camera footage | You can't investigate what you didn't record ([Monitoring & Cost Optimization](monitoring-and-cost-optimization.md)) |
| Review with Storage Lens and Storage Class Analysis regularly | Check the head-office dashboard | Cost creep is invisible until someone looks ([Monitoring & Cost Optimization](monitoring-and-cost-optimization.md)) |
| Design key names for your access pattern, not for looks | Label boxes for how you'll search, not how they look on a shelf | Flat namespace performance depends on prefix design ([Buckets & Objects](buckets-and-objects.md)) |

## The "BLOCK" memory device for daily habits

- **B**lock Public Access stays on by default
- **L**ifecycle rules clean up old versions and incomplete uploads
- **O**bject Lock for anything that must never be altered
- **C**loudTrail/access logging for anything sensitive
- **K**MS encryption for anything worth an audit trail

## A one-paragraph mental model to keep forever

> Every bucket starts locked. You open exactly the doors you mean to — a bucket policy here, an access point there — never more. Every box gets encrypted automatically, and the important ones get a keymaster's lock and a vault that can't be opened early. Old editions of a box eventually retire to cheaper shelves and then the shredder, on a schedule you set once. Head office watches the whole chain from one dashboard, so nothing silently drifts, and nothing costs more than it has to.

## Next up

Ready to test yourself? Skim the [Mnemonics Cheat Sheet](mnemonics-cheatsheet.md) once more, then check unfamiliar terms in the [Glossary](glossary.md).

[^aws-s3-best-practices]: Amazon S3 User Guide, "Security best practices for Amazon S3."
