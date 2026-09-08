---
name: async-video-job-orchestration
description: Handle APImage video jobs correctly — they are always asynchronous, so poll with get_video_generation rather than regenerating, and use webhooks at volume. Use whenever the user generates video with APImage, asks why a call returned no video, mentions polling or webhooks or job IDs, or is running a batch.
---

# Async Video Jobs

**Video generation on APImage is always asynchronous.** `generate_video` returns a job ID immediately, not a video. This is the single most common source of double-billing, because the natural reaction to "no video came back" is to call it again.

Don't. Poll.

## The correct sequence

```
1. generate_video(...)           → returns job_id, immediately
2. get_video_generation(id=...)  → status: pending / processing
3. wait                          → a few seconds
4. get_video_generation(id=...)  → status: completed, plus the URL
```

`get_video_generation` and `list_video_generations` are **free**. There is no cost reason to avoid polling and every reason to avoid regenerating.

```python
import time

job = generate_video(
    mode="image-to-video",
    model="flux-3-video-draft",
    reference_images=[still],
    prompt=prompt,
    aspect_ratio="9:16",
    duration=5,
    seed=4271,
)

deadline = time.monotonic() + 600          # cap the wait
while time.monotonic() < deadline:
    r = get_video_generation(id=job["id"])
    if r["status"] == "completed":
        break
    if r["status"] == "failed":
        break                               # already refunded. Read the error
    time.sleep(5)
```

Three things in there matter:

- **A deadline.** An unbounded poll loop on a stuck job runs forever.
- **Handle `failed` explicitly.** Failed renders are already refunded — you don't need to reconcile credits, but you do need to read the error and fix the input rather than retrying blindly.
- **Sleep between polls.** Polling is free but the endpoint is rate limited.

## Webhooks, at volume

Polling is fine for a handful of clips. For a batch, pass `webhook_url` and let APImage tell you.

```
generate_video(
  ...,
  webhook_url="https://your-endpoint.example.com/apimage"
)
```

The job then calls you on completion rather than you asking repeatedly. Worth it once you're running more than a few clips in a session, and essential in any automated pipeline. It also composes with the platform integrations — Zapier, n8n and Pipedream — where a webhook is the natural trigger.

Keep the poll path as a fallback. Webhooks get lost.

## Rate limits and concurrency

Two separate constraints, and both bite on batches.

| Limit | Value |
|---|---|
| Video endpoint requests | **30 per minute** |
| Background tools | 30 per minute |
| `/ai-image-generate` | 60 per minute |
| `/generations` and `/brand-assets` | 120 per minute |
| Concurrent in-flight video jobs | **Capped** (plan-dependent) |

The concurrency cap is the one that surprises people. Firing twenty video jobs at once doesn't queue them for you — some will be rejected. Submit in waves.

```python
from collections import deque

def run_batch(specs, max_inflight=3, poll_every=5):
    queued, inflight, done = deque(specs), {}, []
    while queued or inflight:
        while queued and len(inflight) < max_inflight:
            spec = queued.popleft()
            job = generate_video(**spec)
            inflight[job["id"]] = spec

        time.sleep(poll_every)

        for jid in list(inflight):
            r = get_video_generation(id=jid)
            if r["status"] in ("completed", "failed"):
                done.append((jid, inflight.pop(jid), r))
    return done
```

Start `max_inflight` at 3 and raise it only if nothing is being rejected.

## Keep the job IDs

Job IDs are how you retrieve results, enhance a draft, and reconcile a batch. Losing them means regenerating, which means paying again.

Log the ID alongside the input as you go:

```
job_a91f  seed=4271  9:16  5s  "hook, mid-action"      → completed  ✓ enhance this
job_b02c  seed=4271  9:16  5s  "hook, slow push"       → completed  ✗ drifts
job_c73e  seed=9902  9:16  8s  "body, product demo"    → processing
```

And use the history endpoint rather than your memory:

```
list_video_generations(status="completed", mode="image-to-video")
```

Paginated, filterable by status and mode. This is how you find a clip from earlier in the session, or audit what a batch actually produced.

## Reading failures

Failures come back as MCP tool errors carrying the **upstream HTTP status**, which means the error is diagnosable rather than opaque.

| Status | Usually means | Do |
|---|---|---|
| 400 | Bad parameter — unsupported duration, resolution or mode for that model | Check the model's constraints |
| 401 | Bad or missing API key | Check the bearer token |
| 402 / 403 | Out of credits, or a feature not on your plan | `check_credits`. Video and advanced models need a paid plan |
| 422 | Prompt or reference rejected by moderation | See `video-brand-safety-moderation` |
| 429 | Rate limited or over the concurrency cap | Back off, reduce `max_inflight` |
| 5xx | Upstream problem | Retry with backoff. The render itself is refunded if it failed |

**Don't retry a 400 or a 422 unchanged.** They're deterministic — the same input fails the same way, and you'll just generate a series of identical errors.

## Don't

- **Don't call `generate_video` again because nothing came back.** It returned a job ID. Poll it.
- **Don't poll without a deadline.**
- **Don't fire a large batch at once.** There's a concurrency cap.
- **Don't lose job IDs.** Log them with the input.
- **Don't retry deterministic errors** (400, 422) without changing the input.
- **Don't reconcile credits for failed renders.** They're already refunded.
- **Don't skip `webhook_url`** in an automated pipeline.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

Codex, via `mcp-remote`:

```toml
[mcp_servers.apimage]
command = "npx"
args = ["-y", "mcp-remote", "https://mcp.apimage.org/mcp", "--header", "Authorization: Bearer sk_your_api_key"]
```

## Related skills

`draft-then-enhance-workflow`, `video-model-selection`, `video-credit-cost-management`, `video-brand-safety-moderation`
