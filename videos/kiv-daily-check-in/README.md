# Kiv Shorts Template

Vertical YouTube Short (1080x1920, 30fps, ~24-60s) for **Kiv** — a dark, minimal micro-journaling app. Pure-dark canvas, one electric-lime accent, Syne typeface. Notion / Linear energy: clean, confident, no decoration.

The full design system (colors, type scale, motion, surface detail, SFX cues, anti-patterns) lives in [DESIGN.md](./DESIGN.md). **Read it before editing** — especially the "What NOT to Do" list, which encodes the brand discipline (no gradients, no noise, single accent, one typeface).

> **This is a real brand template, not a placeholder reskin.** The wordmark "Kiv" and the CTA URL `kiv.samkiel.tech` are the final values — unlike the brand-neutral `classic` template, you do NOT swap them per video.

## Brand tokens (canonical — do not change)

| Token | Value |
|---|---|
| Background | `#0A0A0A` (pure dark) |
| Accent | `#E8FF47` (electric lime) — the only accent |
| Accent ink (on lime) | `#0A0A0A` |
| Typeface | **Syne** (Google Fonts), weights 400–800 |
| Primary text | `#FFFFFF` · Dim `#8A8A8A` |
| Wordmark | "Kiv" — Syne Bold, white (mixed-case, never "KIV") |
| CTA URL | `kiv.samkiel.tech` |

## What this template ships

A self-contained 24-second demo composition (`index.html`) with the four reusable phase archetypes, redesigned for Kiv's flat-minimal language:

| Phase | Pattern | Use for |
|---|---|---|
| 1 — Hero slam | overline + pre-line + 220px **solid lime** slam word + caption pill | scroll-stop hook |
| 2 — Stat row | overline + headline + 2 flat dark stat cards (one may be a lime `.accent` card) | numbers, the "why it sticks" |
| 3 — Feature cards | overline + 3 uniform flat cards with lime index tiles | how it works, steps, features |
| 4 — CTA / thumbnail hold | overline + CTA headline + lime URL pill + outcome line | closing call-to-action; thumbnail-grade final frame |

Each phase is mutex-visible (one at a time), separated by a clean opacity **crossfade** (no blur — flat aesthetic). Entrance animations only; the transition handles the exit (HyperFrames rule).

The composition keeps the same structural contract as the sibling templates (phase IDs `#phase1-4`, inner element IDs `#p1-hero`, `#p1-overline`, `#p2-pill-1/2`, `#p3-card-1/2/3`, `#p4-url`, timing constants `T1/T2/T3`, `P2/P3/P4`, `slam_t`, `shake_offsets`), so `scripts/compute_timings.py` and the `new-kiv-short` playbook work unchanged.

---

## Spawn a new video from this template

Use the Archon workflow (recommended):

```bash
archon workflow run create-kiv-short --no-worktree "Kiv — your daily check-in"
```

Or manually from the repo root:

```bash
# 1. Pick a slug (kebab-case, descriptive)
SLUG="my-kiv-short"

# 2. Copy the template
cp -r templates/shorts/kiv videos/$SLUG          # PowerShell: Copy-Item -Recurse templates/shorts/kiv videos/$SLUG

# 3. Update meta.json → { "id": "my-kiv-short", "name": "My Kiv Short" }

# 4. Edit videos/$SLUG/index.html
#    - Fill each phase's text (overline, pre-line, hero word, stats, cards, CTA)
#    - Keep the "Kiv" wordmark and kiv.samkiel.tech URL (real brand values)
#    - Drop narration at videos/$SLUG/audio/narration.wav, uncomment the <audio> block
#    - Recompute transition timestamps from the transcript (see playbook step 7)

# 5. Validate
npx hyperframes lint    videos/$SLUG
npx hyperframes inspect videos/$SLUG

# 6. Preview (never auto-render)
npx hyperframes preview videos/$SLUG

# 7. Render (manual, when ready)
npx hyperframes render  videos/$SLUG -o videos/$SLUG/out/$SLUG.mp4
```

## Customizing per video

The text changes per video; the **brand tokens do not**. All tokens live as CSS variables on `#root`:

```css
#root {
  --bg:         #0A0A0A;   /* pure dark canvas */
  --accent:     #E8FF47;   /* electric lime — the ONLY accent */
  --accent-ink: #0A0A0A;   /* text on a lime fill */
  --text:       #FFFFFF;
  --text-dim:   #8A8A8A;
  /* ... */
}
```

**Single-accent discipline:** the per-card classes `.cyan` / `.magenta` / `.purple` / `.blue` are retained from the sibling templates for structural parity but all render identically (flat dark card). To make one stat card pop, add the `.accent` class (lime fill). Never introduce a second hue.

## Adding narration

1. Drop your TTS audio at `audio/narration.wav` (Kokoro is the repo default — free, local).
2. Uncomment the `<audio id="narration">` block at the bottom of `index.html`.
3. Recompute phase timestamps from the transcript: `python scripts/compute_timings.py videos/<slug>`.

## Adding SFX

Kiv suits a light touch — a single soft cue on the hero slam is plenty; don't clutter the calm. Each SFX is a separate `<audio>` element on its own `data-track-index`, capped per [`.claude/rules/audio-design.md`](../../../.claude/rules/audio-design.md) (never exceed `0.25`). **No background music on Shorts.**

## Don'ts

See `DESIGN.md` "What NOT to Do" for the full list. The big ones:

- **No gradients anywhere** (no hero text gradient, no card gradient fills, no ambient wash).
- **No film grain / noise overlay** — the canvas stays clean.
- **No glow halos / colored drop-shadows** — flat lime, hard edges.
- **No second accent** — `#E8FF47` is the only color.
- **No second typeface** — Syne everywhere.
- **No blur transitions** — crossfade only.
- **No light canvas** — pure dark `#0A0A0A`.
- **No `<br>` in content text** — use `max-width`.
- **No `position: absolute; top: Npx` on `.phase-content`** — use padding.
- **No all-caps wordmark** — it's "Kiv", not "KIV".
