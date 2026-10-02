---
type: Guide
title: "Productivity: Auto-Prompt, Completion, Aliases & Scripting"
description: "Go faster and safer — auto-prompt, tab completion, command aliases, command history, and patterns for reliable shell scripts built on the AWS CLI."
tags: [aws, cli, auto-prompt, completion, alias, scripting, history, automation]
sources:
  - id: aws-cli-auto-prompt
    resource: https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-parameters-prompting.html
    title: AWS CLI User Guide — Enabling and using command prompts in the AWS CLI
  - id: aws-cli-completion
    resource: https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-completion.html
    title: AWS CLI User Guide — Configuring command completion
  - id: aws-cli-alias
    resource: https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-alias.html
    title: AWS CLI User Guide — Creating and using aliases
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-10-02T00:00:00Z
---

# Productivity: Auto-Prompt, Completion, Aliases & Scripting

## 📡 The mnemonic

**Learn the remote's shortcuts: it can suggest the next button (auto-prompt/completion), remember macros (aliases), and run a whole evening's routine unattended (scripts).**

## Auto-prompt — suggestions as you type

Auto-prompt (CLI v2) is the friendliest way to learn commands you don't know yet.

```bash
aws --cli-auto-prompt          # for one command
export AWS_CLI_AUTO_PROMPT=on  # for the shell session
```

Or permanently in `~/.aws/config`:

```ini
[default]
cli_auto_prompt = on-partial
```

What it gives you:

- **Command, parameter, and shorthand completion** — with descriptions, required parameters listed first.
- **Resource completion** — it calls AWS to suggest *your* actual table names, bucket names, instance IDs.
- **Region and profile completion** after `--region` / `--profile`, **file completion** after `file://`.
- **Fuzzy search** — type `west` and get every Region containing it.
- **F3** opens the help page for what you're typing; **Ctrl+R** searches history.

Two modes:

| Mode | Setting | Behavior |
|---|---|---|
| **Full** | `on` | Prompts on every `aws` command you run |
| **Partial** | `on-partial` | Prompts only when the command is incomplete or fails client-side validation — **safe to leave on** for existing scripts |

Turn it off for one command with `--no-cli-auto-prompt`. For the JMESPath `--query` experience, press **F5** to preview results as you type.

## Tab completion — for the shell you already love

Auto-prompt is its own full-screen mode; **tab completion** works inside your normal shell prompt.

```bash
# bash  (add to ~/.bashrc)
complete -C '/usr/local/bin/aws_completer' aws

# zsh  (add to ~/.zshrc)
autoload bashcompinit && bashcompinit
autoload -Uz compinit && compinit
complete -C '/usr/local/bin/aws_completer' aws
```

```powershell
# PowerShell — add to $PROFILE  (open it with: notepad $PROFILE)
Register-ArgumentCompleter -Native -CommandName aws -ScriptBlock {
    param($commandName, $wordToComplete, $cursorPosition)
    $env:COMP_LINE=$wordToComplete
    if ($env:COMP_LINE.Length -lt $cursorPosition){ $env:COMP_LINE=$env:COMP_LINE + " " }
    $env:COMP_POINT=$cursorPosition
    aws_completer.exe | ForEach-Object {
        [System.Management.Automation.CompletionResult]::new($_, $_, 'ParameterValue', $_)
    }
    Remove-Item Env:\COMP_LINE
    Remove-Item Env:\COMP_POINT
}
```

If `aws_completer` isn't found, locate it with `which aws_completer` (adjust the path above if the CLI was installed elsewhere, e.g., `~/.local/bin`). Then `aws dyn<TAB>` → `aws dynamodb`, `aws dynamodb d<TAB>` lists every `d…` action. Amazon Linux enables this out of the box.

## Aliases — your own macro buttons

Create `~/.aws/cli/alias` (on Windows, `%USERPROFILE%\.aws\cli\alias`). It must start with `[toplevel]`:

