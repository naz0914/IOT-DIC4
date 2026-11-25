# Spec Delta

## Purpose
Define requirements for `III. Proposed Design` (`003-proposed-design`) covering `PA–PM` A–Z compliance, 4-tier/8-module decoupling, PIT/degradation rigor, and mathematical proof.

## ADDED Requirements

### Requirement: Complete PA–PM Design Exposition
`\section{Proposed Design}` SHALL integrate `PA` (Aim), `PB` (Based-on/mindsets), `PC` (Cure), `PD` (Design trade-offs), `PE` (4 tiers / 8 modules), `PF` (Formulation), `PG` (`fig:arch`, `fig:flow`), `PH` (Module interaction & `<1.5s` latency budget), `PI` (`Algorithm 1`), `PJ` (`BTC/USDT 15m` walkthrough), `PK` (Key advantages), `PL` (Bridge to Section IV), and `PM` (`Proposition 1` proof).

#### Scenario: All PA–PM subsections and Proposition 1 verified
- **WHEN** `\section{Proposed Design}` is audited against `IMG_9464.png`
- **THEN** every `PA–PM` item SHALL map to an explicit subsection, figure, equation, algorithm, or proposition
- **AND** `fig:arch` and `fig:flow` SHALL render inside Section III without drifting past Conclusion.

### Requirement: Four-Tier Decoupling, PIT, and Three-Level Degradation
`III. Proposed Design` SHALL enforce read-only JSON isolation for the LLM (`temperature=0.0`), Platt/Isotonic calibration for XGBoost, PIT boundary `close_time <= t`, and 3-level failure degradation (Level 1 `>2s`/fault -> `WAIT/NO_TRADE`, Level 2 server-side SL check, Level 3 `>30s`/OOD circuit breaker).

#### Scenario: Algorithm 1 and failure paths match system contracts
- **WHEN** `Algorithm~\ref{alg:tpa}` is inspected
- **THEN** it SHALL short-circuit `HeartbeatLatency > 2s`, `FeedOutage > 30s`, and `W < W_min` to `WAIT/NO_TRADE` before LLM invocation.
