---
type: Concept
title: "Best Practices"
description: "How a well-run farm operates: choosing the right herd, when to script vs. wander, and when a private pasture pays for itself."
tags: [aws, device-farm, best-practices]
sources:
  - id: aws-device-farm-guide-practices
    resource: https://docs.aws.amazon.com/devicefarm/latest/developerguide/welcome.html
    title: AWS Device Farm Developer Guide
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Best Practices — Running the Farm Well

1. **Pick a herd that resembles your actual customers.** Build custom [Device Pools](real-device-testing.md) from your own analytics — top manufacturers and OS versions your users actually run — instead of defaulting to "every device in the barn" for every single run.

2. **Run Fuzz before you write a single script.** It costs nothing to set up and catches outright crashes immediately on a brand-new build, before you invest in Appium/Instrumentation/XCTest coverage (see [Automated Testing Frameworks](automated-testing-frameworks.md)).

3. **Size the pool to the moment.** A small, fast pool on every commit; the full fleet reserved for release candidates. You pay per device-minute — a bigger herd on every push is paid-for time you may not need.

4. **Reach for Remote Access before filing a device-specific bug.** If an automated run fails on exactly one device, a two-minute Remote Access session often tells you whether it's a real bug or an environment quirk — before it becomes a ticket.

5. **Use the client-side Appium endpoint for iteration, server-side execution for volume.** Developing and debugging a new test script against one real device is faster through a Remote Access session's Appium endpoint (see [Remote Access](remote-access.md)); once the script is solid, run it server-side across the whole pool.

6. **Reserve a Private Device Lab only when contention or state actually hurts you.** If you're queueing behind other tenants, or your workflow genuinely depends on state surviving between sessions, a [Private Device Lab](private-device-lab.md) pays for itself — otherwise the shared fleet is simpler and cheaper.

7. **Read the video first, the logs second.** A recording tells you in seconds whether a failure is a real functional bug, a timing/flake issue, or an environment problem — cheaper than reading a stack trace cold (see [Test Results & Debugging](test-results-and-debugging.md)).

8. **Let issue clustering do your triage.** When a run fails across many devices at once, fix the one root cause the clustering surfaces before chasing what looks like several unrelated bugs.

9. **Gate your pipeline on both fleets.** A web app deserves both a mobile device-pool run and a [Desktop Browser Testing](desktop-browser-testing.md) run in the same CI/CD gate — a bug that only shows up in Internet Explorer won't show up on a phone.

10. **Pin your app build and test package versions per run.** Reproducible test results depend on knowing exactly which build ran against which test package — treat both as versioned artifacts, not "whatever's currently in the S3 bucket."

## Next up

Everything above compressed onto one page: [Mnemonics Cheat Sheet](mnemonics-cheatsheet.md).
