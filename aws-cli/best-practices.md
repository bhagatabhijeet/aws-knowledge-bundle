---
type: Guide
title: "AWS CLI Best Practices"
description: "How to use the AWS CLI safely and effectively — credentials hygiene, least privilege, profile discipline, scripting habits, and guardrails against destructive commands."
tags: [aws, cli, best-practices, security, scripting, least-privilege]
sources:
  - id: aws-cli-ug
    resource: https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html
    title: AWS CLI User Guide for Version 2
  - id: aws-iam-best-practices
    resource: https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html
    title: IAM User Guide — Security best practices in IAM
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-10-02T00:00:00Z
---

# AWS CLI Best Practices

## 📡 The mnemonic

**A universal remote can switch off every light in the house — or the fridge. Use it like you'd hand it to a toddler's grandparent: clearly labeled, pre-checked, and hard to misuse.**

## 🔐 Credentials

| Do | Don't |
|---|---|
| Use **SSO / IAM Identity Center** or **assumed roles** (temporary credentials) | Use long-lived IAM-user keys when an alternative exists |
| Let compute use **instance / task / Lambda roles** | Store keys on servers, in images, or in repos |
| **Rotate and delete** any access keys you must keep | Leave old keys active "just in case" |
| Protect privileged roles with **MFA** (`mfa_serial`) | Ever create access keys for the [root user](../iam/root-user.md) |
| Keep `~/.aws/credentials` private (`chmod 600`) | Paste keys into chat, tickets, or screenshots |
| Scan commits for secrets (pre-commit hooks, secret scanners) | Assume `.gitignore` saves you after a leak — **a leaked key is a compromised key; revoke it** |

## 🧭 Least privilege

- Grant only the actions and resources a person or job needs. See [IAM Best Practices](../iam/best-practices.md).
- Prefer a **read-only** profile for exploring; switch to a write-capable one only when changing things.
- Use [permission boundaries and SCPs](../iam/permission-boundaries-and-scps.md) as guardrails for teams.

## 🏷️ Profile & Region discipline

- **One profile per account/role**, with a name you can't misread: `acme-prod-readonly`, `acme-dev-admin`.
- Don't rely on `[default]` for anything dangerous — leave it empty or pointed at a sandbox.
- Always run **`aws sts get-caller-identity`** before destructive work. Consider adding the account alias to your shell prompt.
- Set `region` explicitly in each profile (or pass `--region`) so a command never "accidentally" runs in a different Region.
- Be suspicious of leftover **`AWS_*` environment variables** — they silently override your profiles.

## 🛡️ Safe execution

1. **Preview, then perform.** `--dry-run` (EC2), `--dryrun` (S3), or read the command twice.
2. **List before you delete.** Run the matching `ls` / `describe` / `list` with the same filter, check the output, *then* swap in `rm` / `delete`.
3. **Treat `--recursive`, `--force`, and `--delete` as sharp edges.**
4. Turn on **versioning** (S3) and **deletion protection** (RDS, DynamoDB, CloudFormation termination protection) where it matters.
5. For unfamiliar commands, use `aws <service> <command> help` or [auto-prompt](productivity-and-scripting.md).

## 📜 Scripting

- Start scripts with `set -euo pipefail`; set `AWS_PAGER=""`.
- Capture single values with `--query … --output text`, not by parsing human-formatted output.
- Use `aws … wait` instead of `sleep`.
- Specify `--region` and `--profile` explicitly in automation.
- Make scripts **idempotent** (safe to run twice) where you can; log what they did.
- In CI/CD, use **OIDC / web-identity roles** rather than stored keys.
- Keep long parameter sets in **version-controlled JSON/YAML** and load with `--cli-input-json` or `file://`.

## ⚙️ Hygiene

- Keep the CLI **updated** (`aws update`) — new services and flags arrive constantly, and security fixes ship.
- Prefer **v2**; verify with `aws --version`.
- Don't use `--no-verify-ssl`; fix the CA with `--ca-bundle` / `AWS_CA_BUNDLE`.
- Don't rely on **parameter abbreviations** (`--change-set-n`); they can break when new options are added.
- Enable **CloudTrail** so CLI activity is auditable. Set `role_session_name` / `--role-session-name` so assumed-role actions are attributable.
- Use **tags** on resources you create from the CLI (`--tag-specifications`, `--tags`) so they can be found, costed, and cleaned up.

## Quick self-audit

- [ ] I authenticate with SSO or roles, not static keys
- [ ] No keys in repos, images, or shell history
- [ ] My profiles are named by account **and** purpose
- [ ] I check `get-caller-identity` before risky commands
- [ ] My scripts fail fast and never hang
- [ ] Destructive commands go through a dry run or a preview list first

[^aws-cli-ug]: AWS CLI User Guide for Version 2.
[^aws-iam-best-practices]: IAM User Guide, "Security best practices in IAM."
