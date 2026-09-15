---
type: Concept
title: "Best Practices"
description: "How to actually lay out a gated community: subnet tiers by AZ, one NAT Gateway per AZ, endpoints before NAT for AWS-native traffic, and layered security by default."
tags: [aws, vpc, best-practices]
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Best Practices — How to Actually Lay Out a Community

1. **Design a custom VPC for anything real.** The default VPC is a fine starter home, but production workloads deserve a deliberately sized CIDR block and a subnet layout you actually chose (see [What is a VPC?](what-is-a-vpc.md)).

2. **Build at least two subnets per tier, one per AZ.** A public and a private subnet in `us-east-1a`, matched by a public and private subnet in `us-east-1b` (and ideally a third AZ) — never rely on a single AZ's worth of subnets for anything that needs to stay up.

3. **Keep private subnets private by default.** Only put resources in a public subnet if they genuinely need to be reachable from, or economically reach out to, the internet directly — load balancers and NAT Gateways, not application servers or databases.

4. **Give every AZ its own NAT Gateway.** A shared, cross-AZ NAT Gateway reintroduces a single point of failure into an otherwise Multi-AZ design, and adds cross-AZ data transfer charges on top (see [NAT Gateways](nat-gateways.md)).

5. **Add Gateway Endpoints for S3 and DynamoDB before you ever look at your NAT bill.** This is free, removes that traffic from your NAT Gateway's per-GB charges entirely, and is usually the single biggest, easiest win against a surprising network bill.

6. **Use Security Groups for fine-grained rules, NACLs for coarse ones.** Reach for a Security Group to say "only my load balancer may reach my app servers on this port." Reach for a NACL when you need a blanket deny across an entire subnet, regardless of any instance's own rules (see [Security Groups & Network ACLs](security-groups-and-nacls.md)).

7. **Don't build a full mesh of VPC peering connections.** Once you're past two or three VPCs, a [Transit Gateway](connecting-to-on-premises.md) scales linearly where peering scales quadratically — and it's the only one of the two that supports transitive routing and direct on-premises attachment.

8. **Pair Direct Connect with a VPN failover, not either alone.** A dedicated line without a backup path is a single point of failure; a VPN-only connection over the public internet gives up the consistency a serious hybrid workload usually needs.

9. **Never assume a NACL's statelessness is someone else's problem.** Every custom NACL rule needs its explicit return-traffic counterpart in the opposite direction — a request that gets in but whose reply gets silently dropped is one of the most confusing failure modes in VPC networking to debug after the fact.

10. **Turn on VPC Flow Logs before you need them, not after an incident.** They're the record of exactly which IP traffic actually crossed a network interface — invaluable for both troubleshooting a routing mistake and investigating anything that looks like unauthorized access.

## Next up

Everything above, compressed onto one page: [Mnemonics Cheat Sheet](mnemonics-cheatsheet.md).
