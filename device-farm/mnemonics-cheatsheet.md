---
type: Concept
title: "Device Farm Mnemonics Cheat Sheet"
description: "Every Device Farm mnemonic in this folder, on one page — read this the night before an exam or interview."
tags: [aws, device-farm, mnemonics, cheatsheet]
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# The One-Page Device Farm Cheat Sheet

## The farm, in one picture

| Farm piece | Device Farm concept |
|---|---|
| 🌾 The farm itself | AWS Device Farm |
| 📋 The front gate you check in at | A **Project** — the container for your app uploads and run history |
| 🐄 An animal in the barn | A real physical device — real model, real OS, real carrier |
| 🐑 A herd of similar animals | A **Device Pool** — public (AWS-curated) or custom (yours) |
| 🤠 A ranch hand running the whole herd through an obstacle course | An automated test run — server-side execution, in parallel |
| 🕹️ Reaching into the pen and walking one animal yourself | **Remote Access** — a live, interactive session on one real device |
| 🔌 A private phone line straight to one animal's stall | The **Appium endpoint** — a client-side, session-scoped Appium server URL |
| 🌦️ The weather machine over the pen | Simulated network conditions, GPS location, language, and preloaded data |
| 📹🩺 The vet's checkup report | Video, device/framework logs, and performance data from every run |
| 🧩 The farmhand who spots one shared symptom | Automatic issue clustering across failed devices |
| 🚧 A fenced-off private pasture, yours alone | The **Private Device Lab** — exclusive devices with persisted state |
| 🐴 The separate stable of robot horses | **Desktop Browser Testing** — a managed Selenium Grid |
| 📡 The control room wired to your other equipment | Integrations — IDEs, Jenkins, the CLI/API, CI/CD pipelines |

## The mnemonics worth memorizing word-for-word

1. **"Real animals beat cardboard cutouts every time."** — physical devices catch what emulators can't: real CPU/memory pressure, real OEM firmware, real carrier quirks.
2. **"Don't test against every animal on the farm — test against the herd your customers actually resemble."** — build device pools from real usage data, not the whole fleet, for routine runs.
3. **"Let the ranch hand wander the field before you teach it a specific route."** — run built-in **Fuzz** before investing in scripted coverage.
4. **"Server-side: the farm's own ranch hand runs the whole course for you. Client-side: you're handed one animal on a leash and an open gate."** — server-side execution runs your uploaded tests across a pool; a Remote Access session's Appium endpoint lets you drive one device from your own machine instead.
5. **"The farmhand who notices five sick animals share one symptom doesn't file five reports — they file one, with five names attached."** — automatic issue clustering across a pool's failures.
6. **"A fenced pasture means nobody else's cattle trample your test — and your own animal remembers you the next time you visit."** — Private Device Lab: exclusive, contention-free, state-persisting devices.
7. **"The robot horses never get tired of running the same lap."** — Desktop Browser Testing's managed Selenium Grid scales without you provisioning it.

## Frameworks, at a glance

```
Android   → Automatic Appium tests · Instrumentation (Espresso, UI Automator)
iOS       → Automatic Appium tests · XCTest · XCTest UI
Web apps  → Appium
Built-in  → Fuzz (Android & iOS) — the only built-in, no-script test type
```

## Speed-round definitions

| Term | One line |
|---|---|
| Project | The container holding your app uploads and run/session history |
| Device Pool | A curated group of devices to test against — public or custom |
| Automated test run | A scripted or built-in test executed in parallel across a device pool |
| Server-side execution | Device Farm runs your uploaded app + tests on its own managed test hosts |
| Client-side (Appium endpoint) | Your own Appium client drives a real device through a session-scoped server URL |
| Remote Access | An interactive, real-time manual session on one real device |
| Private Device Lab | Devices reserved exclusively for your account, with persisted state |
| Desktop Browser Testing | A managed Selenium Grid across Chrome, Firefox, and Internet Explorer |
| Issue clustering | Grouping related failures across devices into one reported issue |

For full definitions of every term, see the [Glossary](glossary.md). To go deeper on any single row, jump back to the [folder index](index.md).
