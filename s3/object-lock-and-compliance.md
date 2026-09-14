---
type: Concept
title: "S3 Object Lock & Compliance"
description: "A vault that cannot be opened, overwritten, or deleted until a set date — not by an attacker, not by an admin, not even by AWS support."
tags: [aws, s3, compliance, object-lock, worm]
sources:
  - id: aws-s3-object-lock
    resource: https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html
    title: Amazon S3 User Guide — Using S3 Object Lock
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-13T00:00:00Z
---

# Object Lock — the Vault Nobody Can Open Early

## 🔒 The mnemonic

Regular deletion protections (permissions, even [MFA Delete](versioning.md)) still assume *someone* with enough access could remove the data if they really wanted to. **Object Lock** is different: it implements **WORM (Write Once, Read Many)** storage — once locked, an object version genuinely **cannot** be overwritten or deleted before its retention date, by anyone, including the account's own administrators.

**Mnemonic:** *"A time-locked vault door — not even the building owner has a key that opens it early."*

## Requirements

- [Versioning](versioning.md) **must** be enabled on the bucket (Object Lock protects specific object *versions*).
- Object Lock must be enabled **at bucket creation time** — it cannot be turned on for an existing bucket after the fact (only turned on when the bucket is created, though you can still apply retention to individual object versions afterward if the bucket was created with it enabled).

## Two modes, two very different guarantees

| Mode | Who can remove the lock early | Use case |
|---|---|---|
| **Governance mode** | Users with a special `s3:BypassGovernanceRetention` permission | Protect against accidental deletion, while still allowing an authorized administrator an emergency override |
| **Compliance mode** | **Nobody** — not the root user, not AWS support, until the retention date passes | Regulatory requirements (e.g., financial records) where the data must be provably unalterable, full stop |

**Mnemonic:** *"Governance mode has a fire axe behind glass, for emergencies only. Compliance mode has no axe at all."*

## Retention periods vs. Legal Hold

Object Lock offers two independent mechanisms, and they're often confused:

1. **Retention period** — "locked until this specific date," set in Governance or Compliance mode.
2. **Legal Hold** — no expiration date at all; it stays in effect **until someone with permission explicitly removes it**, independent of any retention period. Useful for "hold this indefinitely, we don't yet know how long" scenarios like active litigation.

**Mnemonic:** *"Retention is a timer. Legal Hold is a sticky note that says 'don't touch until further notice' — with no timer running at all."*

## What this protects against

- Ransomware or a compromised credential trying to delete or encrypt-and-overwrite your backups
- An accidental `DeleteBucket`-style cleanup script run against the wrong bucket
- Regulatory audits requiring proof that records were never altered after being written (e.g., SEC Rule 17a-4(f), FINRA)

## What it does NOT protect against

- Someone reading the object (Object Lock is about **write/delete** protection, not confidentiality — pair it with [Encryption](encryption.md) and [Access Control](security-and-access-control.md) for that)
- Deleting the **bucket itself** before any locked object's retention expires — S3 will actually refuse to delete a bucket containing locked, unexpired object versions, which is itself a useful safety net

## Next up

Locking data down is one discipline; knowing what's actually happening across your entire storage footprint — and what it's costing you — is another: [Monitoring & Cost Optimization](monitoring-and-cost-optimization.md).

[^aws-s3-object-lock]: Amazon S3 User Guide, "Using S3 Object Lock."
