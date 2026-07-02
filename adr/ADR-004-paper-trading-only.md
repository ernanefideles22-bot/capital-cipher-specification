# ADR-004 — Paper Trading Only in Phase 1

## Status

Accepted

## Context

The project may eventually support live trading, but early implementation will be unstable by nature. Allowing real execution before architecture, risk, audit, tests and observability are validated would create unacceptable risk.

## Decision

Phase 1 is restricted to PAPER mode only.

No real exchange order execution, private API keys, real leverage or real account balance access is allowed in Phase 1.

## Consequences

### Positive

- Lower financial risk.
- Safer development.
- Easier testing.
- Cleaner architecture.
- Better validation of risk and audit flows.

### Negative

- No real-money validation initially.
- Simulated fills may differ from real market execution.
- Requires careful slippage and fee modeling.

## Enforcement

- System mode must default to PAPER.
- LIVE and LIVE_LOCKED are future phases.
- Exchange execution adapters must not be implemented for real orders in Phase 1.
- PaperOrder requires Decision and RiskCheck.

## Related documents

- `docs/18-paper-trading.md`
- `docs/19-live-trading.md`
- `docs/16-security-rules.md`
- `contracts/paper-order.schema.json`
