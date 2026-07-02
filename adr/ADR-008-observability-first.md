# ADR-008 — Observability First

## Status

Accepted

## Context

Capital Cipher AI must be able to explain system behavior, agent behavior, simulated orders, risk checks and failures.

A system that cannot explain what happened cannot be trusted or improved.

## Decision

Observability is a first-class architectural requirement.

Logs, metrics, audit records and correlation IDs must be included from the beginning, not added later.

## Consequences

### Positive

- Easier debugging.
- Better auditability.
- Better operational control.
- Easier performance analysis.
- Better incident investigation.

### Negative

- More implementation work.
- More storage usage.
- Requires log hygiene.

## Enforcement

- Structured logs required.
- correlation_id required for critical flows.
- Agent latency must be tracked.
- Risk and audit failures must be visible.
- Dashboard must expose operational status.

## Related documents

- `docs/20-observability.md`
- `docs/14-dashboard-specification.md`
- `contracts/audit-log.schema.json`
- `contracts/error.schema.json`
