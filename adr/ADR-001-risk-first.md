# ADR-001 — Risk-First Architecture

## Status

Accepted

## Context

Capital Cipher AI is a trading system that may eventually interact with financial markets. Any architecture that allows analysis components to directly execute trades creates unacceptable operational and financial risk.

The system must preserve capital and operational safety before optimizing for profit, speed or autonomy.

## Decision

Risk Management has absolute veto authority over all candidate decisions.

No agent, orchestrator, dashboard endpoint or executor may bypass the Risk Management layer.

All candidate decisions must pass through a RiskCheck before any paper order or future real order can be created.

## Consequences

### Positive

- Safer architecture.
- Clear authority hierarchy.
- Easier auditing.
- Lower chance of uncontrolled execution.
- Better preparation for future live trading.

### Negative

- More implementation complexity.
- More latency.
- Some valid opportunities may be blocked.

## Enforcement

Implementation must enforce:

- no order without risk_check_id;
- no paper order when risk_status is BLOCKED;
- no live order without explicit future approval;
- kill switch overrides all components.

## Related documents

- `docs/06-risk-management.md`
- `docs/16-security-rules.md`
- `contracts/risk-check.schema.json`
