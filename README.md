# h100 cloud pricing: real rates from $1.38 to $12.29 per GPU-hour, what drives the spread, and how to pick the right H100 for your workload

An on-demand NVIDIA H100 rents for anywhere between $1.38 and $12.29 per GPU-hour right now, and the difference comes down almost entirely to which provider you ask. Most dedicated, no-commitment listings cluster between $2 and $3.50. The hyperscalers charge two to five times that for the same silicon. Two years ago the walk-up rate was $8–$10/hr, so if you priced H100 capacity back in 2024 and decided to wait, the market has thoroughly changed its mind.

This guide covers current rates across the providers that matter, the three variables that make identical GPUs quote so differently, and what real workloads cost once the hours stack up. It also puts Vultr's H100 pricing, $2.99 per GPU-hour for an 8-GPU node, into that context, because mid-market providers are where most production teams end up.

> **The short version:** the $2–$3 per GPU-hour band is the sweet spot for dedicated hardware with real cloud services around it, and [👉 Vultr's 8x H100 node at $2.99 per GPU-hour](https://www.vultr.com/products/cloud-gpu/?ref=7268283) sits right in it, billed hourly with no commitment. Chasing the absolute floor gets you community-marketplace PCIe cards around $1.99. Enterprise-bound with flexible scheduling? AWS Capacity Blocks bring the same hardware down to roughly $5.19 per GPU.

## What H100 cloud pricing looks like right now

H100 cloud pricing currently spans a roughly 8.5x range between the cheapest and most expensive tracked listings. DeployBase's March 2026 survey of 28+ providers put the floor at $1.38 per GPU-hour and the top of its list at $11.68; by August, Spheron's tracker had Azure's on-demand rate at $12.29 per GPU. Most verified on-demand listings sit between $2.89 and $5.19, and an aggregator covering 47 providers put the market average at $3.41 per GPU-hour.

Here's the field, sorted from cheapest up. All figures are on-demand, per-GPU rates tracked between March and August 2026. Treat them as ballparks rather than quotes, because these numbers move weekly.

| Provider | Configuration | $/GPU-hour | Notes |
| --- | --- | --- | --- |
| ThunderCompute | H100 (virtualized) | $1.38 | Shared infrastructure, lower uptime guarantees |
| Vast.ai | H100 SXM | from $1.92 | Peer-to-peer marketplace; varies by host |
| RunPod (Community) | H100 PCIe | $1.99 | Cheapest dedicated single-GPU tier |
| Spheron | H100 PCIe | $2.64 | Per-minute billing, no node minimum |
| RunPod | H100 SXM | $2.69 | NVLink pod pricing |
| Nebius | H100 | $2.95 | Single-GPU dedicated |
| Vultr | 8x H100 node | $2.99 | Rate covers the full node: CPUs, RAM, NVLink |
| DigitalOcean | 8x H100 | $2.99 | Node pricing |
| Lambda | H100 PCIe / SXM | $2.86–$3.99 | Self-serve rates shifted between trackers |
| CoreWeave | HGX H100, 8-GPU bundle | $6.16 | 8-GPU bundles only; spot around $2.46 |
| AWS (EC2 P5) | 8x H100 SXM5 | $6.88 | $55.04/hr per node; Capacity Blocks ~$5.19/GPU |
| Oracle Cloud | 8x H100 bare metal | $10.00 | Flat published rate |
| Google Cloud (A3 High) | 8x H100 SXM5 | $10.98 | $87.83/hr per node; preemptible $3.30–$3.69 |
| Azure (ND H100 v5) | 8x H100 SXM5 | $12.29 | $98.32/hr per node; highest tracked on-demand rate |

Three things stand out. The hyperscaler premium is real: AWS, Google Cloud, and Azure all charge multiples of the mid-market rate for identical chips. The middle of the market, roughly $2 to $3, is where you get dedicated GPUs plus enough operational maturity for production work. And anything under $2 deserves a fine-print read before you plan a project around it.

## Why the same GPU rents for wildly different prices

The hourly number hides three variables: which physical variant you're renting, what's bundled into the rate, and which purchase model you're on. Get any of the three wrong and a "cheap" H100 turns out to be neither cheap nor the H100 you needed.

### Form factor: PCIe, SXM, or NVL

The H100 ships in three configurations, and the performance gaps are large enough to show up in your bill.

- **H100 PCIe (80GB):** fits standard servers, 2.0 TB/s memory bandwidth, 350W. Most boutique providers run this variant because integration is simple. Best for inference and single-GPU work.
- **H100 SXM5 (80GB):** 3.35 TB/s bandwidth, 700W, and NVLink at 900 GB/s per GPU, which is what lets eight GPUs train as one unit. Cloud rates run roughly 20–50% above PCIe, and for multi-GPU training the premium pays for itself in faster gradient synchronization. For single-GPU inference, it's wasted money.
- **H100 NVL (188GB paired):** two dies bridged for 3.9 TB/s bandwidth, built for models that exceed 80GB without model parallelism. Availability is still thin; RunPod lists it at $2.59 while Azure's NVL listing sits at $6.98.

### What the rate actually includes

This is where most price comparisons quietly break down. An "8x H100" listing bundles the host CPUs, system RAM, networking, and NVLink fabric into the per-GPU figure, so it isn't directly comparable to a bare single-GPU rental. CoreWeave takes this further: its HGX H100 is only sold as an 8-GPU bundle at $49.24/hr, with no single-GPU option at all. AWS's headline $6.88/GPU is really a $55.04/hr node divided by eight.

The sub-$2 tier needs its own caveat. ThunderCompute's $1.38 rate, the cheapest tracked listing, is a virtualized GPU on shared infrastructure with lower uptime guarantees than a dedicated rental. And above roughly $5 per GPU-hour, you're mostly paying for managed services, SLAs, and compliance infrastructure rather than raw compute. None of that is a scam; it just means you should know which of those things you're actually buying.

## Vultr's H100 plans and pricing

Vultr rents its 8x H100 node at **$2.99 per GPU-hour**, about $23.92/hr for the complete node, which lands it in the mid-market band while including hardware that marketplace listings make you assemble yourself: the host CPUs, system RAM, and the NVLink fabric that lets all eight GPUs train as one unit.

Billing is hourly and on-demand with no minimum term. You deploy, run the job, destroy the node, and the meter stops. And because Vultr is a general-purpose cloud rather than a GPU-only marketplace, the node can live in the same account as your VPC network, block storage, load balancers, or a managed Kubernetes cluster, which matters once inference services need to talk to the GPUs.

| Vultr plan | What you get | Price | Billing | Where to buy |
| --- | --- | --- | --- | --- |
| 8x H100 GPU node (Bare Metal) | 8x NVIDIA H100 80GB, NVLink-connected; rate covers the full node | $2.99 per GPU/hr (≈ $23.92/hr per node) | Hourly, on-demand | [ Check current H100 pricing](https://www.vultr.com/products/cloud-gpu/?ref=7268283) |
| Other GPU plans | NVIDIA accelerators below H100 (A100-class and lighter) for inference and smaller jobs | Per-model rates listed on the pricing page | Hourly, on-demand | [ See all GPU plans](https://www.vultr.com/pricing/?ref=7268283) |

Honest limits: Vultr isn't the cheapest per-GPU rate on the board. RunPod's community cloud undercuts it, and if you need a single H100 by the hour rather than an eight-way node, single-GPU PCIe listings elsewhere fit better. For very large multi-node runs with managed scheduling, the CoreWeave and hyperscaler tier exists for a reason. Vultr's number makes sense when you want a full NVLink node, on demand, from an established provider, without signing anything. [👉 Vultr's live GPU pricing page](https://www.vultr.com/pricing/?ref=7268283) has the rates behind this table, and GPU pricing pages update more often than any article can.

## On-demand, spot, or reserved? The 2026 market is weird

Three purchase models, three prices for identical silicon, and this year they've moved in opposite directions, which breaks the usual "commit and save" advice.

On-demand has collapsed. From $8–$10/hr at the 2024 peak, rates fell 64–75% to a $1.80–$3.50 range by Q2 2026. If your workload is bursty or you haven't pinned down your steady-state GPU count, this remains the right default even at the upper end.

Spot capacity drops lower still. CoreWeave's HGX spot sits around $2.46 within its 8-GPU bundle, GCP's preemptible A3 tier runs $3.30–$3.69, and marketplaces have shown spot listings as low as $0.34. The catch is reclaim windows: AWS spot can be taken back in about two minutes, GCP's preemptible tier gives roughly 30 seconds of notice. Only put checkpointable jobs here.

Reserved is the strange one. One-year H100 contract pricing rose almost 40%, from $1.70 to $2.35 per GPU-hour between October 2025 and March 2026, moving against the rest of the market. SemiAnalysis's reporting ties this to hoarding: on-demand capacity has been sold out across GPU types, and those holding it aren't giving it back. The practical takeaway is that committing for a year no longer guarantees savings; run the math before signing. Azure is the standing exception, where reserved discounts of up to 60% still beat its $12.29 list price.

## What real H100 workloads cost

Hourly rates mean nothing until they're multiplied by the hours a job actually runs. Three examples, with assumptions stated so you can swap in your own numbers.

**Fine-tuning a 7B model.** One tracker's worked example: a quantized Mistral 7B with LoRA over 100K training examples takes roughly 6.5 hours on a single H100 PCIe. At a $1.99 rate that's about $13 total. The same job on an A100 needed around 18 hours and cost more despite the lower hourly rate; the H100's speed is the discount.

**Serving a 70B model.** The same source measured ~850 tokens/sec on a 4-bit quantized 70B with one H100 PCIe, about $0.65 per million tokens versus $1.19 on an A100. Higher hourly rate, 45% lower cost per token.

**A 70B pretraining run.** Using a published worked example, a 70B-parameter model over 1 trillion tokens needs roughly 221 node-hours on an 8x H100 SXM cluster. The same run costs:

| 8x H100 node | Node rate | Cost for 221 node-hours |
| --- | --- | --- |
| RunPod SXM | $21.52/hr | ~$4,760 |
| Vultr | $23.92/hr | ~$5,290 |
| AWS p5.48xlarge | $55.04/hr | ~$12,160 |

Same model, same token count, about $7,400 between the cheapest and priciest option. If the node runs around the clock instead, here's the standing cost at 730 hours per month:

| Provider (8x H100 node) | Node rate | 24/7 monthly cost |
| --- | --- | --- |
| RunPod SXM | $21.52/hr | ~$15,710 |
| Vultr | $23.92/hr | ~$17,460 |
| Lambda 8-GPU pod | $27.52/hr | ~$20,090 |
| CoreWeave HGX | $49.24/hr | ~$35,945 |
| AWS p5.48xlarge | $55.04/hr | ~$40,179 |
| Google Cloud A3 High | $87.83/hr | ~$64,116 |
| Azure ND H100 v5 | $98.32/hr | ~$71,774 |

Blended on-demand pricing has drifted down about 4% since July 2025, so these standing costs are more likely to shrink than grow over a multi-month project. That's one more reason not to rush into a long commitment.

## H100 cloud pricing FAQ

**Why do AWS, Google Cloud, and Azure charge 2–5x more for the same H100?**
Because the list price bundles far more than the GPU: managed infrastructure, SLAs, compliance tooling, and quota systems that gate access. Azure's ND H100 v5 runs about $12.29 per GPU behind a quota approval process; AWS only sells H100s inside the p5 instance family, with capacity block minimums on top. Their discount programs narrow the gap when your schedule is flexible: AWS Capacity Blocks bring p5 down to about $5.19 per GPU, and GCP's preemptible A3 tier runs $3.30–$3.69.

**Is the cheapest H100 listing too good to be true?**
Sometimes, yes. The $1.38 floor in the table above is a virtualized GPU on shared infrastructure with weaker uptime guarantees than a dedicated rental. A mid-2026 roundup found a $1.49 promotional low sitting against a $6.98 regional high, with most verified dedicated listings between $2.89 and $5.19. Before comparing any two rates, confirm both GPUs are dedicated hardware.

**How much does an 8-GPU H100 node cost per month running around the clock?**
Between roughly $15,700 and $71,800 depending on provider; the full table is above. It's the same 2–5x hyperscaler premium, just showing up on a monthly invoice instead of an hourly one.

**Should I sign a one-year reserved H100 contract?**
Usually not right now. On-demand rates fell 64–75% from their 2024 peak while one-year reserved pricing climbed almost 40% between October 2025 and March 2026, so the committed rate got more expensive while the walk-up rate got cheaper. Azure's reserved discounts, up to 60% off list, are the exception. Outside that case, commit only if the GPUs will run hot 24/7 for the full term.

## Which H100 provider fits your workload

- If budget is the whole story and you can manage the infrastructure yourself, the marketplace tier ($1.38–$1.99) is where the floor lives. Verify the GPU is dedicated before you compare it against anything.
- If you need dedicated hardware, real cloud services around it, and hourly flexibility, stay in the $2–$3 band: Nebius at $2.95, DigitalOcean at $2.99, Lambda's PCIe cards from $2.86, or [👉 deploy an H100 node on Vultr](https://bit.ly/VulTR) at $2.99, where the full 8-GPU NVLink node is included in that per-GPU math.
- If multi-GPU training is the job and per-GPU price is the metric, RunPod's SXM pods at $2.69 are the cheapest NVLink option tracked.
- If you're running multi-node pretraining with enterprise requirements, the CoreWeave and hyperscaler tier buys scheduling, SLAs, and support the mid-market doesn't offer. AWS-bound teams should price Capacity Blocks before paying the $6.88 walk-up rate.

Three checks before you pay for any of it: confirm the form factor matches the job (SXM for multi-GPU training, PCIe for single-GPU work), confirm whether the quoted price is per GPU or per node, and confirm the hardware is dedicated rather than shared. Rates have been drifting down since mid-2025, but they move in both directions week to week, so [👉 check Vultr's current H100 pricing](https://www.vultr.com/products/cloud-gpu/?ref=7268283) before you lock anything in. And if the job can survive interruptions, get a spot quote too.
