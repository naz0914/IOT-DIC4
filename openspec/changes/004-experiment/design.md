# Design

## Context
See `proposal.md` for scope. `\section{Experiment}` spans lines 247–291 of `ICEIB paper/main.tex`, including `Table III` (`tab:targets`).

## Goals / Non-Goals

**Goals:**
- Map `RQ1–RQ5` directly to baselines `B1–B3` + `Ablation` and four metric families (`Q–V`).
- Allow seamless future replacement of `Table III` once real PIT backtest logs exist, without altering section structure.

**Non-Goals:**
- Generating synthetic equity curves or claiming unexecuted backtests.

## Decisions
- **Choice**: Pair `Q–T` in `\subsection{Metrics, Baselines, Simulation Setup, and Parameter Tuning (Q--T)}` and `U–V` in `\subsection{Hypothesized Expected Targets and Effectiveness Verification (U--V)}` with `\setlength{\tabcolsep}{3.5pt}` on `Table III`.
- **Rationale**: Keeps the section compact (fitting cleanly on p.5), prevents `18.14pt` `Overfull \hbox`, and foregrounds falsification rules before `Table III`.
- **RQ & Tier Mapping**: `RQ1` (Layer 2.1+2.2: Precision, ROC-AUC, Brier, PF), `RQ2` (Layer 3.2: Sharpe/MDD vs. Ablation), `RQ3` (Layer 3.2+4.1: Consistency `>95%`, 0% numeric/risk violations), `RQ4` (Layer 4.1 `WAIT` counterfactuals), `RQ5` (Layer 1.2 PIT + Oct 2025 stress + forward sim).

## Risks / Trade-offs
- **[Reviewers Mistaking Expected Targets for Results]** -> Maintain the explicit `\textit{Disclosure:}` lead-in and `(Not Verified Results)` in `Table III`'s caption until empirical runs complete.
