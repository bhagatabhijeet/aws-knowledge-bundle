---
type: Concept
title: "Local Zones & Wavelength Zones"
description: "Small satellite outposts built even closer than a Region can reach — one for a specific metro area, one built right inside a telecom's own 5G network."
tags: [aws, global-infrastructure, local-zones, wavelength, outposts]
sources:
  - id: aws-local-zones
    resource: https://aws.amazon.com/about-aws/global-infrastructure/localzones/
    title: AWS Local Zones
  - id: aws-wavelength
    resource: https://aws.amazon.com/wavelength/
    title: AWS Wavelength
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Local Zones & Wavelength Zones — Satellite Outposts

## 🏬 The mnemonic

Sometimes a corner store still isn't close enough, and building a whole new city (Region) is overkill for one customer's needs. So AWS builds a **satellite outpost** instead — a small, single-purpose extension, placed exactly where it's needed, without the overhead of standing up an entire independent city.

## Local Zones — a mall in the next suburb over

An **AWS Local Zone** is an extension of a parent Region, placed physically close to a large population or industry center that doesn't have (and doesn't need) a full Region of its own. It runs a subset of AWS services — enough for latency-sensitive workloads — while everything else your application needs still comes from the parent Region it's attached to.

- **What it's for:** media & entertainment production, real-time gaming, live video processing, and other workloads where shaving single-digit milliseconds off the round trip to a specific metro area genuinely matters
- **How it connects:** high-bandwidth, low-latency private networking back to its parent Region, so a Local Zone is never really "on its own" — it's borrowing the parent Region's control plane and remaining services

**Mnemonic:** *"A Local Zone is a mall built in the suburbs, still owned and stocked by the same city downtown — closer to the customer, without becoming its own city."*

## Wavelength Zones — a kiosk inside the phone company's own building

An **AWS Wavelength Zone** goes even further: AWS compute and storage embedded **directly inside a telecommunications provider's own 5G network**. Traffic from a mobile device never has to leave the telecom's network and cross onto the public internet to reach your application — it's served from inside the carrier's own infrastructure.

- **What it's for:** workloads where mobile-network latency itself is the bottleneck — augmented/virtual reality, connected vehicles, real-time industrial IoT, and other use cases where every extra network hop is felt directly by the end user
- **How it connects:** back to a parent Region over AWS's network, the same way a Local Zone does — Wavelength just moves the compute one step further, into the carrier's network itself

**Mnemonic:** *"A Wavelength Zone doesn't wait for the customer to leave the phone company's building — it sets up a kiosk right there."*

## And the most extreme option: AWS Outposts

If even a Wavelength Zone isn't close enough — because the requirement is "inside my own building, on my own premises" — **AWS Outposts** ships you the actual hardware: a rack of real AWS infrastructure, installed in your own data center, running the same APIs and services as the public cloud, managed by AWS but physically sitting on your floor.

**Mnemonic:** *"Outposts doesn't build a satellite outpost near you — it ships you a shipping container of the city itself."*

## Comparing the three

| | Local Zone | Wavelength Zone | Outposts |
|---|---|---|---|
| Physically located | A major metro area | Inside a telecom's 5G network | Your own premises |
| Connects back to | A parent Region | A parent Region | A parent Region |
| Typical use case | Media, gaming, latency-sensitive metro workloads | Mobile/5G edge computing (AR/VR, connected vehicles) | On-premises requirements, data residency down to the building |
| Services available | A curated subset | A curated subset | A curated subset, run on your hardware |

## Next up

None of these satellite outposts change the core lesson: spreading a workload across independent failure domains — AZs, Regions, or both — is what actually protects you. See [Designing for High Availability](designing-for-high-availability.md).

[^aws-local-zones]: AWS Local Zones, aws.amazon.com/about-aws/global-infrastructure/localzones.
[^aws-wavelength]: AWS Wavelength, aws.amazon.com/wavelength.
