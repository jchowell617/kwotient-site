# Public results — what may appear on kwotient.io

This repository is **public** and serves kwotient.io. The Kwotient record repository
(`Kwotient`) is **private** and holds all proprietary data. The two must never be confused.

The governing rule is that **everything published here must be publishable without a
licence from anyone**. That means one of three things, and nothing else:

1. **Kwotient's own output** — projections, bands, probabilities, scores, the rubric.
   Kwotient made these.
2. **Bare facts** — a player's name, team, position and age; a contract's dollars, years,
   guarantee share and dates; games played; cap figures; calendar dates. Facts are not
   anybody's property.
3. **Kwotient's own words** — its paraphrase of a reported situation, carrying attribution
   to whoever reported it.

Attribution is not a licensing problem and is encouraged: naming ESPN or Over the Cap as
the origin of a figure is how the record stays checkable.

## What is published

- `index.html` — the cohort board.
- `players/` — per-player pages for cohorts that have filed projections.
- `api/players/` — the same records as JSON.
- `results.json` — aggregate scorecards as each cohort settles (see the field list below).

Per-player pages are public by decision of 16 August 2026. They carry Kwotient's own
projections against public facts, which is the product. They are subject to the same rule
as everything else: no licensed data, no reproduced prose.

## Never published here

- **The model / engine** (`model/**`), the methodology internals, or the manifest.
- **Comparables** and the frozen input record files.
- **Reproduced prose.** A quoted sentence from an article is that outlet's expression, not
  a fact, however freely the outlet is cited elsewhere. Paraphrase it and attribute it.
- **Rival forecasters' projections.** Kwotient scores itself against Eric Pincus, Yossi
  Gozlan, Bobby Marks and Keith Smith. Their numbers are their work product. The
  comparison panel stays in the private register unless they give permission — none has
  been contacted, and the methodology says so.
- Anything sourced from a provider recorded as `blocked` in the record's
  `site/licensing.json`.

## How this is enforced

Not by trust. `site/publish-gate.js` in the private record runs before any file is copied
here, and refuses the publish if the set contains uncleared sources, reproduced prose,
rival forecasts, or per-player files at a time when this document forbids them. It fails
closed: an unreviewed source publishes nothing, and so does a missing licensing file.

If this document and that gate ever disagree, **the gate wins and the publish stops**.
Change this document first, then the policy flags in `site/licensing.json`.

## results.json — the aggregate whitelist

`results.json` carries settled cohort summaries, and may contain only these fields:

| Field | Meaning | Example |
|---|---|---|
| `league` | NBA, NFL, NHL or WNBA | `"NFL"` |
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

When a cohort settles, the settlement produces an aggregate scorecard in the **private**
record. A person transcribes **only** the whitelisted fields above into `results.json`
here. The private record and its proprietary inputs never change visibility.
