# Design

## Context
See `proposal.md` for scope. `\section{Proposed Design}` spans lines 144–245 of `ICEIB paper/main.tex` and consolidates the 4-tier/8-module architecture, `Algorithm 1`, `fig1_arch.png`, `fig2_flow.png`, equations, walkthrough, and `Proposition 1`.

## Goals / Non-Goals

**Goals:**
- Cover all 4 tiers / 8 modules, JSON/API contracts, probability calibration, 3-level failure recovery, and `RQ1–RQ5` design linkages cleanly.
- Keep `fig1_arch.png` and `fig2_flow.png` plus `Algorithm 1` within pages 3–4 with zero `Overfull \hbox`.

**Non-Goals:**
- Writing Python runtime code or claiming empirical backtest validation inside Section III.

## Decisions
- **Choice**: Size `fig2_flow.png` via `height=0.38\textheight,keepaspectratio` and shorten `Algorithm 1` helper calls (`Out.ok`, `Out.wait`, `Risk.degrade`).
- **Rationale**: Prevents tall flowchart float displacement past Section V and eliminates `75pt` `Overfull \hbox` warnings in `algorithmic`.
- **4-Tier / 8-Module & RQ Coverage**: L1.1/L1.2 (`RQ4, RQ5` PIT/costs), L2.1/L2.2 (`RQ1` S&D + calibrated XGBoost), L3.1/L3.2 (`RQ2, RQ3` KB + `temp=0.0` JSON CoT), L4.1/L4.2 (`RQ3, RQ4, RQ5` RR/Expectancy/WAIT/3-level degradation/hash-stamped HITL).

## Risks / Trade-offs
- **[Dense Technical Content Expanding Page Count]** -> Keep `PJ` walkthrough and `PM` proof crisp so Section III fits across pages 3–4 without pushing the paper past 6 pages.
