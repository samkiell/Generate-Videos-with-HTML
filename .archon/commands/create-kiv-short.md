---
description: Spawn a previewable HyperFrames Short from templates/shorts/kiv (dark-minimal Kiv brand) with a research-grounded script + Kokoro TTS, paced to a target duration
argument-hint: (no arguments — reads slug/topic/duration from $parse-input.output)
---

# Create Kiv Short

**Workflow ID**: $WORKFLOW_ID
**Artifacts directory**: $ARTIFACTS_DIR

---

## Inputs (from upstream nodes)

The `parse-input` script node has already normalized the user's message into a
JSON object. Access the fields via dot notation -- Archon substitutes them at
invocation time:

- **Topic**: `$parse-input.output.topic`
- **Slug**: `$parse-input.output.slug`
- **Duration target (seconds)**: `$parse-input.output.duration`
- **Title (rough)**: `$parse-input.output.title`

The `precheck` bash node has already confirmed that `templates/shorts/kiv/`
exists and that `videos/$parse-input.output.slug/` does NOT exist. You can
proceed without re-verifying those facts.

Echo the inputs in your first message so the user sees what's being built.

---

## Mission

Spawn a previewable HyperFrames Short at `videos/$parse-input.output.slug/` in
the **Kiv** dark-minimal aesthetic (pure dark `#0A0A0A`, one electric-lime
accent `#E8FF47`, Syne typeface, Notion/Linear energy). Narration must clock in
at approximately **$parse-input.output.duration seconds** (+-10%). The
composition must lint clean, inspect clean, and open in `hyperframes preview` at
the end.

**You MUST NOT auto-render.** The user always triggers `npx hyperframes render`
manually. Stop at preview.

**You MUST NOT fabricate facts.** Kiv is a product, so most copy is feature
benefits (mood logging, streaks, weekly review, a 60-second habit) you can state
from the brief. Do NOT invent hard metrics (download counts, user counts,
ratings). If the topic implies an external stat you can't source via
`WebSearch` / `WebFetch`, drop it or phrase it as a benefit, not a number.

**You MUST NOT modify `templates/shorts/kiv/` itself.** Only edit the copy
under `videos/$parse-input.output.slug/`.

**Brand strings are FIXED — do NOT treat them as placeholders.** Unlike the
classic template, Kiv ships real brand values:
- The top-banner wordmark is **"Kiv"** — keep it exactly. Never change to "KIV".
- The Phase 4 CTA URL is **`kiv.samkiel.tech`** — keep it (a campaign path under
  `samkiel.tech` is acceptable only if the user explicitly asks). Do NOT use
  `usekiv.com` or any other domain.

**Single-accent / no-gradient / no-noise discipline (HARD).** Lime is the only
color. No gradient text fills, no gradient card fills, no ambient radial wash,
no film-grain overlay, no glow halos / colored drop-shadows, no blur
transitions. See `templates/shorts/kiv/DESIGN.md` "What NOT to Do".

---

## Authoritative Playbook

The end-to-end recipe lives at
[`.claude/skills/diy-yt-creator/new-kiv-short.md`](../../.claude/skills/diy-yt-creator/new-kiv-short.md).
**Read it now, in full.** It covers every step: template copy, `meta.json`
update, script drafting rules, hero-word fit, the Kokoro TTS call, transcript-
driven phase boundary computation, the exact `index.html` edit order (including
which `compute_timings.py` outputs are intentionally UNUSED in Kiv), the
lint/inspect fix table, and the report format.

Sibling rules the playbook depends on — read them too:

- [`CLAUDE.md`](../../CLAUDE.md) — repo-wide rules and scope discipline.
- [`templates/shorts/kiv/DESIGN.md`](../../templates/shorts/kiv/DESIGN.md) — the Kiv visual system (single accent, no gradients, no noise, Syne).
- [`templates/shorts/kiv/PRONUNCIATION.md`](../../templates/shorts/kiv/PRONUNCIATION.md) — `Kiv`, `60 seconds` vs `60s`, the live/log heteronyms.
- [`.claude/rules/shorts-typography.md`](../../.claude/rules/shorts-typography.md) — minimum font sizes for 1080x1920.
- [`.claude/rules/shorts-thumbnail-final-frame.md`](../../.claude/rules/shorts-thumbnail-final-frame.md) — the final frame MUST be thumbnail-grade.
- [`.claude/rules/step-by-step-reveal.md`](../../.claude/rules/step-by-step-reveal.md) — enumerated lists reveal item-by-item.
- [`.claude/rules/visual-pacing-5s.md`](../../.claude/rules/visual-pacing-5s.md) — never let the foreground stay static more than 5s.
- [`.claude/rules/tts-pronunciation.md`](../../.claude/rules/tts-pronunciation.md) — heteronym audit before TTS.

