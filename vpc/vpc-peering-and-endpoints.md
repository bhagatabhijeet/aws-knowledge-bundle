---
type: Concept
title: "VPC Peering & Endpoints"
description: "A private footbridge to a neighboring community, and a private tunnel straight to an AWS service's own building — neither one ever touches the public internet."
tags: [aws, vpc, peering, vpc-endpoints, privatelink]
sources:
  - id: aws-vpc-peering
    resource: https://docs.aws.amazon.com/vpc/latest/peering/what-is-vpc-peering.html
    title: Amazon VPC Peering Guide — What is VPC peering?
  - id: aws-vpc-endpoints
    resource: https://docs.aws.amazon.com/vpc/latest/privatelink/vpce-gateway.html
    title: Amazon VPC User Guide — Gateway endpoints
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# VPC Peering & Endpoints — Footbridges and Private Tunnels

## VPC Peering — a footbridge to a neighboring community

A **VPC Peering connection** is a private, direct network link between two VPCs — built by request and accepted by both sides — that lets resources in each VPC talk to each other using private IP addresses, as if they were on the same network. Traffic never touches the public internet, and never leaves AWS's own network.

**Mnemonic:** *"A private footbridge, built by mutual agreement, straight between two otherwise-separate gated communities."*

Two structural facts matter more than any other:

| Fact | What it means in practice |
|---|---|
| **Non-transitive** | If community A peers with B, and B peers with C, **A cannot reach C** through B. Each peering connection only connects the two VPCs directly attached to it. |
| **No overlapping CIDR blocks** | Two VPCs with overlapping address ranges cannot be peered — there'd be no way to tell which "10.0.0.5" you meant. |

**Mnemonic:** *"A footbridge only connects the two communities it was built between — it's not a highway interchange."* If you need more than a couple of VPCs to reach each other, a full mesh of peering connections gets unwieldy fast — that's exactly the problem [Transit Gateway](connecting-to-on-premises.md) exists to solve.

Peering also requires **route table updates on both sides** — each VPC's route table needs an explicit route pointing the other VPC's CIDR at the peering connection; nothing is automatic.

## VPC Endpoints — a private tunnel straight to an AWS service

A **VPC Endpoint** lets resources inside your VPC talk to an AWS service **without ever routing through an Internet Gateway or NAT Gateway** — a private tunnel straight from a house to a specific city service building, bypassing the public road entirely.

**Mnemonic:** *"Don't send a resident out the front gate and down the public highway just to visit a building that's technically part of the same city."*

There are two kinds, and the distinction matters for both cost and design:

| | Gateway Endpoint | Interface Endpoint (AWS PrivateLink) |
|---|---|---|
| Supports which services | **Only Amazon S3 and DynamoDB** | Most other AWS services, and many third-party/partner services |
| How it attaches | A **target in a route table** — no network interface involved | An **Elastic Network Interface (ENI)** with a private IP, placed directly in your subnet |
| Cost | No additional charge | Hourly charge, plus a per-GB data processing charge |
| Relevance to NAT Gateways | Removes S3/DynamoDB traffic from the NAT Gateway entirely — see [NAT Gateways](nat-gateways.md) | Removes that service's traffic from the NAT Gateway path too, at its own cost |

**Mnemonic:** *"A Gateway Endpoint is a free extra street sign pointing straight at the warehouse. An Interface Endpoint is a small private tunnel entrance built right on your own street, for services that need more than a sign."*

## Why this is the single biggest NAT Gateway cost-saver

Traffic to S3 or DynamoDB routed through a NAT Gateway pays that gateway's per-GB data processing charge for data that never actually needed to leave AWS's own network. Adding a Gateway Endpoint for S3/DynamoDB removes that traffic from the NAT Gateway's path entirely — free, and it stops competing for that NAT Gateway's bandwidth too. This is usually the first thing worth checking when a NAT Gateway's data-processing bill looks larger than expected.

## Next up

For connections that need to reach further than another VPC — all the way back to your own on-premises network: [Connecting to On-Premises](connecting-to-on-premises.md).

[^aws-vpc-peering]: Amazon VPC Peering Guide, "What is VPC peering?"
[^aws-vpc-endpoints]: Amazon VPC User Guide, "Gateway endpoints."
