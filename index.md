---
type: Bundle Index
title: "AWS Knowledge Bundle"
description: "An Open Knowledge Format (OKF) bundle that explains AWS services through memorable, visual, mnemonic-driven concept docs."
okf_version: "0.2"
tags: [aws, cloud, okf, index]
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-12T00:00:00Z
---

# AWS Knowledge Bundle

A growing [Open Knowledge Format](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md) (OKF) bundle for understanding AWS, one service at a time — written to be **remembered**, not just read.

Every service gets its own folder, its own running analogy, and its own set of hand-built diagrams so the mental model sticks long after you close the tab.

## Topics covered

* [cloud-computing/](cloud-computing/index.md) — Cloud computing fundamentals, taught through **Moving Day**: leaving a fixed house (on-premises/traditional hosting) for an elastic apartment complex (the cloud), a brief history of AWS, the IaaS/PaaS/serverless spectrum, and decomposing one server into EC2/RDS/S3. Start here if you're new to the cloud.
* [iam/](iam/index.md) — AWS Identity and Access Management, taught through **The Secure Office Building** analogy: root keys, employee badges, visitor passes, rulebooks, and the security guard who enforces them all.
* [s3/](s3/index.md) — Amazon S3, taught through **The Self-Storage Facility** analogy: rented units, boxes on shelves, and the shelf you pick trading price against retrieval speed.
* [device-farm/](device-farm/index.md) — AWS Device Farm, taught through **The Real Device Farm** analogy: real animals in the barn (physical devices), a ranch hand running automated tests in parallel, and a separate stable of robot horses (a managed Selenium Grid) for browser testing.
* [global-infrastructure/](global-infrastructure/index.md) — AWS Global Infrastructure, taught through **The World Map** analogy: a Region is a city, an Availability Zone is a borough with its own power and water, and an Edge Location is a corner store near every neighborhood on Earth.
* [vpc/](vpc/index.md) — Amazon VPC, taught through **The Gated Community** analogy: subnets are streets, route tables are street signs, an Internet Gateway is the front gate, and a NAT Gateway is the mail-forwarding kiosk that lets private streets send mail out without letting strangers mail back in.
* [dynamodb/](dynamodb/index.md) — Amazon DynamoDB, taught through **The Valet Parking Garage** analogy: hand over the right ticket (primary key) and the valet walks straight to your car (item), instantly, no matter how many cars are in the garage (table). Includes a hands-on tutorial building a real customer-orders table.
* [aws-cli/](aws-cli/index.md) — The AWS Command Line Interface, taught through **The Universal Remote** analogy: one remote (`aws`), a device button per service, an action per button, and dials for everything else. Covers install, configure, SSO/roles, command structure, `--query` filtering, pagination, S3, scripting, and troubleshooting.

More topics and services will be added over time — see [log.md](log.md) for the history of this bundle.
