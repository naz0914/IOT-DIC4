# Proposal

## Why

`I. Introduction` must establish market urgency, synthesize three generations of trading support systems, expose their empirical failure modes (noise whipsaws, LLM seed variance, historical data leakage), state four testable contributions, and conclude with a paper roadmap (`G–L` in `IMG_9464.png`). Isolating `001-introduction` as an OpenSpec change ensures every research claim (`RQ1–RQ5`) is grounded in verified facts and directly motivates the decoupled architecture.

## What Changes

- Manage `\section{Introduction}` (`\label{sec:intro}`) in `ICEIB paper/main.tex` as a standalone spec-driven unit mapped to `G–L`.
- Ground urgency (`G–H`) in verified SEBI (2026) and Reuters (Oct 2025) figures and literature critique (`I–J`) in Koviazin et al. (2026) and FinForesight (2026) without extrapolating Indian equity F&O rates directly to crypto perpetuals.
- Maintain four testable contributions (`K`) and an explicit section-by-section organization paragraph (`L`) pointing to Sections II–V.

## Capabilities

### New Capabilities
- `section-introduction`: Specification for `G–L` narrative flow, verified empirical motivation, research-question framing, testable contributions, and explicit section roadmap.

### Modified Capabilities
<!-- None -->

## Impact

- **Target Section**: `ICEIB paper/main.tex` (`\section{Introduction}`), cross-checked against `docs/introduction.md` and `docs/references.md`.
- **Overleaf Sync**: Syncs to Overleaf project `DIC4` (`6b064825cdb12bbb4e48c26a`) on apply.
