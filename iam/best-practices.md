---
type: Concept
title: "IAM Best Practices"
description: "How a well-run security office actually operates, day to day — the checklist AWS itself recommends."
tags: [aws, iam, security, best-practices]
sources:
  - id: aws-iam-best-practices
    resource: https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html
    title: AWS IAM User Guide — Security Best Practices in IAM
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-12T00:00:00Z
---

# Best Practices — Running a Well-Managed Building

Every rule below maps back to a piece of the building you've already met. Read this doc last — it's the "put it all together" checklist.

## 🏢 The building-wide checklist

| ✅ Practice | Building analogy | Why |
|---|---|---|
| Lock away the root user; add MFA | The master key stays in the safe | One leak = total compromise ([Root User](root-user.md)) |
| Require MFA for humans, especially admins | A second lock on sensitive doors | Stops a stolen password alone from working ([MFA](mfa.md)) |
| Prefer roles over long-lived user access keys | Hand out visitor passes, not spare badges | Temporary credentials auto-expire even if leaked ([Roles](roles.md)) |
| Grant least privilege; start with nothing, add as needed | Print the smallest badge that does the job | Limits blast radius of any single compromised identity |
| Use groups, not per-user inline policies | Manage by department, not by name | One update instead of fifty ([Users & Groups](users-and-groups.md)) |
| Use permission boundaries when delegating IAM creation | Fence the badge printer itself | Prevents privilege escalation by trusted-but-limited admins |
| Use SCPs at the Organization level for hard limits | Company-wide fire code | Caps every account, even root, uniformly |
| Rotate and remove unused credentials | Recall badges from people who've left | Old keys are free attack surface |
| Use IAM Access Analyzer | A second guard reviewing every rulebook page | Flags resources shared outside your account/org unintentionally |
| Review with the last-accessed data | Check which badges haven't scanned a door in months | Deprovision safely, backed by evidence, not guesswork |

## The "PALS" memory device for daily habits

- **P**rincipal of least privilege — smallest badge that works
- **A**udit regularly — IAM Access Analyzer + Credential Reports
- **L**everage roles over long-lived keys
- **S**eparate duties with groups, boundaries, and SCPs

## A one-paragraph mental model to keep forever

> Your account is a building. Root is the master key — locked away. Everyone else carries the smallest badge (user) or visitor pass (role) that does their job, checked against a rulebook (policy) written for the least access needed, fenced in by a boundary, and bound by a company-wide fire code (SCP). A guard (the evaluation engine) checks every applicable rule on every door, and a single "no" anywhere ends the conversation.

## Next up

Ready to test yourself? Skim the [Mnemonics Cheat Sheet](mnemonics-cheatsheet.md) once more, then check unfamiliar terms in the [Glossary](glossary.md).

[^aws-iam-best-practices]: AWS IAM User Guide, "Security best practices in IAM."
