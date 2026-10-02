---
type: Concept
title: "AWS CLI Glossary"
description: "Every AWS CLI term used in this folder, defined in one line, with its universal-remote analogy equivalent."
tags: [aws, cli, glossary]
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-10-02T00:00:00Z
---

# AWS CLI Glossary

| Term | Definition | Remote analogy |
|---|---|---|
| **AWS CLI** | The command-line tool (`aws`) that calls AWS service APIs | The universal remote |
| **Service (command)** | The top-level word after `aws`, usually one AWS service | A device button |
| **Subcommand** | The operation to perform, mirroring an API action | The action on a device |
| **Option / parameter** | A `--flag` or value that tunes a command | A dial |
| **Global option** | An option accepted by every command (`--region`, `--output`, `--profile`, `--query`, `--debug`) | Dials every device has |
| **Profile** | A named group of settings (credentials/role/Region/output) | One paired house |
| **`default` profile** | The profile used when none is specified | The remote's factory pairing |
| **config file** | `~/.aws/config` — Region, output, SSO, role settings | The remote's settings menu |
| **credentials file** | `~/.aws/credentials` — secret keys and tokens | The pairing code card |
| **Access key** | An access key ID + secret access key pair for an IAM user; long-lived | A permanent code taped to the remote |
| **Session token** | Extra secret that accompanies temporary credentials | The code's expiry stamp |
| **Temporary credentials** | Short-lived keys issued by STS, SSO, or a role | A pairing code that expires |
| **IAM Identity Center (SSO)** | Browser-based sign-in that issues temporary role credentials | Pairing by scanning a QR code each morning |
| **`sso-session`** | A config section holding SSO start URL/Region, shareable across profiles | One QR sign-in, many houses |
| **Assume role** | Temporarily taking on an IAM role's permissions via STS | Borrowing another remote for a job |
| **STS** | Security Token Service — issues temporary credentials | The pairing-code printer |
| **Region** | The geographic AWS location a command targets | Which room/city in the house |
| **Precedence** | The order in which settings sources override each other | Which instruction the remote obeys first |
| **Environment variable** | A shell variable (e.g., `AWS_PROFILE`) that overrides config files | A sticky note on the remote |
| **`--query`** | Client-side filtering of the response with JMESPath | Filtering the channel guide on screen |
| **JMESPath** | The query language behind `--query` | The guide's search syntax |
| **`--filters`** | Service-side filtering performed by AWS before replying | Asking the cable company to send fewer channels |
| **Output format** | How results are displayed: json, yaml, yaml-stream, text, table, off | Guide display style |
| **Pagination** | Retrieving large results in pages; automatic by default | Flipping channel-guide pages |
| **`--max-items` / `--page-size`** | Cap total items printed / items per API call | Items on screen / items per fetch |
| **`NextToken`** | Marker returned when output is capped, used with `--starting-token` | A bookmark to the next page |
| **Pager** | Client-side program (like `less`) that scrolls long output | The scroll wheel |
| **Skeleton** | A generated template of a command's parameters (`--generate-cli-skeleton`) | A blank form for the button's dials |
| **`--cli-input-json` / `--cli-input-yaml`** | Load parameters from a JSON/YAML file | Handing in the filled-in form |
| **`file://` / `fileb://`** | Prefixes that load a parameter from a text / binary file | Feeding a card into the remote |
| **Shorthand syntax** | Compact `Key=value,Key2=value2` parameter form | Typing dials in short form |
| **Dry run** | A preview that checks a command without performing it | Preview before the movie starts |
| **Waiter (`wait`)** | A subcommand that blocks until a resource reaches a state | Pausing until the TV warms up |
| **Auto-prompt** | v2 interactive mode that suggests commands and resources as you type | The remote's suggestion screen |
| **Command completion** | Tab-key completion via `aws_completer` in your shell | Autocomplete on the buttons |
| **Alias** | A user-defined shortcut in `~/.aws/cli/alias` | A macro button |
| **`aws s3`** | High-level, file-like S3 commands (`cp`, `sync`, `ls`, …) | The big friendly buttons |
| **`aws s3api`** | Low-level commands mapping 1:1 to the S3 API | The service panel |
| **Multipart upload** | Large objects split into parts for upload; automatic in `aws s3` | Shipping a big sofa in sections |
| **Presigned URL** | A time-limited URL granting access to one object | A guest pass for one movie |
| **CloudShell** | A browser-based shell with the CLI pre-installed and signed in | A remote built into the wall |
| **`--debug`** | Verbose output showing credential lookup, request, and response | Opening the remote's back panel |
| **Exit code** | Process result: `0` means success, non-zero means failure | The remote's beep |
