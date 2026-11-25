# Spec Delta

## Purpose
Define requirements for `II. Related Work` (`002-related-work`) covering `M–O` progression, balanced AI/Domain literature coverage, and verified IEEE tables.

## ADDED Requirements

### Requirement: Balanced M–O Literature Review
`\section{Related Work}` SHALL cover `M` (Methods ordered simple-to-complex across 50% AI Technology and 50% Financial Domain Knowledge), `N` (New proposed positioning), and `O` (Organize structured Literature Summary and Architectural Capability comparison matrices).

#### Scenario: Equal AI and Domain subsections with synthesis gap
- **WHEN** `II. Related Work` is reviewed
- **THEN** it SHALL contain dedicated AI Technology and Domain Knowledge subsections referencing only verified entries in `docs/references.md`
- **AND** SHALL conclude with `\subsection{Research Gap and New Proposed Positioning}` citing `Table~\ref{tab:arch_compare}`.

### Requirement: Clean IEEE Table Formatting and PIT / Friction Grounding
`Table~\ref{tab:lit_summary}` and `Table~\ref{tab:arch_compare}` SHALL highlight PIT leakage controls (`FinForesight`), LLM variance (`Koviazin et al.`), and microstructure fee/slippage friction (`Almeida & Goncalves`), and SHALL compile within IEEE column margins without `Overfull \hbox`.

#### Scenario: Tables compile with zero horizontal overflow
- **WHEN** `ICEIB paper/main.tex` compiles via Overleaf `pdfLaTeX`
- **THEN** `output.log` SHALL report zero `Overfull \hbox` warnings on `tab:lit_summary` and `tab:arch_compare`.
