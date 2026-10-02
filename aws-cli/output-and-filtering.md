---
type: Guide
title: "Output & Filtering"
description: "Choose an output format and shape results with server-side --filters and client-side --query (JMESPath) — from your first one-liner to sorting, labeling, and combining both."
tags: [aws, cli, output, query, jmespath, filters, json, yaml, table, text]
sources:
  - id: aws-cli-output
    resource: https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-output-format.html
    title: AWS CLI User Guide — Setting the output format in the AWS CLI
  - id: aws-cli-filter
    resource: https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-filter.html
    title: AWS CLI User Guide — Filtering output in the AWS CLI
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-10-02T00:00:00Z
---

# Output & Filtering

## 📡 The mnemonic

**Pick the display (`--output`), then filter the channel guide (`--filters` at the source, `--query` on your screen).**

A raw `describe-instances` is hundreds of lines. Nobody wants hundreds of lines. They want *three columns*.

## 1. Choose how it's displayed: `--output`

| Format | Looks like | Best for |
|---|---|---|
| `json` *(default)* | Nested JSON | Piping into `jq` or another program |
| `yaml` | Nested YAML | Feeding YAML tooling (e.g., CloudFormation work) |
| `yaml-stream` | YAML streamed page by page | Huge result sets — start reading before it finishes |
| `text` | Tab-separated lines | `grep` / `awk` / `cut` in shell scripts |
| `table` | ASCII table | Reading with human eyes |
| `off` | *(nothing on stdout)* | CI/CD — you only care about the exit code |

Set it **per command** (`--output table`), **per session** (`export AWS_DEFAULT_OUTPUT=table`), or **permanently** (`output = table` in your profile in `~/.aws/config`).

```bash
aws iam list-users --output table
aws s3api head-bucket --bucket my-bucket --output off && echo "exists"
```

> **Rule of thumb:** `table` for humans, `json` for programs, `text` + `--query` for shell scripts.

## 2. Filter at the source: `--filters` *(server-side)*

Many `describe-*` commands take a service-defined filter. **AWS does the filtering and sends back only matches** — faster and cheaper for big accounts.

```bash
aws ec2 describe-instances \
  --filters "Name=instance-state-name,Values=running" "Name=tag:Env,Values=prod"
```

The parameter name varies by service: `--filters` (EC2, RDS, Auto Scaling), `--filter` (SES, Cost Explorer), or `--filter-expression` (DynamoDB `scan`). Check the command's `help` page. **Use server-side filters first whenever they exist.**

## 3. Filter on your screen: `--query` *(client-side, JMESPath)*

