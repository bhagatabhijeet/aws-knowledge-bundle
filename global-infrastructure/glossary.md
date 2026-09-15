---
type: Concept
title: "Global Infrastructure Glossary"
description: "Every Global Infrastructure term used in this folder, defined in one line, with its world-map analogy equivalent."
tags: [aws, global-infrastructure, glossary]
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Global Infrastructure Glossary

| Term | Definition | World-map analogy |
|---|---|---|
| **Region** | A fully independent, isolated geographic area running AWS services | A city |
| **Availability Zone (AZ)** | One or more discrete data centers within a Region, with independent power, cooling, and networking | A borough |
| **AZ Name** | The per-account label for an AZ, e.g. `us-east-1a` — not consistent across accounts | What one neighbor calls their borough |
| **AZ ID** | A stable identifier for a physical AZ, e.g. `use1-az1`, consistent across every account | The borough's real address |
| **Data center** | The physical building housing racks, cooling, and security for part of an AZ | A single building |
| **Opt-in Region** | A newer Region not enabled by default; must be explicitly turned on before use | A new district you must formally join |
| **AWS GovCloud (US)** | An isolated Region partition built for US government compliance requirements | A restricted district |
| **Edge Location** | A CloudFront point of presence that caches content close to end users | A corner store |
| **Regional Edge Cache** | A larger cache between Edge Locations and the origin, holding content longer | The regional warehouse |
| **Point of Presence (PoP)** | Collectively, Edge Locations and Regional Edge Caches — the entry points onto AWS's backbone | The on-ramp to the citywide network |
| **Amazon CloudFront** | AWS's content delivery network (CDN), serving content from the nearest Edge Location | The corner-store chain |
| **AWS Global Accelerator** | Routes non-cacheable, dynamic traffic onto the AWS backbone at the nearest edge | The express lane onto the highway |
| **Lambda@Edge / CloudFront Functions** | Run custom code at the edge, before a request reaches the origin | The corner store's own clerk |
| **Local Zone** | An extension of a parent Region, placed in a specific large metro area | A mall in the next suburb |
| **Wavelength Zone** | AWS compute embedded directly inside a telecom's 5G network | A kiosk inside the phone company's building |
| **AWS Outposts** | Real AWS hardware, shipped and run inside a customer's own data center | A shipping container of the city itself |
| **Multi-AZ** | A deployment spread across two or more Availability Zones in one Region | Building across several boroughs |
| **Multi-Region** | A deployment spread across two or more independent Regions | Building twin cities |
| **Pilot light (DR pattern)** | A minimal, idling copy of core data/systems in a second Region, scaled up only during failover | Keeping the gas on in an empty second city |
| **Warm standby (DR pattern)** | A scaled-down but fully functional copy running continuously in a second Region | Keeping the lights dim in a second city |
| **Active-active (DR pattern)** | A full-scale copy actively serving live traffic in more than one Region simultaneously | Both twin cities fully open, all the time |

Back to the [folder index](index.md) · [mnemonics cheat sheet](mnemonics-cheatsheet.md).
