# Design

## Context
See `proposal.md` for scope. `\section{Conclusion}` spans lines 292–301 of `ICEIB paper/main.tex`, folding `Limitations` directly into `Z` rather than leaving a detached top-level section.

## Goals / Non-Goals

**Goals:**
- Deliver crisp `W–Z` closure (~0.5 columns) with honest scope boundaries and negative-result commitment (`RQ2`, `RQ5`).
- Keep `V. Conclusion` + `References` ending cleanly on page 6.

**Non-Goals:**
- Introducing new technical claims or unverified metrics not discussed in Sections I–IV.

## Decisions
- **Choice**: Combine `W` and `X` into a single tight paragraph (`Summary and Strengths (W--X)`), followed by `Expected Yields (Y)` and `Limitations and Future Work (Z)`.
- **Rationale**: Eliminates redundancy while keeping every A–Z letter explicitly traceable.
- **RQ Mapping**: `Y` summarizes target verification for `RQ1–RQ5`; `Z` enforces honest reporting for `RQ2` (ablation cost/latency trade-off) and `RQ5` (domain transfer boundaries).

## Risks / Trade-offs
- **[Last-Page Column Imbalance]** -> Monitor `IEEEtran` last-page notice in `output.log` and apply `\IEEEtriggeratref{...}` during camera-ready polish if references leave one column short.
