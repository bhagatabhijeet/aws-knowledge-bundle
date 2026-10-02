---
type: Guide
title: "Configuring the AWS CLI: Profiles & Settings"
description: "Pair the remote to your account — aws configure, the config and credentials files, named profiles, environment variables, and the precedence order that decides which setting wins."
tags: [aws, cli, configure, profiles, credentials, config, environment-variables, precedence]
sources:
  - id: aws-cli-configure
    resource: https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html
    title: AWS CLI User Guide — Configuring settings for the AWS CLI
  - id: aws-cli-config-files
    resource: https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html
    title: AWS CLI User Guide — Configuration and credential file settings
  - id: aws-cli-envvars
    resource: https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html
    title: AWS CLI User Guide — Configuring environment variables
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-10-02T00:00:00Z
---

# Configuring the AWS CLI: Profiles & Settings

![Where does the CLI look for its settings?](assets/images/cli-config-precedence.svg)

## 📡 The mnemonic

**Pairing the remote means answering three questions: *Who am I?* (credentials) · *Which house?* (profile/account) · *Which room?* (Region).** Everything else is a preference.

There are several places those answers can live. The CLI checks them in a fixed order, and **the closest, most specific one wins.**

## The fast path: `aws configure`

```bash
aws configure
```

```
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: us-east-1
Default output format [None]: json
```

It writes two plain-text files in your home directory:

| File | Linux / macOS | Windows | Holds |
|---|---|---|---|
| **credentials** | `~/.aws/credentials` | `C:\Users\<you>\.aws\credentials` | Secrets: access key ID, secret key, session token |
| **config** | `~/.aws/config` | `C:\Users\<you>\.aws\config` | Everything else: region, output, SSO, roles |

> ⚠️ **Access keys are long-lived passwords.** Use `aws configure` with keys only for learning or when nothing better exists. For real work, prefer [SSO or roles](authentication-sso-and-roles.md) — temporary credentials that expire on their own.

Verify you're paired and *who* AWS thinks you are:

```bash
aws sts get-caller-identity
```

```json
{
    "UserId": "AIDAEXAMPLE",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/me"
}
```

This is the single most useful sanity-check command in the CLI. **Run it whenever you're unsure which account or identity you're using.**

## What the files look like

```ini
# ~/.aws/credentials   — section headers are just the profile name
[default]
aws_access_key_id     = AKIAIOSFODNN7EXAMPLE
aws_secret_access_key = wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

[work]
aws_access_key_id     = AKIAI44QH8DHBEXAMPLE
aws_secret_access_key = je7MtGbClwBF/2Zp9Utk/h3yCo8nvbEXAMPLEKEY
```

```ini
# ~/.aws/config   — non-default profiles need the word "profile" in the header
[default]
region = us-east-1
output = json

[profile work]
region = eu-west-1
output = table
```

**Mnemonic:** *"credentials = secrets, bare names. config = everything else, and says `profile`."*

## Named profiles: many remotes, one drawer

A **profile** is a named bundle of settings — typically one account/role plus its default Region and output. Keep one per account or role.

```bash
aws configure --profile work          # create or edit the 'work' profile

aws s3 ls --profile work              # use it for one command
export AWS_PROFILE=work               # …or for the whole shell session
$Env:AWS_PROFILE = "work"             # PowerShell equivalent
```

If you set nothing, the CLI uses the profile named `default`.

## Inspect and edit settings without opening a file

```bash
aws configure list                    # what is the CLI actually using, and from where?
aws configure list --profile work
aws configure list-profiles           # every profile it can see
aws configure get region --profile work
aws configure set region us-west-2 --profile work
aws configure set output yaml
```

`aws configure list` is the **debugger for configuration** — it shows each value *and which source supplied it* (env var, config file, command line):

```
NAME       : VALUE                : TYPE                    : LOCATION
profile    : work                 : manual                  : --profile
access_key : ****************ABCD : shared-credentials-file :
region     : us-west-2            : env                     : AWS_DEFAULT_REGION
```

Got a CSV of keys downloaded from the Console? `aws configure import --csv file://credentials.csv` loads them into a profile.

