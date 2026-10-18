# h100 rental: current hourly rates compared, when renting beats a monthly server, and what actually fits in 80 GB

An H100 rents for anywhere from about $2.29 to $6.98 per GPU-hour right now, and that spread isn't random — the cheap numbers usually mean spot capacity, marketplace hosts, or PCIe cards, while the high ones are hyperscalers and reservation-only clouds. Vultr sits near the middle at **$3.49/hour** for a 1× H100 with 80 GB of VRAM, billed hourly with no commitment: deploy it, run the job, destroy the instance, stop paying. Whether that's the cheapest route to H100 time comes down to one number — how many hours a month you actually need — and this article works that out, along with a current price snapshot, the break-even math against monthly servers, and a realistic look at what 80 GB holds.

> **Quick take:** if your H100 work is measured in hours or days — experiments, burst inference, a weekend fine-tune — hourly rental wins, and Vultr's on-demand 1× H100 is the simplest first-party option. If you'll pin the GPU 24/7 for months, dedicated monthly servers at $1,249–$1,690 cost $860–$1,300 less per month than 24/7 hourly. [👉 check Vultr's current H100 hourly pricing](https://www.vultr.com/pricing/?ref=7268283) before you plan the run.

## What an H100 costs to rent right now

Across providers with live H100 listings, on-demand-equivalent rates run from $2.29/hour at the low end to $6.98 at the high end, and a market index covering 30–40 providers puts the on-demand median near $3.35/hour. Committed monthly bare metal is a separate market: dedicated-server vendors currently advertise $1,249 and $1,690 per month for an H100 SXM — roughly $1.71 and $2.35 per GPU-hour if you use every hour of every day.

| Provider | What you're buying | Published H100 rate | Worth knowing |
| --- | --- | --- | --- |
| Theta EdgeCloud | decentralized marketplace | $2.29/h | cheapest live listing at check time |
| Vast.ai | marketplace, individual hosts | $0.51–$1.74/h | range spans spot to on-demand; hardware varies by host |
| Spheron | decentralized | $2.75/h dedicated · $2.10/h spot | spot is reclaimable at any time |
| RunPod | cloud + marketplace | $2.69/h community · $3.49/h secure | community tier runs on third-party hosts |
| Google Cloud | hyperscaler | $3.00/h | — |
| AWS | hyperscaler | $3.90/h | — |
| Lambda | GPU cloud | $3.99–$4.29/h | — |
| CoreWeave | reservation only | $6.16/h per GPU | no on-demand tier for this card |
| Azure | hyperscaler | $6.98/h | — |
| GPU Rent Hub | dedicated monthly | $1,690/mo per H100 SXM | crypto payment only |
| GPU Cloud HQ | dedicated monthly | $1,249/mo per H100 SXM | launch rate; $1,439 from Nov 1 |

Rates are each provider's own published figures, checked from their pricing pages between late September and early October 2026. All prices in USD; GPU pricing moves often, so treat this as a snapshot rather than a guarantee.

Two caveats before comparing rows. First, a published rate is not the same as available capacity — H100 stock is tight industry-wide, and several providers note low availability on this card. Second, the "h100 rental" search results mix together two genuinely different products: hourly on-demand capacity and committed monthly bare metal. The sections below unpack those differences, because that's where the real price lives.

## Vultr's H100 plans and pricing

Vultr rents the H100 as cloud GPU instances billed by the hour. The current lineup is 1×, 2× and 4× H100 at $3.49 per GPU-hour, so a 2× instance runs $6.98/hour and a 4× runs $13.96/hour. There's no contract and no minimum term — the rate applies for every hour the instance exists, and destroying it ends the billing. The full lineup sits on the Cloud GPU product page: [👉 see Vultr's full Cloud GPU lineup](https://www.vultr.com/products/cloud-gpu/?ref=7268283).

| Plan | GPU memory | Hourly rate | 24/7 month (730 h) | Deploy |
| --- | --- | --- | --- | --- |
| 1× H100 | 80 GB | $3.49 | ≈ $2,548 | [ Deploy 1× H100](https://www.vultr.com/products/cloud-gpu/?ref=7268283) |
| 2× H100 | 160 GB | $6.98 | ≈ $5,095 | [ Deploy 2× H100](https://www.vultr.com/products/cloud-gpu/?ref=7268283) |
| 4× H100 | 320 GB | $13.96 | ≈ $10,191 | [ Deploy 4× H100](https://www.vultr.com/products/cloud-gpu/?ref=7268283) |
| 8× H100 (bare metal) | 640 GB, HGX board with NVSwitch | per-server rate on the bare-metal page | — | [ See bare metal GPU servers](https://www.vultr.com/products/bare-metal/?ref=7268283) |

The bare-metal row is a different product: a complete dedicated server built around an HGX board, aimed at tensor-parallel training across a node, where the NVSwitch interconnect does the heavy lifting.

A few practical notes on the hourly instances. The meter runs while the instance exists, busy or not, so an idle GPU is the classic money leak — destroy it between jobs instead of leaving it parked. Deployment works like any Vultr cloud server: pick a region, an image and an SSH key, and you get root on a machine with the GPU attached. Vultr operates data center regions across North America, Europe, Asia-Pacific and South America, and takes credit cards and PayPal — which matters if you'd rather not pay a dedicated-server vendor in crypto, as one of the monthly options requires.

## The break-even math: hourly rental vs a monthly server

Below roughly **360 hours a month** — about 12 hours a day — Vultr's hourly rate is the cheaper way to hold an H100. Above it, a dedicated monthly server wins, and the gap widens fast.

The benchmark: the cheapest dedicated H100 SXM server currently advertised is $1,249/month, a launch rate scheduled to end October 31, after which it rises to $1,439. Divided by Vultr's $3.49 hourly rate, $1,249 buys about 358 hours — call it 12 hours a day, every single day. The second dedicated vendor, at $1,690/month, moves the break-even to about 484 hours, roughly 16 hours a day.

| Usage pattern | Hours/month | Vultr 1× H100 cost | Cheapest dedicated monthly | Better value |
| --- | --- | --- | --- | --- |
| Dev and eval, ~3 h/day | ~90 | $314 | $1,249 | hourly |
| Weekday training, 8 h/day | ~176 | $614 | $1,249 | hourly |
| Heavy fine-tune, 16 h/day | ~480 | $1,675 | $1,249 | monthly, but close |
| 24/7 | 730 | $2,548 | $1,249 | monthly, by $1,299 |

The middle rows are where people overpay in both directions. If training runs overnight and on weekends, add the hours up honestly — 12 hours a day is heavy utilization for a development GPU, and most teams don't get near it. Hourly also keeps optionality a fixed server can't offer: scale up to a 2× or 4× instance for one big run, then drop back to a single card, without renegotiating anything.

## Why the cheapest listings are usually spot (and when that's fine)

The lowest H100 prices advertised anywhere — from $0.51 up to about $2.15/hour — are almost always spot or preemptible capacity, which the provider can reclaim at any time.

The specifics: Vast.ai's range spans $0.51–$1.74 depending on the host; Verda lists spot at $1.67; Spheron's $2.10 spot is, in its own wording, reclaimable at any time; Nebius's preemptible tier sits at $2.15 and Together AI's at $1.99. These are real rates, and for the right job they're perfectly usable. A batch inference sweep that can restart from scratch, or a training run with aggressive checkpointing, doesn't care if the GPU vanishes at hour nine. An uncheckpointed 12-hour fine-tune very much does.

On-demand is a different product: the instance stays yours until you release it, at a published rate that doesn't move mid-run. That's the class Vultr's $3.49 sits in, alongside RunPod Secure Cloud at the same $3.49 and Lambda at $3.99–$4.29. When you compare listings, sort them into spot and on-demand first, then compare within each class — mixing the two is how "cheap H100" comparisons end up weighing a lottery ticket against a lease.

## PCIe or SXM: check which card is behind the rate

At the same "H100" label, a PCIe card and an HGX SXM board differ by about a third in raw throughput — enough to explain most of the gaps between listings that look comparable.

|  | H100 PCIe | H100 SXM (HGX) |
| --- | --- | --- |
| Memory | 80 GB HBM2e | 80 GB HBM3 |
| Bandwidth | 2.0 TB/s | 3.35 TB/s |
| FP8 tensor (dense) | 1,513 TFLOPS | 1,979 TFLOPS |
| GPU-to-GPU | 600 GB/s bridge to one neighbor | 900 GB/s per GPU via NVSwitch |
| Power | 350 W | up to 700 W |

NVIDIA's own H100 whitepaper puts a single PCIe card at about 65 percent of SXM5 performance across ten data analytics, AI and HPC workloads, while drawing half the power. For inference of a quantized model or single-GPU fine-tuning, PCIe is usually all you need. For training across many GPUs, the interconnect matters more than the silicon: a PCIe card bridges to exactly one neighbor, while an HGX board's NVSwitch gives every GPU pair the full 900 GB/s.

Which one is behind a given rate? Hourly single-GPU cloud instances — including Vultr's 1× H100 — are PCIe cards, since SXM5 silicon only exists on HGX boards sold as multi-GPU nodes or bare metal. QuantaCloud's $2.59/hour listing, for example, is a PCIe card, while Hyperstack's $3.20 on-demand rate is SXM. Those two numbers describe different hardware, so check the card type before you rank anything by price.

## What actually fits in 80 GB

More than most people assume. With vLLM's default settings — it claims 92 percent of GPU memory — an 80 GB card leaves a working budget of about 73 GiB for weights, KV cache and activations. Plenty of production-scale models fit on one card with room to spare.

| Workload | One 80 GB card? | Detail |
| --- | --- | --- |
| gpt-oss-120b (MXFP4) | Yes | ~61 GiB of weights; room for about two full 131k-token sequences |
| Qwen3-32B at FP8 | Yes, easily | 32 GiB checkpoint; roughly 5 concurrent 32k sequences |
| Llama-3.3-70B at 4-bit (AWQ) | Yes | 37 GiB; about 119k tokens of KV cache left over |
| Llama-3.3-70B at FP8 | Short contexts only | 67.7 GiB of weights leaves ~18k tokens; long context needs 2 GPUs or a bigger card |
| QLoRA fine-tune, 70B model | Yes | 40–48 GB per the Unsloth and Axolotl documentation |

The pattern worth internalizing: 30B-class models are comfortable, 70B-class dense models fit with 4-bit quantization (or FP8 plus short contexts), and OpenAI's 120B mixture-of-experts model fits because MXFP4 shrinks the weights to around 61 GiB. Long-context serving of a dense 70B is the one case that genuinely overflows — that's when you move to a 2× H100 instance rather than a bigger single card, and the 2× hourly rate keeps the same per-GPU math.

## Renting an H100 on Vultr, step by step

The flow takes minutes and needs nothing beyond a payment method.

1. Create a Vultr account and add a payment method — cards and PayPal both work.
2. Open the Cloud GPU product page and pick a 1×, 2× or 4× H100 configuration, a region, and an image (plain Ubuntu or one of the prebuilt AI stacks), then add your SSH key.
3. Deploy. Billing starts at the hourly rate from the moment the instance is live.
4. Work, then destroy the instance. Billing stops when it's gone.

One check before you commit to a schedule: H100 stock is finite everywhere and specific regions run dry. Deploy a short test instance in your target region first — it costs a few dollars — to confirm capacity is actually there. [👉 deploy a 1× H100 on Vultr](https://www.vultr.com/products/cloud-gpu/?ref=7268283) when you're ready.

## H100 rental FAQ

**How much does an H100 cost per day or per week?**
At $3.49/hour, a 1× H100 runs $83.76 for a 24-hour day and $586.32 for a full week of continuous use. Destroy the instance when the job ends and nothing further accrues.

**Is there a contract or minimum term?**
No. Vultr's cloud GPU instances bill hourly with no commitment; the total is just the hourly rate times the hours the instance exists. Multi-month discounts only exist on the dedicated-server side of the market, where 3-to-12-month terms currently knock 5–15% off the monthly rate.

**Is Vultr the cheapest H100 rental?**
No. Theta EdgeCloud lists $2.29/hour, RunPod's community tier is $2.69, and spot capacity drops below $2.20 at several providers. What $3.49 at Vultr buys is the boring version: a first-party cloud, a rate that doesn't change mid-run, nothing that gets reclaimed, card-or-PayPal payment, and hourly billing you can switch off. If you're shopping purely on price and can tolerate spot or marketplace variance, the decentralized options cost less.

**What's the cheapest way to run an H100 around the clock for a month?**
A dedicated monthly server. At $1,249–$1,690/month versus about $2,548 for 730 hourly hours at Vultr, 24/7 operation is the one scenario where hourly clearly loses. Everything shorter — and especially anything under ~360 hours a month — flips the other way.

## Which one should you book?

- A few hours to a few days of GPU time, or inference that scales up and down → hourly. Vultr's 1× H100 at $3.49 is the low-friction version; [👉 compare Vultr's H100 plans and current pricing](https://www.vultr.com/pricing/?ref=7268283).
- More than 80 GB — long-context 70B serving, larger fine-tunes → the 2× or 4× instance at the same per-GPU rate.
- Training across a node where interconnect speed matters → an 8× HGX bare-metal server with NVSwitch.
- A GPU pinned 24/7 for months → dedicated monthly bare metal at $1,249–$1,690; hourly is the expensive habit there.

Whichever route you take, do the two checks people skip: confirm the card type (PCIe vs SXM) matches what your comparison price assumed, and confirm the region actually has stock before you schedule the big run. Both take one click and either one can save a very annoying Saturday.
