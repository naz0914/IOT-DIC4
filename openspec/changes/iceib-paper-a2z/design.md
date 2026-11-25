# Design

## Context
See `proposal.md` for motivation and scope. Currently, `ICEIB paper/main.tex` (292 lines) uses `\documentclass[conference]{IEEEtran}` and already includes `fig1_arch.png`, `fig2_flow.png`, `Algorithm 1`, two comparison tables, and one hypothesized target table. However, its section organization diverges from the requested 7-part ICEIB structure (`Abstract`, `I. Introduction`, `II. Related Work`, `III. Proposed Design`, `IV. Experiment`, `V. Conclusion`, `References`) and omits explicit 2020 A–Z SOP (`IMG_9464.png`) traceability (`L` in Introduction, consolidated `PA–PM` in Proposed Design, `Q–V` in Experiment, `W–Z` in Conclusion).

## Goals / Non-Goals

**Goals:**
- Refactor `ICEIB paper/main.tex` into the exact 7-part IEEE ICEIB structure while ensuring 100% letter-by-letter coverage of the 2020 A–Z SOP (`A–Z`).
- Consolidate the 4-tier / 8-module architecture, mathematical formulation + boundary proof (`PM`), `Algorithm 1` (`PI`), `BTC/USDT 15m` walkthrough (`PJ`), and 3-level failure degradation cleanly under `III. Proposed Design` (`PA–PM`).
- Preserve full traceability from research claims `RQ1–RQ5` to evaluation metrics (`IV. Experiment`, `Q–V`) and maintain strict separation between hypothesized Expected Targets and completed empirical findings.
- Verify clean `pdfLaTeX` / Overleaf compilation within the 4–6 page IEEE ICEIB limit.

**Non-Goals:**
- Implementing Python trading engine source code or running historical backtests (covered separately by `refactor-hybrid-tpa-agent`).
- Fabricating or backfilling empirical backtest metrics.
- Replacing verified citations in `docs/references.md` with unverified references.

## Decisions

### Decision 1: Section Consolidation (`Core Design Principles` → `III. Proposed Design`)
- **Choice**: Fold the existing standalone `Core Design Principles and Trade-offs` section into `III. Proposed Design` as subsections covering `PA–PF` (Aim, Based-on, Cure, Design Principles & Trade-offs, Elements, and Mathematical Foundation).
- **Rationale**: Matches both the user's requested 7-section outline (`Proposed Design` directly after `Related Work`) and the 2020 A–Z SOP (`III. Proposed Scheme: PA–PM`), eliminating redundant narrative while preserving all 5 theoretical mindsets and 3 design trade-offs.
- **Alternative Considered**: Keeping `Core Design Principles` as Section III and shifting `Proposed Design` to Section IV. Rejected because it breaks the user's requested section sequence and separates `PB/PC/PD` from `PE–PM`.

### Decision 2: Mathematical Proof (`PM`) Framing Without Overclaiming
- **Choice**: Implement `PM` (Math proof / theoretical guarantee) as a concise, rigorous **Proposition on Cost-Adjusted Positive Expectancy and Bounded Capital Risk** inside `III. Proposed Design` (proving the exact minimum calibrated win-rate threshold $W > \frac{1 + c_{\text{rt}}}{1 + \mathrm{RR}}$ under round-trip fee/slippage friction $c_{\text{rt}}$ and bounded single-trade equity loss $\le R_{\text{tol}}$).
- **Rationale**: Satisfies `PM` mathematically without making false empirical profitability claims.
- **Alternative Considered**: Claiming asymptotic profitability convergence. Rejected because financial return distributions are non-stationary and cannot support unconditional convergence proofs.

### Decision 3: 4-Tier / 8-Module and `RQ1–RQ5` Traceability Mapping
All 4 tiers / 8 modules, API/JSON contracts, uncertainty/calibration, failure recovery, and `RQ1–RQ5` claims are mapped into the manuscript as follows:

