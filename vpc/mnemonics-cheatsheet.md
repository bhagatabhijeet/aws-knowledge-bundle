---
type: Concept
title: "VPC Mnemonics Cheat Sheet"
description: "Every VPC mnemonic in this folder, on one page — read this the night before an exam or interview."
tags: [aws, vpc, mnemonics, cheatsheet]
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# The One-Page VPC Cheat Sheet

## The gated community, in one picture

| Gated-community piece | VPC concept |
|---|---|
| 🏘️ The gated community itself | A VPC — an isolated network, scoped to one Region |
| 🔢 The community's total address range | The VPC's CIDR block, e.g. `10.0.0.0/16` |
| 🏠 A single street, built entirely inside one borough | A Subnet — tied to exactly one AZ |
| 🚦 The street signs at every intersection | A Route Table |
| 🚪 The one public front gate | An Internet Gateway (IGW) — two-way |
| 📫 The mail-forwarding kiosk just inside the gate | A NAT Gateway — outbound-only |
| 🔒 The lock on one house's front door | A Security Group — stateful, allow-only, per-instance |
| 🚧 The guard booth at the entrance to a street | A Network ACL — stateless, allow+deny, per-subnet |
| 🌉 A private footbridge to a neighboring community | VPC Peering — direct, non-transitive |
| 🚇 A private tunnel straight to a city service building | A VPC Endpoint (Gateway or Interface/PrivateLink) |
| 🏗️ A central roundabout connecting many communities | A Transit Gateway |
| 🛰️ An encrypted courier route over the public highway | Site-to-Site VPN |
| 🔌 A dedicated private road, never touching the highway | AWS Direct Connect |
| 🎫 A reserved address plaque that never changes | An Elastic IP |

## The mnemonics worth memorizing word-for-word

1. **"A street doesn't cross borough lines."** — a subnet is always confined to exactly one Availability Zone.
2. **"A subnet isn't public because of a sign on the lawn — it's public because its street sign literally points at the front gate."** — public vs. private is decided entirely by the route table.
3. **"The front gate lets visitors knock. The mail-forwarding kiosk only lets residents send mail out."** — Internet Gateway is two-way; NAT Gateway is outbound-only.
4. **"The kiosk has to stand on the public street to reach the gate."** — a NAT Gateway lives in a public subnet, even though it serves private ones.
5. **"Give every borough its own kiosk."** — one NAT Gateway per AZ, to avoid cross-AZ charges and a hidden single point of failure.
6. **"Don't pay the kiosk to hand-carry a package to a building already inside the gate."** — route S3/DynamoDB traffic through a Gateway Endpoint, not a NAT Gateway.
7. **"The lock remembers who it let in. The guard booth has no memory at all."** — Security Groups are stateful; NACLs are stateless.
8. **"The guard booth reads its rules in order and stops at the first match. The lock just checks its whole approved list."** — NACL rule order matters; Security Group rules don't conflict, since all are "allow."
9. **"A footbridge only connects the two communities it was built between."** — VPC Peering is non-transitive.
10. **"Instead of a footbridge between every pair of communities, build one central roundabout."** — Transit Gateway replaces a full mesh of peering connections.

## Public vs. private subnet, at a glance

```
Public subnet  → route 0.0.0.0/0 → Internet Gateway   (two-way)
Private subnet → route 0.0.0.0/0 → NAT Gateway          (outbound-only)
Private subnet → no 0.0.0.0/0 route at all              (fully isolated)
```

## Speed-round definitions

| Term | One line |
|---|---|
| VPC | An isolated virtual network, scoped to one Region, defined by a CIDR block |
| Subnet | A slice of the VPC's address range, tied to exactly one AZ |
| Route table | Rules pairing a destination CIDR with a target — decides where traffic goes |
| Internet Gateway | A two-way, redundant gateway between public subnets and the internet |
| NAT Gateway | A managed, one-way (outbound-initiated) gateway for private subnets |
| Security Group | A stateful, allow-only firewall at the instance/ENI level |
| Network ACL | A stateless, allow-and-deny firewall at the subnet level |
| VPC Peering | A direct, non-transitive private link between two VPCs |
| Gateway Endpoint | A free route-table target for private access to S3/DynamoDB |
| Interface Endpoint (PrivateLink) | An ENI-based private connection to most other AWS services |
| Transit Gateway | A regional hub connecting many VPCs, VPNs, and Direct Connect links |
| Site-to-Site VPN | An encrypted tunnel to on-premises, over the public internet |
| Direct Connect | A dedicated physical link to on-premises, bypassing the public internet |
| Elastic IP | A static public IPv4 address you allocate and attach |
| VPC Flow Logs | A record of the IP traffic crossing a network interface in your VPC |

For full definitions of every term, see the [Glossary](glossary.md). To go deeper on any single row, jump back to the [folder index](index.md).
