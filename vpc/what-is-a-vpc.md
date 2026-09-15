---
type: Concept
title: "What is Amazon VPC?"
description: "Your own gated community carved out of the AWS cloud: a private, isolated network with an address range only you control, scoped to a single Region."
tags: [aws, vpc, networking, fundamentals, cidr]
sources:
  - id: aws-vpc-guide
    resource: https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html
    title: Amazon VPC User Guide — What is Amazon VPC?
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# What is Amazon VPC?

**Amazon VPC (Virtual Private Cloud)** is a logically isolated section of the AWS cloud where you launch resources in a network you define and fully control — your own address space, your own routing rules, your own gates in and out. Nothing enters or leaves without a route you created on purpose.

## 🏘️ The mnemonic: a gated community

Think of a VPC as a **gated community** built inside a city ([an AWS Region](../global-infrastructure/regions.md)). The community has its own private address range — nobody outside can just wander in, and residents inside can't leave except through gates you build and control. Everything else in this folder — streets (subnets), street signs (route tables), the front gate (Internet Gateway), the mail-forwarding kiosk (NAT Gateway) — is a piece of that one community's layout.

![The VPC Gated Community](assets/images/vpc-overview.svg)

## The VPC is defined by its address range

Every VPC starts with a **CIDR block** — the total range of private IP addresses available inside it, e.g. `10.0.0.0/16` (65,536 addresses). You choose this range when you create the VPC, and everything you build inside it — every subnet — carves out a piece of that range.

| Fact | Detail |
|---|---|
| CIDR block size | From `/16` (65,536 addresses) down to `/28` (16 addresses) for IPv4 |
| Scope | A VPC lives inside exactly **one Region**, but can span every Availability Zone in it |
| Secondary CIDR blocks | You can add more address ranges to a VPC later, if you outgrow the original one |
| Reserved addresses | AWS reserves 5 IP addresses in every subnet (network address, VPC router, DNS, a future-use address, and the broadcast address) — plan your subnet sizes with that in mind |

**Mnemonic:** *"The community's total address range is fixed the day you found it — choose it generously, because splitting streets later is easier than running out of house numbers."*

## Default VPC vs. custom VPC

Every AWS account gets a **default VPC** in every Region, pre-built so you can launch an instance immediately without designing any networking yourself:

| | Default VPC | Custom VPC |
|---|---|---|
| Created by | AWS, automatically, per Region | You, deliberately |
| Subnets | One public subnet per AZ, pre-configured | Whatever you design |
| Internet access | Attached Internet Gateway, instances get public IPs by default | You decide public vs. private, subnet by subnet |
| Best for | Quick experiments, learning | Anything production, anything with real security requirements |

**Mnemonic:** *"The default VPC is a starter home AWS builds for you on day one — comfortable to move into, but not where you want to raise a real production workload."*

## What actually lives inside a VPC

- **Subnets** — the streets, each tied to one Availability Zone (see [Subnets](subnets.md))
- **Route tables** — the street signs directing traffic (see [Route Tables](route-tables.md))
- **Gateways** — the Internet Gateway (the front gate) and NAT Gateway (the mail-forwarding kiosk) that connect the community outward (see [Internet Gateway](internet-gateway.md) and [NAT Gateways](nat-gateways.md))
- **Security controls** — Security Groups and Network ACLs, the lock on each door and the guard at each street (see [Security Groups & Network ACLs](security-groups-and-nacls.md))
- **Peering and endpoints** — footbridges to other VPCs and private tunnels straight to AWS services (see [VPC Peering & Endpoints](vpc-peering-and-endpoints.md))

## Next up

Start by laying out the streets: [Subnets](subnets.md).

[^aws-vpc-guide]: Amazon VPC User Guide, "What is Amazon VPC?"
