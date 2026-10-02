---
type: Guide
title: "Installing & Updating the AWS CLI"
description: "Install AWS CLI v2 on Linux, macOS, and Windows, verify it works, keep it updated, and fix the common 'command not found' problem."
tags: [aws, cli, install, update, linux, macos, windows]
sources:
  - id: aws-cli-install
    resource: https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
    title: AWS CLI User Guide — Installing or updating to the latest version of the AWS CLI
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-10-02T00:00:00Z
---

# Installing & Updating the AWS CLI

## 📡 The mnemonic

**Get the remote out of the box, put batteries in, and press one button to confirm it lights up: `aws --version`.**

Pick your operating system, run one install command, then verify. That's it.

## Before you start

- You need a **64-bit** OS (Linux x86-64 or ARM, macOS 11+, or a Microsoft-supported 64-bit Windows).
- Linux needs `glibc`, `groff`, and `less` — present on nearly every mainstream distro.
- You need an AWS account and credentials *after* install — that's the next page, [Configuring](configuring-profiles-and-settings.md).

## Linux

**Recommended — the install script** (x86 and ARM, installs for your user, no `sudo`):

```bash
curl -fsSL https://awscli.amazonaws.com/v2/install.sh | bash
```

For all users instead (installs to `/usr/local/aws-cli`, needs `sudo`):

```bash
curl -fsSL https://awscli.amazonaws.com/v2/install.sh | sudo bash -s -- --system
```

**Alternative — the zip installer** (good when you want to pin a version):

```bash
# x86-64
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
# ARM:  https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip
unzip awscliv2.zip
sudo ./aws/install
```

**Alternative — snap** (auto-updates, but you can't pin a version):

```bash
sudo snap install aws-cli --classic
```

> **Amazon Linux first-timers:** the preinstalled `yum` copy is old. Remove it first with `sudo yum remove awscli`, then install as above.

## macOS

**Recommended — the install script:**

```bash
curl -fsSL https://awscli.amazonaws.com/v2/install.sh | bash
```

**Alternative — the GUI installer:** download [AWSCLIV2.pkg](https://awscli.amazonaws.com/AWSCLIV2.pkg), double-click, follow the prompts.

**Alternative — command-line `.pkg` for all users:**

```bash
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
```

## Windows

**Recommended — the PowerShell install script** (current user, no admin needed):

```powershell
irm https://awscli.amazonaws.com/v2/install.ps1 | iex
```

**Alternative — the MSI installer:**

```powershell
# all users (admin required)
msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi
# current user only (no admin)
msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2-User.msi
```

Add `/qn` for a silent install in automation.

## Verify it lit up

```bash
aws --version
# aws-cli/2.x.x Python/3.x.x Linux/… exe/x86_64 …
```

Any `aws-cli/2.…` line means success. **If the shell says "command not found":** open a *new* terminal (the `PATH` update only applies to new sessions); if that fails, see [Troubleshooting](troubleshooting.md).

## Update

If you installed with the script or an official installer (zip, `.pkg`, MSI), the CLI can update itself:

```bash
aws update
```

For an all-users install, run it elevated (`sudo` on Linux/macOS, an Administrator prompt on Windows). Snap installs auto-refresh. New AWS features land in CLI releases constantly, so if a command or parameter from the docs "doesn't exist", **update first.**

## Uninstall

Uninstall the same way you installed — if you used a package manager, use that same package manager to remove it. Mixing methods is the #1 cause of "I uninstalled it but `aws --version` still works".

## Don't want to install anything?

**AWS CloudShell** (the terminal icon in the Console's top bar) gives you a browser shell with the CLI already installed and already signed in as the user who opened it. Great for a quick command, not for long-lived scripts.

## Next up

The remote is in your hand but not paired to anything yet: [Configuring: Profiles & Settings](configuring-profiles-and-settings.md).

[^aws-cli-install]: AWS CLI User Guide for Version 2, "Installing or updating to the latest version of the AWS CLI."
