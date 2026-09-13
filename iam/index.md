---
type: Directory Index
title: "AWS IAM — Knowledge Folder"
description: "Index of the AWS Identity and Access Management (IAM) concept docs, taught through the Secure Office Building analogy."
tags: [aws, iam, security, index]
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-12T00:00:00Z
---

# AWS IAM — The Secure Office Building

![The IAM Office Building](assets/images/iam-building-analogy.svg)

Every concept in this folder maps onto **one building**. Once you can picture the building, you can't forget how IAM works — you just ask "where does this fit in the building?"

| Building piece | IAM concept |
|---|---|
| 🏢 The building itself | Your AWS Account |
| 🔑 The master key | The Root User |
| 🪪 Employee badge | IAM User |
| 🗂️ Badge template for a department | IAM Group |
| 🎫 Self-expiring visitor pass | IAM Role |
| 📜 The security rulebook | IAM Policy |
| 🚧 An internal fence around a badge | Permission Boundary |
| 🏛️ Corporate fire code for every building | Service Control Policy (SCP) |
| 🔐 The second lock on the front door | MFA |
| 🛎️ The front desk issuing visitor passes | AWS STS (Security Token Service) |
| 🕵️ The guard who checks every rule | The Policy Evaluation Engine |

## Read in this order

1. [What is IAM?](what-is-iam.md) — the building, the guard, and why IAM exists
2. [The Root User](root-user.md) — the master key you should lock away
3. [Users & Groups](users-and-groups.md) — employee badges and department templates
4. [Roles](roles.md) — visitor passes that expire on their own
5. [Policies](policies.md) — the rulebook, written in JSON
6. [Policy Evaluation Logic](policy-evaluation-logic.md) — how the guard makes a decision
7. [MFA](mfa.md) — the second lock on the door
8. [Federation & STS](federation-and-sts.md) — letting outsiders in without giving them a badge
9. [Permission Boundaries & SCPs](permission-boundaries-and-scps.md) — fences within fences
10. [Best Practices](best-practices.md) — how well-run buildings stay secure
11. [Mnemonics Cheat Sheet](mnemonics-cheatsheet.md) — the one page to review before an exam or interview
12. [Glossary](glossary.md) — every term, one line each

## Official AWS references

* [AWS IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)
* [IAM Policy Evaluation Logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html)
* [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
* [AWS Security Token Service (STS)](https://docs.aws.amazon.com/STS/latest/APIReference/welcome.html)

See [log.md](log.md) for this folder's update history.
