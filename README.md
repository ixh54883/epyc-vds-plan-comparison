# greencloud epyc vds: Dedicated EPYC Cores From $10/Month — Full Plan Comparison, APAC Traffic Limits, and the Fine Print Before You Order

GreenCloud's EPYC NVMe KVM VDS is a virtual **dedicated** server line that reserves 100% of a physical AMD EPYC core for your VM — Milan (Gen 3) silicon on NVMe 4.0 storage in RAID-10 — starting at **$10/month** for 1 dedicated core, 8GB RAM, 80GB NVMe, 4TB traffic and a 10Gbps port. If you searched for this product, odds are you're fighting CPU steal or throttled disk I/O on a shared VPS, and this line is built to end exactly that. But at $10–$80/month it's also GreenCloud's most expensive product family, and there are a few catches buried in the order flow and terms of service that the product page doesn't spell out. All five plans, the APAC traffic quirk, per-location quirks, and the refund rules are covered below.

> **Quick pick:** For most people, the entry **EPYCVDS-1 at $10/month** (1 dedicated EPYC Milan core, 8GB RAM, 80GB NVMe, 4TB traffic) is the right starting point — one uncontended Milan core already outperforms a "2 cores" shared plan under load. Running a database, game server, or compile-heavy workload? Start at EPYCVDS-2 or -3 instead.
> 👉 [Check current EPYC VDS plans and live stock](https://greencloudvps.com/billing/aff.php?aff=6679)

## What "dedicated core" actually buys you

The difference is in the name: VDS, not VPS. On a regular KVM VPS, several customers share the same physical cores, so your "4 cores" is a best-effort allocation that degrades when neighbors spike. On this line, GreenCloud's official page states it flatly: you get 100% physical CPU cores, RAM, and RAID-10 NVMe 4.0, with no contention.

Three practical consequences follow from that:

- **Predictable CPU.** Steal time stays at essentially zero because nobody else is scheduled on your cores. This is what matters for databases, game servers, CI runners, and anything latency-sensitive.
- **Fast, consistent disk.** NVMe 4.0 in RAID-10 on every tier. Self-reported benchmarks that GreenCloud customers have posted on LowEndTalk for the company's NVMe nodes show roughly 290 MB/s at 4K random read/write and multi-GB/s sequential throughput — a different class of storage performance than budget SSD lines deliver.
- **A fat network port.** 10Gbps on every plan, with 1 IPv4 and a /64 IPv6 allocation (one location excepted, covered below).

Every plan ships with Linux OS only, the VirtFusion control panel, and 1 free backup/snapshot slot. There is no Windows option here — GreenCloud sells separate Windows VPS lines starting at $8/month, but the EPYC VDS line is Linux, full stop. Support is 24/7/365, though read the terms section near the end before assuming that means they'll fix your nginx config. They won't.

## Full plan and price table

Five tiers, currently listed on the official product page. Prices are flat monthly USD, identical for US/EU and APAC locations — only the traffic allowance changes. Each step up adds exactly one dedicated core, 8GB RAM, 80GB NVMe, and 4TB (US/EU) of traffic:

| Plan | Dedicated cores | RAM / NVMe 4.0 | Traffic (US/EU / APAC) | Price/mo | Order |
| --- | --- | --- | --- | --- | --- |
| EPYCVDS-1 | 1 × EPYC Milan | 8GB / 80GB | 4TB / 2TB | **$10** | [ 订购 EPYCVDS-1](https://greencloudvps.com/billing/aff.php?aff=6679) |
| EPYCVDS-2 | 2 × EPYC Milan | 16GB / 160GB | 8TB / 4TB | **$20** | [ 订购 EPYCVDS-2](https://greencloudvps.com/billing/aff.php?aff=6679) |
| EPYCVDS-3 | 3 × EPYC Milan | 24GB / 240GB | 12TB / 6TB | **$30** | [ 订购 EPYCVDS-3](https://greencloudvps.com/billing/aff.php?aff=6679) |
| EPYCVDS-4 | 4 × EPYC Milan | 32GB / 320GB | 16TB / 8TB | **$40** | [ 订购 EPYCVDS-4](https://greencloudvps.com/billing/aff.php?aff=6679) |
| EPYCVDS-5 | 8 × EPYC Milan | 64GB / 640GB | 16TB / 8TB | **$80** | [ 订购 EPYCVDS-5](https://greencloudvps.com/billing/aff.php?aff=6679) |

Every tier includes the same baseline: 10Gbps port, 1 IPv4, Linux OS, VirtFusion, 1 free backup/snapshot, 24/7/365 support. GreenCloud has operated its own hardware since 2013, runs 30+ locations across four continents, and was voted LowEndTalk's #1 Top Provider and #1 Most Stable Provider in 2024 — relevant background when you're putting production workloads on a $10 box.

One structural limitation to note before picking a tier: since every step adds 1 core + 8GB together, you can't buy more RAM without more cores. A memory-hungry but lightly-threaded app still pays $20 at EPYCVDS-2, partly for cores it won't use. The workarounds are listed in the "who doesn't need this" section below.

## The APAC traffic catch most reviews skip

Same price, half the traffic — that's the detail most coverage of this product leaves out. The APAC variants of all five plans (Singapore, Japan, Vietnam, Hong Kong) come with 2TB / 4TB / 6TB / 8TB / 8TB of monthly traffic instead of the 4TB / 8TB / 12TB / 16TB / 16TB you get in the US and Europe. On EPYCVDS-5, that's a 50% cut at the same $80.

This matters most when you overspend on traffic later: GreenCloud's terms price extra KVM bandwidth at roughly $3–4 per TB per month in the US/EU, but **$15 per TB in APAC**. A database replica pulling a few extra terabytes monthly in Singapore costs about five times what it would in Ashburn. The rule is straightforward: if latency to Asian users isn't the point, order US/EU and keep the full quota; pick APAC only when low latency is the actual product requirement — a proxy endpoint, a game gateway, a CDN origin serving regional users.

One Asia-specific caveat from the terms themselves: bandwidth to China is provided on a best-effort basis unless a plan explicitly says otherwise, and GreenCloud recommends testing with their provided test IPs before buying. If your use case depends on China reachability, don't skip that step.

## Location choice: check stock, and watch the Kansas City IPv6 quirk

EPYC VDS inventory is tracked per location and moves daily, so any list in a review is a snapshot. At the time of checking the official order page, San Jose had stock on tiers 1–4, Kansas City had 10 units of every tier, Ogden had one or two units left, and Staten Island (NY), Los Angeles, Ashburn (VA), and Chicago DC2 were sold out across the board. Nodes in LA and Ogden run Genoa (Gen 4); the rest are Milan. Both are fast, but Genoa is the newer silicon if you're choosing between two in-stock locations.

There's one quirk hidden in the Kansas City listings: IPv6 there is a **/112 allocation instead of the /64** you get everywhere else. A /64 gives you effectively unlimited addresses for a single server; a /112 caps you at 65,536. For a machine with a handful of IPv6 addresses that changes nothing. For IPv6-heavy setups — mail servers doing per-address outbound, proxy pools — it's a real ceiling, and San Jose or another /64 location is the better call.

Because availability shifts weekly, confirm live stock before committing to a datacenter:

👉 [See which EPYC VDS locations have units available right now](https://greencloudvps.com/billing/aff.php?aff=6679)

If you pick wrong, it's fixable: the terms allow a one-time location change for $3, with EU/US orders movable between any locations in the region and APAC orders restricted to the same country.

## Who doesn't need the EPYC VDS at all

The honest test for this product is whether shared cores actually hurt you. GreenCloud's own lineup gives you two cheaper ways to find out:

- **Budget KVM VPS, from $6/month**: shared cores on RAID-10 enterprise drives, 10Gbps port, 1 free backup. A longtime LowEndTalk user reports running Mastodon, Nextcloud, Redis, and several Rails apps on a ~$45/year Budget KVM plan with CPU steal at zero. If your workload looks like that, the EPYC line's premium buys you nothing you'd notice.
- **Ryzen KVM VPS, from $6/month**: shared AM4/AM5 Ryzen cores with NVMe storage. A middle ground for CPU-heavy apps and fast databases — newer consumer silicon and NVMe speed, but still shared cores.

The EPYC VDS earns its price when neighbors genuinely hurt: production databases under sustained load, game servers, compile farms — anything where performance swings from neighbor activity cost you more than the $4/month difference. Buy the cheapest line that doesn't show steal time in `top`; upgrade to this one when it does.

## Refunds, billing, and the fine print

The refund window exists, but it's narrower than "7-day money-back" sounds. Per GreenCloud's terms of service, the refund applies to a **new client's first VPS plan within 7 days of account registration** — and several exclusions apply:

- Crypto payments are non-refundable.
- Plans that have used 10% or more of their allocated bandwidth are not eligible.
- Promotional and flash-sale plans are excluded, as are add-ons like cPanel or DirectAdmin.
- Extra orders beyond the first service don't qualify.

Billing runs on a 14-day advance invoice; accounts 2 days overdue pick up a 10% late fee, and service can be suspended after 1 day and terminated after 3. Opening a PayPal or credit card chargeback suspends or terminates all your services — dispute through a ticket instead.

There's also a real dispute on record worth knowing about. A LowEndTalk thread from late 2025 describes a customer who requested cancellation of a 3-year promotional plan after service failures: GreenCloud approved the cancellation, terminated the service, and then declined to refund the remaining balance, citing the "promotional plans are non-refundable" clause — with support responding that the policy was stated on the order page and in the TOS. The thread was closed without resolution. Two takeaways: pay monthly rather than multi-year upfront unless you're confident in the plan, and don't assume cancellation plus refund come as a package.

Also worth knowing before you file your first ticket: this is an **unmanaged** service. The terms state support covers hardware and server functionality, with software support limited to the default installation you picked at order time. Configuration problems and misconfiguration downtime are on you. GreenCloud's docs site carries 700+ guides (Linux, Windows, VirtFusion, cPanel) if you need to self-serve.

Payments go through cards, PayPal, Alipay, UnionPay, crypto, Perfect Money, and bank transfer. A canceled VPS that doesn't qualify for a refund can be rebuilt later for a $5 fee if you change your mind.

## FAQ

**Does greencloud epyc vds support Windows?**
No — this line is Linux-only. GreenCloud sells separate Windows VPS products from $8/month, but those are shared-core machines.

**Is the EPYC core really 100% dedicated?**
Yes. The official product page and order listings both specify dedicated physical EPYC cores, dedicated RAM, and RAID-10 NVMe 4.0 per VM. This is the core difference between this line and the Budget/Ryzen VPS lines.

**What's the cheapest plan and what do I get?**
EPYCVDS-1 at $10/month: 1 dedicated EPYC Milan core, 8GB RAM, 80GB NVMe, 4TB traffic (2TB in APAC), 10Gbps port, 1 IPv4, /64 IPv6 — Kansas City being the exception with /112.

**Can I add more traffic later?**
Yes. The terms list extra KVM bandwidth at a few dollars per TB monthly in the US/EU and $15/TB in APAC — which is why location choice matters if you expect to overspend.

**What control panel do I get?**
VirtFusion, with 1 free backup/snapshot included. GreenCloud's docs have step-by-step VirtFusion guides covering reinstalls, snapshots, rDNS, and network settings.

**What's the uptime record like?**
GreenCloud was voted LowEndTalk's #1 Most Stable Provider in 2024, and user-posted benchmarks and long-uptime reports (67+ days on EPYC nodes, months-long Docker stacks without reboots) back that up anecdotally. There's no published SLA percentage in the materials reviewed here, so don't budget around a specific uptime guarantee.

## Which plan to order

Matching the common cases:

- **Replacing a laggy shared VPS for a website or API** → EPYCVDS-1, $10/month. One uncontended core plus NVMe is more headroom than most single-app workloads touch.
- **Database, game server, or CI/compile work** → EPYCVDS-2 or -3. Extra dedicated cores get genuinely used, and 160–240GB of NVMe fits realistic datasets.
- **Memory-heavy but lightly-threaded app** → check the Budget or Ryzen lines first; at this line's fixed 1-core-per-8GB ratio you'd be paying for cores you can't use.
- **Asian user base** → APAC location if latency is the priority and half the traffic quota works for you; otherwise US/EU with the full 4TB+.
- **Need a large IPv6 block** → skip Kansas City (/112); pick a /64 location.

Two things to confirm before checkout: live stock at your chosen location, and that you're okay with the refund exclusions above — pay monthly, skip crypto for the first payment, and keep bandwidth usage under 10% until you're sure the plan fits. Stock and location options change weekly, so pick from what's actually available:

👉 [Order your EPYC VDS plan on GreenCloud](https://greencloudvps.com/billing/aff.php?aff=6679) — and if you're torn between two tiers, start lower; an unused dedicated core is just $10 a month of waste you chose in advance.
