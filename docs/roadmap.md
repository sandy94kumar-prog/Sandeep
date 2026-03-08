# Delivery Roadmap

## Phase 0 — Foundations (Week 1-2)
- Finalize canonical symbols and provider mapping.
- Set up PostgreSQL schema + Redis cache keys.
- Build market and news ingestion pipelines with replay support.

**Exit criteria**
- Data freshness SLO dashboards available.
- 7-day replay of historical ingestion is deterministic.

## Phase 1 — Engine MVPs (Week 3-6)
- Ship News Intelligence v1.
- Ship Macro Regime classifier v1.
- Ship Technical + Stress engines v1.

**Exit criteria**
- Every engine emits timestamped normalized scores.
- Backfill + live mode both operational.

## Phase 2 — Decision + Risk (Week 7-9)
- Implement weighted fusion for 24h/7d probabilities.
- Integrate risk gating (position size, correlation controls, DD limits).
- Add signal audit trail and explainability fields.

**Exit criteria**
- Signals reproducible from stored features.
- Risk-gate pass/fail reason codes complete.

## Phase 3 — Terminal + Alerts (Week 10-12)
- Build dashboard with live WebSocket updates.
- Add Global Macro, Asset Intelligence, Liquidity Map, Stress Radar, Setup panel.
- Enable multi-channel alerts (in-app/email/webhook).

**Exit criteria**
- End-to-end latency budget met for live updates.
- Alert dedupe and cooldown behavior validated.

## Phase 4 — Learning Loop (Week 13+)
- Outcome tracking and calibration monitoring.
- Weekly model weight re-estimation with guardrails.
- Portfolio health reporting and trade journal integration.

**Exit criteria**
- Rolling performance reports auto-generated.
- Safe auto-tuning with rollback on degradation.
