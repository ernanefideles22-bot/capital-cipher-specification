# ADR-005 — Multi-Agent Orchestrator

## Status

Accepted

## Context

Capital Cipher AI uses multiple specialized agents. If agents communicate freely or execute actions independently, the system becomes hard to reason about, test and audit.

A central coordination layer is required.

## Decision

Use an Orchestrator as the central coordination component.

Agents do not call each other directly. The Orchestrator receives events, invokes agents, collects outputs and creates candidate decisions for Risk Management.

## Consequences

### Positive

- Clear control flow.
- Better auditability.
- Easier agent replacement.
- Prevents uncontrolled agent interactions.
- Better testability.

### Negative

- Orchestrator becomes a critical component.
- Requires careful timeout and failure handling.
- Adds latency.

## Enforcement

- Agents must implement contracts.
- Agents must not execute orders.
- Agents must not call other agents directly.
- Orchestrator must preserve correlation_id.
- Risk validation happens after candidate decision creation.

## Related documents

- `docs/04-orchestrator.md`
- `docs/25-decision-engine.md`
- `docs/28-agent-lifecycle.md`
- `contracts/orchestrator-request.schema.json`
- `contracts/orchestrator-response.schema.json`
