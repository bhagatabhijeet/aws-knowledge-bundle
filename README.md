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

More services are on the way — each one gets its own folder, its own analogy, and its own set of diagrams. See [log.md](log.md) for the bundle's history.

## How this bundle is organized

This repository follows the [Open Knowledge Format (OKF) v0.2](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md):

```
├── index.md              # bundle-root index (OKF)
├── log.md                # bundle changelog
└── iam/                  # one folder per AWS service
    ├── index.md           # folder index — start here
    ├── log.md             # folder changelog
    ├── *.md               # one concept per file, YAML frontmatter + sources
    └── assets/images/     # original SVG diagrams for that service
```

Every concept doc cites its official AWS source in a `sources:` frontmatter block and a matching footnote, so you can always jump from the mnemonic straight to the primary documentation.

## Start here

👉 **New to IAM?** Begin at [`iam/index.md`](iam/index.md).
👉 **Cramming before an exam or interview?** Jump straight to the [Mnemonics Cheat Sheet](iam/mnemonics-cheatsheet.md).
