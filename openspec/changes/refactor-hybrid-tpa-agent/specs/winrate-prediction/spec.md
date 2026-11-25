## ADDED Requirements

### Requirement: Microstructure Feature Extraction
The Machine Learning Win-Rate Engine SHALL extract a deterministic feature vector from the PIT OHLCV window and active S&D zones, including Volume Z-score, candle body ratio, wick penetration depth, and normalized distance to zone boundaries.

#### Scenario: Price retests unmitigated demand zone
- **WHEN** `current_price` enters or tests an `UNMITIGATED` demand zone
- **THEN** the feature extractor SHALL compute the microstructure feature vector strictly from data at `<= t`
- **AND** SHALL validate that no `NaN` or infinite feature values are passed to the classifier.

### Requirement: Calibrated Conditional Win-Rate Prediction
The ML Win-Rate Engine SHALL use a trained and probability-calibrated XGBoost classifier to output conditional win probability `W in [0, 1]` within `5 ms` per evaluation.

#### Scenario: High-probability zone retest passes initial filter
- **WHEN** XGBoost predicts calibrated `W >= W_min` (baseline threshold `0.55`)
- **THEN** the engine SHALL forward `W` and `feature_snapshot` to the Planner & Reasoning Agent
- **AND** SHALL record the model version and calibration metadata.

#### Scenario: Low-probability setup short-circuits before LLM
- **WHEN** XGBoost predicts calibrated `W < W_min`
- **THEN** the pipeline SHALL immediately terminate in `WAIT/NO_TRADE` with `reason="Win Rate below threshold"`
- **AND** SHALL NOT invoke the LLM reasoning layer.
