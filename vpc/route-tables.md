---
type: Concept
title: "Route Tables"
description: "The street signs at every intersection: a set of rules that decide where a subnet's traffic actually goes — and the single fact that makes a subnet public or private."
tags: [aws, vpc, route-tables, routing]
sources:
  - id: aws-vpc-route-tables
    resource: https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html
    title: Amazon VPC User Guide — Route tables
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Route Tables — The Street Signs at Every Intersection

## 🚦 The mnemonic

A **route table** is the set of street signs posted at every intersection on a street: for any destination address, the sign tells traffic exactly which direction to go. Every subnet is associated with exactly one route table, and that table's rules determine everything about how traffic leaving that subnet gets where it's going.

## What's actually in a route table

Each rule — a **route** — pairs a **destination** (a CIDR range) with a **target** (where matching traffic should be sent):

| Destination | Target | What it means |
|---|---|---|
| `10.0.0.0/16` | `local` | Traffic to anything inside the VPC stays inside the VPC — this route exists automatically and can't be removed |
| `0.0.0.0/0` | An Internet Gateway | "Everything else" goes out to the internet — this is what makes a subnet **public** |
| `0.0.0.0/0` | A NAT Gateway | "Everything else" goes out through the NAT Gateway — outbound-only, from a **private** subnet |
| A peered VPC's CIDR | A peering connection | Traffic bound for a peered VPC crosses the private footbridge (see [VPC Peering & Endpoints](vpc-peering-and-endpoints.md)) |
| An AWS service's prefix list | A VPC endpoint | Traffic bound for that service goes through a private tunnel instead of the public internet |

**Mnemonic:** *"Every street sign has exactly one rule that can't be erased: home addresses always stay local."* — the `local` route to the VPC's own CIDR is always present and always wins for in-VPC traffic.

## The main route table vs. custom route tables

Every VPC gets one **main route table** automatically. Any subnet you don't explicitly associate with a different route table uses the main one by default. In practice, most real VPCs create **custom route tables** — one for public subnets, one (or more) for private subnets — and explicitly associate each subnet with the correct one, rather than leaving everything on the main table.

**Mnemonic:** *"The main route table is the city's default signage — fine to start with, but a real community posts its own signs street by street."*

## Most specific route wins

When more than one route in a table could match a destination, AWS picks the **most specific** one (the longest prefix match) — not the first one listed, and not the newest one added.

**Mnemonic:** *"The street sign with the most precise address always wins the argument, no matter which sign went up first."*

## Route table associations decide public vs. private — nothing else does

There's no special "make this subnet private" setting. A subnet is public purely because its route table has a `0.0.0.0/0` route pointed at an Internet Gateway; it's private purely because that route is missing, or points at a NAT Gateway instead. Change the association, or change the route, and you've changed the subnet's whole nature.

## Next up

See exactly what sits at the other end of a public subnet's route: [Internet Gateway](internet-gateway.md).

[^aws-vpc-route-tables]: Amazon VPC User Guide, "Route tables."
