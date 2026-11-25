# Proposal

## Why

Academic credibility requires every `\cite{...}` in the ICEIB manuscript to map to a verified, accurately formatted IEEE `\bibitem{...}` entry with real authors, venues, years, and DOIs—never fabricated citations or mismatched keys. Isolating `006-reference` as its own OpenSpec change provides a dedicated audit gate for bibliography completeness, IEEE style compliance, and cross-reference integrity.

## What Changes

- Manage `\begin{thebibliography}...\end{thebibliography}` in `ICEIB paper/main.tex` as an independent OpenSpec verification unit.
- Enforce 1-to-1 correspondence between `ICEIB paper/main.tex` `\bibitem` entries, in-text `\cite{...}` keys, and verified sources in `docs/references.md` (plus `fission2026openspec`).
- Maintain standard IEEE Transactions/Conference bibliography style (`\textit{...}`, `pp.`, `doi:`, `Oct.~2025`, `\url{...}`) with zero undefined citations.

## Capabilities

### New Capabilities
- `section-references`: Specification for verified IEEE bibliography entries, zero fabricated DOIs/titles, and 100% `\cite`/`\bibitem` cross-reference resolution.

### Modified Capabilities
<!-- None -->

## Impact

- **Target Section**: `ICEIB paper/main.tex` (`\begin{thebibliography}`), cross-checked against `docs/references.md` and `README.md` Section 15.
- **Overleaf Sync**: Syncs to Overleaf project `DIC4` (`6b064825cdb12bbb4e48c26a`) on apply.
