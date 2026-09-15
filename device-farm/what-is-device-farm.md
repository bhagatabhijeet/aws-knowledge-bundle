---
type: Concept
title: "What is AWS Device Farm?"
description: "Device Farm rents you real physical devices and desktop browsers in the AWS Cloud, so you can test how your app actually behaves without owning a single one of them."
tags: [aws, device-farm, testing, fundamentals]
sources:
  - id: aws-device-farm-product
    resource: https://aws.amazon.com/device-farm/
    title: AWS Device Farm product page
  - id: aws-device-farm-guide
    resource: https://docs.aws.amazon.com/devicefarm/latest/developerguide/welcome.html
    title: AWS Device Farm Developer Guide — What Is AWS Device Farm?
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# What is AWS Device Farm?

**AWS Device Farm** is an app-testing service: you hand it a build of your web or mobile app, it hands you back test results — videos, logs, and performance data — from real physical devices and real desktop browsers, hosted entirely in the AWS Cloud. You never provision, rack, cable, or maintain a single phone, tablet, or browser VM yourself.

## 🌾 The mnemonic: it's a literal farm

The product's own name gives you the analogy for free. Picture a working **farm**: a **barn** full of real animals (physical devices), a **ranch hand** who can run the whole herd through the same obstacle course at once (automated tests), a gate you can reach through yourself to walk one animal on a leash (Remote Access), and — next door — a separate **stable** of tireless robot horses that run laps all day (a managed Selenium Grid for browsers).

![The Device Farm barn](assets/images/device-farm-overview.svg)

## Real animals beat cardboard cutouts

The single most important idea in Device Farm is that it tests on **real, physical hardware** — not emulators or simulators.

| | Emulator / Simulator | Device Farm (real device) |
|---|---|---|
| CPU & memory behavior | Modeled, approximated | The device's actual chipset and RAM, under actual load |
| Carrier & manufacturer customizations | Absent | Present — real OEM firmware skins, real carrier bloatware |
| Location, sensors, battery | Faked | Real GPS, real accelerometer, real battery drain |
| Network conditions | Simulated in software | Configurable real network profiles (3G, offline, etc.) |
| "Does it actually work for a real customer?" | An educated guess | An observed fact |

**Mnemonic:** *"Real animals beat cardboard cutouts every time."* An emulator can tell you your code runs. Only a real device can tell you it runs the way your actual customer will experience it.

## Two ways to test, one farm

Device Farm gives you two fundamentally different modes of interacting with the herd:

1. **Automated testing** — a ranch hand walks every animal in the pool through the same scripted or built-in test, in parallel, so your whole suite finishes in the time one device would take alone. See [Automated Testing Frameworks](automated-testing-frameworks.md).
2. **Remote Access** — you reach into the pen yourself: gesture, swipe, and type on one real device in real time, streamed straight to your browser. See [Remote Access](remote-access.md).

And a third, separate offering entirely:

3. **Desktop Browser Testing** — the stable next door, a fully managed Selenium Grid running your WebDriver tests across Chrome, Firefox, and Internet Explorer. See [Desktop Browser Testing](desktop-browser-testing.md).

## What you'll use Device Farm for, in practice

- Running an existing Appium, Instrumentation (Espresso/UI Automator), or XCTest suite against dozens of real devices in parallel, on every commit
- Manually reproducing a bug a customer reported on one specific phone model
- Running a quick, no-scripting-required smoke test (built-in Fuzz) against a brand-new build
- Cross-browser regression testing a web app's Selenium suite before a release
- Reserving a private, exclusive slice of the fleet for a team that tests constantly

## Next up

Start with the herd itself: [Real Device Testing](real-device-testing.md) — what's actually in the barn, and how you group animals into pools.

[^aws-device-farm-product]: AWS Device Farm product page, aws.amazon.com/device-farm.
[^aws-device-farm-guide]: AWS Device Farm Developer Guide, "What Is AWS Device Farm?"
