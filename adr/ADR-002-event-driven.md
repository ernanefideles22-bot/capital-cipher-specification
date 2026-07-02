# ADR-002 — Event-Driven Architecture

## Status

Accepted

## Context

Capital Cipher AI depends on many independent components: market data, agents, orchestrator, risk, paper trading, audit and dashboard streams.

Direct coupling between these components would make the system fragile, difficult to test and hard to scale.

## Decision

Use an event-driven architecture with explicit events, correlation IDs and topic-based message flow.

Events must carry enough metadata to reconstruct the full decision chain.

## Consequences

### Positive

- Better decoupling.
- Easier audit and replay.
- Easier future scaling.
- Agents can evolve independently.
- Observability becomes stronger.

### Negative

- More complexity.
- Requires event contracts.
- Requires idempotency rules.
- Debugging requires correlation discipline.

## Enforcement

- Every critical event must include `event_id`.
- Every decision chain must include `correlation_id`.
- Event types must be documented.
- Critical events must be persisted or auditable.

## Related documents

- `docs/10-system-events.md`
- `docs/23-message-bus.md`
- `contracts/system-event.schema.json`
- `contracts/market-event.schema.json`
