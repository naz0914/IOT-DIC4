# Design

## Context
See `proposal.md` for scope. `\begin{thebibliography}{9}` spans lines 302–331 of `ICEIB paper/main.tex` and currently holds 9 verified entries (`sebi2026` through `fission2026openspec`).

## Goals / Non-Goals

**Goals:**
- Guarantee 100% IEEE reference formatting consistency and zero dangling `\cite` or unused `\bibitem` entries.
- Support optional migration to an external `.bib` file (`references.bib`) if later requested, while keeping inline `thebibliography` self-contained for single-file Overleaf compilation.

**Non-Goals:**
- Adding unverified AI-generated citations or padding reference counts artificially.

## Decisions
- **Choice**: Keep inline `\begin{thebibliography}{9}` inside `ICEIB paper/main.tex` rather than splitting out a separate `.bib` file right now.
- **Rationale**: Single-file `main.tex` + `main.txt` mirroring requires zero separate BibTeX pass and compiles reliably in 1–2 `pdfLaTeX` runs on Overleaf.
- **Scope & Verification Mapping**: All citations trace back to empirical motivation (`sebi2026`, `reuters2025`), AI models & reproducibility (`chen2016`, `arxiv2407`, `koviazin2026`, `finforesight2026`, `fission2026openspec`), and microstructure/Price Action (`almeida2023`, `brooks2012`), directly supporting `RQ1–RQ5`.

## Risks / Trade-offs
- **[Preprint Metadata Updates]** -> `finforesight2026` and `arxiv2407` are preprints; re-verify arXiv IDs/publication venues before final camera-ready submission.
