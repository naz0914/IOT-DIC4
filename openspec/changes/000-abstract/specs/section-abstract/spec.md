# Spec Delta

## Purpose
Define observable structural, technical, and integrity requirements for the IEEE ICEIB single-paragraph Abstract (`000-abstract`) under the 2020 A–Z SOP (`A–F`).

## ADDED Requirements

### Requirement: A–F Single-Paragraph IEEE Abstract Structure
The Abstract SHALL appear as a single continuous paragraph inside `\begin{abstract}` covering `A` (Attention Getter), `B` (But/Challenge), `C` (Cure), `D` (Development), `E` (Experiments), and `F` (Findings) in order, followed by `\begin{IEEEkeywords}`.

#### Scenario: All six A–F beats present in order
- **WHEN** the Abstract text is audited against `IMG_9464.png`
- **THEN** it SHALL introduce SEBI 2026 (87.7% retail loss) (`A`), contrast indicator/ML/LLM gaps (`B`), propose Hybrid TPA-Agent (`C`), summarize deterministic geometry + calibrated XGBoost + read-only JSON LLM CoT + programmatic risk/PIT/3-level degradation (`D`), summarize the 4-stage PIT protocol (`E`), and state falsifiable hypothesized outcomes (`F`).

### Requirement: Functional Decoupling and Expected-Target Framing
The Abstract SHALL distinguish deterministic geometry, calibrated ML inference, constrained LLM reasoning, and programmatic risk gating, and SHALL frame all target metrics as hypothesized benchmarks rather than executed results.

#### Scenario: No unexecuted backtest results claimed
- **WHEN** target metrics such as `<= 12%` drawdown or `> 95%` consistency appear in the Abstract
- **THEN** the wording SHALL explicitly qualify them as falsifiable hypothesized targets rather than completed empirical results.
