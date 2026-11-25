# Spec Delta

## Purpose
Define verification requirements for `References` (`006-reference`) ensuring IEEE formatting compliance, verified source provenance, and zero dangling `\cite` keys.

## ADDED Requirements

### Requirement: Verified IEEE Bibliography Provenance
Every `\bibitem` in `ICEIB paper/main.tex` SHALL correspond to a verified source in `docs/references.md` or verified project documentation, preserving accurate authors, years, venues, and DOIs without fabricating citation details.

#### Scenario: All bibliography entries verified against docs/references.md
- **WHEN** `\begin{thebibliography}` is audited against `docs/references.md`
- **THEN** all 8 domain/AI references (`sebi2026`, `reuters2025`, `brooks2012`, `chen2016`, `arxiv2407`, `koviazin2026`, `finforesight2026`, `almeida2023`) plus `fission2026openspec` SHALL match verified bibliographic metadata.

### Requirement: Zero Undefined or Unused Citations in IEEE Format
Every `\cite{...}` across Sections I–V SHALL resolve to a defined `\bibitem{...}` formatted in IEEE style, and LaTeX compilation SHALL emit zero undefined-citation warnings (`?`).

#### Scenario: Bidirectional cite-bibitem audit passes cleanly
- **WHEN** `ICEIB paper/main.tex` is compiled via Overleaf `pdfLaTeX`
- **THEN** `output.log` SHALL contain zero `Citation ... undefined` warnings and every `\bibitem` SHALL be cited at least once in the manuscript body.
