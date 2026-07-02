# ADR-009 — Security Model

## Status

Accepted

## Context

Capital Cipher AI may eventually connect to external services and handle sensitive configuration. Poor security design at the MVP stage creates expensive rework and unsafe defaults later.

## Decision

Security rules must be designed into the architecture from the beginning.

Phase 1 must not store private exchange credentials, must not execute real orders, and must not expose critical operations without planned authorization boundaries.

## Consequences

### Positive

- Safer development path.
- Lower chance of accidental real execution.
- Cleaner separation of environments.
- Better future readiness.

### Negative

- More initial setup.
- Some features are delayed until security requirements are met.

## Enforcement

- No secrets in repository.
- No private API keys in Phase 1.
- No real execution in Phase 1.
- Sensitive endpoints must be protected before production.
- Kill switch must be available.

## Related documents

- `docs/16-security-rules.md`
- `.ai/engineering-principles.md`
- `.ai/implementation-rules.md`
