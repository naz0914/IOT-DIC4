## 1. Specification Review & Academic Alignment (Gate Before Coding)

- [ ] 1.1 Review `openspec/changes/refactor-hybrid-tpa-agent/proposal.md`, `design.md`, and all five `specs/*/spec.md` files against `RQ1`–`RQ5` and `期末專題提案作業規範`.
- [ ] 1.2 Verify `DESIGN.md` and `docs/*.md` maintain strict separation between hypothesized Expected Targets and verified experimental results.
- [ ] 1.3 Run `openspec validate refactor-hybrid-tpa-agent` and resolve any schema or formatting warnings before starting `/opsx:apply`.

## 2. Layer 1 — Data Ingestion & Point-in-Time Engine (`src/data/`)

- [ ] 2.1 Implement `UserPreferenceConfig` schema and validation (`symbol`, `timeframe`, `risk_tolerance_pct`, fee/slippage). Acceptance: rejects out-of-range `risk_tolerance_pct` in unit tests.
- [ ] 2.2 Implement `DataIngestionEngine` for historical `BTC/USDT` & `ETH/USDT` (`5m`, `15m`, 2023–2024+) with strict `bar.close_time <= decision_t` PIT slicing. Acceptance: PIT leakage unit test verifies zero future bars exposed.
- [ ] 2.3 Implement WebSocket/REST heartbeat monitor emitting `DEGRADED_L1` (`>2s`) and `DEGRADED_L3` (`>30s`) health events.

## 3. Layer 2 — Deterministic S&D Zone & ML Win-Rate Engines (`src/zones/`, `src/ml/`)

- [ ] 3.1 Implement `DeterministicSDZoneEngine` ($\mathcal{O}(N)$ Swing High/Low + $\mathcal{O}(N \log B)$ Volume Profile HVN) with zone lifecycle (`UNMITIGATED`, `TESTED`, `SR_FLIP`, `INVALIDATED`) and low-liquidity filter. Acceptance: determinism test confirms bit-identical output and `< 15 ms` runtime on `N = 500` bars.
- [ ] 3.2 Implement `FeatureExtractor` (Volume Z-score, body ratio, wick penetration, zone distance) using strictly PIT inputs.
- [ ] 3.3 Implement `MLWinRateEngine` wrapping chronological XGBoost training + probability calibration (Platt/Isotonic) and Brier Score evaluation. Acceptance: inference `< 5 ms` and calibrated probabilities `in [0, 1]`.

## 4. Layer 3 — Domain Knowledge Base & Decoupled LLM CoT Agent (`src/agent/`)

- [ ] 4.1 Encode structured Price Action rule definitions (`Pinbar`, `Liquidity Sweep`, `ChoCh`, `S/R Flip`) in `DomainKnowledgeBase`.
- [ ] 4.2 Implement `PlannerReasoningAgent` with `temperature = 0.0`, pinned model ID, strict JSON schema validation, and safe `WAIT/NO_TRADE` fallback on timeout or invalid JSON. Acceptance: mock tests confirm zero numeric overrides and deterministic fallback.

## 5. Layer 4 — Risk Engine, Output Interface & Degradation (`src/risk/`, `src/output/`)

- [ ] 5.1 Implement `DecisionRiskEngine` calculating ATR-buffered SL, opposing-zone TP, `RR`, cost-adjusted expectancy `E`, and position size `Pos`, plus `WAIT/NO_TRADE` counterfactual logger. Acceptance: unit tests verify end-to-end BTC/USDT 15m walkthrough (`$63,950` entry, `$63,750` SL, `$65,150` TP, `RR=6.0`, `E=+3.34`).
- [ ] 5.2 Implement `OutputInterface` generating JSON + Markdown decision cards with config/PIT hash and HITL confirmation state.
- [ ] 5.3 Implement 3-level degradation state machine (`Level 1` freeze, `Level 2` server-side SL check, `Level 3` outage alert/flatten).

## 6. Four-Stage Verification & Ablation Experiments (`experiments/`, `tests/`)

- [ ] 6.1 Implement Baseline 1 (`EMA+RSI`), Baseline 2 (`Pure XGBoost`), Baseline 3 (`Pure LLM`), and Ablation (`S&D + XGBoost`) under unified capital, `0.04%` Taker fee, and slippage.
- [ ] 6.2 Execute Stage 1 (Chronological split) & Stage 2 (PIT control) and record Precision, ROC-AUC, Brier Score, Net Return, Profit Factor, Sharpe, MDD.
- [ ] 6.3 Execute Stage 3 (Extreme volatility stress test, e.g., Oct 2025 $19B liquidation event) and Stage 4 (Forward simulation with pre-committed decision hashes).
- [ ] 6.4 Measure RQ3 Multi-Run Consistency Rate & Numerical Error Rate and RQ4 `WAIT/NO_TRADE` counterfactual outcomes, then update `DESIGN.md` and `docs/` with actual measured results (never mixing targets with verified outcomes).
