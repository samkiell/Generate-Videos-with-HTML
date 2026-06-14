# new-kiv-short — Playbook

End-to-end pipeline that turns a **topic prompt** into a previewable HyperFrames Short for **Kiv** — a dark, minimal micro-journaling app (pure-dark canvas `#0A0A0A`, one electric-lime accent `#E8FF47`, Syne typeface, Notion/Linear energy). Zero manual steps; ends at preview, never renders.

This is a **brand template** (Kiv is a real brand with fixed tokens), so it differs from the brand-neutral `new-classic-short.md` in three ways:

- **No placeholder strings to replace.** The wordmark "Kiv" and the CTA URL `kiv.samkiel.tech` are the real, final values — do NOT swap them per video (unlike classic's "YOUR BRAND" / "your-domain.com").
- **Single-accent, no-gradient, no-noise discipline.** Lime is the only color; there are no gradient text fills, no card gradients, no film-grain overlay, no glow halos, no blur transitions. See [`templates/shorts/kiv/DESIGN.md`](../../../templates/shorts/kiv/DESIGN.md) "What NOT to Do".
- **Kokoro TTS by default** (free, local) — same engine selection as classic.

The structural shape (phase IDs, timing constants, `compute_timings.py` math) is identical to the sibling playbooks, so if you've run `new-classic-short.md` the steps will feel familiar.

## Inputs

User provides ONE of:

- A **topic / prompt** (e.g. _"Kiv — your daily check-in. Log how you feel, track your streak."_) — agent drafts the full script
- A **title + key facts** — agent uses the facts verbatim
- A **pre-written `script.txt`** — agent skips drafting (jump to step 5)

Kiv is a product, not a research topic — most facts are product features (mood logging, streaks, weekly review, 60-second habit), not external stats that need web verification. **Do not fabricate hard numbers** (download counts, user counts, ratings) unless the user provides them. If the topic implies a stat you can't source, drop it or phrase it as a benefit, not a metric.

## Outputs

A previewable HyperFrames project at `videos/<slug>/` with:

- `script.txt` — narration script (with `<break>` tags + the Kiv pronunciation comment)
- `audio/narration.wav` — Kokoro TTS narration (or ElevenLabs if configured)
- `transcript.json` — word-level timestamps
- `index.html` — composition filled with real content, transitions sync'd to spoken-word frames
- Preview studio open in the browser at the URL printed by `hyperframes preview`

The user runs render themselves: `npx hyperframes render videos/<slug> -o videos/<slug>/out/<slug>.mp4`.

---

## Steps

### 1. Confirm slug + title

Derive a kebab-case slug from the topic (3-6 words, no stopwords). Examples:

- "Kiv — your daily check-in" → `kiv-daily-check-in`
- "Track your mood in 60 seconds" → `kiv-track-mood-60-seconds`

Confirm in one line if the user hasn't already given you both. Skip confirmation if they have.

### 2. Copy the template

```bash
cp -r templates/shorts/kiv videos/<slug>
```

PowerShell: `Copy-Item -Recurse templates/shorts/kiv videos/<slug>`

Verify the copy: `videos/<slug>/index.html`, `meta.json`, `hyperframes.json`, `DESIGN.md`, `README.md`, `PRONUNCIATION.md`, `audio/`, `assets/`, `compositions/` should all exist.

### 3. Update meta.json

```json
{
  "id": "<slug>",
  "name": "<Title Case Name>"
}
```

### 4. Draft the script

Map narration to the four phase archetypes the template ships:

| Phase | Duration target | What to write |
|---|---|---|
| **1 — Hero hook** | 5-7s | Overline (2-3 words, ALL CAPS): the section label. Pre-line (4-7 words): the setup. ONE slam word/short phrase (**≤7 wide characters at 220px** — see "Hero word fit"): the payoff. Caption pill (3-6 words): the receipt. |
| **2 — Stat row** | 5-7s | Overline. Headline (5-9 words). Two numbers with 2-3 word labels. Numbers should be product-true (e.g. `60` seconds, `1` tap) — not fabricated download/user counts. |
| **3 — Feature cards** | 6-8s | Overline. 3 cards: index (`01/02/03`), title (3-5 words), sub (3-6 words). For Kiv these are the steps: log → streak → review. |
| **4 — CTA / thumbnail hold** | 4-6s | Overline. CTA headline (3-5 words, e.g. "Start journaling free."). URL pill = `kiv.samkiel.tech`. Outcome line (1 short sentence). |

**Hero word fit:** at 220px Syne 800 with `letter-spacing: -6px`, the hero fits ~7 wide characters before overflowing the 936px max-width. For 8-10 char words, drop `#p1-hero` font-size to 160-190px in step 8 — the CSS has a comment marking the line. Or shorten via synonym. Short numeric/punchy heroes work great for Kiv: `60s.`, `Daily.`, `One tap.`

**Style rules for narration text:**

- Short sentences. Periods = full stops; commas = mid-sentence breaths.
- **`<break time="...s"/>` SSML tags between sentences are mandatory.** (Kokoro strips them and inserts deterministic 0.4s phase-boundary silence on blank lines; ElevenLabs honors them inline. Either way, keep them — they document phase structure and pace ElevenLabs if that engine is used.)
- Numbers: write the spoken phrase. Per [`templates/shorts/kiv/PRONUNCIATION.md`](../../../templates/shorts/kiv/PRONUNCIATION.md), write `60 seconds` (full phrase), NOT bare `60s` (reads as "sixties"). The visual stat pill shows `60`; the narration says "60 seconds".
- **Brand name:** write `Kiv` as a word — never `K I V`.
- **Selective CAPS** on 1-2 power words per phase max. The hero slam is always its natural case (often a number like `60s.`).
- Total target: 24-45s of narration; longer is fine for content-dense topics.

**Break tag layout:**

| Position | `<break time>` | Why |
|---|---|---|
| Between sentences inside a phase | `0.35s` | Natural sentence-final intonation |
| Between list items (3 cards in phase 3) | `0.45s` | Marks each item as discrete |
| **End of each phase** | `0.4s` | Critical: longer causes a re-entry artifact |

Save to `videos/<slug>/script.txt`, one phase per blank-line block with inline breaks:

```
[phase 1 sentence one]. <break time="0.35s"/> [phase 1 sentence two]. <break time="0.4s"/>

[phase 2 narration]. <break time="0.4s"/>

[phase 3 narration]. <break time="0.4s"/>

[phase 4 narration].
```

The blank lines are NOT spoken — they map narration to phases AND (for Kokoro) become the 0.4s phase-boundary silence.

### 4.5. (Optional) Ground in real source content

**Skip if:** the topic is product-feature copy you can state from the brief (the common Kiv case), OR the user gave full facts verbatim.

**Use when:** the user claims an external stat (a rating, a study, a comparison) — verify it with `WebSearch`/`WebFetch` or drop it. Never preserve a fabricated number.

**Pre-step (TTS pronunciation pass — MANDATORY before TTS).** Audit the draft against three sources, narrowest wins:

1. **`templates/shorts/kiv/PRONUNCIATION.md`** — Kiv-specific (the brand name `Kiv`, `60 seconds` vs `60s`, the "live"/"log" heteronym guidance). Overrides the generic rules.
2. **`.claude/rules/tts-pronunciation.md`** → "Acronym vs Word".
3. **`.claude/rules/tts-pronunciation.md`** → "Tech & brand pronunciation pitfalls" + the Heteronyms table.

Prepend the leading HTML comment from `PRONUNCIATION.md` to `videos/<slug>/script.txt`.

### 5. Generate TTS

**Default: Kokoro (free, local).** Use unless the user has explicitly chosen ElevenLabs:

```bash
python scripts/kokoro-tts.py videos/<slug> --shorts
```

Open-weight, Apache-licensed, runs on CPU, no API key. First run downloads a ~325MB model. Native word-level timestamps come back inline — **no separate transcribe step**. Requires `pip install kokoro soundfile numpy` and `espeak-ng` system-wide.

Optional env (loaded from `.env` and `.archon/.env`, all with defaults):
- `KOKORO_VOICE` (default `af_heart` — American female; warm/calm fits Kiv's tone)
- `KOKORO_LANG_CODE` (default `a`)
- `KOKORO_SPEED_SHORTS` (default `1.15`)

**Alternative: ElevenLabs** (paid, if the user configured `ELEVENLABS_API_KEY` and prefers it):

```bash
python scripts/elevenlabs-tts.py videos/<slug> --shorts --no-chunk
```

Same output contract either way: `videos/<slug>/audio/narration.wav` + `videos/<slug>/transcript.json` (flat `[{word,start,end}]`). `compute_timings.py` is engine-agnostic.

### 6. ~~Transcribe~~ (SKIPPED — step 5 emits the transcript inline)

### 7. Compute phase boundaries

```bash
python scripts/compute_timings.py videos/<slug>
```

Prints every timing value: `phase_ends`, `total_duration` (= phase4_end + 1.5), `narration_data_duration` (= phase4_end), `T1/T2/T3` (= phaseNe - 0.2), `P2/P3/P4` (= TN + 0.4), `slam_t`, `hero_entrance` (= slam_t - 0.4), `shake_offsets`, `gradient_drift_duration` (**emitted but UNUSED in Kiv — there is no gradient drift**), `ambient_breath_half_period` (**also unused — Kiv has no ambient layer**).

Show the user the computed values in an interim status update.

### 8. Edit `videos/<slug>/index.html`

Always invoke the `/hyperframes` skill before this step. Edit in this exact order (one Edit per change):

1. **`<title>`** → the video title.
2. **`<div id="root">`** `data-duration` → `total_duration` (rounded to 0.1s).
3. **Wordmark** — leave `#top-banner-wordmark` as **"Kiv"**. Do NOT change it (real brand mark).
4. **Phase 1**: `#p1-overline`, `#p1-pre`, `#p1-hero` (slam — ≤7 chars at 220px, or drop to 160-190px in CSS), `#p1-caption`. The hero is **solid lime** — there is no gradient fill to manage.
5. **Phase 2**: `#p2-overline`, `#p2-headline`, both `.stat-pill` blocks (`.stat-num` + `.stat-label`). One pill may keep the `.accent` class (lime fill) for emphasis; the other stays flat dark. Do NOT add color rotation.
6. **Phase 3**: `#p3-overline`, all three `.tl-card` blocks (`.tl-date`, `.tl-title`, `.tl-sub`). Cards stay **uniform** (no per-card color). The lime lives on the `.tl-date` tiles + `.tl-index` text.
7. **Phase 4**: `#p4-overline`, `#p4-headline` (CTA), `#p4-url` (keep `kiv.samkiel.tech` — or a campaign path under `samkiel.tech` if the user specifies), `#p4-outcome`, `#p4-subscribe`.
8. **Transition timestamps**: replace `const T1 = 5.6;`, `const T2 = 11.6;`, `const T3 = 17.6;` with computed values.
9. **Phase anchors**: replace `const P2 = 6.4;`, `const P3 = 12.4;`, `const P4 = 18.4;` with computed values.
10. **Progress bar tween**: set `duration` (in `#progress-fill` `fromTo`) to `narration_data_duration` (= `phase4_end`), NOT `total_duration`. The bar must reach full BEFORE the thumbnail hold and then stay static — any motion during the final held frame violates `shorts-thumbnail-final-frame.md`. (The template ships this comment inline.)
11. **Hero entrance + slam shake** (both tied to `slam_t`):
    - **Entrance**: replace the `tl.from("#p1-hero", { scale: 0.80, ... }, 1.4)` anchor with `hero_entrance`.
    - **Shake**: replace the `slam_t` value and the four `tl.to("#p1-hero", { x: ... })` offsets with `shake_offsets`.
12. **Audio element**: insert just before `</div>` (closing tag of `#root`), at the same indent as `<div id="phase4">`:

```html
  <audio id="narration"
         class="clip"
         src="audio/narration.wav"
         data-start="0"
         data-duration="<phase4_end>"
         data-track-index="2"
         data-volume="1"></audio>
```

> **Kiv has no ambient-breath tween and no gradient-drift tween** (the sibling templates do). So unlike classic, there is NOTHING to retime for `ambient_breath_half_period` or `gradient_drift_duration` — those values from `compute_timings.py` are intentionally unused here. Don't add an ambient or gradient tween to "match" the other templates.

### 9. Lint

```bash
npx hyperframes lint videos/<slug>
```

Must report `0 errors`. Common fixes:

| Error | Likely cause | Fix |
|---|---|---|
| `audio_src_not_found` (narration) | `narration.wav` missing / path wrong | Confirm `videos/<slug>/audio/narration.wav` exists; paths are case-sensitive |
| `overlapping_gsap_tweens` | Two tweens hit the same prop at the same time | Add `overwrite: "auto"` to the later tween, or shift its start |
| `missing_track_index` | Timed element without `data-track-index` | Add it (2 for narration) |

Warnings are advisory.

### 10. Inspect for layout overflow

```bash
npx hyperframes inspect videos/<slug>
```

Common Kiv overflows:
- **Hero too wide** — 220px + a long word exceeds 936px. Drop `#p1-hero` to 160-190px or shorten.
- **Stat label wraps** — labels >16 chars overflow the 458px card. Shorten.
- **Card title overflows** — 42px + a long title overflows. Shorten or drop title to 38px.

Fix overflow at the content level (shorten words) before adjusting CSS.

### 11. Open preview (final step — never render)

Run in the background so the studio stays open:

```bash
npx hyperframes preview videos/<slug>
```

Capture the URL it prints (read the port from CLI output — may shift if 5173 is taken).

### 12. Report to the user

One concise message: slug + path, total duration, voice (from `.env`), preview URL, the render command (do NOT run it), and any content tradeoffs from inspect.

---

## Quality bar — required before reporting done

- [ ] `npx hyperframes lint videos/<slug>` → 0 errors
- [ ] `npx hyperframes inspect videos/<slug>` → no overflow on hero, stat cards, or feature cards
- [ ] All four phases have real content (no leftover demo text: "60s.", "Small enough to actually do.", "Log how you feel", etc.)
- [ ] Wordmark is "Kiv" (kept) and URL is `kiv.samkiel.tech` (kept)
- [ ] Phase transition timestamps computed from transcript, NOT left at template defaults
- [ ] Audio element wired with correct `data-duration`
- [ ] Hero fits at the chosen `#p1-hero` font-size
- [ ] **No gradients anywhere** (no hero text gradient, no card gradient fills, no ambient wash)
- [ ] **No film grain / noise overlay**, **no glow halos / colored drop-shadows**
- [ ] **Single accent** — only `#E8FF47` lime; no second hue
- [ ] **Syne only** — no second typeface
- [ ] Final frame is thumbnail-grade (wordmark + CTA headline + URL pill + outcome, held still ≥1.5s) per [`.claude/rules/shorts-thumbnail-final-frame.md`](../../../.claude/rules/shorts-thumbnail-final-frame.md)
- [ ] Preview URL is reachable

If any item fails, fix it before reporting. Don't claim success on a half-built composition.

## Don'ts

- Never auto-render — user always triggers render manually.
- Never fabricate stats/ratings/user counts. State product benefits, not invented metrics.
- Never modify `templates/shorts/kiv/` — only the copy under `videos/<slug>/`.
- Never add a gradient (text fill, card fill, or ambient) — Kiv is flat by design.
- Never add a film-grain / noise overlay or a colored glow/drop-shadow.
- Never introduce a second accent color — `#E8FF47` is the only one.
- Never use a second typeface — Syne everywhere.
- Never use a blur phase transition — crossfade only.
- Never change the "Kiv" wordmark to "KIV" or "kiv", and never swap the `kiv.samkiel.tech` URL for a placeholder.
- Never use `Math.random()` / `Date.now()` in the composition (HyperFrames is deterministic).
- Never write `<br>` in content text — use `max-width`.
- Never animate `visibility`/`display` via tween — use opacity. `tl.set({visibility: ...})` IS allowed.
- Never skip the `/hyperframes` skill before editing the composition HTML.
- Never run multiple `new-kiv-short` invocations in parallel against the same slug.
