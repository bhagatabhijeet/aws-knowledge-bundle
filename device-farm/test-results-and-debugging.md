---
type: Concept
title: "Test Results & Debugging"
description: "The vet's checkup report: videos, logs, and performance data from every run, plus automatic grouping of related failures."
tags: [aws, device-farm, debugging, logs, performance]
sources:
  - id: aws-device-farm-results
    resource: https://aws.amazon.com/device-farm/
    title: AWS Device Farm — Reproduce and fix issues faster
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Test Results & Debugging — The Vet's Checkup Report

## 📹🩺 The mnemonic

Every animal that runs the course comes back with a checkup: a video of the whole run, the vet's notes (logs), and vital signs (performance data). You don't have to guess what happened — Device Farm hands you the evidence for every device, on every run, automatically.

## What Device Farm captures for you

| Artifact | What it tells you |
|---|---|
| **Video recording** | A full screen recording of the device throughout the run — watch exactly what the user (or your script) saw |
| **Device logs** | Platform-level logs from the device itself during the run |
| **Test framework logs** | Output from Appium/Instrumentation/XCTest/Fuzz — assertions, steps, stack traces on failure |
| **Performance data** | CPU, memory, and other resource metrics captured while the test ran |
| **Screenshots** | Captured at test steps, for automated runs, so you can scan a run visually without watching the whole video |

For [Desktop Browser Testing](desktop-browser-testing.md), the equivalent artifacts are video, **browser console logs**, **action logs**, and **WebDriver logs**.

## Automatic issue clustering

Run the same automated test across twenty devices and you don't want to manually read twenty failure reports to notice they're all the same root cause. Device Farm **groups related failures together**, so you see "this one issue affected these five devices" instead of five separate, disconnected reports.

**Mnemonic:** *"The farmhand who notices five sick animals share one symptom doesn't file five separate reports — they file one, with five names attached."* This lets you triage by root cause first, then decide which device-specific quirks (if any) still need individual attention.

## Debugging workflow, end to end

1. A run finishes (automated, across a pool) or a Remote Access session ends.
2. Pull the video first for a fast, human read of what happened.
3. Cross-reference the timestamp of a failure against the device/framework logs for the actual error or stack trace.
4. Check performance data if the symptom is a slowdown, crash under load, or memory issue rather than a functional bug.
5. For automated runs across a pool, let issue clustering tell you whether you're looking at one root cause or several.

## Next up

If you find yourself needing the exact same handful of devices, in the exact same state, run after run, you want your own dedicated pasture: [Private Device Lab](private-device-lab.md).

[^aws-device-farm-results]: AWS Device Farm, "Reproduce and fix issues faster," aws.amazon.com/device-farm.
