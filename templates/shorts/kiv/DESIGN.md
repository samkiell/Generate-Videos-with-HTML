# DESIGN — Kiv Shorts (Dark Minimal / Micro-Journaling)

Visual system for **YouTube Shorts** (1080x1920, 30fps, ~24-60s) for **Kiv**, a dark, minimal micro-journaling app. Pure-dark canvas, one electric-lime accent, Syne typeface. The energy is Notion / Linear: clean, confident, no decoration. The discipline IS the brand — restraint reads as product taste.

## Provenance

Kiv is a real brand with fixed tokens (not a placeholder reskin). These are the canonical values — do not swap them per video:

- **Canvas:** Pure dark `#0A0A0A`. Not navy (that's Classic), not warm near-black (that's Anthropic), not blue-grey. A true neutral dark so the lime reads as the only color in the frame.
- **Accent — electric lime:** `#E8FF47`. The single accent. Used on the wordmark tile, overlines, hero word, stat numbers, index/date tiles, the URL pill fill, and the progress bar. **There is no second accent.** Adding one breaks the brand.
- **Accent ink:** `#0A0A0A` — the text color placed on top of a lime fill (lime pills, lime cards). Lime is bright; dark ink on it is the only readable pairing.
- **Type:** **Syne** (Google Fonts), the single brand typeface. Weights 400/500/600/700/800. No mono pairing — Kiv uses one family everywhere, which is part of the clean look.
- **White:** `#FFFFFF` primary, `#8A8A8A` dim. Crisp white type on pure dark.

> Kiv ships **no placeholder strings**. The wordmark "Kiv" and the CTA URL `kiv.samkiel.tech` are the real, final values. (Per-video the URL may point to a campaign path, but the apex domain is `samkiel.tech`.)

## Style Prompt

A pure-dark stage for a calm, minimal journaling app. The canvas is true neutral dark; the only color is one electric lime. Type is Syne throughout — geometric, slightly futuristic, confident. Layout is left-aligned and vertically centered, with generous negative space (Notion/Linear, not a busy infographic). One phase visible per frame across a four-act structure; a 240px top safe-zone reserves the small left-aligned "Kiv" wordmark. Motion is crisp and grounded — a slam-in hero word with a tight inline shake, stat cards that rise, feature rows that slide in from the left. No gradients, no film grain, no glow halos. Reads like a beautifully restrained product launch.

## Canvas

- Resolution: **1080 x 1920** (vertical Shorts)
- Frame rate: **30fps**
- Duration target: **24-60s** typical (YT Shorts hard max 180s)
- Background: solid `#0A0A0A`. **No gradients of any kind** (no hero text gradient, no card gradient fills, no radial ambient wash). Flat fills only.

## Colors

| Role | Hex | Notes | Usage |
|---|---|---|---|
| Background | `#0A0A0A` | Pure neutral dark | Page canvas |
| Surface | `#141414` | One step up, hairline-bordered | Stat cards, feature cards |
| Surface elevated | `#161616` | Slightly lifted | Elevated cards |
| Primary text | `#FFFFFF` | Crisp white | Wordmark, hero, headlines, body |
| Secondary text | `#8A8A8A` | Muted grey | Captions, labels, sub-lines, outcome |
| Accent — electric lime | `#E8FF47` | The ONLY accent | Tile, overline, hero, stat numbers, index/date tiles, URL pill, progress bar |
| Accent ink | `#0A0A0A` | Dark text on lime | Text/numbers sitting on a lime fill |
| Hairline | `rgba(255,255,255,0.10)` | Subtle border | Card borders, chip borders |
| Hairline strong | `rgba(255,255,255,0.16)` | Stronger border | Index tile, subscribe pill border |
| Chip fill | `rgba(255,255,255,0.04)` | Neutral chip | Caption pill, index tile, subscribe pill |

**Variable name note:** To stay structurally diffable against the sibling templates (classic / archon / anthropic), the per-element class names from those templates are retained — `.stat-pill.cyan`, `.stat-pill.magenta`, `.tl-card.cyan/.purple/.blue`. In Kiv these classes all render **identically** (flat dark surface + hairline border); the single accent lives on tiles and numbers, not on per-card color rotation. An optional `.stat-pill.accent` class fills a single stat card with lime for emphasis. This keeps the playbook's element targets unchanged while expressing Kiv's single-accent discipline.

