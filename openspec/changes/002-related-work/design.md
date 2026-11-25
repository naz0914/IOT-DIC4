# Design

## Context
See `proposal.md` for scope. `\section{Related Work}` spans lines 85–143 of `ICEIB paper/main.tex`, including `Table I` (`table*` spanning two columns) and `Table II` (`table` single-column).

## Goals / Non-Goals

**Goals:**
- Preserve 50/50 balance between AI methods (Chen & Guestrin 2016, arXiv:2407.11786, Koviazin et al. 2026, FinForesight 2026) and Domain Knowledge (Almeida & Goncalves 2023, Brooks 2012, SEBI 2026).
- Guarantee zero horizontal table overflow in `IEEEtran`.

**Non-Goals:**
- Adding unverified citations or unimplemented baseline benchmarks.

## Decisions
- **Choice**: Use `\scriptsize` and `\setlength{\tabcolsep}{3.5pt}` with tuned `p{...}` column widths on both `Table I` and `Table II`.
- **Rationale**: Standard `6pt` `\tabcolsep` caused `32.17pt` and `13.44pt` `Overfull \hbox` overflows in `IEEEtran`; `3.5pt` eliminates all overflow while keeping readability intact.
- **RQ Mapping**: `II. Related Work` grounds the necessity of spatial features (`RQ1`), Price Action semantics (`RQ2`), deterministic math/consistency (`RQ3`), abstention (`RQ4`), and PIT/fee rigor (`RQ5`).

## Risks / Trade-offs
- **[Two-Column Float Displacement]** -> `table*` floats to the top of the next page in `IEEEtran`; keep `tab:lit_summary` anchored inside Section II so it lands on p.2–p.3 near its discussion.
