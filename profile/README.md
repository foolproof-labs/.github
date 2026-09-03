# Holdout

Evidence infrastructure for financial AI research and AI-generated outputs.

## What we do

We build a governance suite that makes research easier to audit and harder to overstate.

- `ashare-data-immunity` for data quality and snapshots
- `pit-adjuster` for point-in-time price meaning
- `lookahead-free` for timing checks
- `factor-qc` for backtest quality
- `falsification-ledger` for claims and evidence trails
- `lesson-book` for surfaced past mistakes
- `holdout-governance` for AI-assisted research receipts

The backbone is one flow:

`data -> adjust -> timing -> backtest -> falsify -> review -> publish`

`holdout-governance` sits across that flow as the release gate. It checks what was used, what passed, what is missing, and whether a human approved the result.

| Flow stage | Repository | What it does |
| --- | --- | --- |
| entry / release gate | [`holdout-governance`](https://github.com/holdout-labs/holdout-governance) | the wrapper: one artifact, one verdict (also `gov mcp` for agents) |
| data | [`ashare-data-immunity`](https://github.com/holdout-labs/ashare-data-immunity) | A-share daily-bar quality, snapshots, SHA-256 manifests |
| adjust | [`pit-adjuster`](https://github.com/holdout-labs/pit-adjuster) | point-in-time back-adjustment with drift detection |
| timing | [`lookahead-free`](https://github.com/holdout-labs/lookahead-free) | verifiable look-ahead-freedom for pipelines |
| backtest | [`factor-qc`](https://github.com/holdout-labs/factor-qc) | fail-closed backtest quality gate (DSR/PBO/MinTRL) |
| falsify | [`falsification-ledger`](https://github.com/holdout-labs/falsification-ledger) | pre-registration, hash-chained ledger, adjudication |
| learn (loop-back) | [`lesson-book`](https://github.com/holdout-labs/lesson-book) | surfaced past mistakes become the next round's checks |

> The pinned repositories on this profile follow this order: release gate
> first, then the data pipeline in flow order — read the org alphabetically
> and you miss the chain; read it as pinned and the pipeline reads top-down.

## Why the name

A **holdout set** is the data you don't touch until the very end — it keeps
your story honest. A **holdout juror** is the one who refuses to go along
until the evidence is in. Every research claim deserves both.

## What we do not do

- We do not place orders.
- We do not change trading rules.
- We do not give investment advice.
- We do not treat one passing check as proof of profit.

## Start here

If you are doing AI-assisted financial research, start with
`holdout-governance`. It records the evidence cutoff, checks that passed,
the AI identity, and the human review state in one manifest.

## Field notes

- [Three data incidents in one night](https://github.com/holdout-labs/.github/blob/main/docs/case-study-three-incidents-one-night.md) —
  a production watchdog caught a ×100 scale corruption, then the same
  investigation surfaced a missing corporate-action event and a mixed-source
  unit defect. No single check caught all three — the layers did. Both
  failure signatures are reproducible offline in `pit-adjuster`'s examples.

- [Listed on awesome-quant](https://github.com/wilsonfreitas/awesome-quant) —
  `falsification-ledger` was accepted (PR #593, merged 2026-08-29); a
  family-wide entry PR
  ([#620](https://github.com/wilsonfreitas/awesome-quant/pull/620)) is open
  for `pit-adjuster`, `factor-qc`, `lookahead-free`, `lesson-book` and
  `ashare-data-immunity`.

## Maintaining this profile

GitHub lists an org's repositories alphabetically and offers no custom
ordering, so the profile pins carry the design order. Keep the pinned
repositories in flow order (max 6):

1. `holdout-governance` — entry / release gate
2. `ashare-data-immunity` — data
3. `pit-adjuster` — adjust
4. `lookahead-free` — timing
5. `factor-qc` — backtest
6. `falsification-ledger` — falsify

To pin: https://github.com/orgs/holdout-labs/repositories → pin each repo in
that order (the pinned section shows them top-down in pin order). `lesson-book`
(learn / loop-back) and `.github` are intentionally not pinned; their role is
documented in the flow table above. When a new family member arrives, decide
its flow stage first, then update the table and the pins together — the
alphabetical list below is not the message.