Invoke the `/hyperframes` skill before editing `index.html`.

---

## Duration Discipline ($parse-input.output.duration second target)

For a **30-second default** target, map narration to the four phase archetypes
roughly as follows. Scale linearly for other targets:

| Phase | Share of total | At 30s | Narration content |
|---|---|---|---|
| 1 -- Hero hook | ~22% | ~6.5s | Overline, pre-line, ONE slam word/phrase, caption pill |
| 2 -- Stat row | ~22% | ~6.5s | Overline, headline, two product-true numbers with labels |
| 3 -- Feature cards | ~33% | ~10s | Overline + 3 step cards (log -> streak -> review) |
| 4 -- CTA | ~17% | ~5s | Overline, CTA headline, URL pill (kiv.samkiel.tech), outcome line |
| Tail (no narration) | ~6% | ~1.5s | Reading time on the held thumbnail frame |

These are guidelines -- let the topic's natural cadence drive the actual seconds.
Total narration MUST land within +-10% of the target. If the topic is too rich,
narrow the angle rather than running long.

**Speaking-rate reference**: ~2.3 words/sec of comfortable English at the shorts
speed, with `<break time="0.35s"/>` between sentences. For a 30s target budget
~70 words (+-10); 45s -> ~105 words; 60s -> ~140 words. Break tags add ~3-4s of
silence on top of spoken words.

---

## Step-by-step

Follow the playbook at
`.claude/skills/diy-yt-creator/new-kiv-short.md` in order. The exact parameter
substitutions for THIS run:

- Step 1 (slug + title) -- done. Use `$parse-input.output.slug` and the title
  `$parse-input.output.title` (refine to a punchier headline if awkward).
- Step 2 (copy template) -- run:
  ```bash
  cp -r templates/shorts/kiv "videos/$parse-input.output.slug"
  ```
  PowerShell: `Copy-Item -Recurse templates/shorts/kiv videos/$parse-input.output.slug`
- Step 3 (meta.json) -- overwrite with:
  ```json
  {
    "id": "$parse-input.output.slug",
    "name": "<refined title>"
  }
  ```
- Step 4 (draft the script) -- target ~`$parse-input.output.duration` seconds
  per the table above. Save to `videos/$parse-input.output.slug/script.txt`.
  Write `60 seconds` (not bare `60s`) and `Kiv` as a word (not `K I V`).
- Step 4.5 (ground in real source content) -- only if the user claims an
  external stat. Otherwise state product benefits. Never preserve a fabricated
  number.
- **Pre-step (TTS pronunciation pass -- MANDATORY before TTS).** Audit the
  script against, narrowest-wins:
  1. `templates/shorts/kiv/PRONUNCIATION.md` (Kiv brand name, `60 seconds`,
     live/log heteronyms) -- overrides the generic rules.
  2. `.claude/rules/tts-pronunciation.md` -> "Acronym vs Word".
  3. `.claude/rules/tts-pronunciation.md` -> "Tech & brand pronunciation
     pitfalls" + the Heteronyms table.
  Prepend the leading HTML comment from `PRONUNCIATION.md` to
  `videos/$parse-input.output.slug/script.txt`.
- Step 5 + 6 (TTS + transcript, single command) -- **default to Kokoro**
  (free, local). Run:
  ```bash
  python scripts/kokoro-tts.py videos/$parse-input.output.slug --shorts
  ```
  Writes `audio/narration.wav` + `transcript.json` (word-level timestamps) in
  one call. No separate transcribe step. Optional env (all defaulted):
  `KOKORO_VOICE` (default `af_heart`), `KOKORO_LANG_CODE` (default `a`),
  `KOKORO_SPEED_SHORTS` (default `1.15`). If `kokoro`/`soundfile` is missing,
  surface `pip install kokoro soundfile numpy` and that `espeak-ng` is required
  system-wide, then stop.

  **Alternative:** if the user has `ELEVENLABS_API_KEY` set and prefers it:
  ```bash
  python scripts/elevenlabs-tts.py videos/$parse-input.output.slug --shorts --no-chunk
  ```
  Same output contract.
