# Spec Delta

## Purpose
Define the structural, methodological, and academic integrity requirements for authoring and refactoring the Hybrid TPA-Agent ICEIB conference manuscript in strict alignment with the 2020 A–Z SOP (`IMG_9464.png`) and the requested seven-section IEEE conference outline.

## ADDED Requirements

### Requirement: Seven-Section IEEE ICEIB Paper Outline
The manuscript SHALL be organized into exactly seven top-level academic parts matching the ICEIB submission structure: `Abstract` (with `IEEEkeywords`), `I. Introduction`, `II. Related Work`, `III. Proposed Design`, `IV. Experiment`, `V. Conclusion`, and `References`, without leaving detached top-level sections outside this outline.

#### Scenario: Section structure verified against outline
- **WHEN** the manuscript source (`ICEIB paper/main.tex`) is inspected from `\begin{abstract}` through `\end{thebibliography}`
- **THEN** top-level `\section{...}` headings SHALL appear in the exact sequence `Introduction`, `Related Work`, `Proposed Design`, `Experiment`, and `Conclusion`
- **AND** `Core Design Principles` and `Limitations` SHALL be integrated inside `Proposed Design` and `Conclusion` respectively rather than existing as separate top-level sections.

### Requirement: Front-Matter A–O SOP Coverage (`Abstract`, `Introduction`, `Related Work`)
`Abstract`, `I. Introduction`, and `II. Related Work` SHALL cover all `A–O` items from the 2020 A–Z SOP (`IMG_9464.png`), balancing 50% AI Technology and 50% Financial Domain Knowledge in the literature review.

#### Scenario: Abstract covers A–F in a single IEEE paragraph
- **WHEN** `Abstract` is inspected
- **THEN** it SHALL present `A` (Attention Getter), `B` (But/Challenge), `C` (Cure), `D` (Development), `E` (Experiments), and `F` (Findings) sequentially in one IEEE conference paragraph.

#### Scenario: Introduction covers G–L with explicit roadmap
- **WHEN** `I. Introduction` concludes after `K` (Keypoint contributions)
- **THEN** it SHALL cover `G` (General), `H` (However), `I` (In literature), `J` (Judgement), `K` (Keypoints), and end with an explicit `L` (List the organization) paragraph introducing Sections II through V.

#### Scenario: Related Work covers M–O with balanced comparison matrices
- **WHEN** `II. Related Work` is inspected
- **THEN** it SHALL organize `M` (Methods from simple to complex), `N` (New proposed positioning), and `O` (Literature Summary Table and Architectural Capability Matrix) with balanced 50% AI and 50% Domain references.

### Requirement: Technical and Experimental A–Z SOP Coverage (`Proposed Design`, `Experiment`, `Conclusion`)
`III. Proposed Design`, `IV. Experiment`, and `V. Conclusion` SHALL cover all `PA–PM`, `Q–V`, and `W–Z` items from the 2020 A–Z SOP (`IMG_9464.png`).

#### Scenario: Proposed Design covers PA through PM cleanly
- **WHEN** `III. Proposed Design` is reviewed against `PA–PM`
- **THEN** it SHALL cover `PA` (Aim), `PB` (Based-on), `PC` (Cure), `PD` (Design paradigm shift), `PE` (Elements: 4 tiers / 8 modules), `PF` (Formulation), `PG` (Graph: architecture & flow figures), `PH` (How modules interact), `PI` (Implementation / Algorithm 1), `PJ` (Jump to `BTC/USDT 15m` example), `PK` (Key points contribution), `PL` (Later simulation bridge), and `PM` (Math proof of cost-adjusted expectancy & bounded risk).

#### Scenario: Experiment covers Q–V and Conclusion covers W–Z
- **WHEN** `IV. Experiment` and `V. Conclusion` are reviewed
- **THEN** `IV. Experiment` SHALL cover `Q` (metrics), `R` (baselines B1–B3 & Ablation), `S` (4-stage PIT setup), `T` (tuning parameters $W_{\min}, \mathrm{RR}_{\min}$), `U` (hypothesized targets), and `V` (RQ1–RQ5 verification criteria)
- **AND** `V. Conclusion` SHALL cover `W` (What proposed), `X` (eXcel/strengths), `Y` (Yields summary), and `Z` (Zen/limitations & future work).

### Requirement: Decoupled Architecture, PIT Leakage Prevention, and Degradation Fidelity
The `III. Proposed Design` and `IV. Experiment` sections SHALL explicitly distinguish deterministic spatial computation (Module 2.1 S&D zones and Volume Profile HVN), calibrated ML probability inference (Module 2.2 XGBoost), read-only JSON-constrained LLM Chain-of-Thought reasoning (Module 3.2), and programmatic risk gating (Module 4.1), and SHALL specify point-in-time (PIT) data leakage prevention and 3-level failure degradation scenarios.

#### Scenario: PIT leakage prevention and 3-level degradation documented
- **WHEN** the data pipeline and risk gating mechanisms are described in `III. Proposed Design` and `IV. Experiment`
- **THEN** the manuscript SHALL enforce the PIT constraint `close_time <= t` across all historical slices and stress windows
- **AND** SHALL specify Level 1 (`> 2s` latency or LLM schema/timeout failure -> `WAIT/NO_TRADE`), Level 2 (open-position server-side stop-loss verification), and Level 3 (`> 30s` outage or extreme OOD circuit breaker).

### Requirement: Empirical Integrity and Verified Citation Discipline
The manuscript SHALL strictly distinguish hypothesized Expected Targets from completed empirical backtest results and SHALL cite only verified references present in `docs/references.md` or verified project sources.

#### Scenario: No unexecuted backtest results claimed as empirical findings
- **WHEN** `Abstract`, `IV. Experiment`, or `V. Conclusion` references target metrics (e.g., `~59.2%` win rate, `1.94` Profit Factor, `<= 12%` MDD, `> 95%` consistency)
- **THEN** the text and table captions SHALL explicitly label those numbers as hypothesized Expected Targets for future falsification rather than completed experimental outcomes.
