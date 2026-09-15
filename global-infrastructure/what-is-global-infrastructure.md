---
type: Concept
title: "What is AWS Global Infrastructure?"
description: "The whole world map, zoomed out: cities (Regions), boroughs (Availability Zones), and corner stores (Edge Locations), and why AWS built it as a hierarchy instead of one giant data center."
tags: [aws, global-infrastructure, fundamentals, regions, availability-zones]
sources:
  - id: aws-global-infra
    resource: https://aws.amazon.com/about-aws/global-infrastructure/
    title: AWS Global Infrastructure
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# What is AWS Global Infrastructure?

**AWS Global Infrastructure** is the physical layer underneath every AWS service: real buildings, real fiber, real power substations, spread across the planet in a deliberate hierarchy. Every other AWS concept — an EC2 instance, an S3 bucket, an RDS database — ultimately lives somewhere on this map, and *where* it lives determines its latency, its blast radius, and, sometimes, whether it's even legal to put your data there.

## 🌍 The mnemonic: a world map of cities, boroughs, and corner stores

Picture AWS as a company that doesn't build one giant data center — it builds an entire **world map**:

- A **city** is a **Region** — a fully independent metro area with its own government, power, and services.
- A **borough** inside that city is an **Availability Zone** — its own substation, its own water main, its own fire department, but connected to its sibling boroughs by dedicated, private tunnels.
- A **corner store** near every neighborhood on Earth is an **Edge Location** — stocking the popular stuff close to the customer, so nobody has to drive downtown for it.

![The AWS World Map](assets/images/global-infrastructure-overview.svg)

## The hierarchy, top to bottom

| Layer | What it is | Roughly how many exist |
|---|---|---|
| **Region** | A fully independent, isolated geographic area | Dozens worldwide, and growing |
| **Availability Zone (AZ)** | One or more discrete data centers within a Region, with independent power/cooling/networking | Every Region has multiple — most have three or more |
| **Data center** | The actual physical building — racks, cooling, security | Multiple per AZ |
| **Edge Location** | A CloudFront cache point, far more numerous and widely spread than Regions | Hundreds worldwide |
| **Local Zone / Wavelength Zone** | A small satellite extension of a Region, placed even closer to a specific metro area or telecom network | A growing set, in major metros and inside carrier networks |

**Mnemonic:** *"A Region is a city; an AZ is a borough; a data center is a building — and a corner store (Edge Location) doesn't need any of that to serve you fast."*

## Why build it this way, instead of one giant data center?

| Problem | The World Map's answer |
|---|---|
| One power outage takes down everything | Split into AZs — independent power/cooling/networking, so one AZ's failure doesn't touch its siblings |
| One region's disaster (earthquake, flood, war) takes down everything | Split into Regions — physically distant, fully isolated, so a Region-level disaster stays contained |
| A user on the other side of the planet gets slow load times | Edge Locations cache content physically close to that user, regardless of which Region hosts the origin |
| Some countries legally require data to stay within their borders | Choose the Region whose country matches your compliance requirement — a Region never silently replicates data elsewhere |

## What you'll actually decide, using this map

- **Which Region** to run your application in (see [Regions](regions.md)) — driven by latency to your users, compliance/data-residency law, and which services you need
- **How many Availability Zones** to spread across within that Region (see [Availability Zones](availability-zones.md)) — driven by how much downtime you can tolerate
- **Whether to add Edge caching** in front of your Region (see [Edge Locations & CloudFront](edge-locations-and-cloudfront.md)) — driven by how globally distributed your users are
- **Whether a Local Zone or Wavelength Zone** is worth the extra complexity (see [Local Zones & Wavelength Zones](local-zones-and-wavelength-zones.md)) — driven by whether a specific workload needs latency lower than a full Region can deliver

## Next up

Start at the top of the hierarchy: [Regions](regions.md) — the cities on the map, and how to pick the right one.

[^aws-global-infra]: AWS Global Infrastructure, aws.amazon.com/about-aws/global-infrastructure.
