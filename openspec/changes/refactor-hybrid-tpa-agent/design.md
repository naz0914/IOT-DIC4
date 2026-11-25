## Context

Hybrid TPA-Agent currently exists as a complete academic proposal (`README.md`, `期末專題提案_Hybrid_TPA_Agent(最新檔).pdf`, `架構圖.png`, `流程圖flowchart.png`, and `Hybrid TPA-Agent 期末專題提案.pptx`) defining a 4-tier, 8-module architecture and `Algorithm 1`. However, the repository lacked a standalone `DESIGN.md`, formal OpenSpec machine-checkable requirements, explicit API data contracts, uncertainty calibration rules, failure-recovery state machines, and a complete 5-mindset / 3-trade-off / Article Summary Table research package.

This design converts the architectural blueprint into an auditable engineering specification anchored to `RQ1`–`RQ5`.

## Goals / Non-Goals

**Goals:**
- Enforce strict separation of concerns: deterministic Python algorithms for spatial zones and risk math; calibrated XGBoost for conditional probability $W$; constrained LLM JSON output for Price Action semantic validation.
- Guarantee zero future-information leakage via Point-in-Time (PIT) snapshot slicing at decision timestamp $t$.
- Make `WAIT/NO_TRADE` a first-class auditable state with post-hoc counterfactual outcome tracking (RQ4).
- Meet end-to-end latency budget `< 1,500 ms` on `N = 500` bars (`< 15 ms` geometry + `< 5 ms` XGBoost + `800–1200 ms` structured LLM).
- Map every research claim (`RQ1`–`RQ5`) to specific OpenSpec requirements and verification experiments.

**Non-Goals:**
- Fully autonomous live capital execution without human-in-the-loop (HITL) sign-off or exchange server-side hard stop orders.
- Sub-minute (`1m` or tick-level) HFT market making, where microstructure noise and 0.04% Taker fees + slippage erode expectancy.
- Using an LLM to directly compute prices, zone coordinates, stop-loss levels, position sizes, or win probabilities.

## Decisions

### 1. Four-Tier, Eight-Module Decoupled Pipeline (`Algorithm 1`)
- **Layer 1 (Input & Data)**:
  - `1.1 User Preference Block`: Symbol `S` (e.g., `BTC/USDT`), timeframe `TF` (`5m`, `15m`), per-trade risk tolerance `R_tol` (default `1%`), fee/slippage model.
  - `1.2 Data Ingestion Engine`: Fetches OHLCV window (`N = 200..500` closed bars up to $t$) with strict timestamp guard (`bar.close_time <= t`) and WebSocket heartbeat monitor.
- **Layer 2 (Core Analytical Engines)**:
  - `2.1 Deterministic S&D Zone Engine`: $\mathcal{O}(N)$ dual-pointer Swing High/Low detection + $\mathcal{O}(N \log B)$ Volume Profile HVN + zone lifecycle (`UNMITIGATED` → `TESTED` → `BROKEN` / `SR_FLIP`).
  - `2.2 ML Win-Rate Engine`: Extracts microstructure feature vector (Volume Z-score, candle body ratio, wick penetration depth, distance to zone) and outputs calibrated probability $W \in [0, 1]$ via XGBoost ($\mathcal{O}(T \times D)$, `< 5 ms`). If $W < W_{\min}$ (baseline candidate `0.55`, parameterized for sensitivity sweep), short-circuit immediately to `WAIT/NO_TRADE` without invoking the LLM.
- **Layer 3 (LLM Reasoning & Domain Knowledge)**:
  - `3.1 Domain Knowledge Base`: Structured rule definitions for `Pinbar` passive absorption, `Liquidity Sweep`, `ChoCh` (Change of Character), and `S/R Flip`.
  - `3.2 Planner & Reasoning Agent`: Consumes **only** read-only structured JSON from Layers 1–2 plus retrieved Price Action rules, pinned at `temperature = 0.0` (with seed/model version logging), and returns strict schema `{is_valid_setup: bool, pattern_type: str, structural_evidence: list[str], invalidation_flags: list[str]}`.
- **Layer 4 (Risk Management & Output)**:
  - `4.1 Decision & Risk Engine`: Computes entry `P_entry`, ATR-buffered zone-outer `P_sl`, opposing-zone `P_tp`, geometric `RR = |P_tp - P_entry| / |P_entry - P_sl|`, cost-adjusted expectancy $E$, and position size `Pos = (Equity * R_tol) / |P_entry - P_sl|`. Gates approval on `RR >= RR_min` (candidate `2.0`), `E > 0`, and `reasoning_result.is_valid_setup == true`.
  - `4.2 Output Interface`: Emits immutable JSON + Markdown decision card (`APPROVED` or `WAIT/NO_TRADE`) with decision hash and HITL confirmation hook.

