# ADR-007 — Domain Vocabulary

## Status

Accepted

## Context

Capital Cipher AI contains many domain objects: decisions, agent outputs, risk checks, simulated orders, strategies, market events, system events and audit logs.

Without a shared vocabulary, implementation will drift and components may use inconsistent names for the same concept.

## Decision

Use a consistent domain vocabulary across documentation, contracts, backend, frontend and AI prompts.

Core terms include:

- Decision
- AgentOutput
- RiskCheck
- PaperOrder
- Strategy
- MarketEvent
- SystemEvent
- AuditLog
- CorrelationId
- SystemMode

## Consequences

### Positive

- Shared language across docs, code and AI tools.
- Easier onboarding.
- Better schema consistency.
- Lower ambiguity.

### Negative

- Requires naming discipline.
- Some shortcut implementations should be rejected.

## Enforcement

- Use domain names consistently.
- Avoid synonyms for core objects.
- Keep schemas aligned with the domain model.
- Update docs when the domain changes.

## Related documents

- `docs/29-domain-model.md`
- `docs/12-database-specification.md`
- `contracts/`
