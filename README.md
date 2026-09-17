# Cloud Storage Service: Sort Out Object, Block, and Backup Storage, Then Pick a Plan That Fits Your Budget

Search "cloud storage service" and you'll mostly find lists of consumer apps — Google Drive, Dropbox, OneDrive, pCloud — compared on sync features and free-tier sizes. That's fine if you want a folder that follows you across devices. But a large chunk of people typing this phrase are trying to solve a different problem: where to put backups, archives, application data, or a growing pile of media files without getting killed on egress fees or per-GB pricing.

This article covers both sides of that split. First, how to figure out which *type* of cloud storage you actually need — because "storage" means at least three different things in the cloud world, and picking the wrong one costs real money. Then, a concrete look at what one provider, Sharktech, currently charges for each storage type, with prices pulled from its live order pages, so you have real numbers to benchmark against.

## Three Different Things People Call "Cloud Storage Service"

Before comparing providers, it helps to know that the phrase maps onto three distinct products:

**1. File sync and share.** Dropbox, OneDrive, Google Drive. Desktop apps, mobile apps, version history, shared links. Built for people working with documents day to day. priced per user, usually a few hundred GB to a few TB.

**2. Object storage.** S3-style storage built for machines rather than humans. You (or your application) read and write "objects" through an API. It's where you put backups, archives, log files, images served by a website, or build artifacts from a CI pipeline. Almost every serious backup tool and DevOps toolchain speaks the S3 API, which is why it became the de facto standard.

**3. Block storage.** The disks attached to cloud servers — the volumes your VMs boot from and write databases to. When a cloud provider offers NVMe, SSD, and HDD tiers, this is what they're talking about. Faster tiers cost more per GB; slower tiers make sense for large, rarely-touched data.

If you search "cloud storage service" because you want a place to keep your family photos, category 1 covers it and most comparison articles will serve you well. If you're backing up servers, archiving compliance data, or running applications, you're shopping in categories 2 and 3 — and that's where pricing structures (per-TB flat rates vs. per-GB with egress fees) start to matter a lot.

## What Actually Determines Your Cloud Storage Bill

The sticker price per GB is the least of it. When you evaluate any cloud storage service, four factors decide what you'll really pay:

**Egress fees.** Many large providers charge for data leaving their platform. Move a 10 TB archive out and you can rack up hundreds of dollars in "download" fees — and those same fees make it expensive to switch providers later, which is a form of lock-in that has nothing to do with contracts. Sharktech's approach here is worth noting: incoming traffic is free, cloud services include 5,000 GB of outgoing bandwidth per month, and additional outbound data is billed at a flat $0.002 per GB. At that rate, pulling a full terabyte out costs about $2 — dramatically cheaper than typical hyperscaler egress pricing.

**Storage tier.** NVMe for databases and hot data, SSD for general workloads, HDD for archives. Sharktech publishes estimated per-volume performance numbers: NVMe at roughly 1.2 GB/s and 18,000 IOPS, SSD at 350 MB/s and 6,000 IOPS, and HDD at 120 MB/s and 3,000 IOPS. The gap between tiers is large enough that matching the tier to the workload is one of the biggest cost levers you have.

**Pricing model.** Flat per-TB rates are predictable. Utility pricing (per-hour, per-resource) is flexible but requires monitoring. Contract requirements — some providers only give you their best object-storage rates if you commit to storing tens of terabytes — penalize small users.

**Refund policy.** Easy to overlook until you need it. Sharktech does not offer a general money-back guarantee; payments are non-refundable, with the exception of billing disputes raised within 30 days, which result in account credit rather than a refund. If you're evaluating them, it's worth starting small before committing to a large allocation.

## When a Consumer Cloud Storage Service Stops Being Enough

Consumer sync apps are genuinely good at their job. Where they fall short:

- **API-driven workflows.** Backup software, deployment pipelines, and data tools speak S3, not Dropbox's sync protocol.
- **Scale and cost.** Per-user pricing gets expensive fast once you're storing tens of terabytes of archives rather than documents.
- **Predictable machine access.** Object storage gives you buckets, keys, and programmatic access patterns that applications can rely on.

The practical pattern a lot of small teams land on: a sync service for active documents, object storage for backups and archives, and block storage (via cloud servers) for anything an application touches. You can see all three priced side by side below.

## S3 Object Storage: The Cheapest Way to Hold Large Amounts of Data

Object storage is the category to look at first if your data is "write once, read occasionally" — backups, media libraries, archives, artifacts. Sharktech sells S3-compatible object storage with a deliberately simple invoice: storage and bandwidth are the only line items, no API charges or term contracts.

The headline rate is **$4.90 per TB**, and the entry bundle on their S3 product page is 1 TB of storage plus 1 TB of bandwidth for $4.90/month. Their order portal lists S3 packages starting from $6.00/month, with allocations scalable from 1 TB up toward 1 PB, selectable across Los Angeles, Las Vegas, Denver, Chicago, and Amsterdam. The S3 API compatibility means standard tools — backup clients, rclone, Jenkins, Terraform — connect without custom work.

