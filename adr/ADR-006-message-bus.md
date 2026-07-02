# ADR-006 — Message Bus Prepared Architecture

## Status

Accepted

## Context

The first version can start with simple in-process orchestration, but the system is expected to evolve toward independent workers, event replay, agent scaling and real-time dashboard streams.

Designing without a message bus boundary would make future scaling harder.

## Decision

Design the architecture as message-bus-ready from the beginning.

Phase 1 may use in-memory dispatch or asyncio queues, but events, topics and payload contracts must be compatible with a future Redis Streams, RabbitMQ, NATS or Kafka implementation.

## Consequences

### Positive

- Easier future scaling.
- Better decoupling.
- Clear event boundaries.
- Supports replay and observability.

### Negative

- More initial design overhead.
- Requires event discipline.
- May seem over-engineered during the MVP.

## Enforcement

- Define event topics.
- Preserve message_id, event_id and correlation_id.
- Avoid direct cross-component calls where event flow is appropriate.
- Ensure idempotency for critical events.

## Related documents

- `docs/23-message-bus.md`
- `docs/10-system-events.md`
- `contracts/system-event.schema.json`
