---
type: Concept
title: "What is the AWS CLI?"
description: "The AWS CLI is one command-line tool that talks to every AWS service's API — why it beats clicking, how it works under the hood, and what changed between v1 and v2."
tags: [aws, cli, overview, api, v2]
sources:
  - id: aws-cli-welcome
    resource: https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html
    title: AWS CLI User Guide for Version 2 — What is the AWS CLI?
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-10-02T00:00:00Z
---

# What is the AWS CLI?

## 📡 The mnemonic

**The CLI is the universal remote. Every button on it is just an API call wearing a short name.**

Behind every AWS service is a web API: "create this bucket", "describe these instances". The Console is a website that calls those APIs for you when you click. The CLI is a program that calls the *same* APIs when you type. Same house, same devices — a different way of pressing the buttons.

## Why reach for the remote instead of the touch panel?

| Use the CLI when you want to… | Because… |
|---|---|
| **Repeat** something | A command pasted into a runbook gives the same result every time; clicks don't |
| **Automate** | Shell scripts, cron jobs, CI/CD pipelines all speak CLI |
| **Inspect fast** | `aws ec2 describe-instances --query …` beats paging through five Console tabs |
| **Work in bulk** | One `aws s3 sync` replaces thousands of clicks |
| **Share exact steps** | A command is unambiguous; "click the blue button" is not |
| **Reach new features early** | New services and parameters usually land in the CLI immediately |

**Mnemonic:** *"If you'll do it twice, type it once."*

## How it works, in four beats

1. You type `aws s3 ls`.
2. The CLI finds your **credentials** and **Region** (see [Configuring](configuring-profiles-and-settings.md)).
3. It builds the matching API request and **cryptographically signs** it with your credentials.
4. AWS verifies the signature, checks your IAM permissions, and returns a response the CLI formats for you.

Two consequences worth remembering:

- **Your clock matters.** The signature includes a timestamp; if your computer's clock is far off, AWS rejects the request. See [Troubleshooting](troubleshooting.md).
- **Permissions are IAM's job, not the CLI's.** The CLI can only press buttons your identity is allowed to press. See [IAM — Policies](../iam/policies.md).

## Version 1 vs. Version 2

Both use the same command name, `aws`. **Use version 2.** It is the current major version and adds:

- Self-contained installers (no Python install needed)
- **IAM Identity Center (SSO)** support built in
- **Auto-prompt** (interactive suggestions as you type)
- A **client-side pager** and more output formats (`yaml`, `yaml-stream`, `table`, `off`)
- Wizards such as `aws configure sso`

If a machine still has v1, see AWS's migration guide before upgrading, and check `aws --version` to know which one you're actually running.

## Where the CLI fits among AWS's other tools

| Tool | Best for |
|---|---|
| **Console** | Learning, exploring, one-off visual tasks |
| **AWS CLI** | Everyday admin, scripting, quick inspection |
| **SDKs** (Python/boto3, JavaScript, Java, …) | Calling AWS from inside an application |
| **CloudFormation / CDK / Terraform** | Repeatable, versioned infrastructure definitions |
| **CloudShell** | A browser-based shell with the CLI preinstalled and already authenticated |

The CLI is the middle rung: more repeatable than the Console, lighter than writing an application.

## Next up

Get the remote into your hands: [Installing & Updating](installing-and-updating.md).

[^aws-cli-welcome]: AWS CLI User Guide for Version 2, "What is the AWS Command Line Interface?"
