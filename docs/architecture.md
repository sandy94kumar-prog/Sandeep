# System Architecture

## 1) Layered design

The platform is organized into 4 primary layers:

1. **Data Layer**
2. **AI Analysis Layer**
3. **Decision + Risk Engine**
4. **Frontend Trading Terminal**

```
Market / News / Economic / Positioning / Flow Data
                    ↓
            AI Analysis Engines
                    ↓
      Decision Engine + Risk Engine
                    ↓
       Trading Terminal + Alerting
```

## 2) Data layer

The data layer ingests and normalizes five streams.

### A. Market data
- Real-time and historical prices: `XAUUSD`, `XAGUSD`, `SPY`
- Context series: `DXY`, US yields (2Y/10Y)
- Candidate providers: Polygon.io, Alpha Vantage

### B. Economic data
- CPI, NFP, unemployment, ISM, GDP surprises
- US 10Y yield curve dynamics

### C. News data
- Near real-time headlines from institutional feeds
- Metadata: source, timestamp, entities, confidence

### D. Positioning data
- CFTC Commitments of Traders (COT)
- Net long/short and changes by participant type

### E. ETF flow data
- GLD, SLV, and key macro ETF daily/near-real-time flows
- Rolling flow z-scores and divergence signals

## 3) AI analysis layer

Six engines run independently and publish scored outputs.

### 1. News intelligence engine
- Headline classification (`inflation`, `policy`, `geopolitics`, `growth`)
- Sentiment polarity and intensity
- Asset impact scoring for gold/silver/equities

### 2. Macro regime engine
- Regime classes: `risk_on`, `risk_off`, `inflation_shock`, `liquidity_expansion`, `monetary_tightening`
- Outputs regime probability vector (not single hard label)

### 3. Liquidity engine
- M2 trend acceleration
- Central-bank balance sheet delta
- Funding stress / repo dislocation proxy

### 4. Market flow engine
- ETF flows + futures positioning + options pressure
- Detects accumulation/distribution footprints

### 5. Technical engine
- EMA structure, RSI, support/resistance, volume imbalance
- False-breakout and liquidity-trap detection

### 6. Market stress radar
- VIX and cross-asset stress composite
- Produces normalized stress score (`0-10`)

## 4) Decision engine

The decision engine fuses all engine outputs into asset-level directional probabilities.

### Fusion approach (v1)
- Weighted ensemble over normalized engine features.
- Time horizons:
  - `24h_probability_up`
  - `7d_probability_up`
- Setup generator proposes entry/stop/target using technical structure + volatility bands.

### Sample output
- Asset: Gold
- 24h Up Probability: 61%
- 7d Up Probability: 68%
- Setup: Entry 2330, Stop 2295, Target 2400
- Confidence: 79%

## 5) Risk engine

Mandatory constraints before publishing any trade setup.

- Max risk per trade
- Volatility-adjusted position sizing
- Correlation-aware exposure controls (gold/silver linkage)
- Portfolio drawdown circuit breakers

Any breach downgrades setup confidence or blocks publication.

## 6) Databases and state

### PostgreSQL (system of record)
- OHLCV bars and derived indicators
- News and macro event history
- Signal snapshots and realized outcomes

### Redis (low-latency layer)
- Real-time feature cache
- Live dashboard state
- Alert debounce and rate-limit primitives

## 7) Backend services

- `market-data-service` (Node.js/Python adapters)
- `news-processing-service` (NLP preprocessing + tagging)
- `ai-analysis-service` (FastAPI orchestrating engines)
- `signal-generator-service` (fusion + risk gating)
- `alert-engine-service` (rules + delivery)

Inter-service communication:
- Event stream (pub/sub)
- Request/response APIs for snapshots
- WebSocket fan-out for terminal updates

## 8) Frontend trading terminal

Main panels:
- **Global Macro Panel** (regime + liquidity + central bank state)
- **Asset Intelligence Panel** (price, sentiment, probabilities)
- **Liquidity Map** (SR zones, imbalances, pools)
- **Stress Radar** (cross-market risk heat)
- **Trade Setup Panel** (entry/stop/target/confidence)

## 9) Alert engine

Alert triggers:
- Sentiment regime shift
- Liquidity spike/drop
- Volatility shock
- Institutional flow detection

Alert payload includes reason, impacted assets, confidence delta, and expiry time.

## 10) Continuous learning loop

Track for every published setup:
- Hit rate / calibration
- Profit factor
- Drawdown profile
- Regime-conditional performance

Model weights are adjusted periodically based on out-of-sample performance windows.
