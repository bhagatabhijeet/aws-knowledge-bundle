---
type: Concept
title: "S3 Access Points"
description: "Instead of one giant front door with one giant rulebook, give each department its own private entrance with its own rules."
tags: [aws, s3, security, access-points, multi-region]
sources:
  - id: aws-s3-access-points
    resource: https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-points.html
    title: Amazon S3 User Guide — Managing Access with Access Points
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-13T00:00:00Z
---

# Access Points — Separate Doors, Same Warehouse Unit

## 🚪 The mnemonic

A large, shared bucket used by many teams tends to grow one **enormous, tangled bucket policy** trying to express every team's rules at once. An **Access Point** gives each team its **own named door** into the same unit, each with its **own policy** and even its **own network restrictions** — instead of everyone squeezing through one door with one increasingly complicated sign.

**Mnemonic:** *"One warehouse unit, many private entrances — the finance door only opens for finance, the analytics door only opens for analytics, and each door has its own hostname."*

## What an access point actually is

Every access point gets its own:
- **DNS hostname**, used instead of the bucket name in requests
- **Access policy**, scoped independently of (and in addition to) the bucket policy
- **Network origin control** — restrict it to only accept requests from inside a specific VPC

```
Bucket: shared-data-lake
  ├─ Access Point: finance-ap     (policy: finance role only, from Finance VPC)
  ├─ Access Point: analytics-ap   (policy: read-only, from Analytics VPC)
  └─ Access Point: public-ap      (policy: read-only, no VPC restriction)
```

**A request must satisfy both** the access point's policy *and* the underlying bucket's policy — same "every applicable gate must open" logic as everywhere else in [IAM's evaluation model](../iam/policy-evaluation-logic.md).

## VPC-only access points

An access point can be configured to accept traffic **only from a specific VPC**, using a VPC endpoint. This is a powerful pattern for keeping sensitive buckets completely unreachable from the public internet, even if a policy elsewhere would otherwise allow it — the network restriction acts as one more locked gate.

## Multi-Region Access Points

A **Multi-Region Access Point (MRAP)** goes one step further: it's a single global endpoint that sits in front of **buckets in multiple Regions** (typically kept in sync via [Replication](replication.md)), automatically routing each request to the lowest-latency, available copy.

**Mnemonic:** *"One phone number for the whole chain of sister warehouses — call it, and you're automatically connected to the nearest branch that has your box."*

This is the pattern behind globally-distributed applications that need active-active reads and writes across regions without hardcoding a specific bucket/Region into client applications.

## Why bother, instead of just writing a smarter bucket policy?

- **Blast radius**: a mistake in one team's access point policy can't accidentally affect another team's door.
- **Delegation**: you can let a team manage their own access point policy without giving them rights to edit the shared bucket policy.
- **Auditability**: it's far easier to reason about "who can use the finance door" than to trace one condition buried in a 200-line bucket policy.

## Next up

Doors and policies control *who* gets in. [Encryption](encryption.md) covers what happens even if someone gets past every door — whether they can actually read what's inside the box.

[^aws-s3-access-points]: Amazon S3 User Guide, "Managing access with Amazon S3 access points."