For context: that $4.90/TB works out to roughly $0.0048 per GB per month, which undercuts the standard-tier rates at the big hyperscalers, before you even factor in their egress fees.

If you're weighing this route, you can 👉 check Sharktech's S3 object storage plans and current rates directly on their order page.

## Cloud Backup: Fixed Price, Ransomware-Resistant Copies

A related but distinct product: managed backup with its own storage attached. Sharktech's Acronis Cloud Backup starts at **$4.00/month for 200 GB** of cloud backup and protection, with additional storage at **$0.02 per GB**, and a Files Sync & Share component that scales up to 100 TB. During cloud-server checkout it also appears as an optional add-on at $4.00/month.

The distinction from raw object storage matters: a backup service handles scheduling, retention, and — in Acronis's case — anti-ransomware features, whereas S3 storage is just a destination your backup software writes to. If you don't want to manage backup tooling yourself, the managed route at $4/month entry is one of the cheaper ways in. If you already run Veeam, restic, or similar, plain S3 storage at $4.90/TB is usually the better math once you pass a couple of terabytes.

👉 View Acronis Cloud Backup plans and pricing if a fixed-fee managed backup sounds like what you need.

## Block Storage: Choosing Between NVMe, SSD, and HDD

If your storage is attached to running workloads, tier selection is the whole game. Sharktech's OpenStack-based cloud lets you allocate compute and storage from a shared pool — you're not locked into fixed VM presets, and you can split an allocation like 8 vCPUs / 8 GB RAM / 300 GB SSD across multiple VMs however you like.

A third-party review at HostAdvice, which ran benchmarks on the platform, found the difference between tiers was stark in practice: sequential reads on the NVMe layer measured around 5,020 MB/s — territory that competes with hyperscalers — while the default SSD tier behaved like conventional SSD storage, fine for websites and small databases but a bottleneck for heavy I/O work. The same review measured roughly 10 Gbps network throughput on a test VM with 0.17 ms internal latency, and support ticket replies arriving in about 39 minutes during off-peak hours. HostAdvice's overall score for the service was 9.4/10, with "limited number of global regions" listed as the main drawback. On the consumer-review side, Sharktech's Trustpilot rating sits at 3.5/5, though from a small sample of about 13 reviews — not much to draw conclusions from either way.

The official uptime commitment is 99.999%, and the platform includes DDoS protection, private networking, load balancers, Kubernetes support, and free native VPN for bridging to on-premises infrastructure.

## Every Storage-Relevant Plan, Side by Side

Here is the full set of storage-bearing plans currently listed on Sharktech's order portal, with pricing as displayed at the time of writing. Public Cloud tiers are shown for the Los Angeles location; other data centers are selectable at checkout.

