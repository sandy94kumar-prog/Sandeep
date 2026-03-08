# Data Contracts (v1)

## 1) Normalized market tick

```json
{
  "symbol": "XAUUSD",
  "ts": "2026-01-15T12:30:00Z",
  "bid": 2331.15,
  "ask": 2331.35,
  "last": 2331.22,
  "volume": 1289,
  "source": "polygon"
}
```

## 2) News intelligence output

```json
{
  "headline_id": "news_93813",
  "ts": "2026-01-15T12:31:02Z",
  "classification": ["inflation", "policy"],
  "sentiment": {
    "score": 0.64,
    "label": "bullish_metals"
  },
  "impact": {
    "XAUUSD": 0.72,
    "XAGUSD": 0.66,
    "SPY": -0.43
  },
  "confidence": 0.81
}
```

## 3) Macro regime output

```json
{
  "ts": "2026-01-15T12:35:00Z",
  "regime_probs": {
    "risk_on": 0.08,
    "risk_off": 0.31,
    "inflation_shock": 0.44,
    "liquidity_expansion": 0.07,
    "monetary_tightening": 0.10
  },
  "dominant_regime": "inflation_shock"
}
```

## 4) Engine feature vector for fusion

```json
{
  "asset": "XAUUSD",
  "ts": "2026-01-15T12:36:00Z",
  "features": {
    "news_impact": 0.72,
    "macro_bias": 0.68,
    "liquidity_score": 0.55,
    "flow_score": 0.61,
    "technical_score": 0.58,
    "stress_score": 0.49
  }
}
```

## 5) Decision engine output

```json
{
  "asset": "XAUUSD",
  "ts": "2026-01-15T12:36:05Z",
  "probability_up": {
    "24h": 0.61,
    "7d": 0.68
  },
  "setup": {
    "entry": 2330,
    "stop": 2295,
    "target": 2400
  },
  "confidence": 0.79,
  "risk_gate": {
    "status": "pass",
    "notes": []
  }
}
```

## 6) Alert payload

```json
{
  "alert_id": "al_10283",
  "type": "institutional_flow_detection",
  "asset": "XAUUSD",
  "severity": "high",
  "message": "Large gold accumulation detected",
  "confidence_delta": 0.11,
  "expires_at": "2026-01-15T15:00:00Z"
}
```
