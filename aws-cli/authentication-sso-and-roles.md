---
type: Guide
title: "Authentication: SSO, Roles & Credentials"
description: "How the CLI proves who you are — IAM Identity Center (SSO), assumed roles, and long-lived access keys — and why temporary credentials should be your default."
tags: [aws, cli, authentication, sso, iam-identity-center, assume-role, sts, credentials]
sources:
  - id: aws-cli-sso
    resource: https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html
    title: AWS CLI User Guide — Configuring IAM Identity Center authentication with the AWS CLI
  - id: aws-cli-configure
    resource: https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html
    title: AWS CLI User Guide — Configuring settings for the AWS CLI
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-10-02T00:00:00Z
---

# Authentication: SSO, Roles & Credentials

## 📡 The mnemonic

**Three ways to prove the remote is yours: a permanent key taped to it (access keys), a pairing code that expires (SSO), or a borrowed remote for one job (assumed role).** Prefer the ones that expire.

| Method | Credentials last | Where it fits |
|---|---|---|
| **IAM Identity Center (SSO)** | Hours; refreshable | **Humans** at a laptop — the modern default |
| **Assume role** | Minutes to hours | Cross-account access, least-privilege jobs, MFA-gated admin |
| **Instance/task/container role** | Auto-rotated | Code running **on** EC2, ECS, Lambda — no keys to manage at all |
| **IAM user access keys** | **Until you delete them** | Legacy systems; last resort |

This builds directly on [IAM — Roles](../iam/roles.md), [Users & Groups](../iam/users-and-groups.md), and [Federation & STS](../iam/federation-and-sts.md).

## Method 1 — IAM Identity Center (SSO)

If your organization uses IAM Identity Center (the successor to AWS SSO), you sign in through a browser and the CLI receives short-lived credentials. No secret keys on disk.

**You need:** your **SSO start URL** (or issuer URL) and the **SSO Region** — find them in your AWS access portal, or ask your cloud admin.

### One-time setup

```bash
aws configure sso
```

```
SSO session name (Recommended): my-sso
SSO start URL [None]: https://my-sso-portal.awsapps.com/start
SSO region [None]: us-east-1
SSO registration scopes [None]: sso:account:access
```

A browser opens; approve the request. Back in the terminal, pick the **account** and **role** from the lists, then choose a default Region, output format, and a **profile name** (e.g., `my-dev-profile`).

The wizard writes this to `~/.aws/config`:

```ini
[profile my-dev-profile]
sso_session    = my-sso
sso_account_id = 123456789011
sso_role_name  = ReadOnly
region         = us-west-2
output         = json

[sso-session my-sso]
sso_region              = us-east-1
sso_start_url           = https://my-sso-portal.awsapps.com/start
sso_registration_scopes = sso:account:access
```

One `[sso-session]` can be **shared by many profiles** — one sign-in, many accounts/roles:

```ini
[profile dev]
sso_session = my-sso
sso_account_id = 111122223333
sso_role_name = Developer

[profile prod]
sso_session = my-sso
sso_account_id = 444455556666
sso_role_name = ReadOnly
```

### Day-to-day

```bash
aws sso login --profile my-dev-profile        # browser sign-in; caches a token
aws sts get-caller-identity --profile my-dev-profile
aws s3 ls --profile my-dev-profile
aws sso logout                                 # delete cached sign-in
```

- While your SSO session is valid, the CLI **renews role credentials automatically**. When the session itself expires, run `aws sso login` again.
- Cached tokens live in `~/.aws/sso/cache/`.
- Sign-in uses PKCE (browser on the same machine) by default in v2.22+. On a machine with no browser — a remote server, say — add `--use-device-code` to get a URL and a code you can open on another device.
- `aws configure sso-session` edits only the `[sso-session]` block if that's all you need.

**Mnemonic:** *"`configure sso` once, `sso login` each morning."*

## Method 2 — Assuming a role

A **role** is a borrowed remote with specific powers. You hold your normal identity, then temporarily *become* the role.

### Declaratively, in `~/.aws/config`

```ini
[profile admin-role]
role_arn       = arn:aws:iam::123456789012:role/AdminRole
source_profile = default            # whose credentials are used to ask for the role
mfa_serial     = arn:aws:iam::111122223333:mfa/me   # optional: prompt for an MFA code
region         = us-east-1
```

```bash
aws s3 ls --profile admin-role      # the CLI calls sts:AssumeRole for you, and caches the result
```

If you're running on EC2 or ECS, use `credential_source = Ec2InstanceMetadata` (or `EcsContainer`) instead of `source_profile`.

### Manually

```bash
aws sts assume-role \
  --role-arn arn:aws:iam::123456789012:role/AdminRole \
  --role-session-name my-session
```

The response contains `AccessKeyId`, `SecretAccessKey`, and `SessionToken`. Export all **three** (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN`) — the token is required for temporary credentials. The declarative profile above is far less error-prone.

## Method 3 — Roles for code running in AWS

If the CLI runs on an EC2 instance, ECS task, or in CloudShell, **don't configure keys at all.** Attach a role to the compute resource and the CLI discovers temporary credentials automatically from the environment (the instance metadata service or container credentials endpoint). This is the lowest-maintenance and most secure setup.

## Method 4 — IAM user access keys (last resort)

Create an access key for an IAM user, run `aws configure`, done. It's simple and it works anywhere, which is exactly why it leaks: keys end up in Git history, Slack, and screenshots.

If you must use them:

- Give the user **least-privilege** permissions — never admin.
- **Rotate** keys regularly and **delete** unused ones.
- Never put them in code, a repo, or a container image. (`.gitignore` will not save you after a push.)
- Enable [MFA](../iam/mfa.md) on the user and never create keys for the [root user](../iam/root-user.md).

## Which identity am I right now?

Always the same answer command:

```bash
aws sts get-caller-identity
```

Check the `Account` and `Arn` **before** running anything destructive.

## Next up

Credentials sorted — now learn the grammar of a command: [Command Structure & Getting Help](command-structure-and-help.md).

[^aws-cli-sso]: AWS CLI User Guide for Version 2, "Configuring IAM Identity Center authentication with the AWS CLI."
[^aws-cli-configure]: AWS CLI User Guide for Version 2, "Configuring settings for the AWS CLI" (credential precedence and role configuration).
