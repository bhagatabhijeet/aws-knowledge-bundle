---
type: Concept
title: "Internet Gateway"
description: "The community's one public front gate — a horizontally scaled, redundant, highly available two-way door between public subnets and the internet."
tags: [aws, vpc, internet-gateway, networking]
sources:
  - id: aws-vpc-igw
    resource: https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html
    title: Amazon VPC User Guide — Internet gateways
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Internet Gateway — The Community's Front Gate

## 🚪 The mnemonic

An **Internet Gateway (IGW)** is the community's one public front gate: attach it to the VPC, point a subnet's route table at it, and residents on that street can walk out to the internet — and be walked up to from the internet — through the exact same gate, in both directions.

## The core facts

| Fact | Detail |
|---|---|
| Attachment | Exactly **one** Internet Gateway per VPC, attached directly to the VPC (not to any one subnet) |
| Direction | **Two-way** — outbound traffic leaves through it, and inbound traffic (to a resource with a public IP) arrives through it |
| Availability | Horizontally scaled, redundant, and highly available by design — there's no single instance to fail, and no bandwidth constraint it imposes itself |
| What makes it work | It performs **1:1 network address translation (NAT)** for any instance assigned a public IPv4 address — translating between the instance's private VPC address and its public address |
| Cost | No hourly charge for the gateway itself — you pay only for the data transfer that flows through it |

**Mnemonic:** *"The front gate doesn't rent by the hour — it's free to have, and residents just pay for what they carry through it."*

## What makes a subnet's traffic actually flow through it

Attaching an Internet Gateway to a VPC is necessary but not sufficient. A subnet only gets to use it once **two** things are true:

1. The subnet's route table has a route for `0.0.0.0/0` targeting the Internet Gateway.
2. The specific instance you want to reach has a **public IPv4 address** (or an [Elastic IP](nat-gateways.md)) assigned to it — the IGW only NATs traffic for addresses it's been told about.

**Mnemonic:** *"Attaching the gate to the community isn't the same as giving one house a street sign pointing at it, or that house a name the mail carrier recognizes."*

## Internet Gateway vs. NAT Gateway, at a glance

This is the distinction that trips people up most, so it's worth stating plainly before the deep dive in [NAT Gateways](nat-gateways.md):

| | Internet Gateway | NAT Gateway |
|---|---|---|
| Direction | Two-way — inbound and outbound | One-way — outbound-initiated only |
| Used by | Public subnets | Private subnets |
| Requires a public IP on the instance? | Yes, for the instance to be directly reachable | No — the NAT Gateway's own public IP is what the outside world sees |
| Cost | Free (data transfer only) | Hourly charge **plus** per-GB data processing charge |

**Mnemonic:** *"The front gate lets visitors knock. The mail-forwarding kiosk only lets residents send mail out — nobody outside can knock back through it."*

## Next up

Now the mail-forwarding kiosk itself, in full detail — the piece most VPC designs get wrong or pay too much for: [NAT Gateways](nat-gateways.md).

[^aws-vpc-igw]: Amazon VPC User Guide, "Internet gateways."
