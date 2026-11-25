# Proposal

## Why

`II. Related Work` must establish balanced 50% AI Technology and 50% Financial Domain Knowledge coverage, organize methods from simple to complex (`M`), position our proposed hybrid decoupling (`N`), and present structured comparison matrices (`O` in `IMG_9464.png`). Managing `002-related-work` as a dedicated OpenSpec change prevents citation drift, domain imbalance, or IEEE two-column table overflow.

## What Changes

- Manage `\section{Related Work}` (`\label{sec:related}`), `Table I` (Literature Summary), and `Table II` (Architectural Capability Matrix) in `ICEIB paper/main.tex` as an independent OpenSpec unit.
- Enforce `M–O` structure across AI Technology (`XGBoost`, `Crypto XGBoost`, `TradingAgents`, `FinForesight`) and Domain Knowledge (`Crypto Microstructure`, `Brooks Price Action & Static SNR`, `SEBI DEPA`).
- Maintain strict citation verification against `docs/references.md` and `Overfull \hbox`-free table formatting.

## Capabilities

### New Capabilities
- `section-related-work`: Specification for `M–O` literature review, 50% AI / 50% Domain balance, verified citation discipline, and IEEE table layout integrity.

### Modified Capabilities
<!-- None -->

## Impact

- **Target Section**: `ICEIB paper/main.tex` (`\section{Related Work}`, `tab:lit_summary`, `tab:arch_compare`), cross-checked against `docs/related-work.md` and `docs/references.md`.
- **Overleaf Sync**: Syncs to Overleaf project `DIC4` (`6b064825cdb12bbb4e48c26a`) on apply.
