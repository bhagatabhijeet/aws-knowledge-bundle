---
type: Concept
title: "Integrations & Workflow"
description: "The farm's control room, wired into the rest of your equipment: IDEs, CI/CD pipelines, the CLI, and the API."
tags: [aws, device-farm, integrations, ci-cd]
sources:
  - id: aws-device-farm-integrations
    resource: https://aws.amazon.com/device-farm/
    title: AWS Device Farm — Integrate with your development workflow
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Integrations & Workflow — The Control Room

## 📡 The mnemonic

A farm this size isn't useful if you have to walk out to the barn every time you want to check on the herd. Device Farm wires straight into the control room you already sit in — your IDE, your CI server, your pipeline — so runs kick off and results come back without anyone leaving their desk.

## Where Device Farm plugs in

- **IDEs** — plugins for environments like Android Studio let you kick off a device run without leaving your editor
- **CI servers** — integrations for tools like Jenkins let a build trigger a device run as a pipeline step
- **AWS CLI** — script uploads, run creation, and result polling from the command line
- **Device Farm API/SDK** — build any custom automation on top of the same operations the console and CLI use

**Mnemonic:** *"The control room's dashboard doesn't care whether the signal came from your editor, your build server, or a raw API call — it all drives the same barn."*

## A typical pipeline run, end to end

1. Your CI pipeline builds the app (and, for automated tests, the test package).
2. A pipeline step uploads both artifacts to Device Farm — server-side execution stores them in a service-managed S3 bucket (see [Automated Testing Frameworks](automated-testing-frameworks.md)).
3. The step specifies the device pool and run configuration to use.
4. Device Farm runs the suite in parallel across the pool and returns pass/fail plus the full set of debugging artifacts.
5. The pipeline gates on the result — a failing run blocks the deploy, a passing run lets it through.

## Paying for it

Device Farm's real-device and browser-grid testing are metered **pay-as-you-go**, based on the device-minutes or browser-minutes your tests actually consume — you aren't paying for idle capacity between runs. A [Private Device Lab](private-device-lab.md) is a different arrangement: devices reserved exclusively for you, rather than metered per minute against a shared pool.

## Next up

With the mechanics covered, here's how to actually run a farm well: [Best Practices](best-practices.md).

[^aws-device-farm-integrations]: AWS Device Farm, "Integrate with your development workflow," aws.amazon.com/device-farm.
