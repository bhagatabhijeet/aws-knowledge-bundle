---
type: Concept
title: "S3 Static Website Hosting"
description: "Turning your storage unit's front wall into a storefront window — no server required, just an index page and a public sign."
tags: [aws, s3, static-website, cloudfront]
sources:
  - id: aws-s3-website-hosting
    resource: https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html
    title: Amazon S3 User Guide — Hosting a Static Website Using Amazon S3
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-13T00:00:00Z
---

# Static Website Hosting — the Storefront Window

## 🪟 The mnemonic

Normally a storage unit is a private room — you go in, you grab a box, you leave. **Static website hosting** turns one wall of the unit into a **storefront window**: point a browser at it, and S3 serves up HTML, CSS, JS, and images directly, with no server process running anywhere.

**Mnemonic:** *"No cash register, no staff, no back office — just a window display anyone can walk up to."* That's exactly what "static" means here: no server-side code execution (no PHP, no Node.js backend) — just files served as-is.

## What you configure

1. Enable **static website hosting** on the bucket, specifying:
   - An **index document** (e.g. `index.html`) — served for the root and any "folder" path
   - An **error document** (e.g. `error.html`) — served for 4xx errors
2. Make the relevant objects **publicly readable** (a bucket policy allowing `s3:GetObject` to everyone — this requires deliberately relaxing [Block Public Access](security-and-access-control.md) for this bucket)
3. Use the bucket's special **website endpoint** — a different hostname than the standard S3 API endpoint:

```
Standard API endpoint:   my-bucket.s3.amazonaws.com
Website endpoint:        my-bucket.s3-website-us-east-1.amazonaws.com
```

**Mnemonic:** *"Two different doors into the same unit — one for API requests, one that acts like a storefront and understands index/error pages."* Only the website endpoint understands "index document" and redirect rules; the API endpoint does not.

## Why you almost always put CloudFront in front of it

A bare S3 website endpoint works, but pairing it with **CloudFront** (AWS's CDN) adds what S3 alone doesn't provide:

| Bare S3 website endpoint | S3 + CloudFront |
|---|---|
| HTTP only (no HTTPS on the website endpoint itself) | HTTPS with a custom domain and certificate |
| No caching at edge locations | Cached at edge locations worldwide — faster for global visitors |
| Bucket often needs to be fully public | Can keep the bucket private, using an **Origin Access Control (OAC)** so only CloudFront can read it |
| No custom domain support | Full custom domain support |

**Mnemonic:** *"S3 is the storefront's back-room inventory. CloudFront is the storefront window on every street corner in the world, all pulling from the same back room."*

## What static hosting is not for

Anything requiring server-side logic at request time — authentication checks, database queries, form processing — needs compute somewhere (Lambda, EC2, or a PaaS). A common pattern is a **static front-end on S3 + CloudFront**, calling out to an API (API Gateway + Lambda) for anything dynamic. That's the essence of the "JAMstack" architecture pattern.

## Next up

Whether serving files to a website or an application, upload and download speed matters at scale — see [Performance & Transfer](performance-and-transfer.md).

[^aws-s3-website-hosting]: Amazon S3 User Guide, "Hosting a static website using Amazon S3."
