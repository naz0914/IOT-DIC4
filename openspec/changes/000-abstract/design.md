# Design

## Context
See `proposal.md` for scope. The Abstract lives at lines 44–51 of `ICEIB paper/main.tex` and synthesizes the 4-tier/8-module architecture and `RQ1–RQ5` evaluation framework from `docs/abstract.md` into an IEEE conference single-paragraph format.

## Goals / Non-Goals

**Goals:**
- Keep the Abstract concise (approx. 200–250 words) so `I. Introduction` starts cleanly on page 1 of the 4–6 page IEEE ICEIB layout.
- Ensure explicit traceability across all 4 tiers (deterministic S&D/HVN, calibrated XGBoost, zero-temp JSON-constrained LLM CoT, ATR/RR/Expectancy/WAIT/3-level degradation) and PIT isolation (`RQ5`).

**Non-Goals:**
- Repeating full equations, tables, or citation keys (`\cite` is avoided in IEEE abstracts).

## Decisions
- **Choice**: Encode `A–F` inline within one continuous paragraph annotated by `% A: ... | B: ...` comments in `main.tex`, rather than structured sub-headings.
- **Rationale**: IEEE `IEEEtran` conference style requires a single-paragraph Abstract without subheadings or `\cite{}` commands.
- **RQ Mapping**: Summarizes `RQ1–RQ5` collectively via functional decoupling (`C–D`), PIT 4-stage protocol (`E`), and consistency/drawdown falsification (`F`).

## Risks / Trade-offs
- **[Word Bloat Pushing Introduction to Page 2]** -> Keep sentence density tight and verify via Overleaf `label_pages` that `sec:intro` starts on `p.1`.
