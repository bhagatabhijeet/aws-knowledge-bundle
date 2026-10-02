---
type: Guide
title: "Troubleshooting the AWS CLI"
description: "A symptom-to-fix table for the errors every CLI user hits — command not found, access denied, invalid credentials, signature mismatch, SSL, invalid JSON — plus the --debug workflow."
tags: [aws, cli, troubleshooting, errors, debug, access-denied, credentials]
sources:
  - id: aws-cli-troubleshooting
    resource: https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-troubleshooting.html
    title: AWS CLI User Guide — Troubleshooting errors for the AWS CLI
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-10-02T00:00:00Z
---

# Troubleshooting the AWS CLI

## 📡 The mnemonic

**When the remote misbehaves: check the batteries (version), the pairing (identity), the room (Region), the clock, then press `--debug` and read what it says.**

## The five-check routine (do these first)

```bash
aws --version                      # 1. Am I on a recent v2?
aws sts get-caller-identity        # 2. Who does AWS think I am? (account + ARN)
aws configure list                 # 3. Where did that identity/Region come from?
date                               # 4. Is my clock correct?
aws <your-command> --debug 2> debug.log   # 5. Show me everything
```

Most problems die at step 2 or 3: wrong profile, stale environment variable, wrong Region, or expired SSO session.

## Symptom → cause → fix

| Symptom | Likely cause | Fix |
|---|---|---|
| `aws: command not found` | Terminal predates install, or `PATH` missing the install dir | Open a **new** terminal; else add `/usr/local/bin` (or `~/.local/bin`) to `PATH`; on Windows check `C:\Program Files\Amazon\AWSCLIV2\` |
| `aws --version` shows an **old** version | Multiple installs (e.g., pip + installer) | Uninstall **all**, using the method you installed with, then reinstall one |
| `argument operation: Found invalid choice 'xyz'` or *"Unknown options"* | Typo, or CLI too old for a new command | Check spelling with `aws <service> help`; run `aws update` |
| `Parameter validation failed` | Wrong parameter name/shape | `aws <service> <cmd> help`; generate a [skeleton](pagination-and-input-files.md) |
| `Unable to locate credentials` | No profile/keys/role resolved | `aws configure`, `aws sso login`, or set `AWS_PROFILE`; check `aws configure list` |
| `You must specify a region` | No Region set anywhere | `aws configure set region us-east-1` or pass `--region` |
| `AccessDenied` / `UnauthorizedOperation` / `is not authorized to perform: …` | Your **IAM identity** lacks permission — or you're using a different identity than you think | `aws sts get-caller-identity`, then check [IAM policies](../iam/policies.md); remember [explicit deny & boundaries](../iam/policy-evaluation-logic.md) |
| `InvalidAccessKeyId` | Key doesn't exist (typo, deleted, wrong account) | Re-check keys with `aws configure list`; create a new key |
| `InvalidClientTokenId` / `ExpiredToken` / `The security token included in the request is expired` | Temporary credentials expired or incomplete (missing `AWS_SESSION_TOKEN`) | `aws sso login`; re-assume the role; export **all three** variables |
| `SignatureDoesNotMatch` / `RequestTimeTooSkewed` | **Clock drift**, or special characters in a key mangled by a script | Fix the system clock (NTP); regenerate the secret key if it contains odd characters |
| `[SSL: CERTIFICATE_VERIFY_FAILED]` | Corporate proxy re-signs TLS and the CLI doesn't trust its CA | `--ca-bundle`, `ca_bundle` in config, or `AWS_CA_BUNDLE=/path/to/corp.pem` |
| `Error parsing parameter … Invalid JSON` | Shell quoting mangled your JSON | See [quoting](command-structure-and-help.md#quoting-the-1-source-of-invalid-json-errors); easiest fix: `file://params.json` |
| Result is empty but resources exist | **Wrong Region** or wrong account/profile | Add `--region`; verify identity |
| Command hangs at `(END)` | Pager waiting for `q` | `--no-cli-pager` or `AWS_PAGER=""` |
| Command times out on huge lists | Page size too large | `--page-size 100` |
| Sudden throttling (`ThrottlingException`, `RequestLimitExceeded`) | Too many calls too fast | Slow down; set `AWS_RETRY_MODE=adaptive` and `AWS_MAX_ATTEMPTS=10` |
| Unexpected abbreviation worked / ignored | CLI accepts unique parameter prefixes | **Don't rely on abbreviations** — spell options in full |

## Using `--debug`

```bash
aws iam list-groups --debug 2> debug.log
```

`--debug` shows the CLI version, **which credential source it chose**, the arguments as it parsed them, the exact HTTP request (signed), and the raw response. Look for:

- `Looking for credentials via: …` and `Found credentials in …` → identifies the source
- `Arguments entered to CLI: [...]` → shows what your shell *really* passed (great for quoting bugs)
- The `Making request … url:` line → reveals the Region/endpoint in use
- The HTTP status and error body → the service's actual complaint

Output goes to stderr, so redirect with `2>`. **Scrub the log before sharing it** — it can contain account IDs and request details.

## Access Denied vs. "can't find it"

For some services, AWS deliberately returns `AccessDenied` or *not found* for missing permissions, so it doesn't reveal what exists. If you're certain the resource exists and permissions look fine, check:

1. Right **account** (`get-caller-identity`)?
2. Right **Region**?
3. An **SCP**, **permission boundary**, or **resource policy** denying you? ([Boundaries & SCPs](../iam/permission-boundaries-and-scps.md))
4. For S3/KMS: does the **bucket/key policy** allow your principal?

## Still stuck?

- `aws <service> <command> help` for the exact syntax and examples
- [AWS CLI GitHub issues](https://github.com/aws/aws-cli/issues) and [AWS re:Post](https://repost.aws/)
- Include `aws --version`, the command (secrets removed), and the error text when asking

[^aws-cli-troubleshooting]: AWS CLI User Guide for Version 2, "Troubleshooting errors for the AWS CLI."
