# cloud service providers: how to compare pricing, egress fees, and lock-in before you commit, and when a smaller platform like Sharktech makes more sense

Search for "cloud service providers" and you'll mostly get listicles ranking AWS, Azure, and Google Cloud in some order. That's fine if you want trivia. It's not so useful if you're actually trying to pick one for a specific project, because the ranking barely matters — what matters is how a provider bills you, what it costs to get your data back out, and how hard it is to leave. Those three factors sink more cloud budgets than any benchmark score.

This guide covers what a cloud provider actually sells, how the market is structured, the five things genuinely worth comparing, and a concrete look at how a mid-sized provider prices the same job — using Sharktech, an OpenStack-based host with data centers in five cities, as the worked example.

## What a cloud service provider actually sells

Almost every "cloud service" falls into one of three buckets, and knowing which one you're shopping for narrows the field fast:

- **IaaS (Infrastructure as a Service)** — raw compute, storage, and networking that you configure yourself. You get virtual machines, disks, and IP addresses; you handle the OS and everything above it. This is what AWS EC2, Google Compute Engine, and Sharktech's cloud platform all are.
- **PaaS (Platform as a Service)** — a managed runtime where you deploy code and the provider runs the infrastructure underneath. Think managed databases, container platforms, or app hosting.
- **SaaS (Software as a Service)** — finished applications delivered over the web. Gmail, Salesforce, Notion. You're not really choosing infrastructure here at all.

When people compare cloud service providers by name, they're almost always shopping for IaaS — that's where pricing models diverge, where bills surprise people, and where the decision actually costs money. The rest of this guide assumes that's you.

## The market, briefly

The big picture: one industry tracker put AWS at roughly **30% of global cloud infrastructure spending** in the first quarter of 2026, with Microsoft Azure second and Google Cloud third — together the big three absorb about two-thirds of the market. Everyone else, from Oracle and IBM to hundreds of regional and specialist providers, fights over the remainder.

That "everyone else" category isn't a consolation bracket. Regional and mid-sized providers often compete on exactly the dimensions hyperscalers are weakest on: transparent flat pricing, human support you can reach by phone, low egress fees, and open-source platforms that don't lock you into proprietary APIs. Whether those advantages matter depends entirely on your workload — which is what the next section is about.

## Five things worth comparing before you sign anything

### 1. Pricing model: metered, flat, or something in between

Hyperscalers are pure metered billing — every vCPU-hour, GB-month, and API call is priced separately, and the only lever you get is reserved-instance discounts in exchange for one- or three-year commitments. Great for elasticity, brutal for budget forecasting if nobody on the team watches the dashboard.

Mid-sized providers more often use a hybrid: a fixed monthly fee covering a base resource pool, plus hourly rates for anything you consume beyond it. That model is easier to forecast — your floor is known, your ceiling is a choice.

### 2. Egress: the fee that ambushes your bill

Outbound data transfer is the classic hidden cost of cloud. After a small free allowance (on the order of 100GB/month at the big providers), outbound traffic runs roughly **$0.09 per GB** — a terabyte leaving AWS costs about $90. Comparison tools that track this stuff across providers have measured egress pricing varying by more than 100x, from literally zero at some object-storage specialists to double-digit cents per GB at the top end.

Egress matters even when you don't think you're a "traffic-heavy" workload, because backups, media serving, replication, and one-off migrations all count. It's also the mechanism that quietly punishes you for leaving: the more data you have in, the more it costs to move out.

### 3. Lock-in and portability

Every provider loves you at sign-up. The question is what leaving costs later. Proprietary managed services — vendor-specific databases, serverless platforms, orchestration tools — are the strongest glue: your architecture literally won't run elsewhere. Open standards like OpenStack, generic VM images, and S3-compatible storage are the weakest glue: you can export a disk image and boot it somewhere else this afternoon.

### 4. Support and SLA

A 99.999% uptime guarantee sounds like marketing fluff until you do the arithmetic: it allows about **five minutes of downtime per year**. Check what the SLA actually promises, what it pays out when breached, and whether support means a ticket queue with a two-day queue or a phone that a human answers at 1 AM.

### 5. Regions and data locality

Latency is physics. If your users are in Chicago, a VM in Chicago beats a VM in Virginia by 20-40ms round trip, every request. Hyperscalers offer 100+ regions; smaller providers offer a handful — usually enough if your audience is concentrated, limiting if it's genuinely global. Data-residency requirements (some jurisdictions demand data stays in-country) can decide this for you.

## A worked example: what a mid-sized provider looks like in practice

To make the comparison concrete, take **Sharktech** — a US-based hosting company that runs an OpenStack cloud platform out of five locations: Los Angeles, Las Vegas, Denver, Chicago, and Amsterdam. It's a useful example because it prices differently from the hyperscalers on almost every axis above.

