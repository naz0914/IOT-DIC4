## ADDED Requirements

### Requirement: Deterministic Supply and Demand Zone Detection
The Deterministic S&D Zone Engine SHALL identify Swing High/Low extrema and aggregate Volume Profile High-Volume Nodes (HVN) using purely deterministic algorithms (`O(N)` swing scan and `O(N log B)` profile binning) so identical OHLCV inputs always yield identical zone coordinates.

#### Scenario: Identical input produces identical zones
- **WHEN** the S&D Zone Engine processes the same closed OHLCV window twice
- **THEN** it SHALL output bit-identical `demand_zones` and `supply_zones` boundaries and `hvn_price` values
- **AND** SHALL complete execution within `15 ms` for `N <= 500` bars.

### Requirement: Zone Lifecycle and S/R Flip Tracking
The S&D Zone Engine SHALL maintain explicit zone states (`UNMITIGATED`, `TESTED`, `SR_FLIP`, `INVALIDATED`) across price updates.

#### Scenario: High-volume breakdown triggers S/R Flip
- **WHEN** price closes decisively below an active `DEMAND` zone lower bound with elevated volume (`Volume Z-score` above threshold)
- **THEN** the engine SHALL transition the zone to `SR_FLIP` (acting as overhead `SUPPLY` resistance)
- **AND** SHALL exclude it from active long-entry `DEMAND` candidates.

### Requirement: Thin-Liquidity Zone Rejection
The S&D Zone Engine SHALL filter out candidate zones formed during abnormally thin liquidity to avoid false Volume Profile peaks.

#### Scenario: Holiday or low-depth vacuum move
- **WHEN** a candidate swing zone forms while rolling bar volume falls below the minimum liquidity threshold
- **THEN** the engine SHALL mark the zone as low-liquidity / unverified
- **AND** SHALL NOT promote it as a valid trading zone.