### 2. Inter-Module Data Contracts (JSON Schema Summary)
- `ZoneRecord`: `{zone_id, type: "DEMAND"|"SUPPLY", lower, upper, hvn_price, created_at, status: "UNMITIGATED"|"TESTED"|"SR_FLIP"|"INVALIDATED"}`
- `MLPrediction`: `{win_prob: float, brier_calibrated: bool, feature_snapshot: dict, model_version: str}`
- `AgentReasoning`: `{is_valid_setup: bool, pattern: str, evidence: list[str], risk_flags: list[str], model_id: str, temperature: float, latency_ms: int}`
- `DecisionReport`: `{status: "APPROVED"|"WAIT/NO_TRADE", reason: str, entry: float|null, sl: float|null, tp: float|null, rr: float|null, expectancy: float|null, position_size: float|null, pit_timestamp: str, config_hash: str}`

### 3. Uncertainty, Calibration & Point-in-Time Guarantees
- **Probability Calibration**: XGBoost raw scores MUST undergo isotonic or Platt calibration on a chronological validation split and be evaluated with Brier Score before threshold gating (`W >= W_min`).
- **Threshold Parameterization**: `W >= 0.55` and `RR >= 2.0` are treated explicitly as *tunable hypotheses*, tested under Taker `0.04%` fee + slippage sensitivity sweeps rather than hard-coded truth.
- **PIT Snapshot Isolation**: All feature windows, zone states, and optional news/regime context are indexed strictly by `<= decision_timestamp` to prevent FinForesight-style look-ahead bias.

### 4. Failure Recovery & 3-Level Degradation State Machine
- **Level 1 (Warning: Heartbeat latency `> 2s` or LLM timeout/schema error)**: Freeze new `APPROVED` signals; emit `WAIT/NO_TRADE` with `reason="DEGRADED_L1_LATENCY_OR_SCHEMA"`.
- **Level 2 (In-Flight Protection)**: Verify exchange server-side hard stop-loss orders are active for any open position; block size increases.
- **Level 3 (Critical: Feed outage `> 30s` or extreme OOD circuit breaker)**: Trigger operator alert and execute defensive hedge/flatten protocol per user preference.

### 5. Research Claim Traceability Matrix
| Research Question | OpenSpec Capability | Verification Metric / Experiment |
| :--- | :--- | :--- |
| **RQ1**: Do S&D + structure features improve pure XGBoost? | `sd-zone-detection`, `winrate-prediction` | ROC-AUC, Precision, Brier Score, Net Return vs. Baseline 2 (Pure XGBoost) |
| **RQ2**: Does LLM contextual reasoning reduce false signals? | `llm-cot-reasoning` | Profit Factor, Sharpe, MDD vs. Ablation (S&D + XGBoost without LLM) |
| **RQ3**: Does numerical-semantic decoupling reduce errors & variance? | `llm-cot-reasoning`, `risk-management` | Multi-run Consistency Rate (>95% target), Numerical Error Rate (=0%), Risk Violation Rate (=0%) vs. Baseline 3 (Pure LLM) |
| **RQ4**: Do `WAIT/NO_TRADE` savings outweigh missed opportunities? | `risk-management` | Trade Retention Rate, Counterfactual Outcome of rejected signals |
| **RQ5**: Do gains hold under PIT, extreme crashes, and forward sim? | `data-ingestion`, `risk-management` | 4-stage validation (Chronological, PIT, Oct 2025 $19B stress window, Forward Sim with hash) |

## Risks / Trade-offs

- **Over-filtering Risk (`W_min`, `RR_min` too strict)**: Excessive `WAIT` signals can starve capital utilization (`Trade-off 1`). Mitigated by logging counterfactual returns on rejected setups and sweeping thresholds in validation.
- **LLM API Variance & Latency**: Even at `temperature = 0.0`, external provider updates can alter outputs (`Koviazin et al., 2026`). Mitigated by pinning model IDs, enforcing JSON schema validation, and short-circuiting low-`W` bars before LLM invocation.
- **Thin Liquidity / Holiday Volume Vacuum**: Volume Profile HVN can degrade during low-liquidity hours (`Deep Question #4`). Mitigated by minimum volume/liquidity filters in `sd-zone-detection` and `WAIT` fallback.