**Ingress is free, and egress is cheap.** Inbound traffic is unlimited and unmetered. Current cloud plans list 20TB of outbound included, and beyond that the metered rate is **$0.002 per GB** — roughly 45x lower than the ~$0.09/GB you'd pay at a hyperscaler once past its free allowance. Moving a terabyte out costs about two dollars instead of about ninety.

**Billing is a hybrid, not a slot machine.** Each plan bundles a base resource pool (cores, RAM, SSD/HDD/NVMe storage). You carve that pool into as many virtual machines as you like — one big box, or a dozen small ones. Consume more than the base and you pay published hourly rates on the overage. Plans below Enterprise tier have a hard resource ceiling, so a misconfigured autoscaler can't produce a four-figure surprise invoice.

**The platform is open, and exit is documented.** It's OpenStack underneath (Virtuozzo Hybrid Infrastructure on top), with REST APIs for compute, storage, networking, and identity. You can upload your own ISOs and disk images, deploy from weekly-updated official Linux cloud images, and — this is the part most providers won't advertise — download your VM disk images whenever you want. Migration off the platform is a designed feature, not a support ticket you're scared to open.

**DDoS protection and uptime are included, not upsells.** The network has built-in DDoS mitigation, and the company publishes a 99.999% uptime guarantee. Storage comes in three tiers with published performance estimates: NVMe at around 1.2GB/s and 18,000 IOPS per volume, SSD at around 350MB/s and 6,000 IOPS, HDD for cheap bulk storage.

Third-party testing backs up the specs reasonably well: HostAdvice's 2026 review of the platform scored it **9.4/10 overall**, measured a ticket reply in **39 minutes at 1 AM**, and clocked ~10Gbps sustained transfer with 0.17ms internal latency between VMs. The same review's honest caveats: the answer to a deep kernel-tuning question was polite but generic, and the region list is short if you need Asia or South America coverage.

Sharktech itself claims 40-80% cost savings versus hyperscalers for equivalent workloads. Treat that as a vendor claim and test it against your own numbers — but with free ingress and a $0.002/GB egress rate, the math does tend to break in their favor for anything that moves data around. 👉 open Sharktech's cloud portal to see live plan pricing and run your own numbers

## Every current Sharktech cloud plan

The lineup has two product lines on the same OpenStack infrastructure: **Public Cloud** (base pool + pay-as-you-go overage) and **Dedicated Cloud** (fixed resources, flat monthly bill). Here's everything currently listed on their ordering portal:

