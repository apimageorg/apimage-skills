---
name: batch-video-production
description: Run video generation at volume — queueing against the concurrency cap, webhooks, budget planning and keeping a batch reconcilable. Use whenever the user is generating many clips, running a content pipeline, mentions batches or automation or scheduling, or needs a month of content.
---

# Batch Production

A batch is not fifteen single generations. It's a queue with a concurrency cap, a credit budget, a reconciliation problem and a review step — and getting any of those wrong turns a productive afternoon into a mess of half-finished jobs you can't account for.

## The constraints that shape a batch

| Constraint | Value |
|---|---|
| Video endpoint | **30 requests/minute** |
| Background tools | 30/minute |
| `/ai-image-generate` | 60/minute |
| `/generations`, `/brand-assets` | 120/minute |
| **Concurrent in-flight video jobs** | **Capped** (plan-dependent) |

The concurrency cap is the one that breaks naive batching. Firing twenty jobs does not queue them for you — some are rejected. Rejections aren't billed but they do leave you with a partial batch and no clear record of which specs never ran.

## The queue

```python
from collections import deque
import time

def run_batch(specs, max_inflight=3, poll_every=5, timeout=1800):
    queued, inflight, done = deque(enumerate(specs)), {}, []
    deadline = time.monotonic() + timeout

    while (queued or inflight) and time.monotonic() < deadline:
        while queued and len(inflight) < max_inflight:
            idx, spec = queued.popleft()
            try:
                job = generate_video(**spec)
                inflight[job["id"]] = (idx, spec)
            except Exception as e:
                queued.appendleft((idx, spec))   # rate limited? retry later
                time.sleep(20)
                break

        time.sleep(poll_every)

        for jid in list(inflight):
            r = get_video_generation(id=jid)
            if r["status"] in ("completed", "failed"):
                idx, spec = inflight.pop(jid)
                done.append({"idx": idx, "job_id": jid, "spec": spec, "result": r})

    return done, [s for _, s in queued]      # completed, and never-submitted
```

Four things in there matter:

- **`max_inflight=3` to start.** Raise only if nothing is being rejected
- **A global timeout.** A stuck job shouldn't hold the batch forever
- **Push the spec back on the queue** if submission fails, so nothing is silently dropped
- **Return the un-submitted remainder**, so you know what didn't run

## Webhooks at real volume

Polling a batch of forty is wasteful even though polling is free. Pass `webhook_url` and let APImage tell you.

```
generate_video(..., webhook_url="https://your-endpoint/apimage")
```

Keep the poll path as a fallback — webhooks get lost. The pattern that works is webhook-primary with a reconciliation sweep at the end:

```
list_video_generations(status="completed")
list_video_generations(status="failed")
```

Both free and paginated. Diff against your submitted list to find anything the webhook missed.

This also composes with the platform's Zapier, n8n and Pipedream integrations, where a webhook is the natural trigger for the next pipeline step.

## Budget the batch before running it

```
check_credits()
→ balance, monthly usage against quota, tier
```

Then do the arithmetic. A batch you can't afford halfway through leaves you with useless partial output.

```
Target:        20 finished clips
Iterations:    ~3 drafts per clip to get it right     = 60 drafts
Enhances:      20
Budget needed: 60 draft-cost + 20 enhance-cost
```

**Draft everything, enhance the keepers.** At batch scale this is not an optimisation, it's the difference between affording the batch and not. See `draft-then-enhance-workflow`.

## Structure the batch for consistency

A batch of twenty clips that don't look related is twenty orphans. Fix the visual world first, vary only the content.

```python
# Fixed for the whole batch
CHARACTER = "<character brand asset>"
PRODUCT   = "<product brand asset>"
BG        = "<background brand asset>"
LOOK      = ("Soft window light from the left, shallow depth of field, "
             "warm neutral grade, handheld with subtle shake.")
BASE      = dict(mode="image-to-video", model="flux-3-video-draft",
                 aspect_ratio="9:16", resolution="hd", seed=4271)

# Varies per clip
CONTENT = [
    ("hook-a", "Hands already mid-pour, no lead-in.", 4),
    ("hook-b", "Product held up, turned once slowly.", 4),
    ("demo",   "Product applied, wiping motion.", 5),
    # ...
]

specs = [
    {**BASE,
     "reference_images": [CHARACTER, PRODUCT, BG],
     "prompt": f"{action} {LOOK} Everything else stays still.",
     "duration": dur}
    for _, action, dur in CONTENT
]
```

Brand assets and a copy-pasted look clause are what make a batch cohere. Paraphrasing the look between clips produces twenty slightly different worlds. See `character-consistency-video`.

## Reconcile, always

The batch record is what makes the output usable. Without it you have a folder of clips and no idea which prompt produced which.

```
idx  job_id    spec                          status      keep
0    job_a91f  hook-a, 4s, seed 4271         completed   ✓ enhance
1    job_b02c  hook-b, 4s, seed 4271         completed   ✗ drifts
2    job_c73e  demo, 5s, seed 4271           completed   ✓ enhance
3    job_d14a  result, 4s, seed 4271         failed      re-run, fix prompt
4    —         cta, 2s, seed 4271            not submitted
```

That last row is why the queue function returns the un-submitted remainder.

**Failed renders are already refunded** — no credit reconciliation needed. But read the error: a 400 or 422 is deterministic and re-running it unchanged fails identically. See `async-video-job-orchestration`.

## Review is not optional at scale

The temptation with a working pipeline is to publish the output. Don't.

- **Every clip gets a human look** before it goes anywhere customer-facing
- **Scrub the high-motion frames** for warping, especially on product labels
- **Check hands, faces and any in-frame text**
- **Confirm claim accuracy** per clip, not per batch

Volume without a review bar is how a brand ends up with a warped logo running as a paid ad. Keep approval mode on for anything commercial. See `video-brand-safety-moderation`.

## Where batching genuinely pays

- **Variant testing.** Six hooks against one body, all drafted. See `video-ab-testing-variants`
- **A content calendar** — a month of clips from one asset set
- **Multi-platform.** The same content at 9:16, 1:1 and 16:9. See `aspect-ratio-strategy`
- **Catalogue coverage.** One clip per product, same treatment throughout
- **Localisation.** Same visual, different overlaid text per market — text goes in the edit, not the render

## Don't

- **Don't fire the whole batch at once.** There's a concurrency cap.
- **Don't batch at full quality.** Draft, then enhance the keepers.
- **Don't run a batch you can't afford.** `check_credits` first.
- **Don't paraphrase the look clause** between clips.
- **Don't lose the spec-to-job mapping.**
- **Don't silently drop un-submitted specs.**
- **Don't retry a deterministic error** unchanged.
- **Don't auto-publish.** Human review on anything customer-facing.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`async-video-job-orchestration`, `draft-then-enhance-workflow`, `video-credit-cost-management`, `video-ab-testing-variants`, `character-consistency-video`, `video-brand-safety-moderation`
