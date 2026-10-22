# Codex Reset Forecast Lab · Day 1–28

[English](README.md) · [简体中文](README.zh-CN.md)

A standalone, browser-based dashboard for exploring the 28-day Codex reset calendar: recorded events, community polls, historical forecast scores, and adjustable reset probabilities. It distinguishes a reset announcement from delivery of the announced credits, so delayed delivery is not counted as a separate new reset.

**Open the dashboard:** [English](https://xiasan1992.github.io/codex-reset-predictor/Codex_Reset_Day1-28_Live_Predictor_EN.html) · [中文](https://xiasan1992.github.io/codex-reset-predictor/Codex_Reset_Day1-28_Live_Predictor.html)

## Features

- A Day 1–28 timeline covering October 5–November 1, 2026, using the source snapshot's Pacific calendar dates.
- Separate states for unknown, announced, delivered Global/Full reset, delivered Banked reset, and improvement days.
- Recursive forecasts for future days, using a first-order Markov model with Beta(2,2) smoothing and an adjustable community-poll coefficient.
- Recorded Day 13–16 forecast scores, including classification accuracy and mean Brier score.
- Public-source synchronization attempts on opening and every 10 minutes while enabled and the page remains open.
- Manual editing, pasted-calendar parsing, JSON import/export, and browser-local storage.
- Dark/light appearance, font size, background color, and content-width controls.

## Use

Open either hosted dashboard above, or download the corresponding HTML file and open it in a modern browser. No installation, build step, backend, or API key is required. The English HTML is self-contained and can be used independently of the Chinese HTML.

If cross-origin restrictions or unavailable sources prevent sync, the embedded snapshot and forecasts remain usable. Paste source calendar text into the sync panel or edit a day manually. Export JSON to back up browser-local records. Both language editions use the same storage keys on the same web origin; existing user-entered notes retain their original language. Local files and the hosted site may have separate browser storage.

## Model

For adjacent known days, the implementation counts reset-to-reset (`rr`), reset-to-improvement (`ri`), improvement-to-reset (`ir`), and improvement-to-improvement (`ii`) transitions. Announced, Global/Full, and Banked reset states all count as reset-type events for this model, while remaining distinct display states.

```text
pRR = (rr + 2) / (rr + ri + 4)
pIR = (ir + 2) / (ir + ii + 4)
raw = previous_reset_probability * pRR
    + (1 - previous_reset_probability) * pIR
forecast = clamp(raw + coefficient * (reset_vote_share - 0.5), 0.08, 0.92)
```

The default coefficient is 0.08, giving at most ±4 percentage points of poll adjustment. It is manually chosen, not statistically fitted. Future dates propagate the preceding probability rather than assuming a known outcome.

## Data and scope

The supplied file labels its embedded snapshot **October 22, 2026, 04:23 Pacific Time**. Its event descriptions, votes, and historical forecasts are preserved from that file; this release does not independently authenticate them or establish an immutable preregistration record. The historical score uses only four embedded forecasts and does not demonstrate calibrated predictive performance. The displayed initial Day 18 forecast is not part of that four-record score, and new daily forecasts are not automatically appended to a scored ledger.

This is an unofficial dashboard, not affiliated with OpenAI. Forecasts are subjective statistical scenarios, not official reset promises. Sync depends on public source availability and browser CORS behavior. Closing the page stops monitoring; it sends no proactive notifications. Public proxy fallbacks may be used to fetch the public calendar. Live sync updates votes and recognized states; event notes and links may still reflect the embedded snapshot or manual edits.

Source URLs referenced by the app:

- [Community 28-day calendar](https://didcodexreset.com/oct28.html)
- [Public Reset API information](https://didcodexreset.com/api.html)
- [Tibo account linked by the original file](https://x.com/thsottiaux)

## Files

- `Codex_Reset_Day1-28_Live_Predictor.html` — original Chinese file, preserved byte for byte.
- `Codex_Reset_Day1-28_Live_Predictor_EN.html` — English interface, event descriptions, and messages, retaining the model and data.
- `index.html` — bilingual entry page.
- `README.md` / `README.zh-CN.md` — English / Chinese introductions and usage notes.
- `.nojekyll` — serves the repository as plain static files on GitHub Pages.

Release prepared on October 22, 2026. Publication of the source snapshot does not imply its records are current.
