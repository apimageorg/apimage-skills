# Skill template

Copy this into `skills/<your-skill-name>/SKILL.md`. Do not put a `SKILL.md` at the repo root or inside `docs/`, or the CLI will discover it as a real skill.

---

```markdown
---
name: <kebab-case, matches the directory name exactly>
description: <What this covers and when to reach for it. Front-load the words a
  user would type — the platform name, the format, the tool, the symptom. This is
  the only thing the agent sees when deciding whether to load the skill.
  One or two sentences, no line breaks.>
---

# <Title>

<Two or three lines. The mistake this prevents, or the thing the model's default
behaviour gets wrong. If it duplicates another skill's premise, it doesn't need
to exist.>

## <The decision or craft section>

<Most APImage mistakes are decisions made early — the wrong model, iterating at
full quality, text-to-video when the subject matters, polish when the platform
wants native. Lead with the decision.>

## The call

<A real, complete tool call with actual APImage parameter names and model
identifiers. Not pseudocode.>

```
generate_video(
  mode="image-to-video",
  model="flux-3-video-draft",
  reference_images=["<still>"],
  prompt="...",
  aspect_ratio="9:16",
  resolution="hd",
  duration=5,
  seed=4271
)
```

## <Craft detail>

<The part that makes the output good rather than merely valid. Prompt phrasing
that works, framing rules, what to prompt against.>

## Cost

<What this costs, and where the waste is. Every skill that spends credits should
say so.>

## Don't

- <Specific mistakes, not general advice.>

## Setup

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

## Related skills

`...`, `...`
```

---

## House rules

- **Use real tool names, model identifiers and parameters.** `flux-3-video-draft`, `seedance-2-0`, `safety_tolerance`, `reference_images`. Invented parameters make a skill worse than nothing.
- **Video is always async.** Any skill that generates video must say so, or point at `async-video-job-orchestration`.
- **State the cost.** Credits are the main constraint users hit. Free operations should be named as free.
- **Draft then enhance.** Any skill that iterates on video should reference the draft workflow rather than implying full-quality iteration.
- **Prompt against polish.** The models default to commercial gloss. Where the destination wants native, say so with negative framing.
- **Be honest about disclosure and rights.** Likeness permission, synthetic-testimonial rules and claim substantiation are real constraints, not caveats to omit.
- **Say what falls flat.** Every format has a default output that doesn't work — the slow push-in, the studio-lit product, the establishing shot. Name it.
- **Cross-reference by exact skill name** in backticks. Check the name exists.

## Adding a skill

1. `mkdir skills/<name>` and write `SKILL.md` from the template.
2. Check `name:` matches the directory name exactly.
3. Add a row to the relevant table in `README.md`, and remove it from the roadmap.
4. Verify: `npx skills add ./ --list` should report one more skill.
5. Check cross-references resolve:

   ```bash
   ls skills > /tmp/have.txt
   grep -h -A2 '^## Related skills' skills/*/SKILL.md \
     | grep -oE '`[a-z][a-z0-9-]+`' | tr -d '`' | sort -u \
     | while read -r r; do grep -qx "$r" /tmp/have.txt || echo "MISSING: $r"; done
   ```

6. Open a pull request.
