---
type: Concept
title: "Device Farm Glossary"
description: "Every Device Farm term used in this folder, defined in one line, with its farm-analogy equivalent."
tags: [aws, device-farm, glossary]
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Device Farm Glossary

| Term | Definition | Farm analogy |
|---|---|---|
| **Project** | The top-level container holding your app uploads, device pools, and run/session history | The front gate you check in at |
| **Real device** | An actual physical phone or tablet, with real manufacturer firmware and carrier modifications | An animal in the barn |
| **Emulator / Simulator** | Software that approximates a device's behavior, without real hardware | A cardboard cutout of an animal |
| **Device list** | AWS's continuously refreshed catalog of available real device/OS combinations | The barn's breed catalog |
| **Device Pool** | A named, curated group of devices to run a test against | A herd |
| **Public device pool** | An AWS-curated pool built from real usage data (e.g., "Top devices") | A herd AWS assembled for you |
| **Custom device pool** | A pool you build yourself from the device list | A herd you handpicked |
| **Automated test run** | A scripted or built-in test executed across every device in a pool, in parallel | The ranch hand walking the whole herd through a course at once |
| **Server-side execution** | Device Farm runs your uploaded app and test package on its own service-managed test hosts | The farm's own ranch hand does the work |
| **Fuzz** | The one built-in test type (Android & iOS): sends pseudo-random touch events looking for crashes | Letting an animal wander the field, no script needed |
| **Appium** | An open-source, W3C WebDriver–based framework for driving native, hybrid, and web apps as a real user would | A leash any trained ranch hand can hold |
| **Instrumentation** | The Android test framework family (covers Espresso and UI Automator tests) | — |
| **XCTest / XCTest UI** | Apple's native iOS test frameworks | — |
| **Appium endpoint** | A managed, session-scoped Appium server URL exposed during a Remote Access session, for client-side testing | A private phone line straight to one animal's stall |
| **Client-side execution** | Running Appium tests from your own local environment against a device via the Appium endpoint | Walking one animal yourself, on your own schedule |
| **Remote Access** | An interactive, real-time manual session on one real device, controlled from your browser | Reaching into the pen and walking one animal on a leash |
| **Network profile** | A configurable simulated network condition (e.g., degraded 3G, offline) applied during a run | The weather machine's wind setting |
| **Device proxy** | An HTTP/S proxy optionally applied to a Remote Access session for its duration | A translator standing at the pen gate |
| **Video recording** | A full screen recording captured for every automated run and Remote Access session | The vet's checkup video |
| **Issue clustering** | Automatically grouping related failures across multiple devices into one reported issue | One farmhand report covering five sick animals |
| **Private Device Lab** | Devices reserved exclusively for one account, with state that can persist between sessions | A fenced-off private pasture |
| **Desktop Browser Testing** | A fully managed Selenium Grid for running WebDriver tests across desktop browsers | The stable of tireless robot horses |
| **Device-minute / Browser-minute** | The metered unit Device Farm bills against — total minutes tests actually executed | Paying only for the minutes an animal spent on the course |

Back to the [folder index](index.md) · [mnemonics cheat sheet](mnemonics-cheatsheet.md).
