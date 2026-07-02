# ADR-003 — Contract-First Development

## Status

Accepted

## Context

The system will be implemented across backend, frontend, agents, events and future workers. If every component invents its own data format, integration errors will multiply quickly.

## Decision

Use contract-first development.

JSON Schemas in `/contracts` are the official source for shared message structures. Backend Pydantic models and frontend TypeScript types should be derived from or kept aligned with these contracts.

## Consequences

### Positive

- Less ambiguity.
- Easier validation.
- Better agent interoperability.
- Better testing.
- Easier use by AI coding tools.

### Negative

- More upfront work.
- Contract changes require discipline.
- Versioning must be maintained.

## Enforcement

- Shared payloads must have contracts.
- Contract changes require documentation updates.
- Tests should validate contract compliance.
- Agents must return structured outputs.

## Related documents

- `docs/11-agent-contracts.md`
- `contracts/`
- `.ai/architecture-memory.md`
