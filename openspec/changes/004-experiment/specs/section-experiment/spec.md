# Spec Delta

## Purpose
Define requirements for `IV. Experiment` (`004-experiment`) covering `Q–V` protocol completeness, PIT isolation, and strict separation of hypothesized targets from empirical results.

## ADDED Requirements

### Requirement: Complete Q–V Experimental Protocol
`\section{Experiment}` SHALL cover `Q` (Prediction, Trading, Consistency, and Abstention metrics), `R` (Baselines B1–B3 & S&D+XGBoost Ablation), `S` (4-stage PIT setup on 5m/15m `BTC/USDT` & `ETH/USDT` under 0.04% taker fee + slippage), `T` (sensitivity thresholds $W_{\min}, \mathrm{RR}_{\min}, N, B$), `U` (hypothesized target table), and `V` (`RQ1–RQ5` verification criteria).

#### Scenario: All Q–V elements and RQ1–RQ5 criteria present
- **WHEN** `\section{Experiment}` is audited against `IMG_9464.png`
- **THEN** each research question `RQ1`–`RQ5` SHALL map to an explicit falsification condition, baseline comparison, and metric family.

### Requirement: PIT Isolation, Cost Fairness, and Target Integrity
`IV. Experiment` SHALL enforce strict `close_time <= t` PIT isolation, Oct 2025 stress testing, 3-level degradation/outage handling checks, and SHALL label unexecuted target metrics (`Table~\ref{tab:targets}`) as hypothesized rather than observed results.

#### Scenario: Hypothesized targets clearly distinguished before execution
- **WHEN** `Table~\ref{tab:targets}` is rendered without completed backtest logs
- **THEN** the Disclosure paragraph and table caption SHALL explicitly mark all values as `Not Verified Results` / hypothesized targets.