| Tier / Module | Paper Section & A–Z Element | Key Contract / Calibration / Recovery Detail | Mapped Research Claim & Verification Metrics |
| :--- | :--- | :--- | :--- |
| **L1.1 User Preference Block** | `III. Proposed Design` (`PE`, `PF`) | Validates `S`, `TF in {5m, 15m}`, `R_tol in (0%, 5%]`, fee/slippage schedules | `RQ4`, `RQ5` — Unified capital & cost fairness |
| **L1.2 Data Ingestion Engine** | `III. Proposed Design` (`PE`, `PI`) | Closed OHLCV `N in [200, 500]`, strict PIT `close_time <= t`, heartbeat monitoring | `RQ5` — PIT snapshot isolation, Oct 2025 stress window |
| **L2.1 Deterministic S&D Zone Engine** | `III. Proposed Design` (`PD`, `PE`, `PH`) | `O(N)` Swing scan + `O(N log B)` Volume Profile HVN (`<15ms`); lifecycle `UNMITIGATED`/`TESTED`/`SR_FLIP`/`INVALIDATED` | `RQ1` — Precision, ROC-AUC, Profit Factor vs. Pure XGBoost |
| **L2.2 ML Win-Rate Engine** | `III. Proposed Design` (`PE`, `PF`, `PI`) | Microstructure features -> Platt/Isotonic calibrated XGBoost `W in [0, 1]` (`<5ms`); short-circuits `W < W_min` | `RQ1`, `RQ4` — Brier Score calibration, ROC-AUC, tuning of `W_min` |
| **L3.1 Domain Knowledge Base** | `III. Proposed Design` (`PB`, `PE`) | Codified Price Action rules: Pinbar absorption, Liquidity Sweep, ChoCh, S/R Flip | `RQ2` — Structural false-breakout filtering gain |
| **L3.2 Planner & Reasoning Agent** | `III. Proposed Design` (`PD`, `PE`, `PH`, `PI`) | Read-only JSON input, `temperature=0.0` CoT, strict schema `{is_valid_setup, pattern_type, evidence, invalidation_flags}`; timeout/malformed -> `WAIT/NO_TRADE` | `RQ2`, `RQ3` — Sharpe, PF, multi-run Consistency Rate (`>95%`), 0% numeric hallucination |
| **L4.1 Decision & Risk Engine** | `III. Proposed Design` (`PF`, `PM`, `PI`) | Deterministic ATR SL, opposing-zone TP, `RR >= RR_min`, cost-adjusted `E > 0`, `Pos`, `WAIT/NO_TRADE` counterfactual log, 3-level degradation | `RQ3`, `RQ4` — Risk Violation Rate (`0%`), MDD, `WAIT` counterfactual return comparison |
| **L4.2 Output Interface** | `III. Proposed Design` (`PE`, `PJ`) | Hash-stamped JSON + Markdown decision card with HITL approval gate | `RQ3`, `RQ5` — Hash-committed forward audit trail |

### Decision 4: Page-Budget Control (4–6 Pages) & Overleaf Delivery
- **Choice**: Target 5–6 pages in `IEEEtran` two-column conference layout by keeping prose tight, placing `Table I` (Literature Summary) as a clean `table*` span and compact `Table II`/`Table III` as single-column `table` floats, and using `ICEIB paper/main.tex` as the canonical source mirrored to `main.txt` and Overleaf via Overleaf MCP.
- **Rationale**: `IEEEtran` conference proceedings enforce strict 4–6 page limits for ICEIB; checking `get_page_count` via Overleaf MCP after `compile_project` gives immediate ground-truth verification.

## Risks / Trade-offs
- **[Page Overflow > 6 Pages]** → Including all `PA–PM` elements, three tables, two figures, and `Algorithm 1` risks spilling onto page 7. Mitigation: Keep `PJ` walkthrough and `PM` proposition concise, tune float placement (`[t]`), and verify exact page count via Overleaf `get_page_count` during apply.
- **[Overleaf Project Selection Ambiguity]** → Multiple Overleaf projects may exist in the user's account. Mitigation: Run `list_projects` at the start of apply and confirm or match the existing ICEIB project before editing/uploading remote files.
- **[Pending Author/Affiliation Metadata]** → `main.tex` currently contains bracketed placeholders (`[Author 1 -- ID: ...]`). Mitigation: Keep team IDs/names clearly marked as `TODO` before final camera-ready submission without inventing affiliations or emails.
