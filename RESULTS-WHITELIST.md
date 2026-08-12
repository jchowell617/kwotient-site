# Public results — what may appear on kwotient.io

This repository is **public** and serves kwotient.io. The Kwotient record repository
(`Kwotient`) is **private** and holds all proprietary data. The two must never be confused.

Only `results.json` carries cohort data onto this site, and it may contain **only the
whitelisted summary fields below**. Nothing else from the private record is ever copied
here — not the model/engine, not rosters, not comparables, not per-player projections,
not raw inputs.

## Allowed fields (the whitelist)

Per cohort, `results.json` may contain:

| Field | Meaning | Example |
|---|---|---|
| `league` | NBA or NFL | `"NFL"` |
| `cohort` | Cohort name | `"Veteran contract guarantees"` |
| `status` | `frozen` \| `filed` \| `settled` | `"settled"` |
| `settled` | Settlement date (ISO) | `"2026-09-12"` |
| `size` | Cohort size (a count) | `8` |
| `baseline.label` | The baseline it had to beat | `"reported guaranteed at face value"` |
| `baseline.value` | The baseline number (optional) | `"46.0%"` |
| `result.beat_baseline` | Did the model beat it | `true` |
| `result.skill` | Skill over baseline, aggregate | `"+0.08 Brier skill"` |
| `result.headline` | One-line plain-English summary | `"Beat the market on 6 of 8."` |
| `metrics[]` | Aggregate metrics only: `{label, value}` | `{"label":"MAE gtd-at-signing","value":"7.2 pts"}` |

## Never allowed here (stays private)

- Any per-player projection, floor, point estimate, band, term or structure.
- Any roster, input row, comparable, or the frozen `*.json` record files.
- The model / engine code (`model/**`), the methodology internals, or the manifest.
- Anything not in the whitelist table above.

## How results reach this file

When a cohort settles, the settlement produces an aggregate scorecard in the **private**
record. A person (or a curated step) transcribes **only** the whitelisted fields above
into `results.json` here, then commits and pushes this public repo. The private record
and its proprietary inputs never change visibility.
