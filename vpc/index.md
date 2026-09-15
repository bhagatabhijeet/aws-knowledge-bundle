---
type: Directory Index
title: "Amazon VPC — Knowledge Folder"
description: "Index of the Amazon VPC concept docs — subnets, route tables, gateways, NAT, and security — taught through the Gated Community analogy."
tags: [aws, vpc, networking, subnets, nat-gateway, index]
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Amazon VPC — The Gated Community

![The VPC Gated Community](assets/images/vpc-overview.svg)

**A VPC is your own gated community, carved out of the AWS cloud — a private slice of network address space that nothing and nobody enters without your explicit permission.**

You pick the community's total address range (the **CIDR block**), lay out **streets** (subnets) inside it — each street built entirely within one borough ([Availability Zone](../global-infrastructure/availability-zones.md)) — and post **street signs** (route tables) at every intersection telling traffic where to go. A single **front gate** (Internet Gateway) lets residents reach the public internet and be reached back. A **mail-forwarding kiosk** just inside that gate (a NAT Gateway) lets residents on private streets send mail out — software updates, API calls — without ever letting a stranger mail them back. Every house has its own **lock** (Security Group), and every street has a **guard booth** (Network ACL) checking traffic in both directions.

| Gated-community piece | VPC concept |
|---|---|
| 🏘️ The gated community itself | A **VPC (Virtual Private Cloud)** — an isolated virtual network, scoped to one Region |
| 🔢 The community's total address range | The VPC's **CIDR block** — e.g. `10.0.0.0/16` |
| 🏠 A single street, built entirely inside one borough | A **Subnet** — tied to exactly one Availability Zone |
| 🚦 The street signs at every intersection | A **Route Table** — rules telling traffic where to go |
| 🚪 The community's one public front gate | An **Internet Gateway (IGW)** — two-way internet access for public subnets |
| 📫 The mail-forwarding kiosk just inside the gate | A **NAT Gateway** — one-way outbound internet access for private subnets |
| 🔒 The lock on one house's front door | A **Security Group** — stateful, per-instance firewall |
| 🚧 The guard booth at the entrance to a street | A **Network ACL (NACL)** — stateless, per-subnet firewall |
| 🏢 Whose building your house actually sits in | **Tenancy** — shared, dedicated-instance, or dedicated-host physical hardware |
| 🌉 A private footbridge to a neighboring community | **VPC Peering** — a direct, non-transitive link between two VPCs |
| 🚇 A private tunnel straight to a city service building | A **VPC Endpoint (PrivateLink)** — private access to an AWS service, no public internet |
| 🏗️ A central roundabout connecting many communities at once | A **Transit Gateway** — a regional network hub |
| 🛰️ An encrypted courier route over the public highway | A **Site-to-Site VPN** — encrypted tunnel back to on-premises |
| 🔌 A dedicated private road, never touching the public highway | **AWS Direct Connect** — a dedicated physical link to on-premises |
| 🎫 A reserved address plaque that never changes | An **Elastic IP** — a static public IPv4 address |

## Read in this order

1. [What is a VPC?](what-is-a-vpc.md) — the community, its address range, and default vs. custom VPCs
2. [Subnets](subnets.md) — the streets, and why public/private is a routing decision, not a label
3. [Route Tables](route-tables.md) — the street signs that make a subnet public or private
4. [Internet Gateway](internet-gateway.md) — the one front gate, and how it's different from a NAT Gateway
5. [NAT Gateways](nat-gateways.md) — the mail-forwarding kiosk, in full detail
6. [Security Groups & Network ACLs](security-groups-and-nacls.md) — the lock on the door vs. the guard at the street
7. [Tenancy](tenancy.md) — whose building your house actually sits in
8. [VPC Peering & Endpoints](vpc-peering-and-endpoints.md) — footbridges to other communities, and tunnels straight to AWS services
9. [Connecting to On-Premises](connecting-to-on-premises.md) — VPN, Direct Connect, and Transit Gateway
10. [Best Practices](best-practices.md) — how to actually lay out a community
11. [Mnemonics Cheat Sheet](mnemonics-cheatsheet.md) — the one page to review before an exam or interview
12. [Glossary](glossary.md) — every term, one line each

## Official AWS references

* [Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
* [VPCs and subnets](https://docs.aws.amazon.com/vpc/latest/userguide/configure-subnets.html)
* [NAT gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)
* [Security groups vs. network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-comparison.html)
* [AWS Transit Gateway](https://aws.amazon.com/transit-gateway/)
* [Dedicated Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/dedicated-instance.html) · [Dedicated Hosts](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/how-dedicated-hosts-work.html)

See [log.md](log.md) for this folder's update history.
