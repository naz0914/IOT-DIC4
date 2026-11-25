# Proposal

## Why

The IEEE ICEIB Abstract is the primary entry gate for reviewers and must compress the entire Hybrid TPA-Agent motivation, decoupling hypothesis, four-tier method, PIT evaluation protocol, and falsifiable outcomes into a single IEEE conference paragraph following the 2020 A–Z SOP (`A–F`). Breaking `000-abstract` into its own OpenSpec change allows targeted review, word-budget tuning, and empirical integrity checks without touching downstream sections.

## What Changes

- Manage `\begin{abstract}...\end{abstract}` and `\begin{IEEEkeywords}...\end{IEEEkeywords}` in `ICEIB paper/main.tex` (and synchronized Overleaf `main.tex`) as an independently reviewable specification unit.
- Enforce sequential `A–F` coverage (`A: Attention Getter`, `B: But/Challenge`, `C: Cure`, `D: Development`, `E: Experiments`, `F: Findings`) and IEEE Index Terms alignment.
- Preserve the strict distinction between hypothesized Expected Targets (e.g., `<= 12%` MDD, `> 95%` consistency) and empirically verified results, avoiding unexecuted backtest claims.

## Capabilities

### New Capabilities
- `section-abstract`: Specification for single-paragraph IEEE conference Abstract (`A–F`), `IEEEkeywords`, functional decoupling summary, and empirical-claim discipline.

### Modified Capabilities
<!-- None -->

## Impact

- **Target Section**: `ICEIB paper/main.tex` (`\begin{abstract}` and `\begin{IEEEkeywords}`), cross-checked against `docs/abstract.md` and `IMG_9464.png`.
- **Overleaf Sync**: Syncs to Overleaf project `DIC4` (`6b064825cdb12bbb4e48c26a`) on apply.
