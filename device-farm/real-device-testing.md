---
type: Concept
title: "Real Device Testing"
description: "The herd in the barn: thousands of real physical devices, grouped into pools, with the weather machine dialed in to simulate real-world conditions."
tags: [aws, device-farm, real-devices, device-pools]
sources:
  - id: aws-device-farm-devices
    resource: https://aws.amazon.com/device-farm/
    title: AWS Device Farm — device list and real device testing
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Real Device Testing — The Herd in the Barn

## 🐄 The mnemonic

Every device in the fleet is a real animal in the barn — an actual phone or tablet, with its actual manufacturer's firmware, its actual carrier modifications, its actual CPU and battery. Device Farm keeps adding new breeds to the barn as new device models ship, and you never have to buy, rack, or update a single one yourself.

## The device list

AWS publishes a continuously refreshed **device list**: a catalog of real device/OS/manufacturer combinations available in the fleet (Android and iOS, phones and tablets, a wide spread of manufacturers, OS versions, and form factors). You can filter it by:

- Manufacturer (Samsung, Google, Apple, and others)
- OS and OS version
- Form factor (phone vs. tablet)
- Screen resolution / density

Because it's a real, physical fleet, the exact roster changes over time — new devices are added as they hit the market, and old ones are retired. Always check the live [device list](https://docs.aws.amazon.com/devicefarm/latest/developerguide/test-types.html) for what's currently in the barn rather than assuming a fixed count.

## Device Pools — grouping the herd

You rarely test against the *entire* barn for every run — that's slow and expensive. Instead, you define a **Device Pool**: a named, curated subset of the fleet to test against.

| Pool type | What it is |
|---|---|
| **Public device pool** | AWS-curated pools (e.g., "Top devices," "Android top devices") built from real usage data |
| **Custom device pool** | A pool you build yourself — the exact models and OS versions your analytics say your customers actually use |
| **Private Device Lab devices** | Devices reserved exclusively for your account (see [Private Device Lab](private-device-lab.md)) |

**Mnemonic:** *"Don't test against every animal on the farm — test against the herd your customers actually resemble."*

## The weather machine — simulating real-world conditions

A real device in a data center still doesn't automatically replicate a customer standing on a subway platform. Device Farm lets you dial in conditions on top of the real hardware:

| Condition | What you can configure |
|---|---|
| **Network profile** | Simulate connection types and quality — e.g., degraded 3G, high latency, packet loss, or fully offline |
| **Location (GPS)** | Set a specific latitude/longitude so location-aware app logic can be exercised |
| **Language & locale** | Run the same test in different device languages to catch localization bugs |
| **Extra data / prerequisite apps** | Preinstall companion apps or seed the device with test data before your app under test even launches |

**Mnemonic:** *"The weather machine over the pen — same real animal, different real-world weather."*

## Why this beats an emulator every time

Real hardware means real behavior under real constraints — actual memory pressure, actual CPU throttling under sustained load, actual battery drain, actual manufacturer skins intercepting permissions dialogs differently than stock Android. These are exactly the class of bugs that only show up on physical hardware, and exactly the class of bugs that ship to production when teams test only on emulators.

## Next up

Now that you know what's in the barn, learn how to run something through it automatically: [Automated Testing Frameworks](automated-testing-frameworks.md).

[^aws-device-farm-devices]: AWS Device Farm, "Real device testing" and device list, aws.amazon.com/device-farm.
