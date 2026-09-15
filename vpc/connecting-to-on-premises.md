---
type: Concept
title: "Connecting to On-Premises"
description: "An encrypted courier route over the public highway (VPN), a dedicated private road that never touches it (Direct Connect), and a central roundabout tying many communities together (Transit Gateway)."
tags: [aws, vpc, vpn, direct-connect, transit-gateway, hybrid]
sources:
  - id: aws-vpn
    resource: https://docs.aws.amazon.com/vpn/latest/s2svpn/VPC_VPN.html
    title: AWS Site-to-Site VPN User Guide
  - id: aws-direct-connect
    resource: https://aws.amazon.com/directconnect/
    title: AWS Direct Connect
  - id: aws-transit-gateway
    resource: https://aws.amazon.com/transit-gateway/
    title: AWS Transit Gateway
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Connecting to On-Premises — Courier Routes, Private Roads, and a Central Roundabout

## 🛰️ Site-to-Site VPN — an encrypted courier route over the public highway

An **AWS Site-to-Site VPN** connects your on-premises network to a VPC over the **public internet**, using an encrypted (IPsec) tunnel — a **Virtual Private Gateway** (or Transit Gateway) on the AWS side, and a **Customer Gateway** representing your on-premises router on the other.

**Mnemonic:** *"A courier still drives the public highway — but everything in the truck is locked in a safe the whole way."*

- **Fast to set up** — no physical infrastructure to install, since it rides over the existing internet
- **Variable performance** — because it still traverses the public internet, latency and throughput aren't guaranteed the way a dedicated line's would be
- **Good for** — quick hybrid connectivity, backup paths for a primary Direct Connect link, or workloads that don't need guaranteed bandwidth

## AWS Direct Connect — a dedicated private road that never touches the highway

**AWS Direct Connect** is a dedicated, physical network connection from your premises (or a colocation facility) straight into AWS — bypassing the public internet entirely.

**Mnemonic:** *"Direct Connect doesn't send the courier down the public highway at all — it pays to pave a private road straight from headquarters to the community gate."*

- **Consistent, predictable performance** — since it never shares the public internet's congestion
- **Slower to provision** — physical circuits take real time to install
- **Good for** — large, steady data transfers, workloads sensitive to latency/jitter, or any case where "reliable, dedicated bandwidth" matters more than "fast to set up"

VPN and Direct Connect aren't mutually exclusive: a common pattern pairs a Direct Connect link as the primary path with a Site-to-Site VPN as an automatic, encrypted failover if the dedicated line goes down.

## Transit Gateway — the central roundabout

Peering every VPC directly with every other VPC (see [VPC Peering & Endpoints](vpc-peering-and-endpoints.md)) turns into an unmanageable mesh once you have more than a handful of VPCs — and peering is non-transitive, so there's no shortcut through a middle VPC. **AWS Transit Gateway** solves this by acting as a single, central **regional hub**: every VPC, VPN connection, and Direct Connect link attaches to the Transit Gateway once, and the Transit Gateway routes between all of them.

**Mnemonic:** *"Instead of building a footbridge between every pair of communities, build one central roundabout and give every community a single ramp onto it."*

| | Full-mesh VPC Peering | Transit Gateway |
|---|---|---|
| Connections needed for *n* VPCs | Grows roughly with *n²* | Just *n* — one attachment per VPC |
| Transitive routing | No — never through a middle VPC | Yes — the hub routes between any attached network |
| On-premises connectivity | Not directly — peering is VPC-to-VPC only | Yes — VPN and Direct Connect can attach directly to the same hub |

## Putting it together

A typical hybrid architecture: on-premises data centers reach AWS over Direct Connect (with VPN as failover), both terminating at a Transit Gateway, which also connects every VPC in the Region — one hub, one consistent set of routing rules, instead of a tangle of individual point-to-point links.

## Next up

With the whole community mapped out, here's how to actually lay one out well: [Best Practices](best-practices.md).

[^aws-vpn]: AWS Site-to-Site VPN User Guide.
[^aws-direct-connect]: AWS Direct Connect, aws.amazon.com/directconnect.
[^aws-transit-gateway]: AWS Transit Gateway, aws.amazon.com/transit-gateway.
