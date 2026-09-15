---
type: Concept
title: "Regions"
description: "The cities on the map: fully independent geographic areas, and the four things that should actually decide which one you pick."
tags: [aws, global-infrastructure, regions, compliance, latency]
sources:
  - id: aws-regions-azs
    resource: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html
    title: AWS Documentation — Regions and Availability Zones
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Regions — The Cities on the Map

## 🏙️ The mnemonic

A **Region** is a **city**: a complete, self-contained metro area with its own power, its own government, its own everything. AWS Regions don't share infrastructure with each other by default — nothing about `eu-west-1` depends on `us-east-1` staying up, and nothing you build in one silently shows up in the other.

## Naming and structure

A Region's code names the geography and a sequence number, e.g. `us-east-1` (Northern Virginia), `eu-west-2` (London), `ap-southeast-1` (Singapore). Inside a Region, individual Availability Zones are suffixed with a letter — `us-east-1a`, `us-east-1b`, `us-east-1c` — covered in depth in [Availability Zones](availability-zones.md).

Some Regions are **opt-in Regions**: newer Regions that aren't enabled by default in your account and must be explicitly turned on before you can use them. This is a deliberate safeguard — it stops resources from silently appearing (and billing) in a Region you never intended to use.

There are also **isolated partitions** for specific compliance needs — for example, **AWS GovCloud (US)**, built to meet US government regulatory and compliance requirements, physically and logically isolated from standard commercial Regions.

## Regions are isolated on purpose

Nearly every AWS service is **Regional** — it runs, and stores its data, entirely within the Region you chose. A handful of services are **global** by design (IAM, Route 53, CloudFront) because they need one consistent view across the whole map — but the default, and the assumption you should make unless told otherwise, is that a Region is its own island.

**Mnemonic:** *"A city doesn't quietly mail your data to another city — if you want it there, you drive it there yourself (replicate it explicitly)."*

## The four things that should actually decide your Region

| Factor | Why it matters |
|---|---|
| **Latency to your users** | The closer the city, the faster the round trip — pick the Region nearest your actual user base |
| **Compliance & data residency** | Some laws require data to stay within a country's borders — a Region is the unit that guarantees that boundary |
| **Service availability** | Not every service launches in every Region on day one — check that the Region you want actually offers what you need |
| **Pricing** | The same service can cost a different amount in different Regions |

**Mnemonic:** *"Pick the city your customer already lives closest to — then check it has the shops (services) you need, and that it's legal for them to shop there."*

## Multi-Region, when you need it

Running in a single Region is the default and the simplest choice. You reach for a **second** Region when:

- Your users are genuinely global, and one city is too far from half of them
- Regulation requires data to be replicated to, or kept out of, a specific country
- You need disaster recovery from a Region-scale event (see [Designing for High Availability](designing-for-high-availability.md))

Adding a Region is a real architectural decision, not a checkbox — data replication, latency between Regions, and doubled operational surface area all come with it.

## Next up

Zoom into one city and look at its boroughs: [Availability Zones](availability-zones.md).

[^aws-regions-azs]: AWS Documentation, "Regions and Availability Zones."
