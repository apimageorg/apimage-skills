---
name: product-photo-batch-pipeline
description: Process a whole catalogue of product images — queueing against rate limits, budgeting credits, and keeping the batch reconcilable. Use whenever the user has many products to process, a catalogue to generate, mentions bulk or batch image processing, or is building an automated image pipeline.
---

# Batch Image Pipeline

A catalogue is not one image done many times. It's a queue with rate limits, a credit budget, a consistency requirement and a review step — and the review step is the one that gets dropped first and matters most.

## The rate limits

| Endpoint | Per minute |
|---|---|
| `/generations`, `/brand-assets` | **120** |
| `/ai-image-generate` | **60** |
| `/image-studio` | see plan |
| Background tools (`remove_background`, `replace_background`) | **30** |

**Background tools are the bottleneck at 30/minute**, and they're usually the first step in a catalogue pipeline. A 300-product cutout job is at least ten minutes of wall clock at the limit.

Images are synchronous, unlike video — so the pipeline is a rate-limited loop rather than a job queue with polling. That's simpler, and it means throttling is the only concurrency concern.

## The pipeline

```python
import time

RPM_BACKGROUND = 30
GAP = 60.0 / RPM_BACKGROUND + 0.3        # margin

LOOK = ("On pure white, soft even studio light from above and slightly "
        "left, neutral white balance, accurate colour, subtle contact "
        "shadow. Product fills 85% of frame. Product unchanged.")

def process(product, log):
    pid = product["id"]
    try:
        cutout = remove_background(image=product["photo"])
        time.sleep(GAP)

        asset = create_brand_asset(type="product", ...)   # free

        shots = {}
        for angle in ("straight on, front elevation",
                      "three-quarter view from the upper left",
                      "side profile from the left"):
            shots[angle] = generate_image(
                model="flux-2-pro",
                reference_images=[cutout],
                prompt=f"The same product, {angle}. {LOOK}",
                aspect_ratio="1:1",
                seed=8812,
            )
            time.sleep(1.1)                                # 60/min headroom

        log.append({"id": pid, "status": "ok", "shots": shots,
                    "asset": asset})
    except Exception as e:
        log.append({"id": pid, "status": "failed", "error": str(e)})

log = []
for p in catalogue:
    process(p, log)
```

Four things in there:

- **Throttle per endpoint**, with margin. The background gap is the binding one
- **Save the brand asset per product** — free, and it's what makes future work cheap
- **One look clause, verbatim, for every product.** Copy-paste, never paraphrase
- **Log per product, including failures**, so the batch is reconcilable

## Budget before you run

```
check_credits()
→ balance, monthly usage against quota, tier
```

Then do the arithmetic. A batch that runs out halfway leaves a half-processed catalogue, which is worse than not starting.

```
Per product:
  remove_background          2 credits
  3 angle generations        3-27 credits (1-9 each)
  create_brand_asset         free
  analyze_image (QA sample)  1 credit on ~15%
  ─────────────────────────────────────
  ~5-30 credits per product

300 products ≈ 1,500-9,000 credits
```

That range is wide because model and resolution drive it. Practical control:

- **Iterate the look on 3-5 products first**, at low resolution, until the prompt is right
- **Then run the batch** at production resolution with a locked prompt and seed
- **Use `flux-2-klein-9b`** for anything that isn't a hero shot

See `image-model-selection` and `video-credit-cost-management`.

## Pilot before the batch

The step that saves the most.

```
1. Pick 5 products spanning the catalogue's variety
     — a simple box, something reflective, something with fine detail,
       something transparent, something dark
2. Run the full pipeline on those 5, at low resolution
3. Review properly against the checklist
4. Fix the prompt, the references, the look clause
5. Only then run the 300
```

**Choose the pilot products for difficulty, not convenience.** A pilot of five simple opaque boxes tells you nothing about how the pipeline handles the glass bottle in row 200. Reflective, transparent and very dark products are where it breaks. See `jewelry-reflective-products`.

## Reconcile

The batch record is what makes the output usable.

```
id     status   cutout  angles  qa      notes
P-001  ok       ✓       3/3     pass
P-002  ok       ✓       3/3     pass
P-003  ok       ✓       3/3     FAIL    halo on edge — glass, needs inpaint
P-004  failed   —       —       —       429, retry
P-005  ok       ✓       2/3     pass    side view refs missing
```

Two things to capture that people skip:

- **Partial successes.** A product with two of three angles is not "done"
- **Which products need manual work.** Glass, chrome and dark products routinely need an inpainting pass. Flag them rather than shipping the halo

## Tiered QA

Never publish a batch unreviewed. Tier it so the review is proportionate.

```
Tier 1 — all products     Grid review at thumbnail size
Tier 2 — all main images  Full checklist
Tier 3 — 10-20% sample    Full checklist, sampled randomly
Tier 4 — flagged products Manual repair
```

**Grid review at thumbnail size is the highest-value check** and it takes minutes. Inconsistency that's invisible per-image is obvious in a grid, and a grid is how customers see a category page. See `product-image-qa-review`.

**Sample randomly.** The first ten are the ones you watched while building the pipeline.

## Handling failures

Errors come back carrying the upstream HTTP status, so they're diagnosable.

| Status | Means | Do |
|---|---|---|
| 429 | Rate limited | Increase the gap, retry |
| 400 | Bad parameter | Fix it. Deterministic — don't retry unchanged |
| 401 | Bad key | Check the bearer token |
| 402/403 | Out of credits, or plan limit | `check_credits` |
| 422 | Moderation rejection | Rewrite the prompt |
| 5xx | Upstream | Retry with backoff |

**Don't retry 400 or 422 unchanged.** They fail identically and you'll generate a run of identical errors.

## Incremental runs

A catalogue is never processed once. Design for the second run.

- **Key the log by product ID**, so a re-run skips completed products
- **Store the seed and prompt** with the batch, so an added product matches
- **Save the look as a preset brand asset**, so the spec survives the person
- **Version the look**, and decide deliberately whether a change means reprocessing the back catalogue

Changing a catalogue's look is a project. Choose a durable look rather than a fashionable one. See `product-photo-consistency`.

## Don't

- **Don't run the batch before piloting on the hard products.**
- **Don't ignore the 30/minute background limit.** It's the bottleneck.
- **Don't paraphrase the look clause** between products.
- **Don't run a batch you can't afford.** `check_credits` first.
- **Don't skip the grid review.**
- **Don't sample the first ten.**
- **Don't retry deterministic errors** unchanged.
- **Don't auto-publish.**

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`product-photo-consistency`, `product-image-qa-review`, `background-removal-workflow`, `image-model-selection`, `marketplace-image-compliance`, `batch-video-production`
