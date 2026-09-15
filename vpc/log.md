# VPC Folder Update Log

## 2026-09-15
* **Creation**: Authored the full VPC concept set (what-is-a-vpc, subnets, route-tables, internet-gateway, nat-gateways, security-groups-and-nacls, vpc-peering-and-endpoints, connecting-to-on-premises, best-practices, mnemonics-cheatsheet, glossary) plus original SVG diagrams under `assets/images/`, all built on the "Gated Community" mnemonic analogy. NAT Gateways were covered in dedicated depth per an explicit request, including the one-per-AZ HA pattern, the NAT Gateway vs. NAT instance comparison, the per-GB cost trap, and the Gateway Endpoint mitigation.
* **Addition**: Added [tenancy.md](tenancy.md), covering default/dedicated-instance/dedicated-host tenancy, the VPC-level instance tenancy attribute and how it constrains per-instance launch choices, and Dedicated Instance vs. Dedicated Host (host affinity, BYOL licensing) — per an explicit request. Updated the folder index, mnemonics cheat sheet, and glossary accordingly.