`--query` takes a [JMESPath](https://jmespath.org/) expression and trims the full response *after* it arrives. It's more powerful than `--filters`, and works on every command.

Using this sample (`aws ec2 describe-volumes`) as the example data:

```json
{ "Volumes": [
  { "VolumeId": "vol-e11a5288", "Size": 30, "VolumeType": "standard", "State": "in-use",
    "Attachments": [ { "InstanceId": "i-a071c394", "State": "attached" } ] },
  { "VolumeId": "vol-2e410a47", "Size": 8,  "VolumeType": "standard", "State": "in-use",
    "Attachments": [ { "InstanceId": "i-4b41a37c", "State": "attached" } ] } ] }
```

### The building blocks

| Goal | Expression | Result |
|---|---|---|
| Everything under a key | `Volumes` | the whole list |
| Every element of a list | `Volumes[*]` | each volume |
| First / last element | `Volumes[0]` / `Volumes[-1]` | one volume |
| A slice | `Volumes[:2]`, `Volumes[::2]`, `Volumes[::-1]` | first two / every other / reversed |
| One field from each | `Volumes[*].VolumeId` | `["vol-e11a5288","vol-2e410a47"]` |
| Reach into nested data | `Volumes[*].Attachments[*].State` | `[["attached"],["attached"]]` |
| **Flatten** with `[]` | `Volumes[].Attachments[].State` | `["attached","attached"]` |
| **Filter by value** | `Volumes[?Size > `20`].VolumeId` | `["vol-e11a5288"]` |
| Pick several fields (list) | `Volumes[].[VolumeId, Size]` | `[["vol-e11a…",30],["vol-2e41…",8]]` |
| Pick & **rename** (hash) | `Volumes[].{ID:VolumeId, GB:Size}` | `[{"ID":…,"GB":30}, …]` |
| Pipe to a second step | `Volumes[].VolumeId \| [0]` | `"vol-e11a5288"` |
| Count | `length(Volumes)` | `2` |
| Sort | `sort_by(Volumes, &Size)[].VolumeId` | ids smallest → largest |
| Newest first, top 5 | `reverse(sort_by(Images,&CreationDate))[:5]` | five most recent |

**Two details that bite:**

- Literal values in a filter need **backticks**: `` ?State==`attached` `` (numbers too: `` ?Size > `20` ``). Plain single quotes inside also work for strings: `?State=='attached'`. Beware your shell's own handling of backticks and quotes — see [Quoting](command-structure-and-help.md#quoting-the-1-source-of-invalid-json-errors).
- `&` before a field name in `sort_by(…, &Field)` means "evaluate this per element" — it's required.

### Real-world one-liners

```bash
# Your account ID, nothing else
aws sts get-caller-identity --query Account --output text

# Names of all buckets, one per line
aws s3api list-buckets --query 'Buckets[].[Name]' --output text

# Running instances as a clean table
aws ec2 describe-instances \
  --filters "Name=instance-state-name,Values=running" \
  --query 'Reservations[].Instances[].{ID:InstanceId,Type:InstanceType,AZ:Placement.AvailabilityZone}' \
  --output table

# The most recent Amazon Linux AMI ID
aws ec2 describe-images --owners amazon \
  --filters "Name=name,Values=al2023-ami-2023*-x86_64" \
  --query 'sort_by(Images,&CreationDate)[-1].ImageId' --output text

# Volumes with no 'test' tag (exclusion with not_null)
aws ec2 describe-volumes \
  --query 'Volumes[?!not_null(Tags[?Value==`test`].Value)] | []'

# How many available volumes exceed 1000 IOPS?
aws ec2 describe-volumes --filters "Name=status,Values=available" \
  --query 'length(Volumes[?Iops > `1000`])'
```

## 4. Combine both: filter at the source, shape on screen

```bash
aws ec2 describe-volumes \
  --filters "Name=availability-zone,Values=us-west-2a" "Name=status,Values=attached" \
  --query 'Volumes[?Size > `50`].{Id:VolumeId,Size:Size,Type:VolumeType}'
```

Server-side filtering runs **first**, trimming what travels over the wire; then `--query` polishes the rest.

## Gotchas

- **`--output text` + `--query` + pagination:** with `text`, the query runs **once per page**, which can produce extra lines. With `json`/`yaml` it runs once over the whole result. When in doubt, query with `json` and add `--output text` only for a single-field extraction, or pipe through `head`.
- **Text columns are ordered alphabetically by key**, and keys can vary between resources — so *always* pair `--output text` with an explicit `--query` list (`[a,b,c]`) to pin column order.
- Missing fields come back as `None` in text and `null` in JSON.
- A single value in `text` output becomes one tab-separated line; wrap it in brackets (`Groups[].[GroupName]`) to get **one value per line**.
- **Experiment safely:** `aws ec2 describe-volumes | jpterm` (JMESPath Terminal) lets you type queries and see results live. `jq` is the other great tool for post-processing JSON.

## Next up

Large accounts return thousands of items. See [Pagination & Input Files](pagination-and-input-files.md).

[^aws-cli-output]: AWS CLI User Guide for Version 2, "Setting the output format in the AWS CLI."
[^aws-cli-filter]: AWS CLI User Guide for Version 2, "Filtering output in the AWS CLI."
