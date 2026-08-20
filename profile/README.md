# Foolproof Labs

## 中文说明

Foolproof Labs 是一组面向量化研究的开源小工具，重点是让数据、时间、统计结论和研究经验更容易检查。

其中 `ashare-data-immunity` 和 `pit-adjuster` 直接面向 A 股常见的数据问题；其他工具也适用于 A 股量化研究流程。工具不荐股、不承诺收益，也不替代交易所规则、数据供应商说明或人工复核。

Open-source tools for making quantitative research easier to audit and harder to fool yourself about.

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

## Shared principles

- Small tools with explicit inputs, outputs, and limits.
- Read-only checks wherever a check is all that is needed.
- Reproducible local workflows and machine-readable results.
- MIT-licensed code with tests and examples.
- Honest boundaries: a warning or pass is evidence about a check, not proof of a profitable strategy.

## Contributing

Issues and pull requests are welcome. Please include a small reproducible example, the expected result, and the data or rule assumption involved. For A-share issues, state the market board, date range, data source, and the relevant exchange rule or vendor convention when available.

All repositories are MIT licensed. See each repository for installation, command examples, current status, and project-specific limitations.
