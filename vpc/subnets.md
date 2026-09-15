---
type: Concept
title: "Subnets"
description: "The streets inside the community — each one built entirely inside a single borough, and public or private only because of where its street signs point."
tags: [aws, vpc, subnets, availability-zones]
sources:
  - id: aws-vpc-subnets
    resource: https://docs.aws.amazon.com/vpc/latest/userguide/configure-subnets.html
    title: Amazon VPC User Guide — VPCs and subnets
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Subnets — The Streets Inside the Community

## 🏠 The mnemonic

A **subnet** is a street: a slice of the VPC's address range, laid out entirely inside **one borough** — one [Availability Zone](../global-infrastructure/availability-zones.md) — and never spanning two. If you want your community present in three boroughs, you build (at least) three streets, one per borough.

## A subnet can't span Availability Zones

This is the single most important structural fact about subnets: **a subnet is always confined to exactly one AZ.** To spread a workload across multiple AZs for high availability, you don't stretch one subnet across them — you create a **separate subnet in each AZ** and place resources across all of them.

**Mnemonic:** *"A street doesn't cross borough lines — build a street in every borough you want the community present in."*

## Public subnet vs. private subnet: it's a routing decision, not a label

Nothing about a subnet's configuration inherently marks it "public" or "private." The distinction comes entirely from its **route table**:

| Subnet type | What its route table points to, for internet-bound traffic | Reachable directly from the internet? |
|---|---|---|
| **Public subnet** | An [Internet Gateway](internet-gateway.md) | Yes — resources with a public IP can be reached directly |
| **Private subnet** | Nothing (fully isolated), or a [NAT Gateway](nat-gateways.md) for outbound-only access | No — never reachable by an inbound connection from the internet |

**Mnemonic:** *"A street isn't public because of a sign on the lawn — it's public because its street sign literally points at the front gate."* See [Route Tables](route-tables.md) for exactly how that pointing works.

## A typical subnet layout

Most real VPCs carve out at least two tiers of subnets, repeated once per Availability Zone:

```
VPC: 10.0.0.0/16
├── Public subnet   — us-east-1a  (10.0.0.0/24)   → route to Internet Gateway
├── Public subnet   — us-east-1b  (10.0.1.0/24)   → route to Internet Gateway
├── Private subnet  — us-east-1a  (10.0.10.0/24)  → route to NAT Gateway (same AZ)
└── Private subnet  — us-east-1b  (10.0.11.0/24)  → route to NAT Gateway (same AZ)
```

Public subnets typically hold load balancers and NAT Gateways — anything that genuinely needs to be reachable from, or reach out cheaply to, the internet. Private subnets hold everything else: application servers, databases — anything that should never accept an inbound connection from the public internet.

**Mnemonic:** *"Put the mailbox and the delivery kiosk on the public street. Keep the house itself on the private one."*

## Sizing a subnet

Like the VPC itself, a subnet is defined by its own CIDR block, carved out of the VPC's range. Remember: AWS reserves **5 addresses in every subnet** (network address, VPC router, DNS, a future-use address, and the broadcast address) — a `/24` subnet (256 addresses) actually gives you 251 usable addresses, not 256.

**Mnemonic:** *"Every street reserves a few house numbers for the utility company before the first resident ever moves in."*

## Next up

Learn exactly how a subnet's street signs decide public vs. private: [Route Tables](route-tables.md).

[^aws-vpc-subnets]: Amazon VPC User Guide, "VPCs and subnets."
