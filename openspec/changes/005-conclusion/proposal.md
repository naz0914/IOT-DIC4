# Proposal

## Why

`V. Conclusion` must concisely synthesize what was proposed (`W`), where the decoupled architecture excels (`X`), what expected yields/outcomes are targeted (`Y`), and what external-validity limitations and future work remain (`Z` in `IMG_9464.png`). Managing `005-conclusion` separately ensures balanced academic humility—explicitly committing to report negative ablation results if LLM CoT gains do not justify latency/cost—and keeps the paper within the 4–6 page ICEIB budget.

## What Changes

- Manage `\section{Conclusion}` (`\label{sec:conclusion}`) in `ICEIB paper/main.tex` as an OpenSpec unit mapped to `W–Z`.
- Integrate external validity limitations (SEBI Indian F&O scope, equity-to-crypto re-verification for TradingAgents & FinForesight, thin-liquidity HVN degradation, and external LLM API drift) inside `Z`.
- Preserve balanced column/page termination before `\begin{thebibliography}`.

## Capabilities

### New Capabilities
- `section-conclusion`: Specification for `W–Z` conclusion synthesis, objective limitation disclosure, negative-ablation commitment, and page-end balance.

### Modified Capabilities
<!-- None -->

## Impact

- **Target Section**: `ICEIB paper/main.tex` (`\section{Conclusion}`), cross-checked against `README.md` Section 14 and `docs/references.md`.
- **Overleaf Sync**: Syncs to Overleaf project `DIC4` (`6b064825cdb12bbb4e48c26a`) on apply.
