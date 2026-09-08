---
name: seed-locked-iteration
description: Use the seed parameter to isolate what a prompt change actually did, instead of rolling new dice every generation. Use whenever the user is iterating on an APImage generation, says output is inconsistent or unpredictable, mentions seeds or reproducibility, or can't tell whether an edit improved anything.
---

# Seed-Locked Iteration

Without a fixed seed, every generation is a fresh roll. You change one word in the prompt, the output changes completely, and you have no idea whether your edit helped or you just got a different dice throw.

Locking the seed makes iteration a controlled experiment instead of a slot machine. It costs nothing and it's the difference between converging in four generations and flailing through twenty.

## How it works

`seed` is accepted on both image and video generation. Same seed plus same prompt plus same parameters gives you the same output. Change the prompt and hold the seed, and the delta you see is attributable to the prompt.

```
seed unset:
  gen 1  "product on marble, soft light"       → overhead shot, warm
  gen 2  "product on marble, soft light, 4k"   → side shot, cool
  Did "4k" change the angle? No idea.

seed=8812:
  gen 1  "product on marble, soft light"       → side shot, warm
  gen 2  "product on marble, soft light, 4k"   → side shot, warm, sharper
  Now you know what "4k" did.
```

## The loop

```
1. Generate unseeded 3-4 times    → find a composition you like
2. Note that job's seed            → from the generation record
3. Lock it                         → seed=<that value>
4. Change ONE thing per generation → prompt, or one parameter
5. Judge the delta                 → it's now attributable
6. Unlock when composition is wrong → back to step 1
```

**Step 1 matters.** Locking a seed too early locks you into a bad composition and you'll spend ten prompts fighting it. Roll a few times first, pick a base you like, *then* lock.

**Step 6 is the escape hatch.** If prompt edits aren't fixing the framing, the seed is the problem, not the prompt. Unlock and re-roll.

## One variable at a time

The discipline that makes this work, and the one people abandon under time pressure.

```
seed=8812  "product on marble, soft morning light, overhead"
seed=8812  "product on marble, soft morning light, overhead, shallow depth of field"
seed=8812  "product on marble, golden hour light, overhead, shallow depth of field"
                              ^^^^^^^^^^^^ one change
```

Change two things and you're back to guessing. It feels slower and it converges faster.

Keep a log. Four iterations in, "the second one was better" is only useful if you know what the second one was.

```
8812  overhead, morning light                      → base
8812  + shallow dof                                → better, product pops
8812  + golden hour (replaced morning)             → too orange, revert
8812  + shallow dof, morning light, 45° angle      → this one ✓
```

## Seeds for consistency across a set

The other use, and it's the one that matters for production: **the same seed produces a coherent look across several generations.**

For a set of product shots, or a multi-scene video where the clips need to feel like one piece, holding the seed constant while varying the subject or angle keeps the lighting, grade and rendering character consistent.

```
seed=8812  "product A on marble, soft light, 45°"
seed=8812  "product B on marble, soft light, 45°"
seed=8812  "product C on marble, soft light, 45°"
→ three products, one consistent visual language
```

It isn't a guarantee — the subject change moves things — but it's a large improvement over unseeded, where each shot arrives from a different visual universe. For genuine character and product consistency, pair it with brand assets. See `character-consistency-video`.

## Seeds in video

Same principle, higher stakes, because video generations cost more.

```
generate_video(
  mode="image-to-video",
  model="flux-3-video-draft",
  reference_images=[still],
  prompt="camera locked, subject moves toward frame left",
  aspect_ratio="9:16",
  duration=5,
  seed=4271
)
```

In video, the seed governs the **motion path** as much as the look. That makes it disproportionately valuable: once you have motion you approve, locking the seed means prompt edits adjust the look without re-rolling the camera move you spent five drafts getting right.

Combine with drafting: draft at a locked seed, iterate the prompt, then `enhance_video_draft` the winner. See `draft-then-enhance-workflow`.

## Where seeds don't help

- **Across models.** A seed on flux 2 pro means nothing on gpt image 2. Different systems.
- **Across major parameter changes.** Change the aspect ratio or resolution substantially and the composition shifts regardless.
- **For guaranteed identity.** A seed does not reliably reproduce the same *face* or the same *product* across different prompts. That's what `reference_images` and brand assets are for.
- **`enhance_video_draft`.** It re-renders the clip you already have; you're not re-rolling.

That third point is the common misunderstanding. Seeds give you a consistent *visual treatment*, not a consistent *subject*. If the same character must appear across six clips, you need reference images and a saved brand asset, not a clever seed.

## Don't

- **Don't iterate without a seed.** You can't attribute anything.
- **Don't lock a seed before you like the composition.** You'll fight it for ten prompts.
- **Don't change two things at once.**
- **Don't rely on seeds for subject identity.** Use `reference_images` and brand assets.
- **Don't expect a seed to transfer across models.**
- **Don't skip the log.** The iteration record is what makes it converge.

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`draft-then-enhance-workflow`, `text-to-video-prompting`, `character-consistency-video`, `video-model-selection`
