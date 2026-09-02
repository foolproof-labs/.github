# Field notes: three data incidents in one night, caught by layered checks

> **Where:** a production A-share research pipeline, one evening (2026-09)
> **What:** the first full-universe run of a nightly price-integrity watchdog
> fired on 21 symbols, and the investigation that followed surfaced two more
> defect classes hiding in the same data paths — all three fixed the same
> night, with repair logs and regression tests.
> **Why it matters here:** no single check caught all three. Each incident
> was caught by a *different* layer, and the layers are exactly the ones this
> toolchain packages — point-in-time price meaning, data-unit discipline, and
> evidence trails. The reproductions are offline and dependency-free.

## The setup

A production A-share research pipeline ingests vendor daily bars ("qfq
current vintage") into per-symbol history files, tagging every bar with its
`source`. Each evening a watchdog job rebuilds each symbol's history against
a point-in-time corporate-action archive and compares the *inverted raw
closes* against an independent live unadjusted-close source — the
`compare_raw_closes` semantics that `padj drift-check` implements, running
over the **full series** (the 0.1.2 default).

The run that night fired on 21 symbols with `worst_deviation ≈ 99.0`, all on
the same trading day.

## Incident 1 — one day of OHLC stored ×100 (21 symbols)

Investigation showed:

1. Only 21 of ~1,760 symbols were affected, and every affected bar carried
   the same `source` tag: a realtime-batch *fallback* path that the pipeline
   uses when the primary daily endpoint is unreachable. It had been silent
   for weeks; that day it answered for exactly these 21 symbols.
2. Prices were exactly ×100 (close `4.02` → `402.0`) while volume and amount
   stayed correct — the signature of a parser that forgot a field-scale
   normalisation on the realtime-quote fields.
3. The corruption was invisible to anything that only reads *returns* (a
   constant scale factor cancels), but silently poisons anything that reads
   absolute prices or mixes sources.

**Fix pattern:** repair the 21 bars from the independent source (append-only
repair log with before/after values) → fix the parser (÷100 on the scaled
fields) → add a regression test that pins the scale semantics. The previous
test suite had baked in the *unscaled* assumption — that is how the bug
survived.

## Incident 2 — a real ex-date missing from the archive

The same full-scan surfaced a different signature: one symbol with a
constant ~31% deviation on **every bar before** a date in March. The vendor's
qfq history already carried the adjustment (factor 0.6845), but the
point-in-time archive had no record of the event — the archive was a pure
projection of one data source with no manual override path, so a source gap
became a permanent gap.

**Fix pattern:** an override mechanism (`overrides/<code>.json`, merged with
PIT filtering and de-duplication) → record the event with its provenance
("watchdog case") → re-run: deviation 0.3155 → 7e-05, with the evidence
package stored next to the watchdog's artifacts.

## Incident 3 — mixed-source unit semantics

While tracing the affected paths, the same investigation found a unit
semantics defect in the main daily feed: `volume` was in **shares** where
the schema (and the primary feed) stored **lots**, and the `turnover` field
was actually mapped to traded amount. Mixed-source series therefore carried
a ×100 volume discontinuity whenever the fallback answered.

**Fix pattern:** normalise in the feed adapter (shares → lots, correct
turnover semantics) with three unit tests, then re-normalise the ~1,100
stored files that the feed had produced, logged for replay.

## Why the layers, not one check

| Incident | What caught it | Layer / tool family |
| --- | --- | --- |
| ×100 scale corruption (old bar, single day) | full-window drift vs an independent raw source; tail-only sampling is vacuous here | point-in-time price meaning (`pit-adjuster`, 0.1.2 full-window default) |
| Missing ex-date in the archive | constant pre-event deviation block; needs the archive/override path to resolve | point-in-time price meaning + evidence trail |
| Mixed volume units across sources | source-tag clustering during the investigation; invisible to any per-symbol check | data-unit discipline + data immunity (source tags, audits) |

Read the signatures, not just the numbers:

| `worst_deviation` | Meaning |
| --- | --- |
| ~99× on one bar | scale corruption |
| ~0.32 constant on a pre-event block | missing corporate action |
| 1–2% | normal dividend-modelling noise |
| ~0 | clean |

That layering is what lets a watchdog output be triaged by humans: the
deviation magnitude and shape tell you which failure class you are looking
at before you open any file.

## Reproduce both signatures offline

```bash
python examples/case_scale_corruption.py   # ×100 old bar:   tail-only MISSED / full-window FIRED (99.0)
python examples/case_missing_event.py      # missing ex-date: tail-only MISSED / full-window FIRED (0.3155)
```

Both scripts are in `holdout-labs/pit-adjuster` — synthetic data, no network,
no dependencies, self-checking exit codes.

## Honest boundaries

This watchdog detects price-level anomalies by comparing rebuilt point-in-time
history against an independent raw source. It cannot see errors that both
sources share, it needs the source-tag discipline to cluster root causes, and
it needs the repair-log discipline to make fixes replayable. The toolchain
exists to make those three habits cheap.
