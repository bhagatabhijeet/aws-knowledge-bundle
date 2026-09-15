---
type: Concept
title: "Global Infrastructure Mnemonics Cheat Sheet"
description: "Every Global Infrastructure mnemonic in this folder, on one page — read this the night before an exam or interview."
tags: [aws, global-infrastructure, mnemonics, cheatsheet]
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# The One-Page Global Infrastructure Cheat Sheet

## The world map, in one picture

| World-map piece | AWS concept |
|---|---|
| 🌍 The world map itself | AWS Global Infrastructure |
| 🏙️ A city — fully independent, its own everything | A Region, e.g. `us-east-1` |
| 🏘️ A borough — own power/water/fire dept, linked by private tunnels | An Availability Zone (AZ) |
| 🏢 A single building inside a borough | A data center |
| 🏪 A corner store near every neighborhood | An Edge Location (CloudFront) |
| 🏭 The regional warehouse behind the corner stores | A Regional Edge Cache |
| 🛣️ The on-ramp to the citywide backbone | A Point of Presence (PoP) |
| 🏬 A mall built in the next suburb over | A Local Zone |
| 📡 A kiosk inside the phone company's building | A Wavelength Zone |
| 📦 A shipping container of the city itself | AWS Outposts |
| 👯 Twin cities built continents apart | Multi-Region architecture |

## The mnemonics worth memorizing word-for-word

1. **"A Region is a city; an AZ is a borough; a data center is a building."** — the core hierarchy.
2. **"A city doesn't quietly mail your data to another city."** — Regions are isolated; cross-Region replication is always something you set up on purpose.
3. **"Far enough apart that one borough's blackout never reaches the next. Close enough together that they still finish each other's sentences."** — AZs: physically separated, but sub-millisecond-linked.
4. **"Two neighbors calling their borough 'downtown' doesn't mean they live in the same borough."** — AZ *names* aren't consistent across accounts; compare AZ *IDs* instead.
5. **"The corner store doesn't stock everything — just what your neighborhood asks for most."** — Edge Locations cache the popular content, close to the user.
6. **"The corner store checks the regional warehouse before it ever calls the factory."** — Regional Edge Caches sit between Edge Locations and your origin.
7. **"A Local Zone is a mall built in the suburbs, still owned by the same city downtown."** — a Local Zone extends, but doesn't replace, its parent Region.
8. **"A Wavelength Zone sets up a kiosk right inside the phone company's building."** — AWS compute embedded in a telecom's 5G network.
9. **"Multi-AZ is the cheapest insurance policy in AWS."** — near-zero latency cost, removes a whole class of failure.
10. **"A fault-isolation boundary you didn't actually build into every layer isn't a boundary — it's a hope."** — audit every layer (NAT, DB endpoints, caches) before trusting a Multi-AZ/Region design.

## The hierarchy, top to bottom

```
AWS Global Infrastructure
└── Region                       (a city — fully independent)
    └── Availability Zone (AZ)   (a borough — independent power/cooling/networking)
        └── Data center          (a building)

Edge Locations / Regional Edge Caches / PoPs   — separate network, no Region required
Local Zones / Wavelength Zones / Outposts      — satellite extensions of a parent Region
```

## Speed-round definitions

| Term | One line |
|---|---|
| Region | A fully independent, isolated geographic area, e.g. `us-east-1` |
| Availability Zone (AZ) | One or more discrete data centers with independent power/cooling/networking |
| AZ ID | A stable identifier for a physical AZ, consistent across accounts (unlike the AZ name) |
| Edge Location | A CloudFront cache point close to end users |
| Regional Edge Cache | A larger cache between Edge Locations and the origin |
| Point of Presence (PoP) | Collectively, Edge Locations and Regional Edge Caches |
| Local Zone | An extension of a Region placed in a specific metro area |
| Wavelength Zone | AWS compute embedded inside a telecom's 5G network |
| AWS Outposts | Real AWS hardware, installed and run in your own data center |
| Multi-AZ | Spreading a workload across two or more AZs in the same Region |
| Multi-Region | Spreading a workload across two or more independent Regions |

For full definitions of every term, see the [Glossary](glossary.md). To go deeper on any single row, jump back to the [folder index](index.md).
