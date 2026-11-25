# Spec Delta

## Purpose
Define the IEEE ICEIB conference LaTeX formatting constraints, visual asset requirements, and Overleaf synchronization and compilation verification criteria for delivering the finalized manuscript.

## ADDED Requirements

### Requirement: IEEE ICEIB Conference Layout Compliance
The LaTeX source (`ICEIB paper/main.tex`) SHALL compile with `pdfLaTeX` using `\documentclass[conference]{IEEEtran}`, 10pt Times two-column layout, and SHALL comply with IEEE ICEIB proceedings constraints (4–6 pages total, no manual page numbers, no custom running headers or footers, and anonymous/placeholder author metadata clearly marked if camera-ready details are pending).

#### Scenario: Page count and IEEEtran formatting validated
- **WHEN** `ICEIB paper/main.tex` is compiled to PDF
- **THEN** the rendered PDF page count SHALL be between 4 and 6 pages inclusive
- **AND** no `\pagestyle{plain}`, `\thispagestyle`, or manual page-number overrides SHALL appear in the document.

### Requirement: Figures, Tables, Equations, and Cross-Reference Integrity
All referenced figures (`fig1_arch.png`, `fig2_flow.png`), tables (Literature Summary Table, Architectural Capability Matrix, Hypothesized Expected Targets Table), numbered equations, `Algorithm 1`, and `\cite{...}` keys SHALL compile without undefined references, missing files, or overfull column-breaking overflows.

#### Scenario: All LaTeX cross-references resolve cleanly
- **WHEN** the LaTeX build log is inspected after compilation
- **THEN** there SHALL be zero fatal compilation errors, zero missing image files, and zero undefined `\ref{...}` or `\cite{...}` warnings (`??`).

### Requirement: Overleaf Synchronization and Dual-Artifact Delivery
Updated manuscript sources in `ICEIB paper/main.tex` (and mirrored `ICEIB paper/main.txt` if retained for plain-text upload convenience) SHALL be synchronized to the target Overleaf project via Overleaf MCP and verified with `compile_project` and `get_page_count`.

#### Scenario: Overleaf remote compilation succeeds after sync
- **WHEN** `ICEIB paper/main.tex` is pushed or updated in the connected Overleaf project
- **THEN** `compile_project` SHALL return success
- **AND** `get_page_count` SHALL confirm the compiled PDF stays within the 4–6 page IEEE ICEIB target range.
