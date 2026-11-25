# Proposal

## Why

The project already contains a preliminary IEEE conference manuscript draft (`ICEIB paper/main.tex`), refactored academic notes (`docs/*.md`), and the **2020 A-Z SOP** writing standard (`IMG_9464.png`), yet the manuscript does not currently enforce the user's required 7-part IEEE ICEIB paper structure (`Abstract`, `Introduction`, `Related Work`, `Proposed Design`, `Experiment`, `Conclusion`, `References`) or full A–Z letter-by-letter structural traceability (for example, `Introduction` omits `L: List the organization`, `Core Design Principles` is separated from `Proposed Design`, and sections are not explicitly mapped to `A–F`, `G–L`, `M–O`, `PA–PM`, `Q–V`, and `W–Z`). Formalizing the ICEIB paper authoring and refactoring workflow as a dedicated OpenSpec change ensures every section, equation, figure, table, and citation satisfies IEEE ICEIB proceedings formatting (4–6 pages, 10pt Times two-column `IEEEtran`), adheres strictly to the 2020 A-Z SOP, preserves the strict boundary between hypothesized Expected Targets and empirically validated results, and compiles cleanly in Overleaf.

## What Changes

- Define an OpenSpec specification-driven manuscript workflow (`iceib-paper-a2z`) targeting `ICEIB paper/main.tex` (and synchronized Overleaf project compilation) without altering core trading engine code or fabricating unexecuted backtest results.
- Align the paper's top-level section structure strictly to the requested ICEIB academic outline:
  1. `Abstract` (A–F) + `IEEEkeywords`
  2. `I. Introduction` (G–L, including explicit `L: List the organization`)
  3. `II. Related Work` (M–O, balancing 50% AI Technology and 50% Financial Domain Knowledge with comparison matrix and summary table)
  4. `III. Proposed Design` (PA–PM: Aim, Based-on, Cure, Design, Elements, Formulation, Graph, How, Implementation/Algorithm 1, Jump Example, Key Contributions, Later Simulation linkage, Math Proof/Expectancy bounds)
  5. `IV. Experiment` (Q–V: Quality/Quantity metrics, Related baselines, Simulation setup with PIT/fees/slippage, Tuning parameters $W_{\min}$/$RR_{\min}$, Useful evaluation protocol & hypothesized targets clearly labeled as non-empirical, Verify effectiveness criteria for RQ1–RQ5)
  6. `V. Conclusion` (W–Z: What proposed, eXcel/strengths, Yields/expected outcomes, Zen/limitations & future work)
  7. `References` (IEEE citation format, verified sources only)
- Enforce strict separation of deterministic geometry, calibrated ML win-rate inference, read-only JSON-constrained LLM CoT reasoning, and programmatic risk gating (including PIT data leakage prevention and 3-level degradation) throughout `Proposed Design` and `Experiment`.
- Define IEEE ICEIB conference formatting compliance checks (page count 4–6 pages, `IEEEtran` conference option, no manual page numbering/headers/footers, clean figure/table placement) and Overleaf MCP sync/compile verification.
- Note on scope and risk: Because empirical backtests have not yet been executed, all performance targets (e.g., expected Profit Factor `1.94`, `MDD <= 12%`, consistency `> 95%`) SHALL remain explicitly labeled as hypothesized Expected Targets rather than observed empirical findings; failing to maintain this distinction or exceeding the 4–6 page ICEIB budget are the primary risks mitigated by this spec.

## Capabilities

### New Capabilities
- `iceib-a2z-manuscript`: Specification of section structure, 2020 A-Z SOP (`A–Z`) content mapping, Hybrid TPA-Agent 4-tier/8-module technical fidelity (PIT control, calibration, risk gating, 3-level degradation), and academic integrity rules (distinguishing hypothesized Expected Targets from validated results and verified citations).
- `iceib-latex-delivery`: Specification of IEEE ICEIB `IEEEtran` conference formatting (10pt Times two-column, 4–6 pages, no manual page numbers/headers/footers, figure/table/algorithm formatting) and Overleaf MCP synchronization and compilation verification.

### Modified Capabilities
<!-- None: openspec/specs/ currently has no durable capabilities; refactor-hybrid-tpa-agent covers the software prototype specs separately. -->

## Impact

- **Affected Files**: `ICEIB paper/main.tex` (and `ICEIB paper/main.txt` / `ICEIB paper/iceib_overleaf_upload.zip` if regenerated during apply), referencing `IMG_9464.png`, `DESIGN.md`, `README.md`, and `docs/{abstract,introduction,related-work,deep-research,references}.md`.
- **External Integration**: Overleaf MCP (`compile_project`, `sync_project` / `upload_file` / `edit_file`, `get_page_count`, `download_log`) during the apply phase.
- **No Unreviewed Execution**: Planning phase produces OpenSpec artifacts only; `ICEIB paper/main.tex` and Overleaf are not modified until the user invokes `/opsx:apply` (or `openspec-apply-change`).