- Step 7 (compute phase boundaries) -- run:
  ```bash
  python scripts/compute_timings.py videos/$parse-input.output.slug
  ```
  Show the user the computed values. NOTE: `gradient_drift_duration` and
  `ambient_breath_half_period` are emitted but **intentionally unused in Kiv**
  (no gradient drift, no ambient layer) -- do not add tweens for them.
- Step 8 (edit `index.html`) -- follow the 12 sub-steps in the playbook. **Run
  `/hyperframes` first.** Keep the "Kiv" wordmark and `kiv.samkiel.tech` URL.
  Hero is **solid lime** (no gradient). Cards stay uniform (no color rotation);
  one stat pill may use `.accent` (lime fill). Crossfade transitions only (no
  blur). The final phase MUST satisfy `shorts-thumbnail-final-frame.md`.
- Step 9 (lint) -- `npx hyperframes lint videos/$parse-input.output.slug` must
  report 0 errors. Iterate from the playbook's error table until clean.
- Step 10 (inspect) -- `npx hyperframes inspect videos/$parse-input.output.slug`
  must show no overflow. Fix at content level (shorten words) before CSS.
- Step 11 (preview) -- run in the **background**:
  ```bash
  npx hyperframes preview videos/$parse-input.output.slug
  ```
  Capture the URL it prints (read the port from CLI output).
- Step 12 (report) -- see the report format below.

---

## Required outputs

Before declaring the workflow complete, all of the following MUST be true:

- [ ] `videos/$parse-input.output.slug/script.txt` exists, paced to
      `$parse-input.output.duration` +-10%.
- [ ] `videos/$parse-input.output.slug/audio/narration.wav` exists and is non-empty.
- [ ] `videos/$parse-input.output.slug/transcript.json` exists with word-level timestamps.
- [ ] `videos/$parse-input.output.slug/index.html` edited: every demo placeholder
      ("60s.", "Small enough to actually do.", "Log how you feel", etc.) replaced
      with real content; transition timestamps + slam-shake offsets recomputed
      from the transcript; `<audio id="narration">` wired. Wordmark stays "Kiv";
      URL stays `kiv.samkiel.tech`.
- [ ] `videos/$parse-input.output.slug/meta.json` has the real slug and refined title.
- [ ] `npx hyperframes lint videos/$parse-input.output.slug` -> **0 errors**.
- [ ] `npx hyperframes inspect videos/$parse-input.output.slug` -> **no overflow**.
- [ ] `npx hyperframes preview videos/$parse-input.output.slug` launched in the
      background and the URL captured.
- [ ] No gradients, no noise overlay, no glow, single lime accent, Syne only.
- [ ] Final phase is thumbnail-grade (wordmark + CTA headline + URL pill +
      outcome, held still >=1.5s).

If any item fails, fix it before reporting. **Don't claim success on a
half-built composition.**

---

## Artifact write-back

Write a short summary to
`$ARTIFACTS_DIR/create-kiv-short-summary.md`:

```markdown
# create-kiv-short -- $parse-input.output.slug

- **Topic**: $parse-input.output.topic
- **Duration target**: $parse-input.output.duration s
- **Actual narration length**: <X.X> s (from transcript)
- **Total composition length**: <Y.Y> s
- **Voice**: (voice id from .env, e.g. Kokoro af_heart)
- **Preview URL**: http://localhost:<port>
- **Render command** (manual): `npx hyperframes render videos/$parse-input.output.slug -o videos/$parse-input.output.slug/out/$parse-input.output.slug.mp4`
- **CTA URL used**: kiv.samkiel.tech
- **Sources used** (one per claim, if any external stats):
  - <URL 1 -- what it backed>
- **Content tradeoffs** (if any): e.g. "shortened hero from MOMENTS to 60s. for 220px fit"
```

---

## Final report to the user

End with one concise message:

```
videos/$parse-input.output.slug/ ready for preview.

- Topic: $parse-input.output.topic
- Duration: <total_duration>s (target was $parse-input.output.duration s)
- Voice: (voice id from .env)
- Preview: <localhost URL>

Render manually with:
  npx hyperframes render videos/$parse-input.output.slug -o videos/$parse-input.output.slug/out/$parse-input.output.slug.mp4

<one-line note on any content tradeoff, if any>
```

Stop. Do not render. Do not push. Wait for the user to iterate or trigger
render themselves.
