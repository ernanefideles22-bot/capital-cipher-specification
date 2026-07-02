# ADR-010 — Deployment Architecture

## Status

Accepted

## Context

Capital Cipher AI has different runtime needs. The frontend dashboard can run on a serverless platform, but market data streams, workers, agents and orchestration require persistent runtime.

## Decision

Use separated deployment responsibilities.

Frontend may be deployed on Vercel. Backend, workers, WebSockets and long-running services should run on VPS, containers or dedicated infrastructure.

## Consequences

### Positive

- Better runtime stability.
- Persistent WebSocket support.
- Cleaner separation of concerns.
- Easier future scaling.

### Negative

- Requires VPS/container management.
- More DevOps work.
- More monitoring requirements.

## Enforcement

- Do not rely on Vercel for 24/7 backend workers.
- Backend must expose health endpoints.
- Use environment variables for configuration.
- Keep dev, staging and production separated.
- Production requires monitoring, backups and rollback.

## Related documents

- `docs/21-deployment.md`
- `docs/20-observability.md`
- `docs/16-security-rules.md`
