# Spec Delta

## Purpose
Define requirements for `V. Conclusion` (`005-conclusion`) following the 2020 A–Z SOP (`W–Z`), external validity humility, and IEEE page-budget discipline.

## ADDED Requirements

### Requirement: Complete W–Z Conclusion Synthesis
`\section{Conclusion}` SHALL present `W` (What proposed), `X` (eXcel/decoupling strengths across deterministic math, `WAIT/NO_TRADE`, and 3-level degradation), `Y` (Yields/targeted benchmarks without claiming unexecuted empirical outcomes), and `Z` (Zen/external validity limitations & future work).

#### Scenario: W–Z beats verified with negative-ablation commitment
- **WHEN** `V. Conclusion` is reviewed against `IMG_9464.png`
- **THEN** it SHALL explicitly cover `W–Z`, disclose all four external validity boundaries (SEBI scope, equity benchmarks, thin-liquidity HVN, API drift), and commit to reporting negative LLM CoT ablation results if observed.

### Requirement: Decoupling Integrity and Page-Budget Fit
`V. Conclusion` SHALL preserve the distinction between deterministic risk/math and constrained LLM semantics and SHALL fit cleanly before `\begin{thebibliography}` so total PDF length remains 4–6 pages.

#### Scenario: Conclusion lands cleanly within 6-page limit
- **WHEN** `ICEIB paper/main.tex` compiles on Overleaf
- **THEN** `sec:conclusion` SHALL appear on p.5–p.6 and total page count SHALL remain `<= 6`.
