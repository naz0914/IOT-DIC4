# Design

## Context
See `proposal.md` for scope. `\section{Introduction}` sits at lines 53–84 of `ICEIB paper/main.tex`, bridging `Abstract` to `II. Related Work` and grounding `RQ1–RQ5`.

## Goals / Non-Goals

**Goals:**
- Maintain a tight `G–L` flow (~1–1.2 columns in `IEEEtran`) with zero unverified claims.
- Explicitly trace how `J` (three-generation critique) motivates `K` (four contributions across the 4-tier/8-module architecture).

**Non-Goals:**
- Detailed mathematical proofs or hyperparameter tuning (deferred to `003-proposed-design` and `004-experiment`).

## Decisions
- **Choice**: Frame `K` as four testable engineering contributions aligned with `RQ1–RQ5` rather than claiming finished empirical performance gains.
- **Rationale**: Upholds `openspec/config.yaml` empirical integrity rules before backtest execution.
- **RQ & Tier Mapping**: Contribution 1 maps to all 4 tiers and `RQ3`; Contribution 2 maps to Layer 2.1 + Layer 3.1 (`RQ1`, `RQ2`); Contribution 3 maps to Layer 4.1 `WAIT/NO_TRADE` (`RQ4`); Contribution 4 maps to Layer 1.2 PIT + 4-stage validation (`RQ5`).

## Risks / Trade-offs
- **[Extrapolating SEBI Equity Stats to Crypto]** -> Pair SEBI (2026) with crypto-specific Reuters (Oct 2025) liquidation evidence and explicitly note scope differences.
