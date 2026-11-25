## ADDED Requirements

### Requirement: Decoupled Price Action Chain-of-Thought Reasoning
The Planner & Reasoning Agent SHALL evaluate price-action setups using structured JSON inputs from Layers 1–2 and rules from the Domain Knowledge Base (`S/R Flip`, `Pinbar` passive absorption, `Liquidity Sweep`, `ChoCh`), and SHALL NOT compute or override zone coordinates, stop-loss prices, position sizes, or ML win probabilities.

#### Scenario: Valid liquidity sweep confirmation
- **WHEN** `current_price` wicks below a demand zone edge with elevated Volume Z-score and closes back inside/above the zone
- **THEN** the agent SHALL match the `Liquidity Sweep / Pinbar` rule from the Domain Knowledge Base
- **AND** SHALL output structured JSON with `is_valid_setup=true` and traceable `evidence` citations.

#### Scenario: Structural rejection overrides high ML score
- **WHEN** `W >= W_min` but the setup occurs directly beneath an unmitigated opposing supply zone or exhibits a low-volume trap
- **THEN** the agent SHALL set `is_valid_setup=false` with explicit `invalidation_flags`
- **AND** SHALL cause the Decision Engine to emit `WAIT/NO_TRADE`.

### Requirement: Schema-Validated and Reproducible LLM Output
The Planner & Reasoning Agent SHALL enforce deterministic decoding settings (`temperature = 0.0`, logged model ID) and strict JSON schema validation, degrading safely on malformed or timed-out responses.

#### Scenario: LLM returns malformed JSON or times out
- **WHEN** the LLM API fails schema validation or exceeds the latency budget timeout
- **THEN** the agent SHALL NOT emit an `APPROVED` signal
- **AND** SHALL degrade to `WAIT/NO_TRADE` with `reason="LLM_SCHEMA_OR_TIMEOUT"`.
