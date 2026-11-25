# Proposal

## Why

`IV. Experiment` defines the falsifiable validation protocol for `RQ1–RQ5` following the 2020 A–Z SOP (`Q–V` in `IMG_9464.png`). Because empirical backtests have not yet been executed, isolating `004-experiment` as a dedicated OpenSpec change enforces strict point-in-time (PIT) isolation, realistic cost modeling (0.04% taker fee + slippage), and an ironclad boundary between hypothesized Expected Targets (`Table III`) and empirical outcomes—making it easy to swap in real backtest tables later via `/opsx:apply 004-experiment`.

## What Changes

- Manage `\section{Experiment}` (`\label{sec:experiment}`) and `Table III` (`tab:targets`) in `ICEIB paper/main.tex` as an independent OpenSpec unit mapped to `Q–V`.
- Enforce 4-stage validation (`chronological split`, `PIT close_time <= t`, `Oct 2025 $19B liquidation stress test`, `hash-committed forward simulation`), 4 baselines (`B1`–`B3` & `Ablation`), and 4 metric families (`Prediction`, `Trading`, `Consistency`, `Abstention`).
- Preserve explicit non-empirical labeling (`Hypothesized Expected Targets (Not Verified Results)`) until actual backtests are executed and reviewed.

## Capabilities

### New Capabilities
- `section-experiment`: Specification for `Q–V` evaluation protocol, PIT leakage prevention, `RQ1–RQ5` falsification criteria, and hypothesized-vs-empirical boundary enforcement.

### Modified Capabilities
<!-- None -->

## Impact

- **Target Section**: `ICEIB paper/main.tex` (`\section{Experiment}`, `tab:targets`), aligned with `DESIGN.md` and `openspec/changes/refactor-hybrid-tpa-agent/tasks.md`.
- **Overleaf Sync**: Syncs to Overleaf project `DIC4` (`6b064825cdb12bbb4e48c26a`) on apply.