**Contrast check (approximate, WCAG):**
- White text on background: ~19:1 (AAA)
- Electric lime on background: ~15:1 (AAA) — extremely high; lime pops hard on pure dark
- Dim grey on background: ~5.2:1 (AA at 30px+)
- Accent ink on lime fill: ~14:1 (AAA)

**Single-accent rule (HARD):** lime is the only color. Never introduce a second hue. Emphasis comes from lime-vs-white-vs-dim and from a lime fill on one element — never from a different color.

## Typography

**Syne only** — one typeface for everything. Loaded via Google Fonts in `<head>`:
`https://fonts.googleapis.com/css2?family=Syne:wght@400;500;600;700;800&display=swap`

| Role | Weight | Treatment |
|---|---|---|
| Hero slam | 800 (Extrabold) | `letter-spacing: -6px`, solid lime, no gradient, no glow |
| Wordmark "Kiv" | 700 (Bold) | white, `letter-spacing: -1px`, 52px |
| Headline | 700-800 | `letter-spacing: -1px` to `-3px`, line-height 0.96-1.06 |
| Stat number | 800 | `letter-spacing: -6px`, lime (or accent-ink on lime card), tabular-nums |
| Card title | 700 | `letter-spacing: -0.5px` |
| Body / pre-line | 500-600 | white or dim |
| Overline | 700 | UPPERCASE, `letter-spacing: 6px`, lime |
| Sub / label / outcome | 500-600 | dim grey |
| URL pill | 700 | accent-ink on lime |

**Type scale (Shorts-tuned):**

| Role | Size |
|---|---|
| Hero slam | 200-240px (drop to 160-190px for 8-10 char words) |
| Hero pre-line | 60-68px |
| Phase headline | 60-64px |
| CTA headline | 88-96px |
| Stat number | 180-200px |
| Card title | 52px (floor 48px — `shorts-typography.md` list-item primary-label rule governs) |
| URL pill | 60-68px |
| CTA pill (`#p4-subscribe`) | 48px (floor 44px) |
| Overline | 32-36px |
| Caption / sub / outcome | 32-36px (32px floor) |

**Minimums** follow [`.claude/rules/shorts-typography.md`](../../../.claude/rules/shorts-typography.md). Tabular numerals on stats: `font-variant-numeric: tabular-nums lining-nums`.

## Layout

**Left-aligned, vertically centered.** Unlike the centered sibling templates, Kiv aligns phase content to the left and centers it vertically (`align-items: flex-start; justify-content: center`). This is the Notion/Linear reading rhythm — text starts at a consistent left margin, eye travels top-to-bottom.

**Safe zones — non-negotiable.** The "Kiv" wordmark sits at `top:72px; left:72px`. Every phase reserves top space for it. A slim lime progress bar sits at the bottom.

```
PHASE_PAD_TOP    = 240px   (clears the top wordmark)
PHASE_PAD_X      = 72px    (side padding — slightly wider than siblings for the airy feel)
PHASE_PAD_BOTTOM = 240px   (clears the progress bar + reading room)
```

`.phase-content` MUST use `width:100%; height:100%; padding:240px 72px 240px; display:flex; flex-direction:column; box-sizing:border-box`. NEVER `position:absolute; top:Npx`.

**Phase mutex:** only one phase visible per frame. Opacity + visibility crossfades on whole phases, not individual elements. 4-12s per phase.

## Motion Language

- **Easing:**
  - `back.out(1.4)`–`back.out(1.6)` for slam-in hero, stat cards, feature rows
  - `power3.out` for headlines and primary text rises
  - `power2.out` for body / chip / overline entrances
  - `power1.inOut` for the phase crossfade
  - **Avoid** `elastic` and `bounce` (toy-like).
