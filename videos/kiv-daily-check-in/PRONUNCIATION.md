# TTS Pronunciation Map — Kiv Shorts Template

Per-template overrides the script writer MUST apply before writing `videos/<slug>/script.txt`. Kiv is a consumer journaling app, so the token set is small — the main risk is the brand name itself and a couple of habit/wellness words.

Authority order, narrowest wins:
1. This file (Kiv-specific decisions)
2. `.claude/rules/tts-pronunciation.md` → "Acronym vs Word — disambiguation for brand / workflow tokens"
3. `.claude/rules/tts-pronunciation.md` → "Tech & brand pronunciation pitfalls" (generic table)

## Decisions for Kiv tokens

| Token | Spoken as | TTS spelling in `script.txt` | Reason |
| ----- | --------- | ----------------------------- | ------ |
| `Kiv` | "kiv" (rhymes with "give") | `Kiv` | The brand name. One syllable, short-i. Kokoro/edge-tts read `Kiv` correctly as a word — do NOT spell it `K I V` (that turns the brand into an initialism "kay-eye-vee"). If a voice ever says "kive" (long-i), respell as `Kihv` in script.txt only. |
| `60s` / `60 seconds` | "sixty seconds" | `60 seconds` | Write the full phrase, not `60s` — TTS may read the bare `60s` as "sixties". The visual stat pill shows `60`; the narration says "60 seconds". |
| `check-in` | "check in" | `check-in` | Reads natively. Hyphen is fine. |
| `journaling` | "JUR-nuh-ling" | `journaling` | Reads natively. |
| `streak` | "streek" | `streak` | Reads natively. |
| `app` | "app" | `app` | Reads natively; never spell out. |
| `AI` | "ay-eye" (native) | `AI` | Only if the script mentions AI features. Native — do NOT space as `A I`. |

## Heteronym watch (from `.claude/rules/tts-pronunciation.md`)

Kiv's journaling copy naturally reaches for a few flagged heteronyms — audit these:

- **"live"** — for the launch/availability sense ("Kiv is live"), prefer `available` / `out today` so it never reads as /lɪv/ ("to live"). Journaling copy also uses the verb sense ("how you live your week") — that one is correct as `live`; just keep the senses unambiguous in context.
- **"read"** — "look back and read your week" reads fine in past/present from context; if it leads a sentence ambiguously, rephrase to `look back on`.
- **"record"** — if used as the verb ("record how you feel"), prefer `log` / `capture` to avoid the noun stress. Kiv's own verb is "log", which is unambiguous — use it.

## Required leading comment in every Kiv-template script

Phase 2a MUST prepend this HTML comment to `videos/<slug>/script.txt`. It is stripped before TTS (HTML comments are inert whitespace to the engine; `kokoro-tts.py` strips them explicitly). It exists so any human editing the script sees the decisions:

```html
<!-- TTS pronunciation overrides — see templates/shorts/kiv/PRONUNCIATION.md
     - Kiv: read as one word "kiv" (rhymes with give) — never spell "K I V"
     - "60 seconds": write the full phrase, never bare "60s" (reads as "sixties")
     - "live" (availability sense): prefer "available"/"out today" to avoid /lɪv/
     - "log": Kiv's verb for journaling — unambiguous, prefer over "record"
     - AI: native "ay-eye" — do NOT space as "A I"
-->
```

## Adding a new token to this map

When a regen cycle reveals a new mispronunciation on this template:

1. Add the row to the table above with the spoken form + script-spelling + reason.
2. If it overrides the generic rule in `tts-pronunciation.md`, note that in the reason column.
3. Update the leading-comment block above so the script writer writes the new entry into future scripts.
4. Commit this file in the same PR as the regenerated `videos/<slug>/script.txt`.