| Plan | Base resources (starting config) | Scale range | Starting price (USD) | Billing model | Buy |
| --- | --- | --- | --- | --- | --- |
| Public Cloud — Small | 4 vCPU, 8GB RAM, 300GB SSD | up to 16 vCPU / 32GB RAM | **$39/mo** | Monthly base + hourly overage | [ Order Small](https://bit.ly/SharKTech) |
| Public Cloud — Medium | 8 vCPU, 16GB RAM, 800GB SSD | up to 32 vCPU / 64GB RAM | **$79/mo** | Monthly base + hourly overage | [ Order Medium](https://bit.ly/SharKTech) |
| Public Cloud — Large | 32 vCPU, 64GB RAM, 1500GB SSD | up to 128 vCPU / 256GB RAM | **$249/mo** | Monthly base + hourly overage | [ Order Large](https://bit.ly/SharKTech) |
| Public Cloud — Enterprise | 64 vCPU, 128GB RAM, 5000GB SSD | **no cap** | **$499/mo** | Monthly base + hourly overage, uncapped | [ Order Enterprise](https://bit.ly/SharKTech) |
| Dedicated Cloud | 8 vCPU, 16GB RAM (configurable) | up to 512 vCPU / 1024GB RAM | **from $86.23/mo** | Fixed monthly fee | [ Configure Dedicated Cloud](https://bit.ly/SharKTech) |

All plans include unlimited VMs within your resource pool, unlimited free inbound traffic, one free public IPv4 address (extra IPs are $1.50/month), and choice of the five data center locations. Public Cloud tiers can add HDD and NVMe storage alongside the included SSD. Beyond the listed tiers, Sharktech quotes custom configurations through sales rather than publishing them, and the broader catalog includes S3-compatible object storage, CDN, an Acronis backup add-on, Smart VPS, and bare-metal servers if your workload outgrows the cloud lineup.

The published overage rates for Public Cloud, billed hourly:

- CPU: $0.0025 per core-hour
- RAM: $0.0035 per GB-hour
- NVMe storage: $0.00009 per GB-hour
- SSD storage: $0.00006 per GB-hour
- HDD storage: $0.00002 per GB-hour
- Outbound bandwidth beyond the included allowance: $0.002 per GB

Two practical notes from the fine print. Payment options are broad — credit card, PayPal, wire transfer, Western Union, and Alipay. And there is **no general money-back guarantee**:

> HostAdvice's review notes that all payments are non-refundable, with the only exception being billing disputes raised within 30 days that Sharktech agrees with — and those are settled as account credit, not cash back. If you're evaluating, use the hourly billing to test cheaply before committing to a large monthly plan.

There's also a cost calculator in the portal that lets you assemble VMs, storage, and bandwidth and see the resulting hourly and monthly totals before you spend anything — worth five minutes if you're unsure which tier fits. 👉 try Sharktech's cloud cost calculator before you commit to a tier

## Public vs Dedicated Cloud: same platform, different bill

The two lines run on identical infrastructure and the same control panel — Kubernetes cluster creation, load balancers, private networks, security groups, floating IPs, VPN, and snapshot scheduling are all in both. The difference is purely the billing model, and Sharktech's own documentation lays out the arithmetic.

Public Cloud: `[included resources] × fixed monthly fee + [extra consumption] × hourly rate = monthly bill`. Say you're on the Large plan ($249 base, 32 cores / 64GB RAM / 1500GB SSD included) and you run six VMs that together consume 48 cores and 96GB RAM around the clock. The overage is 16 cores and 32GB of RAM, so:

$$16 \times \$0.0025 \times 720 + 32 \times \$0.0035 \times 720 = \$28.80 + \$80.64 = \$109.44$$

Total: about **$358/month**, with the SSD storage staying inside the included pool. If those VMs don't run 24/7, the overage shrinks accordingly — you're billed on actual consumption.

Dedicated Cloud: `[fixed resources] × flat monthly fee`, full stop. You prepay for exactly what you ordered — pay for 8 cores, get 8 cores — and the invoice never moves unless you change the plan. It's the model for teams that need to forecast cloud spend to the dollar, and it starts at $86.23/month for an 8 vCPU / 16GB configuration scaling up to 512 vCPU and 1024GB.

The rule of thumb: unpredictable or spiky workloads fit Public Cloud's overage model; steady-state production workloads often cost less overall on Dedicated Cloud's flat rate. If you genuinely can't tell, 👉 book Sharktech's free cloud consultation and let them size it with you

## When you should just stay with a hyperscaler

Fairness requires the counterpoint. Stay with AWS, Azure, or GCP if:

- You need managed ML/AI services, serverless ecosystems, or one of the hundreds of specialized managed services that only exist at hyperscale — the open-source equivalents are DIY.
- Your users are genuinely global and you need presence in dozens of regions. Five data centers can't do that.
- You're already deep in a proprietary stack and migration cost exceeds the savings. Lock-in is expensive to break, and pretending otherwise doesn't help.

The mid-sized provider wins when the workload is straightforward IaaS — web apps, databases, game servers, CDNs, staging environments, backups — and the bill is being driven by data transfer, unpredictable metered pricing, or both. That describes a lot of small and mid-sized projects.

## A five-minute checklist before you commit anywhere

1. **Estimate your egress.** Average monthly outbound GB × the provider's per-GB rate. This single line item disqualifies more providers than any feature list.
2. **Get a real monthly number.** Use the provider's calculator or a pricing sheet to build your actual workload, not a marketing "starting at" price.
3. **Check the exit cost.** Can you export VM images and data in open formats? What's the contractual minimum term?
4. **Read the SLA and refund policy.** Uptime percentage, payout terms, and whether "non-refundable" appears in bold.
5. **Test support before you need it.** Open a pre-sales ticket at an odd hour and see what comes back.

## Quick answers to common questions

**Is the cheapest cloud service provider the best deal?** No — the sticker price on compute is almost never where the money goes. Egress, overage rates, idle-resource charges, and lock-in costs decide the real total. A provider with slightly higher compute pricing and near-zero egress can cost half as much in practice for anything that moves data.

**Can I switch cloud providers later?** Yes, if you plan for it. Deploy on open platforms (OpenStack, plain Linux VMs, S3-compatible storage), keep your data exportable, and avoid proprietary managed services at the core of your architecture. Platforms that let you download your own disk images — as Sharktech does — make this a file transfer instead of a rebuild.

**Is a smaller provider riskier?** Different risks, not necessarily bigger ones. You trade the hyperscaler's global redundancy and ecosystem depth for things like phone-accessible 24/7 support, flat pricing, and a network with DDoS protection built in. For a US or EU-focused workload, the trade frequently favors the smaller operator. For a global platform with exotic regional needs, it doesn't.

The short version of all this: pick the billing model you can forecast, verify the egress rate before you upload anything, and choose infrastructure you can leave. Do those three and even a "wrong" pick is cheap to fix. 👉 compare Sharktech's cloud plans against your current bill and see where you land