- **Duration:** hero 0.6-0.8s · headline 0.5-0.7s · body/chip 0.35-0.5s · crossfade 0.4-0.45s.
- **Direction:** vertical y-rises dominate (`y:+30 → 0`); hero uses `scale:0.80 → 1.0`; feature rows slide from the left (`x:-40 → 0`). No rotation, no scale-pop above 1.06.
- **Inline shake** on the hero impact frame (4 ticks, ±5px). One shake per phase max.
- **Crossfade only — no blur.** The sibling templates blur during phase transitions; Kiv does a clean opacity crossfade (no `filter: blur()`), consistent with the flat aesthetic.
- **No gradient drift.** The hero word is solid lime, so there is no `background-position` animation. `compute_timings.py` still emits `gradient_drift_duration`; it is intentionally unused here.

## Surface Detail

- **Cards:** 20-24px radius, flat `background: #141414`, `border: 1.5px solid rgba(255,255,255,0.10)`. **No gradient fill, no colored box-shadow.** Inner padding 30-44px.
- **Lime accent card (optional):** one stat card may use the `.accent` class — solid lime fill, accent-ink number/label — to create a single point of emphasis. Use at most one per phase.
- **Index / date tile:** solid lime fill, `#0A0A0A` text, 14px radius, Syne 800.
- **URL pill:** solid lime fill, accent-ink text, fully rounded (`border-radius: 9999px`).
- **Caption / subscribe pill:** flat `rgba(255,255,255,0.04)` fill, hairline border, rounded.
- **No ambient layer, no film grain.** The canvas stays pure dark; motion carries the life.

## Audio / SFX Cues

Canonical rules: [`.claude/rules/audio-design.md`](../../../.claude/rules/audio-design.md). Cue files live in [`shared/audio/sfx/`](../../../shared/audio/) (sync via [`scripts/sync-video-sfx.sh`](../../../scripts/sync-video-sfx.sh)).

Narration is one stem. SFX optional and sparse — the minimal aesthetic suits a light touch. **No background music on Shorts.** If you add SFX, a single soft cue on the hero slam and one whoosh-free crossfade beat is plenty; never clutter the calm.

| Cue | Use on | Default `data-volume` |
|---|---|---|
| `impact-slam` | Hero word reveal (optional) | 0.15 |
| `pop` | Feature row / stat card entrance (optional) | 0.10 |
| `cinematic-whoosh` | Phase change (optional, sparse) | 0.11 |

Hard cap: **never** exceed `0.25` on a per-cue SFX.

## What NOT to Do

1. **No gradients — anywhere.** No hero text gradient, no card gradient fills, no radial ambient wash, no gradient borders. This is the single most important Kiv rule. Flat fills only.
2. **No film grain / noise overlay.** The sibling templates ship an SVG `feTurbulence` overlay. Kiv does not. The canvas stays clean.
3. **No glow halos / colored drop-shadows.** No `filter: drop-shadow(... lime ...)`, no `text-shadow` glow on the hero. Flat lime, hard edges.
4. **No second accent.** `#E8FF47` is the only color. No blue, no magenta, no orange, no two-tone anything.
5. **No blur transitions.** Crossfade only.
6. **No second typeface.** Syne everywhere — no JetBrains Mono, no Inter, no mono pairing.
7. **No light canvas.** Pure dark `#0A0A0A`. Don't tint toward grey paper or pure `#000`.
8. **No serif.** Syne only.
9. **No `<br>` in content text.** Use `max-width` for natural wrapping.
10. **No `position: absolute; top: Npx` on `.phase-content`.** Use padding.
11. **No center-aligned phase text** unless a specific layout needs it — Kiv is left-aligned by default.
12. **No all-caps wordmark.** It's "Kiv", not "KIV".
13. **No clutter.** If a frame has more than ~4 distinct elements competing for attention, cut one. Negative space is a feature.

## Thumbnail-grade final frame

Per [`.claude/rules/shorts-thumbnail-final-frame.md`](../../../.claude/rules/shorts-thumbnail-final-frame.md), Phase 4 must hold a thumbnail-grade still ≥1.5s containing: the "Kiv" wordmark (brand chrome), a topic/CTA statement (`#p4-headline`, ≥88px), the URL pill (`#p4-url`, the visual anchor), and an outcome line (`#p4-outcome`). All entrance motion finishes by `P4 + ~1.0s`. The held frame must answer "what is this app and where do I get it" with zero context.
