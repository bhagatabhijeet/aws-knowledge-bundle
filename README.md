# AWS Knowledge Bundle

**AWS concepts that stick.** An [Open Knowledge Format](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md) (OKF) bundle that explains AWS services through a running visual analogy, hand-built diagrams, and word-for-word mnemonics — so you don't just read the concept once, you remember it for good.

## Why this exists

Most AWS documentation is accurate but forgettable — a wall of text describing a JSON schema. This bundle takes the opposite approach:

- 🧠 **One analogy per service, carried through every doc** — so every new concept slots into a mental model you already have, instead of being one more disconnected fact.
- 🎨 **Original, colorful diagrams for every hard idea** — not screenshots, not AWS's own icon set, but purpose-built visuals that make the analogy literal.
- 🔤 **Explicit, quotable mnemonics** — short sentences and acronyms designed to be the thing you recall first, with the detail hanging off them.
- 🤖 **Machine-readable structure** — every doc follows the [OKF v0.2 spec](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md), so this bundle is just as useful to an AI agent doing research as it is to a human studying for an exam.
- 📚 **Grounded in official AWS docs** — every concept links back to the real [AWS documentation](https://docs.aws.amazon.com/) it's built on.

## 📂 What's inside

### [`cloud-computing/`](cloud-computing/index.md) — Cloud Computing Fundamentals

Taught through **Moving Day**: running things the traditional way is owning (or renting) a fixed house; the cloud is moving into an apartment complex that can resize your unit overnight. Covers why organizations move to the cloud, a short history of AWS as the cloud's reference point, and the IaaS → PaaS → Serverless spectrum.

![Moving Day overview](cloud-computing/assets/images/moving-day-overview.svg)

| | |
|---|---|
| [Why the Cloud, and Why AWS?](cloud-computing/why-cloud-and-aws.md) | [A Brief History of AWS](cloud-computing/history-of-aws.md) |
| [Service Models: IaaS, PaaS & Serverless](cloud-computing/service-models-iaas-paas-serverless.md) | [Moving to Cloud Storage](cloud-computing/moving-to-cloud-storage.md) |
| [Glossary](cloud-computing/glossary.md) | |

**New to the cloud? Start here before any service-specific folder below.**

### [`iam/`](iam/index.md) — AWS Identity and Access Management

Taught through **The Secure Office Building**: your AWS account is a building, the root user is the master key, IAM users are employee badges, roles are self-expiring visitor passes, policies are the rulebook, and a guard checks every rule at every door.

![The IAM Office Building](iam/assets/images/iam-building-analogy.svg)

| | | |
|---|---|---|
| [What is IAM?](iam/what-is-iam.md) | [The Root User](iam/root-user.md) | [Users & Groups](iam/users-and-groups.md) |
| [Roles](iam/roles.md) | [Policies](iam/policies.md) | [Policy Evaluation Logic](iam/policy-evaluation-logic.md) |
| [MFA](iam/mfa.md) | [Federation & STS](iam/federation-and-sts.md) | [Permission Boundaries & SCPs](iam/permission-boundaries-and-scps.md) |
| [Best Practices](iam/best-practices.md) | [Mnemonics Cheat Sheet](iam/mnemonics-cheatsheet.md) | [Glossary](iam/glossary.md) |

**The one sentence that unlocks the entire evaluation model:**
> *"Silence means no, a sign saying yes overrides silence, but a sign saying no overrides everything."*

### [`s3/`](s3/index.md) — Amazon S3 (Simple Storage Service)

Taught through **The Self-Storage Facility**: S3 stands for Simple Storage Service — three S's — so think of it as your own Self-Storage Service. Buckets are rented units, objects are boxes on a shelf, and the shelf you pick trades price against how fast you can get the box back.

![The S3 Self-Storage Facility](s3/assets/images/s3-warehouse-overview.svg)

| | | |
|---|---|---|
| [What is S3?](s3/what-is-s3.md) | [Buckets & Objects](s3/buckets-and-objects.md) | [Storage Classes](s3/storage-classes.md) |
| [Versioning](s3/versioning.md) | [Lifecycle Management](s3/lifecycle-management.md) | [Replication](s3/replication.md) |
| [Security & Access Control](s3/security-and-access-control.md) | [Access Points](s3/access-points.md) | [Encryption](s3/encryption.md) |
| [Sharing & Presigned URLs](s3/sharing-and-presigned-urls.md) | [Static Website Hosting](s3/static-website-hosting.md) | [Performance & Transfer](s3/performance-and-transfer.md) |
| [Consistency Model](s3/consistency-model.md) | [Data Processing & Notifications](s3/data-processing-and-notifications.md) | [Object Lock & Compliance](s3/object-lock-and-compliance.md) |
| [Monitoring & Cost Optimization](s3/monitoring-and-cost-optimization.md) | [Best Practices](s3/best-practices.md) | [Mnemonics Cheat Sheet](s3/mnemonics-cheatsheet.md) |
| [Glossary](s3/glossary.md) | | |

**The one sentence that unlocks storage-class selection:**
> *"The closer to the door, the more it costs to store — and the less it costs to grab."*

More services are on the way — each one gets its own folder, its own analogy, and its own set of diagrams. See [log.md](log.md) for the bundle's history.

## How this bundle is organized

This repository follows the [Open Knowledge Format (OKF) v0.2](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md):

```
├── index.md              # bundle-root index (OKF)
├── log.md                # bundle changelog
├── cloud-computing/      # fundamentals — start here
├── iam/                  # one folder per AWS service/topic
└── s3/                   # ...
    ├── index.md           # folder index — start here
    ├── log.md             # folder changelog
    ├── *.md               # one concept per file, YAML frontmatter + sources
    └── assets/images/     # original SVG diagrams for that topic
```

Every concept doc cites its official AWS source in a `sources:` frontmatter block and a matching footnote, so you can always jump from the mnemonic straight to the primary documentation.

## Start here

👉 **New to the cloud entirely?** Begin at [`cloud-computing/index.md`](cloud-computing/index.md).
👉 **New to IAM?** Begin at [`iam/index.md`](iam/index.md).
👉 **New to S3?** Begin at [`s3/index.md`](s3/index.md).
👉 **Cramming before an exam or interview?** Jump to the [IAM](iam/mnemonics-cheatsheet.md) or [S3](s3/mnemonics-cheatsheet.md) Mnemonics Cheat Sheet.
