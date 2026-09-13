---
type: Concept
title: "IAM Glossary"
description: "Every IAM term used in this folder, defined in one line, with its building-analogy equivalent."
tags: [aws, iam, security, glossary]
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-12T00:00:00Z
---

# IAM Glossary

| Term | Definition | Building analogy |
|---|---|---|
| **Root user** | The identity created when the AWS account is opened; unrestrictable, should be locked away | The master key |
| **IAM User** | A permanent, named identity for a person or application | An employee badge |
| **IAM Group** | A named bundle of policies attached to multiple users; not itself an identity | A department badge template |
| **IAM Role** | A temporary identity that trusted principals can assume | A self-expiring visitor pass |
| **Trust Policy** | Defines who/what may assume a role | The front desk's sign-in sheet |
| **Permissions Policy** | Defines what an identity can actually do | What's printed on the badge |
| **Identity-based Policy** | A policy attached to a user, group, or role | A rule stapled to the badge |
| **Resource-based Policy** | A policy attached directly to a resource (e.g. S3 bucket policy) | A sign taped to the door |
| **Permission Boundary** | A policy that sets the maximum permissions a single user/role can ever have | A fence around one badge |
| **Service Control Policy (SCP)** | An AWS Organizations policy capping permissions across an account/OU/org, including root | The company-wide fire code |
| **MFA (Multi-Factor Authentication)** | Requiring a second proof of identity beyond a password | A second lock on the door |
| **STS (Security Token Service)** | The AWS service that issues temporary security credentials | The front desk |
| **Federation** | Trusting an external identity provider instead of creating an IAM user | Accepting an outside visitor's ID instead of printing a badge |
| **Principal** | The "who" in a policy — a user, role, account, or service | The name on the sign-in sheet |
| **Action** | The specific API operation a policy statement covers | The task being requested |
| **Resource** | The specific AWS object (by ARN) a policy statement applies to | The room being entered |
| **Effect** | `Allow` or `Deny` — the outcome a statement produces | A yes-sign or a no-sign |
| **Condition** | Extra fine-print requirements on a policy statement | The guard checking fine print before stamping the badge |
| **Explicit Deny** | A statement that directly says `"Effect": "Deny"` | A posted "NO ENTRY" sign |
| **Implicit Deny** | The default outcome when no policy explicitly allows or denies | Silence — no sign at all means no |
| **Least Privilege** | Granting only the minimum permissions needed to do a job | The smallest badge that does the job |
| **AWS Organizations** | The service for centrally managing multiple AWS accounts | Corporate HQ managing every building the company owns |
| **ARN (Amazon Resource Name)** | The unique identifier for an AWS resource | A room's exact address in the building |
| **Access Key** | A long-lived credential pair (ID + secret) for programmatic access | A spare badge that never expires on its own |
| **Session Token** | A short-lived credential issued alongside temporary access keys | The expiry sticker on a visitor pass |
| **IAM Access Analyzer** | A tool that flags resources shared outside the intended trust zone | A second guard auditing every rulebook page |

Back to the [folder index](index.md) · [mnemonics cheat sheet](mnemonics-cheatsheet.md).
