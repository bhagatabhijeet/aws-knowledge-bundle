---
type: Concept
title: "AWS CLI Mnemonics & Command Cheat Sheet"
description: "Every AWS CLI mnemonic in this folder plus the commands you'll actually type, on one page — keep it open in a tab."
tags: [aws, cli, mnemonics, cheatsheet, commands]
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-10-02T00:00:00Z
---

# The One-Page AWS CLI Cheat Sheet

## The universal remote, in one picture

| Remote piece | AWS CLI concept |
|---|---|
| 📡 The remote | `aws` |
| 📺 Device button | Service (`s3`, `ec2`, `iam` …) |
| 🔘 Action | Subcommand (`ls`, `describe-instances` …) |
| 🎚️ Dials | Options (`--region`, `--output`, `--query` …) |
| 🔑 Pairing code | Credentials (SSO, role, keys) |
| 🏠 Which house | Profile |
| 🗺️ Which room | Region |
| 📖 Channel-guide filter | `--filters` (at the source) / `--query` (on your screen) |
| 🖼️ Display style | `--output` |
| 📄 Pages | Pagination |
| ⭐ Macro buttons | Aliases / scripts |
| 🛋️ Preview | `--dry-run` / `--dryrun` |
| 📕 Manual | `aws … help` |

## The mnemonics

| Topic | Mnemonic |
|---|---|
| Why CLI | *"If you'll do it twice, type it once."* |
| Grammar | *"Device → action → dials."* — `aws <service> <subcommand> [options]` |
| Setup | *"Who am I? Which house? Which room?"* — credentials, profile, Region |
| Config files | *"credentials = secrets, bare names. config = everything else, and says `profile`."* |
| Precedence | *"The closer to your fingertips, the stronger the setting."* — flag > env var > file > machine default |
| Auth | *"Prefer the pairing code that expires."* — SSO/roles over static keys |
| SSO | *"`configure sso` once, `sso login` each morning."* |
| Sanity check | *"When in doubt, `get-caller-identity`."* |
| Filtering | *"Filter at the source (`--filters`), shape on your screen (`--query`)."* |
| Output | *"`table` for humans, `json` for programs, `text` + `--query` for scripts."* |
| S3 | *"`s3` is the big friendly buttons; `s3api` is the service panel."* |
| Sync | *"`sync` copies the gaps; `--delete` also removes the extras."* |
| S3 filters | *"Exclude the world, then include what you want."* |
| Safety | *"Preview, then perform."* |
| Scripts | *"Wait, don't sleep. Zero is good, anything else is a story."* |
| Debug | *"Version, identity, Region, clock — then `--debug`."* |

## Setup & identity

```bash
aws --version                          # confirm v2
aws update                             # self-update (installer-based installs)
aws configure                          # prompt for keys, Region, output
aws configure --profile work           # a named profile
aws configure sso                      # IAM Identity Center wizard
aws sso login --profile work           # sign in
aws sso logout
aws configure list                     # where is each setting coming from?
aws configure list-profiles
aws configure get region
aws configure set region us-west-2
aws sts get-caller-identity            # WHO AM I?
export AWS_PROFILE=work                # PowerShell: $Env:AWS_PROFILE="work"
```

## Global options

```bash
--profile NAME   --region REGION   --output json|yaml|yaml-stream|text|table|off
--query 'JMESPATH'   --debug   --no-paginate   --max-items N   --page-size N
--no-cli-pager   --cli-auto-prompt   --endpoint-url URL   --ca-bundle FILE
--cli-input-json file://x.json   --generate-cli-skeleton
```

## Help

```bash
aws help
aws ec2 help
aws ec2 describe-instances help
```

## JMESPath in 10 lines

```
Items[*].Name                 every Name
Items[].Name                  same, flattened
Items[0]  /  Items[-1]        first / last
Items[:3]                     first three
Items[?Size > `20`].Name      filter  (backticks around literals)
Items[].[Name,Size]           columns (list)
Items[].{N:Name,S:Size}       columns with labels (hash)
sort_by(Items,&Size)[-1]      largest by Size
length(Items)                 count
Items[].Name | [0]            pipe
```

## Everyday service commands

```bash
# S3
aws s3 ls                                   aws s3 ls s3://bkt --recursive --human-readable --summarize
aws s3 cp f.txt s3://bkt/                   aws s3 cp s3://bkt/f.txt ./
aws s3 sync ./dir s3://bkt/dir [--delete]   aws s3 rm s3://bkt/prefix/ --recursive --dryrun
aws s3 mb s3://bkt                          aws s3 presign s3://bkt/f.txt --expires-in 3600

# EC2
aws ec2 describe-instances --query 'Reservations[].Instances[].[InstanceId,State.Name]' --output table
aws ec2 start-instances --instance-ids i-0abc    aws ec2 stop-instances --instance-ids i-0abc
aws ec2 wait instance-running --instance-ids i-0abc

# IAM
aws iam list-users                          aws iam list-roles --query 'Roles[].RoleName'
aws iam get-user                            aws iam list-attached-user-policies --user-name alice

# DynamoDB
aws dynamodb list-tables                    aws dynamodb describe-table --table-name T
aws dynamodb get-item --table-name T --key file://key.json
aws dynamodb scan --table-name T --max-items 10

# CloudFormation / Lambda / Logs
aws cloudformation describe-stacks          aws cloudformation wait stack-create-complete --stack-name S
aws lambda list-functions --query 'Functions[].FunctionName'
aws logs tail /aws/lambda/my-fn --follow
```

## Error → first move

| You see | Try |
|---|---|
| `command not found` | New terminal / fix `PATH` |
| `Unable to locate credentials` | `aws configure` / `aws sso login` / `AWS_PROFILE` |
| `AccessDenied` | `get-caller-identity`, then check IAM policy |
| `ExpiredToken` | `aws sso login` / re-assume role |
| `SignatureDoesNotMatch` | Fix the clock |
| Empty results | Right Region? Right account? |
| Invalid JSON | Quote it properly or use `file://` |
| Hangs at `(END)` | `--no-cli-pager` |
