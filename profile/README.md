# Foolproof Labs

We build infrastructure that makes self-deception structurally impossible.

## 中文说明

Foolproof Labs 是一组面向量化研究的开源小工具，重点是让数据、时间、统计结论和研究经验更容易检查。

其中 `ashare-data-immunity` 和 `pit-adjuster` 直接面向 A 股常见的数据问题；其他工具也适用于 A 股量化研究流程。工具不荐股、不承诺收益，也不替代交易所规则、数据供应商说明或人工复核。

Open-source tools for making quantitative research easier to audit and harder to fool yourself about.

## Why "Foolproof"?

Named after Feynman's first principle:

> "You must not fool yourself — and you are the easiest person to fool." — Richard Feynman

Most tools attack the *math* of overfitting. We attack the *process* — because the hardest bug in quantitative research is not in the code, it is in the story we tell ourselves after the backtest looks good. We do not promise you can't fool yourself. We promise to make it harder: each tool turns one risk or assumption into something explicit, machine-checkable, and hard to revise after the fact.

## Start here

All six tools are published on GitHub and PyPI as `0.1.1` alpha releases. You do not need to install the whole family. Pick the problem closest to your work:

| If you want to... | Start with | First command |
| --- | --- | --- |
| Check whether A-share daily data is safe to use | [`ashare-data-immunity`](https://github.com/foolproof-labs/ashare-data-immunity) | `pip install ashare-data-immunity` |
| Check whether a backtest used information too early | [`lookahead-free`](https://github.com/foolproof-labs/lookahead-free) | `pip install lookahead-free` |
| Record a research claim before seeing the result | [`falsification-ledger`](https://github.com/foolproof-labs/falsification-ledger) | `pip install falsification-ledger` |

Every repository includes a synthetic demo, tests, and command-line help. The tools are local checks and records: they do not fetch market data, choose stocks, promise returns, or place trades.

The projects are small, composable utilities. They do not promise profitable strategies, replace domain review, or make an entire research process automatically correct. Each tool makes one risk or assumption explicit and testable.

## Projects

| Project | Purpose |
| --- | --- |
| [ashare-data-immunity](https://github.com/foolproof-labs/ashare-data-immunity) | Quality checks for A-share daily bars: OHLCV validation, board-aware price limits, suspension detection, listing and continuity audits, and snapshot manifests. |
| [pit-adjuster](https://github.com/foolproof-labs/pit-adjuster) | Point-in-time fixed-basis price reconstruction from corporate-action archives, with adjustment-convention drift checks. |
| [lookahead-free](https://github.com/foolproof-labs/lookahead-free) | Declarative checks that a time-annotated data pipeline does not give a decision data before it was available. |
| [factor-qc](https://github.com/foolproof-labs/factor-qc) | A fail-closed backtest quality gate for DSR, PBO, multiple-testing haircuts, and minimum track record length. |
| [falsification-ledger](https://github.com/foolproof-labs/falsification-ledger) | A hash-chained record for pre-registering research claims, falsification evidence, adjudication, and hit-rate reports. |
| [lesson-book](https://github.com/foolproof-labs/lesson-book) | A local, deterministic mistake ledger that surfaces similar past situations before the next action. |

## Three real workflows

### 1. Clean an A-share data file before research

Use `ashare-data-immunity` when a daily-bar file may contain invalid prices, missing values, silent suspensions, limit moves, or incomplete history.

```bash
pip install ashare-data-immunity
# from a cloned ashare-data-immunity repository:
python examples/demo.py
# with your own JSON bars file:
imm clean --bars bars.json --out clean.json
```

The demo uses synthetic data. For real data, review the reported assumptions and the exchange rules for your market board before relying on the result.

### 2. Check the timing of a backtest pipeline

Use `lookahead-free` for a time-annotated pipeline, and `pit-adjuster` when historical prices depend on corporate-action records.

```bash
pip install lookahead-free pit-adjuster
# from a cloned lookahead-free repository:
python examples/demo.py
# with your own pipeline definition:
lf check --pipeline pipeline.json --json
```

The result is a timing check, not proof that the strategy is profitable. Data-vendor conventions and value-dependent availability still need human review.

### 3. Keep research conclusions auditable

Use `falsification-ledger` to write down a claim and what evidence would disprove it before looking at the result. Add `factor-qc` when a backtest also needs overfitting checks, and `lesson-book` when past mistakes should be surfaced before the next decision.

```bash
pip install falsification-ledger factor-qc lesson-book
# from a cloned falsification-ledger repository:
python examples/demo.py
# verify a ledger created in a state directory:
fl verify --state-dir ~/.research-ledger
```

These tools make the research process easier to inspect; they do not replace statistical judgment or independent review.

## Shared principles

- Small tools with explicit inputs, outputs, and limits.
- Read-only checks wherever a check is all that is needed.
- Reproducible local workflows and machine-readable results.
- MIT-licensed code with tests and examples.
- Honest boundaries: a warning or pass is evidence about a check, not proof of a profitable strategy.

## Related — Metabolism Tools

We also build [workspace-metabolism](https://github.com/metabolism-tools/workspace-metabolism) under the sister organization [Metabolism Tools](https://github.com/metabolism-tools) — policy-driven file lifecycle management for agentic workspaces (audit, recyclable clean, rollback, hash-chained journal). If Foolproof Labs keeps the *research* honest, Metabolism Tools keeps the *workspace* alive.

## Contributing

Issues and pull requests are welcome. Please include a small reproducible example, the expected result, and the data or rule assumption involved. For A-share issues, state the market board, date range, data source, and the relevant exchange rule or vendor convention when available.

All repositories are MIT licensed. See each repository for installation, command examples, current status, and project-specific limitations.
