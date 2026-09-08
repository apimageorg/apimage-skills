---
name: ugc-batch-testing
description: Run structured UGC creative tests at volume — matrix design, draft-first sequencing, job orchestration and credit control. Use whenever the user needs many creative variants, is setting up a creative testing programme, or is producing UGC at volume without a system.
---

# Batch Testing UGC Creative

Generated UGC changes the constraint on creative testing. When a variant costs a few credits instead of a shoot day, the limit stops being production and becomes **experimental design and media budget**.

Which introduces a new failure mode: producing forty variants that differ on everything and learning nothing from any of them.

## Design the matrix before generating anything

Pick your variables, hold everything else fixed, and write it down first.

```
Variables       Levels
hook            6      the highest-variance thing. Test most
creator         3      usually the second-largest
body            2      keep this small
format          2      problem-solution vs tutorial

Full factorial: 6 × 3 × 2 × 2 = 72   too many to read
```

Don't run the full grid. **Test one variable at a time, in sequence:**

```
Round 1   6 hooks    × 1 creator × 1 body     → winning hooks
Round 2   2 hooks    × 3 creators × 1 body    → winning creator
Round 3   2 hooks    × 1 creator × 2 bodies   → winning body
```

Roughly a dozen renders instead of seventy-two, and each round produces a readable answer. **Sequential beats factorial** when your media budget can't power seventy cells — and it almost never can.

## Hold the constants absolutely fixed

```python
PROD  = "<product asset>"
BG    = "<background asset>"
LOOK  = ("Handheld phone footage, slight shake, slightly uneven "
         "exposure. Natural window light from the left, no fill. "
         "Ordinary kitchen with some clutter. Unpolished, shot on "
         "a phone. Not studio, not commercial.")
SEED  = 4271
```

**Copy the look clause verbatim into every prompt. Never paraphrase it.** "Handheld with slight shake" and "slightly shaky handheld" read identically to you and differently to the model, and across a twenty-variant batch the paraphrases become an uncontrolled variable you can't see. See `ugc-character-consistency`.

Same for the seed: a fixed seed holds the visual treatment so the thing you're testing is the thing that differs. See `seed-locked-iteration`.

## Draft everything, enhance survivors

The sequencing that makes volume affordable.

```
20 variants   flux-3-video-draft            → cheap
kill 12       on sight, before spending      → free
enhance 8     enhance_video_draft            → the real cost
media test 8
```

Internal review's job in this pipeline is **removing the broken, not picking the winner** — it's poor at the second and reliable at the first. Deformed hands, drifting labels, wrong motion: all visible at draft quality.

`enhance_video_draft` on a draft you already like is the whole cost story of the platform. See `draft-then-enhance-workflow`.

## Orchestrate the jobs properly

Video generation is **always async**: `generate_video` returns a job ID and you poll `get_video_generation`.

```python
jobs = [generate_video(...) for v in variants]   # fire all, don't wait
# then poll each. Polling is free.
```

The rules that matter at batch scale:

- **Fire the whole batch, then poll.** Serialising twenty jobs wastes hours
- **Never re-call `generate_video` on a pending job.** It bills again and gives you a second render
- **Respect the concurrent in-flight video cap.** Queue beyond it rather than hammering
- **Rate limits:** 30 requests per minute on video endpoints, 120/min on `/generations` and `/brand-assets`. Batch loops hit these
- **Failed renders are already refunded.** Don't build refund logic
- **400 and 422 are deterministic** — a retry produces the same failure. Fix the prompt
- **Poll with backoff.** Tight polling burns rate limit for nothing

See `async-video-job-orchestration` and `batch-video-production`.

## Log the batch, not your memory

The part that turns spend into knowledge. Without a log, round three repeats round one.

```
ID    hook           creator  body  seed   cost   3s-ret  CVR   verdict
b-01  mid-sentence   A        v1    4271   draft   61%    2.1%  enhance
b-02  result-first   A        v1    4271   draft   58%    1.9%  enhance
b-03  problem        A        v1    4271   draft   56%    1.6%  keep
b-04  question       A        v1    4271   draft   47%    —     drop
b-05  slow-push-in   A        v1    4271   draft   29%    —     the default
```

Two things it gives you:

**Patterns instead of one-offs.** After three rounds you know which hook families work for this product, which is reusable across every future campaign.

**Evidence against the model's default.** That bottom row — the slow push-in the model produces when unprompted — is the argument for prompting deliberately.

`list_video_generations(status="completed")` is free and paginated. Use it to reconstruct a batch. Also free: `check_credits`, `enhance_prompt`, all polling, all history, all brand-asset operations. Reach for them without hesitation.

## Credit budgeting

```
Round 1   20 hook drafts, 4s each        smallest spend
Round 2   8 enhancements                 the bulk
Round 3   lip sync, face beats only      3 credits/second, 30s cap
Shared    one enhanced b-roll set        reused across all variants
```

Three habits that control spend:

- **Test at 4-6 seconds.** A hook test doesn't need 18 seconds rendered
- **Share the b-roll.** No face means one render serves every variant
- **Sync only face beats.** Lip sync is the most expensive operation available

Check `check_credits` before firing a large batch — it's free, and running out mid-batch leaves you with half a test.

## Judge it correctly

- **Muted, on a phone, at feed size.** That's the viewing condition
- **Three-second retention and hook rate first.** Fastest-moving, least volume needed
- **Then conversion**, with enough data to trust
- **Equal budget per cell.** Unequal spend isn't a test
- **Segment the read.** "C wins for over-45s" is more useful than one champion
- **Don't call it in a day**

See `video-ab-testing-variants` and `ugc-performance-iteration`.

## Don't

- **Don't run a full factorial** you can't power.
- **Don't vary more than one thing** per round.
- **Don't paraphrase the look clause.**
- **Don't test at full quality.** Draft first.
- **Don't re-call generate on a pending job.** Double billing.
- **Don't serialise the batch.**
- **Don't retry a 400 or 422.** Deterministic.
- **Don't discard the log.** It's the compounding asset.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`batch-video-production`, `draft-then-enhance-workflow`, `async-video-job-orchestration`, `video-ab-testing-variants`, `ugc-performance-iteration`, `video-credit-cost-management`
