---
type: Concept
title: "Why the Cloud, and Why AWS?"
description: "The case for moving out of your own house and into the cloud's apartment complex — and why AWS is the complex worth touring first."
tags: [aws, cloud-computing, fundamentals, history]
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-13T00:00:00Z
---

# Why the Cloud, and Why AWS?

## 🚚 Owning a house vs. moving into the complex

Before "the cloud" existed as a mainstream option, running a website or application meant one of two things:

1. **Owning the house** — buying physical servers and racking them in a room in your own building (on-premises).
2. **Renting a fixed house** — paying a hosting company for a specific physical or dedicated machine, sized once and rarely resized.

Either way, you were locked into whatever capacity you started with. Traffic doubles overnight? You can't magically add a room to a house you own or rented as a fixed unit — you have to buy, ship, and install a *new* machine, which takes weeks.

**The cloud replaces "owning/renting a fixed house" with "moving into an apartment complex that can resize your unit on demand."** The complex already has the power, water, networking, and security in place — you just decide how big a unit you need today, and change your mind next month without moving buildings.

## The big-picture questions that actually matter

Learning cloud computing well means being able to answer three questions, in this order, for any system you're designing:

1. **Why move this into the cloud at all?** — Usually: to trade a large upfront cost (buying servers) for a pay-as-you-go one, and to stop guessing capacity years in advance.
2. **How is the cloud environment configured?** — Which building blocks (compute, storage, networking, identity) are assembled, and how do they talk to each other?
3. **How does that configuration scale as the business grows?** — Can the same architecture handle 10x the traffic by turning a dial, or does it require a redesign?

**Mnemonic:** *"Why, How, and How-far"* — why move, how it's built, how far it can stretch without breaking.

## A short history, treated as the history of "the cloud" itself

Amazon Web Services launched in the mid-2000s, starting with just a couple of building blocks — durable object storage and rentable virtual servers. It's widely regarded as the **first mainstream public cloud platform**, and it remains the **largest cloud provider by market share** years later.

That combination — first mover, plus continued leadership — is exactly why AWS is such a useful *lens* for learning cloud computing in general: the vocabulary and service categories that AWS popularized (object storage, virtual compute instances, managed databases, identity and access management) became the template that other major clouds (Microsoft Azure, Google Cloud) largely followed, even when they picked different product names.

**Mnemonic:** *"Learn the complex that built the neighborhood, and every other complex's floor plan looks familiar."*

## Why concepts outlast interfaces

Cloud provider consoles — the web dashboards you click through — change constantly. AWS's own console has been redesigned many times, and it will be redesigned again. If you memorize "click here, then here," that knowledge expires the next time a button moves.

What doesn't expire:

* Understanding that **compute, storage, networking, and identity** are the four building blocks nearly every cloud service is made of
* Understanding **why** a team would choose one storage tier over another, or one compute model over another
* Understanding that most concepts have a **near-equivalent on every major cloud** — a virtual server is a virtual server whether the button that creates it says "EC2," "Virtual Machines," or "Compute Engine"

**Mnemonic:** *"Learn the plumbing, not the light switches — light switches move, plumbing doesn't."* Once you know *why* a system needs object storage, a virtual server, and an identity layer, you can find the equivalent button on any provider's console in under a minute.

## Next up

Now put these ideas to work on a concrete example — migrating a website into the cloud — and meet the three service models that decide how much of the "apartment" you furnish yourself: [Service Models: IaaS, PaaS & Serverless](service-models-iaas-paas-serverless.md).
