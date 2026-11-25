# Tasks

## 1. Manuscript Section Structure & Front Matter (`Abstract`, `I. Introduction`, `II. Related Work`)

- [x] 1.1 Audit `ICEIB paper/main.tex` front matter (`\documentclass[conference]{IEEEtran}`, `10pt` Times, no manual page numbers/headers/footers) and refine `Abstract` + `IEEEkeywords` to verify all `A–F` elements appear in one IEEE paragraph without claiming unexecuted empirical results.
- [x] 1.2 Refactor `\section{Introduction}` (`G–L`) to verify `G` (SEBI 2026 & Oct 2025 liquidation context), `H` (however/risk containment gap), `I` (3 generations of literature), `J` (judgement/limitations), `K` (4 testable contributions), and add explicit `L` (List the organization of Sections II–V) at the end of the section.
- [x] 1.3 Refactor `\section{Related Work}` (`M–O`) to preserve 50% AI Technology + 50% Financial Domain Knowledge balance, verify `M` (simple-to-complex methods), `N` (new proposed positioning), and `O` (Literature Summary `Table I` + Architectural Capability Matrix `Table II`), checking every `\cite{...}` against `docs/references.md`.

## 2. Core Technical & Methodological Sections (`III. Proposed Design`)

- [x] 2.1 Consolidate `Core Design Principles and Trade-offs` into `\section{Proposed Design}` (`PA–PF`), explicitly covering `PA` (Aim), `PB` (5 core mindsets & Price Action basis), `PC` (Cure/decoupling rationale), `PD` (3 design trade-offs), `PE` (all 4 tiers / 8 modules with read-only JSON & calibrated XGBoost contracts), and `PF` (RR, cost-adjusted Expectancy $E$, and Position Size equations).
- [x] 2.2 Organize `PG–PM` subsections inside `\section{Proposed Design}`, verifying `PG` (`fig1_arch.png`, `fig2_flow.png`), `PH` (how modules interact including `<20ms` short-circuit & `<1.5s` budget), `PI` (`Algorithm 1` pseudocode with PIT `close_time <= t` & 3-level degradation), `PJ` (`BTC/USDT 15m` walkthrough), `PK` (key architectural strengths), `PL` (bridge to Section IV simulation), and `PM` (Proposition proving cost-adjusted positive expectancy threshold $W > (1 + c_{\mathrm{rt}})/(1 + \mathrm{RR})$ and bounded per-trade risk $\le R_{\mathrm{tol}}$).

## 3. Evaluation, Conclusion & References (`IV. Experiment`, `V. Conclusion`, `References`)

- [x] 3.1 Refactor `\section{Experiment}` (`Q–V`) to verify `Q` (Prediction, Trading, Consistency, and Abstention metrics), `R` (Baselines B1–B3 & S&D+XGBoost ablation), `S` (4-stage PIT setup on `BTC/USDT` & `ETH/USDT` 5m/15m under 0.04% taker fee + slippage), `T` (testable parameter thresholds $W_{\min}, \mathrm{RR}_{\min}$), `U` (hypothesized Expected Targets `Table III` explicitly labeled as unexecuted hypotheses), and `V` (mapping to `RQ1–RQ5` falsification criteria).
- [x] 3.2 Refactor `\section{Conclusion}` (`W–Z`) and `\begin{thebibliography}` (`References`), verifying `W` (what proposed), `X` (eXcel/decoupling strengths), `Y` (expected yields summary without overclaiming), and `Z` (limitations, negative-ablation commitment, and future work), plus IEEE reference formatting consistency.

## 4. LaTeX Verification, Overleaf Synchronization & Page-Count Check

- [x] 4.1 Mirror the finalized `ICEIB paper/main.tex` to `ICEIB paper/main.txt` and verify all local figure paths (`fig1_arch.png`, `fig2_flow.png`), `\label`/`\ref` pairs, and `\cite` keys match without dangling references.
- [x] 4.2 Sync `ICEIB paper/main.tex` (and figures if needed) to the target Overleaf project via Overleaf MCP, run `compile_project`, inspect `download_log` for zero fatal errors or `??` undefined references, and verify via `get_page_count` that the PDF is 4–6 pages inclusive.
