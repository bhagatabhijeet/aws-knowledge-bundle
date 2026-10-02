---
type: Guide
title: "Working with Amazon S3 from the CLI"
description: "The high-level aws s3 commands (ls, cp, mv, rm, sync, mb, rb, presign) and when to drop down to aws s3api — with the flags and safety habits that matter."
tags: [aws, cli, s3, sync, cp, presign, s3api]
sources:
  - id: aws-cli-s3
    resource: https://docs.aws.amazon.com/cli/latest/userguide/cli-services-s3-commands.html
    title: AWS CLI User Guide — Using high-level (s3) commands in the AWS CLI
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-10-02T00:00:00Z
---

# Working with Amazon S3 from the CLI

## 📡 The mnemonic

**`aws s3` is the remote's big friendly buttons. `aws s3api` is the service panel with every tiny screw.** Start with the big buttons; open the panel only when they can't do it.

S3 concepts used here (bucket, prefix, object, storage class) are covered in the [S3 folder](../s3/index.md).

| Command family | Style | Use for |
|---|---|---|
| **`aws s3`** *(high-level)* | Unix-like: `ls`, `cp`, `mv`, `rm`, `sync` | File-style work, bulk transfers, everyday tasks |
| **`aws s3api`** *(low-level)* | 1:1 with the S3 API: `put-bucket-versioning`, `list-objects-v2`, … | Settings and features the high-level commands don't expose |

S3 paths look like `s3://bucket-name/optional/prefix/key.txt`.

## The everyday commands

```bash
# Buckets
aws s3 mb s3://my-unique-bucket-name           # make bucket (names are globally unique)
aws s3 ls                                      # list all buckets
aws s3 rb s3://my-unique-bucket-name           # remove bucket (must be empty)
aws s3 rb s3://my-unique-bucket-name --force   # empty it first, then remove ⚠️

# Browsing
aws s3 ls s3://my-bucket                       # top level of a bucket
aws s3 ls s3://my-bucket/logs/ --recursive --human-readable --summarize

# Copy: local → S3, S3 → local, S3 → S3
aws s3 cp report.pdf s3://my-bucket/reports/
aws s3 cp s3://my-bucket/reports/report.pdf ./
aws s3 cp s3://my-bucket/a/ s3://other-bucket/a/ --recursive

# Move (copy, then delete the source)
aws s3 mv old.txt s3://my-bucket/archive/old.txt

# Delete
aws s3 rm s3://my-bucket/reports/report.pdf
aws s3 rm s3://my-bucket/tmp/ --recursive      # everything under the prefix ⚠️
```

Large uploads are automatically split into **multipart uploads** for you. (A failed upload isn't resumable; a hard-killed one can leave orphaned parts — clean up with `aws s3api abort-multipart-upload`, or add a [lifecycle rule](../s3/lifecycle-management.md).)

## `sync` — the star of the show

`aws s3 sync` makes a destination match a source, copying only what's **new or changed** (by size and modified time).

```bash
aws s3 sync ./site s3://my-bucket/site               # upload what changed
aws s3 sync s3://my-bucket/site ./site-backup        # download what changed
aws s3 sync s3://bucket-a s3://bucket-b              # bucket to bucket
aws s3 sync ./site s3://my-bucket/site --delete      # also DELETE destination files missing from source ⚠️
```

**Mnemonic:** *"`sync` copies the gaps. `--delete` also removes the extras."*

## Flags worth knowing

| Flag | Applies to | Effect |
|---|---|---|
| `--recursive` | `cp`, `mv`, `rm`, `ls` | Act on everything under a prefix/directory |
| `--dryrun` | `cp`, `mv`, `rm`, `sync` | **Print what would happen; change nothing** |
| `--exclude` / `--include` | `cp`, `mv`, `rm`, `sync` | Glob filters, **applied in the order written** |
| `--delete` | `sync` | Remove destination items not in the source |
| `--storage-class` | `cp`, `mv`, `sync` | e.g., `STANDARD_IA`, `GLACIER` — see [Storage Classes](../s3/storage-classes.md) |
| `--acl` | `cp`, `mv`, `sync` | Canned ACL such as `private` (see the [security warning](../s3/security-and-access-control.md)) |
| `--no-overwrite` | `cp`, `mv`, `sync` | Don't replace objects that already exist at the destination |
| `--sse`, `--sse-kms-key-id` | `cp`, `sync` | Server-side encryption — see [Encryption](../s3/encryption.md) |
| `--human-readable`, `--summarize` | `ls` | Friendly sizes and totals |

### Filter order matters

Filters apply left-to-right and later ones override earlier ones:

```bash
aws s3 cp . s3://my-bucket/ --recursive --exclude "*" --include "*.jpg"
#   exclude everything, then add back only .jpg files   ← the usual pattern
aws s3 cp . s3://my-bucket/ --recursive --include "*.jpg" --exclude "*"
#   copies NOTHING: the final --exclude "*" overrides the include
```

**Mnemonic:** *"Exclude the world, then include what you want."*

## Piping through stdin/stdout

A lone `-` means "stream":

```bash
echo "hello" | aws s3 cp - s3://my-bucket/hello.txt          # upload from stdin
aws s3 cp s3://my-bucket/hello.txt -                          # print to stdout
aws s3 cp s3://my-bucket/logs.txt - | gzip | aws s3 cp - s3://my-bucket/logs.txt.gz
```

## Share an object temporarily: `presign`

```bash
aws s3 presign s3://my-bucket/report.pdf --expires-in 3600
# → a URL anyone can use for the next hour, with your permissions, no AWS login needed
```

See [Sharing & Presigned URLs](../s3/sharing-and-presigned-urls.md) for how this works and its caveats.

## Dropping down to `s3api`

When the friendly button isn't enough:

```bash
aws s3api list-objects-v2 --bucket my-bucket --prefix logs/ --max-items 10
aws s3api put-bucket-versioning --bucket my-bucket \
  --versioning-configuration Status=Enabled
aws s3api get-object-attributes --bucket my-bucket --key a.txt --object-attributes ObjectSize
aws s3api head-bucket --bucket my-bucket --output off && echo "bucket exists and I can access it"
```

`s3api` supports `--query`, pagination flags, and `--cli-input-json` — the high-level `s3` commands don't support the latter.

## Safety habits

1. **`--dryrun` first** on anything with `--recursive`, `--delete`, `rm`, or `mv`.
2. **Check your identity:** `aws sts get-caller-identity` before touching production buckets.
3. **Versioning on important buckets** turns a bad `rm`/`sync --delete` from a disaster into an inconvenience — see [Versioning](../s3/versioning.md).
4. **Quote globs** (`"*.txt"`) so your shell doesn't expand them first.
5. Moving between buckets via **access point aliases?** Use `--validate-same-s3-paths` so `mv` can't accidentally move an object onto itself.

## Next up

Make all this faster to type: [Productivity: Auto-Prompt, Completion, Aliases & Scripting](productivity-and-scripting.md).

[^aws-cli-s3]: AWS CLI User Guide for Version 2, "Using high-level (s3) commands in the AWS CLI."
