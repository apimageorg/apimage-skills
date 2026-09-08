# APImage Skills

Agent skills for generating marketing video and imagery with [APImage](https://apimage.org) — UGC-style ad video, product clips, spokesperson video and product photography, driven from Claude, Cursor, Codex or any MCP client.

```bash
npx skills add apimageorg/apimage-skills
```

Works with Claude Code, Claude.ai and Claude Desktop, plus 70+ other agents including Cursor, Codex, Copilot, Gemini CLI, Windsurf, OpenCode and Zed.

Maintained by [APImage](https://apimage.org). MIT licensed.

---

## Status

**Viral video generation: 15 of 30 published.** Product photo generation and UGC content generation are next. See [Roadmap](#roadmap).

## Connect APImage first

Every skill assumes the APImage MCP server is connected.

**Claude Code:**

```bash
claude mcp add --transport http apimage https://mcp.apimage.org/mcp \
  --header "Authorization: Bearer sk_your_api_key" -s user
```

**Codex**, via `mcp-remote`:

```toml
[mcp_servers.apimage]
command = "npx"
args = ["-y", "mcp-remote", "https://mcp.apimage.org/mcp", "--header", "Authorization: Bearer sk_your_api_key"]
```

Keys come from the [APImage dashboard](https://apimage.org). One endpoint, one bearer token, no SDK.

## The skills

### Workflow and cost

The four that change the economics of everything else. Read these first.

| Skill | What it prevents |
|---|---|
| [`draft-then-enhance-workflow`](skills/draft-then-enhance-workflow/SKILL.md) | Iterating at full quality — the most expensive habit on the platform |
| [`async-video-job-orchestration`](skills/async-video-job-orchestration/SKILL.md) | Re-calling `generate_video` because nothing came back, and paying twice |
| [`video-credit-cost-management`](skills/video-credit-cost-management/SKILL.md) | Burning a monthly quota in an afternoon |
| [`seed-locked-iteration`](skills/seed-locked-iteration/SKILL.md) | Rolling new dice every generation and learning nothing |

### Generating video

| Skill | Covers |
|---|---|
| [`video-model-selection`](skills/video-model-selection/SKILL.md) | Choosing between the five video models, and matching resolution to destination |
| [`text-to-video-prompting`](skills/text-to-video-prompting/SKILL.md) | Prompting motion and camera, not just a frame |
| [`image-to-video-animation`](skills/image-to-video-animation/SKILL.md) | Animating a real product photo instead of letting the model invent one |
| [`multi-scene-video-assembly`](skills/multi-scene-video-assembly/SKILL.md) | Generating beats and cutting them, rather than one long drifting clip |
| [`character-consistency-video`](skills/character-consistency-video/SKILL.md) | Brand assets, so the presenter and product stay the same across a campaign |
| [`lip-sync-spokesperson-video`](skills/lip-sync-spokesperson-video/SKILL.md) | Talking avatars, the 30s cap and the 3-credits-per-second reality |

### Making it perform

| Skill | Covers |
|---|---|
| [`video-hook-first-3-seconds`](skills/video-hook-first-3-seconds/SKILL.md) | The opening that decides whether anything else is watched |
| [`tiktok-video-generation`](skills/tiktok-video-generation/SKILL.md) | Native formatting, sound-off viewing, and platform AI rules |
| [`aspect-ratio-strategy`](skills/aspect-ratio-strategy/SKILL.md) | Safe areas, and why the bottom third of a vertical video is unusable |
| [`video-ab-testing-variants`](skills/video-ab-testing-variants/SKILL.md) | Testing one variable at a time so results are reusable |
| [`video-brand-safety-moderation`](skills/video-brand-safety-moderation/SKILL.md) | `safety_tolerance`, and why passing moderation isn't the same as brand-safe |

## What the skills are built on

They encode the parts of APImage that aren't obvious from the tool list:

- **Video is always asynchronous.** `generate_video` returns a job ID. Poll with `get_video_generation` — polling is free, regenerating is not.
- **`flux-3-video-draft` → `enhance_video_draft`** lets you iterate cheaply and pay full price once. This changes project cost by a large multiple.
- **Failed renders are already refunded.** No reconciliation, no defensive retries.
- **Lip sync is `seedance-2-0` only**, bills at 3 credits/second, and caps at 30 seconds.
- **Brand assets** — character, product, background, preset — are how consistency works. Seeds give a consistent *look*, not a consistent *subject*.
- **Everything diagnostic is free**: `enhance_prompt`, all polling, all history, all brand asset management, `check_credits`.
- **Errors carry the upstream HTTP status**, so a 400 or 422 is deterministic and shouldn't be retried unchanged.

## A recurring theme

Several skills say the same thing in different contexts, because it's the difference between output that performs and output that reads as generated:

**The model's default is commercial polish, and commercial polish is what feeds skip.** Studio lighting, smooth camera moves, centred products and even exposure all have to be prompted *against* — explicitly, with negative framing — to produce something that looks native.

## Disclosure and rights

The skills take a position rather than staying quiet:

- **Don't use a real person's likeness** without permission, including public figures and stock portraits whose licence excludes synthetic use. Generate a presenter instead.
- **Don't present a synthetic person as a real customer.** That's a false endorsement, and an advertising-standards problem in many jurisdictions independent of platform policy.
- **Label AI content** where the platform requires it. TikTok and others apply their own detection and can limit distribution on undisclosed synthetic media.
- **Generated visuals don't get a lower bar for claim substantiation.** A clip demonstrating something the product doesn't do is a false claim however it was produced.

## Roadmap

Next, in order:

**Viral video (15 remaining)** — `instagram-reels-generation`, `youtube-shorts-generation`, `faceless-video-automation`, `ai-avatar-presenter`, `product-demo-video`, `before-after-transformation-video`, `video-to-video-restyling`, `video-duration-pacing`, `b-roll-generation`, `video-thumbnail-generation`, `talking-head-scripting`, `trend-format-replication`, `batch-video-production`, `video-caption-subtitle-planning`, `music-audio-pairing`

**Product photo generation (30)** — background removal and replacement, relighting, camera angles, upscaling, inpainting fixes, lifestyle and white-background shots, marketplace compliance for Amazon/Shopify/Etsy, category-specific craft for apparel, food, jewellery and furniture, batch pipelines and QA review

**UGC content generation (30)** — UGC ad video, authenticity signals, creator persona design, testimonial and unboxing and tutorial formats, hook libraries, scripting, multi-creator variants, platform-native formatting, disclosure compliance, and performance iteration

## Adding a skill

Copy [`docs/skill-template.md`](docs/skill-template.md), match `name:` to the directory name, add a row above, and verify with `npx skills add ./ --list`. House rules are in the template.

## License

MIT. See [LICENSE](LICENSE).