| Plan | What it is | Storage configuration | Starting price | Billing | Link |
| --- | --- | --- | --- | --- | --- |
| Object Storage (S3) | S3-compatible object storage for backups, archives, app data | Scalable from 1 TB toward 1 PB; flat $4.90/TB rate; 1 TB bandwidth included in entry bundle | From $6.00/mo (portal); $4.90/mo for 1 TB + 1 TB bundle | Monthly | [Get S3 Object Storage](https://portal.sharktech.net/cart.php?a=add&pid=643&carttpl=s3_storage_cart&billingcycle=monthly&configoption[1858]=13673&configoption[1859]=1&aff=1611) |
| Acronis Cloud Backup | Managed backup with anti-ransomware protection | 200 GB included, expandable to 100 TB; +$0.02/GB beyond included | From $4.00/mo | Monthly | [Get Acronis Cloud Backup](https://portal.sharktech.net/cart.php?a=add&pid=648&aff=1611) |
| Public Cloud – Small | Pay-as-you-go cloud resource pool | 4–16 vCPU, 8–32 GB RAM, SSD 300–2400 GB, HDD up to 4800 GB, NVMe up to 1200 GB, 20 TB+ bandwidth | From $39.00/mo | Monthly + hourly overage | [Order Public Cloud Small](https://portal.sharktech.net/index.php?rp=/store/public-cloud-hosting/public-cloud-hosting-los-angeles-small&aff=1611) |
| Public Cloud – Medium | Same platform, larger resource commit | 8–32 vCPU, 16–64 GB RAM, SSD 800–6400 GB, HDD up to 12800 GB, NVMe up to 3200 GB | From $79.00/mo | Monthly + hourly overage | [Order Public Cloud Medium](https://portal.sharktech.net/index.php?rp=/store/public-cloud-hosting/public-cloud-hosting-los-angeles-medium&aff=1611) |
| Public Cloud – Large | For heavier multi-VM workloads | 32–128 vCPU, 64–256 GB RAM, SSD 1500–12000 GB, HDD up to 24000 GB, NVMe up to 6000 GB | From $249.00/mo | Monthly + hourly overage | [Order Public Cloud Large](https://portal.sharktech.net/index.php?rp=/store/public-cloud-hosting/public-cloud-hosting-los-angeles-large&aff=1611) |
| Public Cloud – Enterprise | Uncapped resource ceiling | 64+ vCPU, 128+ GB RAM, SSD 5000+ GB, HDD/NVMe uncapped, 20 TB+ bandwidth | From $499.00/mo | Monthly + hourly overage | [Order Public Cloud Enterprise](https://portal.sharktech.net/index.php?rp=/store/public-cloud-hosting/public-cloud-hosting-los-angeles-enterprise&aff=1611) |
| Dedicated Cloud | Fixed monthly billing on the same infrastructure | 8–512 vCPU, 16–1024 GB RAM, SSD/HDD/NVMe tiers, 5–300 TB transfer | From $86.23/mo | Fixed monthly | [Order Dedicated Cloud](https://portal.sharktech.net/index.php?rp=/store/dedicated-public-cloud-bare-metal/dedicated-cloud&aff=1611) |

A few notes that affect the real bill:

- Overage on Public Cloud is metered hourly: CPU at $0.0025/hr per core, RAM at $0.0035/hr per GB, NVMe at $0.00009/hr per GB, SSD at $0.00006/hr per GB, and HDD at $0.00002/hr per GB. Non-Enterprise Public Cloud plans carry a resource cap so a misconfigured autoscaling group can't quietly run up an enormous invoice.
- Each cloud service includes one free public IPv4 address; additional ones are $1.50/month each.
- Incoming bandwidth is unlimited; outgoing beyond the included 5,000 GB is $0.002/GB.
- Sharktech claims 50–80% cost savings versus hyperscalers on its pricing page and "at least 40%" in its FAQ — treat these as the provider's own marketing math, but the flat-rate structure and near-zero egress fees are the verifiable reasons behind the claim.

## Which Cloud Storage Service Fits Which Situation

Rather than a verdict, some matched recommendations based on the verified numbers above:

**Personal files and documents.** Stay with a consumer sync app. Nothing in the table above replaces Dropbox-style folder sync, and Sharktech isn't trying to.

**Backups of a few servers or workstations.** Acronis Cloud Backup at $4.00/mo for 200 GB is the lowest-friction entry. At about 250 GB of data, the math tips: beyond that, 200 GB at $4 plus $0.02/GB grows faster than S3 at $4.90/TB, so heavy backup volumes belong on object storage with your own backup tool.

**Large archives, media libraries, compliance data.** S3 object storage at $4.90/TB is the clear play — flat rate, no commitments, and cheap egress means you can actually leave with your data if you want to.

**Application data on cloud servers.** Public Cloud Small at $39/mo handles a modest production VM plus storage; pair HDD storage with cold data, NVMe with databases. Dedicated Cloud at $86.23/mo makes sense once you want a predictable fixed bill instead of metered usage.

**Trying before committing.** Hourly billing on Public Cloud means a test VM can cost cents, and you can download your disk images at any time to migrate away — the no-vendor-lock-in positioning is backed by a real export capability, not just messaging. Just remember the no-refund policy: disputes within 30 days yield credit, not cash back.

Payment options, per the HostAdvice review, include credit cards, PayPal, wire transfers, Western Union, and Alipay — unusually flexible for a US-based infrastructure provider.

## Common Questions

**Is object storage slower than a regular drive?** For its intended use, no. Sharktech's S3 clusters sit on 40G connectivity, and access speeds for backups and media are limited by your own network, not the storage. But it's not a substitute for block storage attached to a database — different tools for different jobs.

**What's the fastest storage tier?** NVMe, at roughly 1.2 GB/s and 18,000 IOPS per volume. It's also the most expensive tier, which is why the standard advice is to put only hot data on it.

**Does cheap storage mean contract lock-in?** Not here, at least. No term contracts on the S3 product, and no egress wall preventing you from moving data out. The lock-in risk with big providers usually comes from egress pricing, not the storage rate itself.

**Can I run Windows and Linux VMs on the block storage plans?** Yes — both are supported, with weekly-updated official cloud images, and you can upload your own ISOs or disk images.

## The Short Version

The "cloud storage service" question resolves into three purchases: sync for people, object storage for archives and backups, block storage for applications. Match the category first, then compare per-TB rates, egress fees, and refund policies within it. On those criteria, Sharktech's lineup — $4.90/TB S3 storage, a $4.00/mo managed backup entry point, and tiered NVMe/SSD/HDD block storage from $39/mo — is worth a look, particularly if egress fees and vendor lock-in have burned you before. Just go in knowing the refund policy is strict, so size your first purchase conservatively.

If you want to explore the options hands-on, 👉 browse all of Sharktech's cloud storage and hosting plans to compare configurations and current pricing yourself.