```ini
[toplevel]

whoami = sts get-caller-identity

whoami-short = sts get-caller-identity --query Account --output text

running = ec2 describe-instances
    --filters Name=instance-state-name,Values=running
    --query 'Reservations[].Instances[].[InstanceId,InstanceType,Tags[?Key==`Name`]|[0].Value]'
    --output table

[command ec2]
regions = describe-regions --query Regions[].RegionName --output text
```

```bash
aws whoami
aws running
aws ec2 regions
```

- Aliases can call other aliases; you can still append options (`aws whoami --output text`).
- Prefix a body with `!` to run **any shell command**, including a function (bash-compatible shell required):

```ini
authorize-my-ip =
  !f() {
    ip=$(curl -s https://checkip.amazonaws.com)
    aws ec2 authorize-security-group-ingress --group-id "${1}" --cidr "$ip/32" --protocol tcp --port 22
  }; f
```

Script aliases receive arguments **by position** (`${1}`, `${2}`), not by name. The community [awscli-aliases](https://github.com/awslabs/awscli-aliases) repo has a rich starter set.

## Command history

Turn on a local log of your CLI calls — superb for "what did I just run?" and for debugging:

```bash
aws configure set cli_history enabled
aws history list                 # recent commands with IDs
aws history show                 # details of the latest command
aws history show <command-id>
```

## Scripting reliably

The CLI is built for scripts. A few habits turn a fragile script into a dependable one.

```bash
#!/usr/bin/env bash
set -euo pipefail                       # stop on errors, undefined vars, and pipe failures

export AWS_PAGER=""                     # never wait at an '(END)' prompt
export AWS_PROFILE=${AWS_PROFILE:?set AWS_PROFILE}   # fail fast if no profile chosen
export AWS_DEFAULT_REGION=us-east-1

# Confirm identity before touching anything
aws sts get-caller-identity --query Arn --output text

# Capture a single clean value with --query + --output text
INSTANCE_ID=$(aws ec2 run-instances \
    --image-id "$AMI" --instance-type t3.micro --count 1 \
    --query 'Instances[0].InstanceId' --output text)

# Wait for readiness rather than sleeping
aws ec2 wait instance-running --instance-ids "$INSTANCE_ID"

# Check success by exit code
if aws s3api head-bucket --bucket my-bucket --output off 2>/dev/null; then
  echo "bucket exists"
fi
```

| Habit | Why |
|---|---|
| `--query … --output text` for single values | No JSON parsing needed in bash |
| `aws … wait …` instead of `sleep` | Correct on slow *and* fast days |
| Check **exit codes** (`0` = success) | The CLI exits non-zero on failure — use it |
| `--output off` when you only need success/fail | Quiet CI logs |
| Disable the pager (`AWS_PAGER=""`) | Prevents hangs |
| Set Region **explicitly** | Avoids "wrong Region" surprises |
| `--dry-run` / `--dryrun` first | Preview before destruction |
| Use **`jq`** for heavy JSON reshaping | `--query` handles most things; `jq` handles the rest |
| Never hard-code credentials | Use roles, SSO, or env vars injected by your CI system |

Useful exit codes: `0` success · `1` S3 transfer partly failed (some files skipped) · `2` command-line parsing problem or (for `s3`) files skipped · `130` interrupted · `252` invalid command · `253` configuration/credentials problem · `254` service returned an error · `255` general failure. Don't memorize them — remember **"zero is good, anything else is a story."**

## Next up

When it doesn't work: [Troubleshooting](troubleshooting.md).

[^aws-cli-auto-prompt]: AWS CLI User Guide for Version 2, "Enabling and using command prompts in the AWS CLI."
[^aws-cli-completion]: AWS CLI User Guide for Version 2, "Configuring command completion in the AWS CLI."
[^aws-cli-alias]: AWS CLI User Guide for Version 2, "Creating and using aliases in the AWS CLI."
