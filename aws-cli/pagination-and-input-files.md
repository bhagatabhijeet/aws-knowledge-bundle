---
type: Guide
title: "Pagination & Input Files"
description: "Control how the CLI pages through large result sets (--max-items, --page-size, --starting-token, the client-side pager) and build complex commands from generated skeleton files and --cli-input-json."
tags: [aws, cli, pagination, pager, skeleton, cli-input-json, cli-input-yaml, dry-run]
sources:
  - id: aws-cli-pagination
    resource: https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-pagination.html
    title: AWS CLI User Guide — Using the pagination options in the AWS CLI
  - id: aws-cli-skeleton
    resource: https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-skeleton.html
    title: AWS CLI User Guide — AWS CLI skeletons and input files
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-10-02T00:00:00Z
---

# Pagination & Input Files

## 📡 The mnemonic

**Big channel guides come in pages — the remote flips them for you. And for a button with forty dials, you fill in a form instead of typing forty flags.**

## Part 1 — Pagination

### The default: the CLI fetches *everything*

Services return results in pages (S3 lists up to 1,000 keys per call). By default the CLI **keeps calling until it has all of them** and hands you one combined result. A 3,500-object bucket = four API calls, one output.

Four server-side knobs change that:

| Option | What it does | Use when |
|---|---|---|
| `--no-paginate` | Make **one** call; return only the first page | You only need a sample |
| `--page-size N` | Ask for N items **per API call** (still returns everything) | Calls **time out** on huge lists — smaller pages are gentler |
| `--max-items N` | Stop after N **total** items printed | You want just the top N |
| `--starting-token TOKEN` | Resume from where a previous `--max-items` stopped | Manual paging |

```bash
aws s3api list-objects --bucket my-bucket --no-paginate          # first page only
aws s3api list-objects --bucket my-bucket --page-size 100        # same result, gentler calls
aws s3api list-objects --bucket my-bucket --max-items 100        # prints 100 + a NextToken
```

When `--max-items` cuts the list short, the output includes a `NextToken`. Feed it back to get the next batch:

```bash
aws s3api list-objects --bucket my-bucket --max-items 100 \
  --starting-token eyJNYXJrZXIiOiBudWxsLCAiYm90b190cnVuY2F0ZV9hbW91bnQiOiAxfQ==
```

- `--starting-token` can't be empty — **no `NextToken` means you're done.**
- Use the **same number** for `--page-size` and `--max-items` when paging manually, or you risk missing/duplicated items.
- These options only work where the service API supports pagination (see the command's `help`).

### Not to be confused with the client-side pager

CLI v2 pipes long output through a **pager** (like `less`) so it doesn't flood your terminal. Press `q` to leave.

| You want to… | Do this |
|---|---|
| Turn it off for one command | `--no-cli-pager` |
| Turn it off everywhere | `export AWS_PAGER=""` or `cli_pager=` in your profile |
| Pick your own pager | `export AWS_PAGER="less"` (or `cli_pager=less`) |
| Tweak `less` flags | `export AWS_PAGER="less -S"` (adds to the default `FRX`) |

Precedence: `--no-cli-pager` → `cli_pager` setting → `AWS_PAGER` → `PAGER`. **In scripts, disable the pager** — otherwise a script can hang waiting at `(END)`.

## Part 2 — Skeletons & input files

Some commands (EC2 `run-instances`, `create-db-instance`, …) have dozens of parameters, many of them nested JSON. Typing them inline is miserable. The better workflow:

**1. Generate a template**

```bash
aws ec2 run-instances --generate-cli-skeleton input > run.json      # JSON  (default: 'input')
aws ec2 run-instances --generate-cli-skeleton yaml-input > run.yaml # YAML
aws ec2 run-instances --generate-cli-skeleton output                # shape of the *response*
```

**2. Delete what you don't need, fill in what you do**

```json
{
    "DryRun": true,
    "ImageId": "ami-dfc39aef",
    "InstanceType": "t3.micro",
    "KeyName": "mykey",
    "MinCount": 1,
    "MaxCount": 1
}
```

Skeleton keys use the **API's** names (`ImageId`), not the CLI flag spelling (`--image-id`). That's why you generate the template instead of guessing.

**3. Run it from the file**

```bash
aws ec2 run-instances --cli-input-json file://run.json
aws ec2 run-instances --cli-input-yaml file://run.yaml
```

With `"DryRun": true`, a correct file returns `DryRunOperation: Request would have succeeded` — nothing is created. Set it to `false` (or add `--no-dry-run`) to launch for real.

**Mix and match:** flags typed on the command line **override** the file, so keep reusable defaults in the file and vary the rest:

```bash
aws ec2 run-instances --cli-input-json file://run.json --instance-type t3.small --no-dry-run
```

> The high-level `aws s3` commands (`cp`, `sync`, …) don't support skeleton/input-file options; `aws s3api` does.

**Why it's worth it:** the file can live in Git, be code-reviewed, and be reused — the CLI equivalent of a saved recipe.

## Dry runs: preview before pressing

| Where | Flag | Effect |
|---|---|---|
| EC2 (many commands) | `--dry-run` | Checks permissions and validity; changes nothing. A "success" is reported as a `DryRunOperation` error |
| `aws s3 cp/mv/rm/sync` | `--dryrun` | Prints what *would* happen |

Habit worth building: **dry-run anything destructive, especially `aws s3 sync --delete`.**

## Next up

The service you'll use the most: [Working with Amazon S3 from the CLI](s3-commands.md).

[^aws-cli-pagination]: AWS CLI User Guide for Version 2, "Using the pagination options in the AWS CLI."
[^aws-cli-skeleton]: AWS CLI User Guide for Version 2, "AWS CLI skeletons and input files in the AWS CLI."
