## ADDED Requirements

### Requirement: Programmatic Risk, RR, and Positive Expectancy Gating
The Decision & Risk Engine SHALL compute `P_sl` (zone outer boundary + ATR buffer), `P_tp` (opposing zone edge), geometric `RR`, cost-adjusted expectancy `E`, and position size `Pos` deterministically, and SHALL approve a trade ONLY when `RR >= RR_min` (candidate `2.0`), cost-adjusted `E > 0`, and `is_valid_setup == true`.

#### Scenario: Positive expectancy setup approved
- **WHEN** `W = 0.62`, `RR = 6.0`, `is_valid_setup = true`, and feed health is normal
- **THEN** the engine SHALL compute `Pos` bounded by `R_tol`
- **AND** SHALL emit `DecisionReport(status="APPROVED")` with Entry, SL, TP, RR, Expectancy, and Evidence.

#### Scenario: Insufficient risk-to-reward rejected
- **WHEN** `RR < RR_min` (e.g., `RR = 1.4 < 2.0`) even if `W >= W_min` and `is_valid_setup = true`
- **THEN** the engine SHALL emit `DecisionReport(status="WAIT/NO_TRADE", reason="RR below threshold")`
- **AND** SHALL log the rejected setup for counterfactual RQ4 tracking.

### Requirement: Human-in-the-Loop Execution Gate and Structured Output
The Output Interface SHALL render every decision as a verifiable JSON + Markdown report requiring operator confirmation before live order submission unless explicitly configured otherwise in sandbox mode.

#### Scenario: Approved setup awaiting human review
- **WHEN** `DecisionReport.status == "APPROVED"`
- **THEN** the Output Interface SHALL attach the PIT timestamp, config hash, and full evidence chain
- **AND** SHALL wait for explicit operator confirmation before dispatching live execution.

### Requirement: Three-Level API Degradation and Circuit Breaker
The Decision & Risk Engine SHALL enforce a 3-level degradation protocol during feed delays, outages, or extreme black-swan volatility.

#### Scenario: Feed outage exceeds 30 seconds (Level 3)
- **WHEN** market data feed remains disconnected for `> 30 seconds`
- **THEN** the engine SHALL halt all new entries (`WAIT/NO_TRADE`), ensure server-side stop-loss orders are active for open positions, and trigger an urgent Level 3 flatten/hedge alert.
