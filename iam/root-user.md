---
type: Concept
title: "The Root User"
description: "The root user is the master key to the entire building — powerful, dangerous, and meant to stay locked in a drawer."
tags: [aws, iam, security, root-user]
sources:
  - id: aws-root-user
    resource: https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html
    title: AWS IAM User Guide — AWS Account Root User
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-12T00:00:00Z
---

# The Root User — the Master Key

## 🔑 The mnemonic

The **root user** is the **master key** to the entire office building. It opens every door, disables every alarm, and can even demolish the building. You get exactly one, it's created the moment you sign up for AWS, and you should put it in a drawer and (almost) never touch it again.

**"The master key opens the safe. It shouldn't be the key you use to get coffee."**

## Why root is dangerous

The root user's permissions **cannot be restricted** by any policy. There is no rulebook the guard can hand to root that says "except this" — root ignores every fence, boundary, and rulebook in the building.[^aws-root-user]

That means:
- If root's credentials leak, your **entire account** is compromised — every resource, every dollar of spend, every piece of data.
- You cannot attach an IAM policy to root to limit it (only a tiny handful of account-level actions, like closing the account, truly require root).

## What root should be used for — and nothing else

Almost everything, including account administration, should be done through an **IAM user or role with administrator permissions**, not root. Root is reserved for a short list of tasks that AWS deliberately locks to root only, such as:

- Changing your account's support plan
- Closing the AWS account
- Restoring IAM user permissions if you accidentally locked yourself out
- A handful of legacy billing/account settings

## The three locks you put on the master key

| Lock | What it does |
|---|---|
| 🔐 **MFA (hardware or virtual)** | Requires a physical device or authenticator app in addition to the password — see [MFA](mfa.md) |
| ✉️ **A monitored, unique email alias** | Root sign-in is tied to an email address — don't let it be a personal, unmonitored inbox |
| 🚫 **No access keys** | Root almost never needs programmatic (API/CLI) access — delete root's access keys if they exist |

## Quick self-check

> "Am I logged in as root right now, and is what I'm doing on the AWS-mandated root-only list above?" If not, log out and use an IAM role instead.

## Next up

Now meet the identities you *should* be using every day: [Users & Groups](users-and-groups.md).

[^aws-root-user]: AWS IAM User Guide, "AWS account root user."