## Handy settings you can put in `config`

| Setting | What it does |
|---|---|
| `region` | Default Region for the profile |
| `output` | Default output format — `json`, `yaml`, `yaml-stream`, `text`, `table`, `off` |
| `cli_pager` | Which pager program shows long output (empty string = no pager) |
| `cli_auto_prompt` | `on` / `on-partial` — see [Productivity](productivity-and-scripting.md) |
| `role_arn`, `source_profile`, `mfa_serial` | Assume an IAM role — see [Authentication](authentication-sso-and-roles.md) |
| `sso_session`, `sso_account_id`, `sso_role_name` | IAM Identity Center — see [Authentication](authentication-sso-and-roles.md) |
| `max_attempts`, `retry_mode` | Retry behavior (`standard` or `adaptive` are the modern choices) |
| `ca_bundle` | Custom CA certificates (corporate proxies) |

## Environment variables

Env vars are perfect for scripts, containers, and CI — and they **override** the config files.

| Variable | Purpose |
|---|---|
| `AWS_ACCESS_KEY_ID` / `AWS_SECRET_ACCESS_KEY` / `AWS_SESSION_TOKEN` | Credentials (the token is needed for temporary credentials) |
| `AWS_PROFILE` | Which named profile to use |
| `AWS_REGION` / `AWS_DEFAULT_REGION` | Region (`AWS_REGION` wins if both set) |
| `AWS_DEFAULT_OUTPUT` | Output format |
| `AWS_PAGER` | Pager program (`""` disables paging) |
| `AWS_CONFIG_FILE` / `AWS_SHARED_CREDENTIALS_FILE` | Use files in a non-default location |
| `AWS_ENDPOINT_URL` | Send requests to a custom endpoint (e.g., a local emulator) |
| `AWS_MAX_ATTEMPTS` / `AWS_RETRY_MODE` | Retry tuning |
| `AWS_CA_BUNDLE` | Path to a CA certificate bundle |

```bash
# Linux / macOS
export AWS_DEFAULT_REGION=us-west-2
# Windows PowerShell
$Env:AWS_DEFAULT_REGION = "us-west-2"
# Windows cmd (persistent)
setx AWS_DEFAULT_REGION us-west-2
```

## Precedence: who wins when settings conflict?

From strongest to weakest, the CLI looks at:

1. **Command-line options** — `--region`, `--profile`, `--output`
2. **Environment variables**
3. **Assume role** / **assume role with web identity**
4. **IAM Identity Center (SSO)** configuration
5. **The `credentials` file**
6. **Custom credential process**
7. **The `config` file**
8. **Container credentials** (ECS task role)
9. **EC2 instance profile** credentials

**Mnemonic:** *"The closer to your fingertips, the stronger the setting."* A flag you typed this second beats an env var, which beats a file, which beats whatever the machine was born with.

### The classic trap

You ran `aws configure` with keys for Account A, but `AWS_ACCESS_KEY_ID` is still exported from yesterday for Account B. **The env var wins**, silently. Symptom: commands act on the "wrong" account. Cure: `aws sts get-caller-identity`, then `aws configure list` to find the source, then `unset` the stray variable.

## Region: the room you're controlling

Most resources live in one Region and are invisible from the others. If `aws ec2 describe-instances` shows nothing but you *know* you have instances — you're probably in the wrong Region. Region is resolved in this order: `--region` → `AWS_REGION` → `AWS_DEFAULT_REGION` → profile `region`. (See [Global Infrastructure — Regions](../global-infrastructure/regions.md).)

```bash
aws ec2 describe-instances --region eu-west-1
```

## Next up

Replace long-lived keys with something safer: [Authentication: SSO, Roles & Credentials](authentication-sso-and-roles.md).

[^aws-cli-configure]: AWS CLI User Guide for Version 2, "Configuring settings for the AWS CLI."
[^aws-cli-config-files]: AWS CLI User Guide for Version 2, "Configuration and credential file settings in the AWS CLI."
[^aws-cli-envvars]: AWS CLI User Guide for Version 2, "Configuring environment variables for the AWS CLI."
