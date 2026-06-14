# kiv-daily-check-in — build notes

First Kiv video. Spawned from `templates/shorts/kiv/` (the dark-minimal Kiv brand
template) for the daily check-in topic.

- **Topic**: Kiv — your daily check-in. Log how you feel, track your streak, look
  back on your week. Takes 60 seconds.
- **Aesthetic**: pure dark `#0A0A0A`, single electric-lime accent `#E8FF47`, Syne
  typeface. No gradients, no noise, no glow — flat by design.
- **TTS**: Kokoro (free, local), voice `af_heart`, `--shorts` speed 1.15, generated
  under the Python 3.12 `.venv-tts` (Kokoro's G2P stack `misaki` does not support
  Python 3.13).
- **Script note**: the CTA domain is spoken as "kiv dot samkiel dot tech" so the
  TTS reads it cleanly; the on-screen URL pill shows `kiv.samkiel.tech`.
- **Phase map**:
  - P1 Hero — "Your whole day, captured in **60s.**" / caption "Meet Kiv."
  - P2 Stats — "Most journaling asks too much." → 60 seconds a day · 1 tap to check in
  - P3 Cards — Log how you feel → Build your streak → Look back weekly
  - P4 CTA — "Start journaling free." → `kiv.samkiel.tech` (thumbnail-grade hold)

## Render (manual)

```bash
npx hyperframes render videos/kiv-daily-check-in -o videos/kiv-daily-check-in/out/kiv-daily-check-in.mp4
```
