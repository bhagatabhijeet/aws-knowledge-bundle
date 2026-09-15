---
type: Concept
title: "Private Device Lab"
description: "Your own fenced-off pasture: real iOS and Android devices reserved exclusively for your account, with settings that persist between sessions."
tags: [aws, device-farm, private-device-lab]
sources:
  - id: aws-device-farm-private-lab
    resource: https://aws.amazon.com/device-farm/
    title: AWS Device Farm — Set up your own private device lab in the cloud
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Private Device Lab — Your Own Fenced-Off Pasture

## 🚧 The mnemonic

The public fleet is a shared field — you and every other tenant on the farm take turns with the herd, and the next tenant's animal gets a clean pen with no memory of what you did. A **Private Device Lab** is your own fenced-off pasture on the same farm: specific iOS and Android devices, provisioned exactly the way you asked, that stay yours between visits.

## What you actually get

- **Choice of devices.** You select the exact iOS and Android devices you want, provisioned with the exact OS versions and configurations your project needs.
- **Exclusive use.** Because those devices are reserved for your account alone, you're never waiting behind another tenant's test run finishing up.
- **Persisted state.** Settings and app data can persist **between sessions** — unlike the public fleet, which resets a device to a clean state after each use, your private devices can remember what you left on them.

**Mnemonic:** *"A fenced pasture means nobody else's cattle trample your test — and your own animal remembers you the next time you visit."*

## When this matters

| Situation | Why the public fleet falls short |
|---|---|
| Heavy, continuous testing (many runs per day) | You'd otherwise queue behind other tenants sharing the same public devices |
| A workflow that depends on state carrying over | Public devices reset after every session — you'd have to re-seed state every single run |
| A specific, narrow set of device/OS combinations your team needs constantly | Building and maintaining that exact custom pool from the shared fleet, every time, is wasted setup cost |

## How it fits with the rest of the farm

A Private Device Lab isn't a different service — it's the same automated testing, Remote Access, and debugging artifacts described elsewhere in this folder, just running against devices nobody else can touch. Think of it as a special kind of [Device Pool](real-device-testing.md) that happens to be exclusively yours.

## Next up

Once your app builds and test packages are flowing, you'll want the farm wired into the rest of your toolchain: [Integrations & Workflow](integrations-and-workflow.md).

[^aws-device-farm-private-lab]: AWS Device Farm, "Set up your own private device lab in the cloud," aws.amazon.com/device-farm.
