# Zentence — Public Prediction Log

Every forecast [zentence](https://zentence.ai) makes on a *mention market* — will a given word or
phrase be said during a scheduled speech event (an earnings call, an FOMC presser) — logged here
**before the event**, and scored against reality **after**.

This repository is the track record. It exists so that where we are wrong is as visible as where
we are right.

## How to read it

| File | What it is | When |
|------|------------|------|
| `predictions.csv` / `.json` | Our probability vs. the market price | **Before** the call |
| `resolved.csv` / `.json` | Resolved forecasts, with outcome + Brier | **After** the call |
| `scorecard.md` / `.json` | Aggregate calibration: Brier, skill, error | Each update |

The `.json` twins are for programmatic use — the site reads them directly, no backend.

## Load it in a browser (no server)

The files are plain static JSON on GitHub, so a frontend can `fetch` them straight off a CDN:

```js
const res = await fetch(
  "https://cdn.jsdelivr.net/gh/Zentence-Research/predictions@main/resolved.json"
);
const rows = await res.json();   // array of resolved forecasts
```

jsDelivr is a free, CORS-enabled CDN over GitHub. It caches for up to ~12h; on a fresh push,
purge with `https://purge.jsdelivr.net/gh/Zentence-Research/predictions@main/resolved.json`.

**The commit history is the proof.** Each `predictions.csv` update is committed before the call it
forecasts, so the timestamps show the forecast predated the outcome. No hindsight, no
cherry-picking.

## What's *not* here

- **The model is closed source.** This repo is the output, not the method.
- **No transcripts.** We never redistribute the source speech transcripts — only our forecasts and
  public market data.

## Columns

- `model_p` — our calibrated probability the phrase is said at least `threshold` times.
- `model_p_raw` — before isotonic recalibration against resolved history.
- `market_mid` — the Kalshi market's implied probability (mid of yes bid/ask) at snapshot time.
- `model_minus_market` — the gap: where we disagree with the market, and by how much.
- `reflexive` — flagged when the phrase could be prompted by an interviewer/audience, so historical
  base rates may mislead. We surface it rather than bury it.
- `outcome` — 1 if the phrase cleared the threshold, else 0 (resolved rows only).

## Not financial advice

These are probabilities and a calibration record — data, not advice. We publish numbers; what
anyone does with them is their own decision. Zentence never places trades.

---
_Last generated 2026-08-15 23:24 UTC._
