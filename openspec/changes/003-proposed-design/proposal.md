# Proposal

## Why

`III. Proposed Design` is the technical core of the ICEIB paper and must cover all thirteen `PA–PM` elements from the 2020 A–Z SOP (`IMG_9464.png`)—unifying theoretical mindsets/trade-offs, the 4-tier/8-module architecture, architecture/flowchart figures, mathematical equations, `Algorithm 1`, the `BTC/USDT 15m` walkthrough, three-level failure degradation, and a formal positive-expectancy proof (`PM`). Managing `003-proposed-design` independently guarantees end-to-end alignment with `DESIGN.md` and the core system specs.

## What Changes

- Manage `\section{Proposed Design}` (`\label{sec:design}`) in `ICEIB paper/main.tex` as an OpenSpec unit covering `PA–PM`.
- Enforce strict decoupling across Layer 1 (PIT ingestion), Layer 2 (deterministic S&D/HVN + calibrated XGBoost), Layer 3 (Price Action KB + zero-temp read-only JSON LLM CoT), and Layer 4 (ATR SL/TP, `RR >= 2.0`, cost-adjusted `E_net > 0`, `WAIT/NO_TRADE`, HITL gate, and 3-level degradation).
- Maintain `Proposition 1` (`PM`) proving cost-adjusted positive expectancy threshold $W > (1+c_{\mathrm{rt}})/(1+\mathrm{RR})$ and bounded stop-loss exposure $\le R_{\mathrm{tol}}$.

## Capabilities

### New Capabilities
- `section-proposed-design`: Specification for `PA–PM` architectural exposition, 4-tier/8-module contracts, `Algorithm 1`, walkthrough, 3-level degradation, and `Proposition 1` proof.

### Modified Capabilities
<!-- None -->

## Impact

- **Target Section**: `ICEIB paper/main.tex` (`\section{Proposed Design}`, `fig:arch`, `fig:flow`, `eq:rr`–`eq:pos`, `alg:tpa`, `prop:expectancy`), referencing `DESIGN.md` and `ICEIB paper/fig{1_arch,2_flow}.png`.
- **Overleaf Sync**: Syncs to Overleaf project `DIC4` (`6b064825cdb12bbb4e48c26a`) on apply.
