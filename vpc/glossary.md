---
type: Concept
title: "VPC Glossary"
description: "Every VPC term used in this folder, defined in one line, with its gated-community analogy equivalent."
tags: [aws, vpc, glossary]
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# VPC Glossary

| Term | Definition | Gated-community analogy |
|---|---|---|
| **VPC (Virtual Private Cloud)** | A logically isolated virtual network, scoped to one Region, defined by a CIDR block | The gated community itself |
| **CIDR block** | The range of IP addresses assigned to a VPC or subnet | The community's (or street's) total address range |
| **Default VPC** | A pre-built VPC AWS creates automatically in every Region, with public subnets ready to use | A starter home AWS builds for you |
| **Subnet** | A slice of a VPC's address range, confined to exactly one Availability Zone | A street, built entirely inside one borough |
| **Public subnet** | A subnet whose route table sends internet-bound traffic to an Internet Gateway | A street with a sign pointing at the front gate |
| **Private subnet** | A subnet with no direct route to an Internet Gateway | A street with no sign pointing at the front gate |
| **Route table** | A set of rules pairing destination CIDR ranges with targets | The street signs at every intersection |
| **Local route** | The automatic, un-removable route sending in-VPC traffic to `local` | "Home addresses always stay local" |
| **Main route table** | The default route table every VPC gets automatically | The city's default signage |
| **Internet Gateway (IGW)** | A horizontally scaled, redundant, two-way gateway between a VPC and the internet | The community's one public front gate |
| **NAT Gateway** | A managed, AZ-scoped gateway giving private subnets outbound-only internet access | The mail-forwarding kiosk just inside the gate |
| **NAT instance** | A self-managed EC2 instance performing the same role as a NAT Gateway, without AWS managing it | A folding-table mail kiosk you staff yourself |
| **Private NAT Gateway** | A NAT Gateway variant with no public IP, used to route between VPCs or on-premises networks | An internal-only forwarding kiosk, never facing the highway |
| **Egress-Only Internet Gateway** | The IPv6 equivalent of a NAT Gateway's one-way-out behavior | — |
| **Elastic IP** | A static, allocatable public IPv4 address | A reserved address plaque that never changes |
| **Security Group** | A stateful, allow-only virtual firewall at the instance/ENI level | The lock on one house's front door |
| **Network ACL (NACL)** | A stateless, allow-and-deny virtual firewall at the subnet level | The guard booth at the entrance to a street |
| **VPC Peering connection** | A direct, non-transitive private network link between two VPCs | A private footbridge to a neighboring community |
| **VPC Endpoint** | A private connection from a VPC to an AWS service, bypassing the public internet | A private tunnel straight to a city service building |
| **Gateway Endpoint** | A free, route-table-based VPC Endpoint — S3 and DynamoDB only | A free street sign pointing straight at the warehouse |
| **Interface Endpoint (AWS PrivateLink)** | An ENI-based VPC Endpoint for most other AWS/partner services | A small private tunnel entrance built on your street |
| **Transit Gateway** | A regional hub connecting many VPCs, VPN connections, and Direct Connect links | A central roundabout |
| **Site-to-Site VPN** | An encrypted (IPsec) tunnel connecting on-premises to a VPC over the public internet | An encrypted courier route over the public highway |
| **Virtual Private Gateway (VGW)** | The AWS-side endpoint of a Site-to-Site VPN or Direct Connect connection | The community's end of the courier route |
| **Customer Gateway** | The on-premises-side representation of a VPN connection | Headquarters' end of the courier route |
| **AWS Direct Connect** | A dedicated physical network connection from on-premises to AWS, bypassing the internet | A dedicated private road, never touching the highway |
| **VPC Flow Logs** | A record of the IP traffic crossing a network interface in a VPC | Security-camera footage of every street |

Back to the [folder index](index.md) · [mnemonics cheat sheet](mnemonics-cheatsheet.md).
