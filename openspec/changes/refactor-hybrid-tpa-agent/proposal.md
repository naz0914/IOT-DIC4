## Why

Short-term cryptocurrency trading faces severe microstructure noise, emotional decision bias, and high retail loss rates (e.g., SEBI 2026 reported 87.7% net loss among FY26 individual equity derivatives traders, while extreme events such as the October 2025 $19B liquidation cascade stress-test naive strategies). Pure indicator strategies lag and lack spatial context, pure ML lacks structural semantics, and pure LLM agents suffer from numerical hallucination, stochastic variance across temperature/seeds (Koviazin et al., 2026), and historical data leakage (FinForesight, 2026). Refactoring the existing Hybrid TPA-Agent proposal into an OpenSpec specification-driven workflow establishes full traceability from academic research claims (RQ1–RQ5) to deterministic system requirements, design contracts, and leakage-free verification tasks.

## What Changes

- Formalize the 4-tier, 8-module Hybrid TPA-Agent architecture into modular OpenSpec capabilities with testable `SHALL` requirements and `WHEN/THEN` scenarios.
- Specify strict decoupling between deterministic geometry (`Deterministic S&D Zone Engine`), calibrated probability estimation (`ML Win-Rate Engine`), semantic Price Action reasoning (`Planner & Reasoning Agent`), and programmatic risk gating (`Decision & Risk Engine`).
- Formalize Point-in-Time (PIT) data slicing, `WAIT/NO_TRADE` abstention logging, and 3-level API/latency degradation recovery.
- Establish `DESIGN.md` and refactored academic documentation (`docs/`) covering A–F Abstract (including IEEE Systems Journal single-paragraph abstract), G–L Introduction, M–O Related Work with Article Summary Table, and Deep Research (5 Core Mindsets, 3 Trade-offs, 10 Deep Questions).
- Note: All performance figures (e.g., expected Profit Factor 1.94, MDD ≤ 12%, consistency > 95%) remain **hypothesized Expected Targets** pending implementation and empirical validation.

## Capabilities

### New Capabilities
- `data-ingestion`: User risk configuration (`User Preference Block`) and strictly point-in-time OHLCV/volume stream acquisition (`Data Ingestion Engine`) with heartbeat monitoring.
- `sd-zone-detection`: Deterministic dual-pointer Swing High/Low detection, unmitigated Supply & Demand zone tracking, Volume Profile HVN aggregation, and S/R Flip state updates (`Deterministic S&D Zone Engine`).
- `winrate-prediction`: Microstructure feature extraction (Volume Z-score, body ratio, wick penetration) and calibrated XGBoost conditional win-rate probability estimation (`Machine Learning Win-Rate Engine`).
- `llm-cot-reasoning`: Structured Price Action knowledge integration (`Domain Knowledge Base`) and decoupled Chain-of-Thought JSON reasoning (`Planner & Reasoning Agent`) with deterministic-input constraints and multi-run consistency checks.
- `risk-management`: Programmatic ATR stop-loss/take-profit calculation, geometric `RR >= 2.0` and expectancy gating, `WAIT/NO_TRADE` abstention with counterfactual tracking, human-in-the-loop approval, and 3-level API degradation (`Decision & Risk Engine` & `Output Interface`).

### Modified Capabilities
<!-- None: initial OpenSpec baseline for Hybrid TPA-Agent -->

## Impact

- **Documentation & Governance**: Adds `DESIGN.md`, `openspec/config.yaml`, `openspec/changes/refactor-hybrid-tpa-agent/`, and `docs/{abstract,introduction,related-work,deep-research,references}.md`.
- **Planned Implementation Modules**: Future `src/`, `tests/`, and `experiments/` packages will implement and verify the five capability specs under strict PIT backtesting (with 0.04% Taker fee + slippage).
- **No Unreviewed Execution**: Specifications must be reviewed and approved before `/opsx:apply` implementation begins; no synthetic backtest results are generated in this phase.
