## ADDED Requirements

### Requirement: User Risk Configuration Validation
The system SHALL accept and validate user trading preferences (`symbol`, `timeframe`, `risk_tolerance_pct`, `taker_fee_pct`, `slippage_bps`) via the User Preference Block before ingesting market data.

#### Scenario: Valid trading configuration
- **WHEN** a user configures `symbol="BTC/USDT"`, `timeframe="15m"`, and `risk_tolerance_pct=1.0`
- **THEN** the system SHALL initialize the pipeline with those parameters
- **AND** SHALL bind the fee and slippage schedule for all downstream expectancy calculations.

#### Scenario: Invalid or unsafe risk tolerance
- **WHEN** a user provides `risk_tolerance_pct <= 0` or `risk_tolerance_pct > 5.0`
- **THEN** the system SHALL reject the configuration
- **AND** SHALL return an explicit validation error without starting signal generation.

### Requirement: Point-in-Time OHLCV Data Ingestion
The Data Ingestion Engine SHALL fetch closed OHLCV bars (`N = 200..500`) and enforce strict Point-in-Time (PIT) boundary filtering so no bar or external context with timestamp `> t` is visible at decision time `t`.

#### Scenario: Standard closed-bar ingestion
- **WHEN** a new `5m` or `15m` candle closes at timestamp `t`
- **THEN** the ingestion engine SHALL return the ordered window of closed bars with `close_time <= t`
- **AND** SHALL record the PIT snapshot timestamp in the pipeline context.

#### Scenario: Future-timestamp data leakage detected
- **WHEN** an incoming record or external context item contains a timestamp `> t`
- **THEN** the ingestion engine SHALL exclude that record from the snapshot
- **AND** SHALL log a PIT leakage prevention event.

### Requirement: Feed Heartbeat Monitoring
The Data Ingestion Engine SHALL monitor WebSocket/REST feed latency and trigger degradation alerts when heartbeat delay exceeds configured thresholds.

#### Scenario: Heartbeat delay exceeds 2 seconds
- **WHEN** market feed heartbeat latency exceeds `2,000 ms`
- **THEN** the system SHALL flag `DEGRADED_L1` status to the Decision & Risk Engine
- **AND** SHALL freeze generation of new `APPROVED` trade signals.
