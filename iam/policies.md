---
type: Concept
title: "IAM Policies"
description: "A policy is the written rulebook the security guard reads before letting anyone through a door — expressed as JSON."
tags: [aws, iam, security, policies, json]
sources:
  - id: aws-iam-policies
    resource: https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html
    title: AWS IAM User Guide — Policies and Permissions
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-12T00:00:00Z
---

# Policies — the Rulebook

## 📜 The mnemonic

A **policy** is a page torn out of the building's rulebook. It's written as **JSON**, and every single one answers the same four questions:

> **Who? Does what? To which room? Allowed or denied?**

**Mnemonic acronym — "PARE" the rulebook down to four questions:**

- **P**rincipal — *who* is asking (only appears in resource-based policies)
- **A**ction — *what* they're trying to do
- **R**esource — *which* room/object they're touching
- **E**ffect — *Allow* or *Deny*

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "LetDevelopersReadOneBucket",
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:ListBucket"],
      "Resource": [
        "arn:aws:s3:::project-photos",
        "arn:aws:s3:::project-photos/*"
      ]
    }
  ]
}
```

## The two places a rulebook page can live

| Type | Attached to | "Who" is implicit or explicit? |
|---|---|---|
| **Identity-based policy** | A user, group, or role | Implicit — it's *this badge holder* |
| **Resource-based policy** | The resource itself (e.g. an S3 bucket policy) | Explicit — must name a `Principal` |

**Mnemonic:** *Identity-based = a rule stapled to the badge. Resource-based = a sign taped to the door.* Either one can grant access; for a same-account request usually only one needs to say "yes," but **cross-account** access typically needs **both** the identity policy (or role) and the resource policy to agree.

## The five policy types you'll actually meet

1. **Identity-based (managed)** — reusable, standalone policies (AWS-managed like `ReadOnlyAccess`, or customer-managed, ones you write)
2. **Identity-based (inline)** — a policy embedded directly in one user/group/role, not reusable
3. **Resource-based** — attached to the resource itself (S3 bucket policy, KMS key policy, SQS queue policy)
4. **Permission boundary** — a ceiling on the *maximum* permissions a user or role can ever have (see [Permission Boundaries & SCPs](permission-boundaries-and-scps.md))
5. **Service Control Policy (SCP)** — an org-wide ceiling set at the AWS Organizations level (see [Permission Boundaries & SCPs](permission-boundaries-and-scps.md))

## Reading a policy statement, step by step

1. **Effect** — is this statement granting or blocking?
2. **Action** — which API calls does this cover? (`s3:*`, `ec2:StartInstances`, `iam:PassRole`…)
3. **Resource** — which specific ARN(s), or `*` for "all resources this action can touch"?
4. **Condition** (optional) — extra fine print, e.g. "only if the request comes from this IP range" or "only if MFA was used"

```json
"Condition": {
  "Bool": { "aws:MultiFactorAuthPresent": "true" }
}
```

**Mnemonic:** *Condition is the guard squinting at the fine print before stamping the badge.*

## Key facts to lock in

- Policies are written in **JSON**, evaluated statement-by-statement.
- `"Resource": "*"` and `"Action": "*"` together is the rulebook equivalent of a master key — avoid it outside of admin roles.
- AWS ships **AWS managed policies** (maintained by AWS, update automatically) and lets you write **customer managed policies** (versioned, up to 5 versions kept, you control updates).

## Next up

With multiple rulebook pages potentially in play at once (identity policy + resource policy + boundary + SCP), how does the guard decide? [Policy Evaluation Logic](policy-evaluation-logic.md).

[^aws-iam-policies]: AWS IAM User Guide, "Policies and permissions in IAM."
