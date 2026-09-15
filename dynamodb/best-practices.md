---
type: Concept
title: "Best Practices"
description: "How to actually run a garage: design the ticket system around your access patterns, avoid hot sections, prefer On-Demand until you know better, and never let a Scan into a hot path."
tags: [aws, dynamodb, best-practices]
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Best Practices — How to Actually Run a Garage

1. **Design the primary key around your access patterns, not your data's natural shape.** Decide how you'll actually query the data first — "get all of a customer's orders," "get one order by ID" — and let the Partition Key and Sort Key follow from that (see [Primary Keys & Partitions](primary-keys-and-partitions.md)).

2. **Choose a high-cardinality Partition Key.** A key like a status flag or a boolean funnels most traffic into one or two partitions — pick something that naturally varies across your dataset (a customer ID, a device ID) to avoid a hot partition.

3. **Default to On-Demand capacity for new or unpredictable workloads.** Switch to Provisioned with Auto Scaling only once traffic is steady enough that reserving capacity ahead of time is measurably cheaper (see [Capacity Modes](capacity-modes.md)).

4. **Treat Scan as an exception, not a tool.** A Scan's cost scales with the whole table, every time — reach for it only for rare, one-off, or admin-side operations, never inside a request path your application depends on regularly (see [Querying vs. Scanning](querying-vs-scanning.md)).

5. **Add a GSI before you reach for a Scan.** If an access pattern doesn't fit your primary key, a Global Secondary Index almost always beats scanning the whole table, at the cost of a small amount of eventual-consistency lag (see [Secondary Indexes](secondary-indexes.md)).

6. **Plan every Local Secondary Index at table-creation time.** Unlike a GSI, an LSI cannot be added after the fact — decide up front whether you need one, because retrofitting means rebuilding the table.

7. **Use eventually consistent reads unless you specifically need otherwise.** They cost roughly half of a strongly consistent read — reserve strong consistency for the narrow cases where reading your own very-recent write actually matters.

8. **Let TTL do your cleanup instead of a scheduled job.** Any data with a known expiration (sessions, temporary tokens, stale cart items) should carry a TTL attribute rather than relying on a separate process to delete it later.

9. **Reach for DAX only once you've measured a real read-latency or read-cost problem.** It's an optimization for a proven hot-read pattern, not a default addition to every table.

10. **Keep items well under the 400 KB ceiling, and watch for unbounded growth.** An ever-appending list or map on a single item is a sign that data belongs in its own item or table instead.

## Next up

Everything above, compressed onto one page: [Mnemonics Cheat Sheet](mnemonics-cheatsheet.md).
