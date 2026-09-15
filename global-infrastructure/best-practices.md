---
type: Concept
title: "Best Practices"
description: "How to actually use the map: picking a Region deliberately, spreading across AZs by default, and knowing which services are global before you assume they aren't."
tags: [aws, global-infrastructure, best-practices]
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Best Practices — How to Actually Use the Map

1. **Pick a Region deliberately, not by default.** Weigh latency to your actual users, compliance/data-residency requirements, and whether the services you need are even available there (see [Regions](regions.md)) — don't just inherit whatever Region a tutorial used.

2. **Spread across at least two AZs by default.** It's close to free in latency and removes an entire class of outage. Treat a single-AZ deployment as a deliberate, documented exception, not the default.

3. **Compare AZ IDs, not AZ names, across accounts.** `us-east-1a` in your account and in a partner's account are not guaranteed to be the same physical Availability Zone — use AZ IDs whenever two accounts need to align (or deliberately avoid) the same AZ.

4. **Put a CDN in front of anything with a global audience.** If your users span continents, Edge Locations and CloudFront will do more for perceived performance than moving your origin Region ever will (see [Edge Locations & CloudFront](edge-locations-and-cloudfront.md)).

5. **Don't reach for Local Zones or Wavelength by default.** They solve a specific, narrow latency problem (a metro area, a mobile 5G network) — adopt them only when a workload demonstrably needs latency lower than a full Region can deliver, not as a general performance upgrade.

6. **Know which services are global before you assume they're Regional.** IAM, Route 53, and CloudFront operate across your whole account, not inside one Region — a Multi-Region architecture doesn't add redundancy for a service that only exists once.

7. **Audit every "Multi-AZ" architecture for hidden single points of failure.** A shared NAT Gateway, a hardcoded database endpoint, or an unreplicated cache in just one AZ can quietly undo the protection you think you have (see [Designing for High Availability](designing-for-high-availability.md)).

8. **Match your disaster-recovery pattern to what you can actually afford to lose.** Backup-and-restore, pilot light, warm standby, and active-active trade recovery speed against cost in a straight line — pick the point on that line your business genuinely needs, not the most impressive-sounding one.

9. **Test failover before you need it.** A Multi-AZ or Multi-Region design you've never actually failed over to is a hope, not a guarantee — run the drill.

## Next up

Everything above, compressed onto one page: [Mnemonics Cheat Sheet](mnemonics-cheatsheet.md).
