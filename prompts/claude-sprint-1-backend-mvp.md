# Claude Prompt — Sprint 1 Backend MVP

## Role

You are the principal backend engineer for Capital Cipher AI.

Your task is to implement Sprint 1 only.

## Mandatory reading before coding

Read these files first:

```text
README.md
.ai/project-context.md
.ai/architecture-memory.md
.ai/engineering-principles.md
.ai/implementation-rules.md
.ai/coding-rules.md
docs/01-visao-do-projeto.md
docs/02-arquitetura-geral.md
docs/04-orchestrator.md
docs/09-coding-standards.md
docs/13-api-specification.md
docs/15-development-workflow.md
docs/16-security-rules.md
docs/30-system-state-machine.md
contracts/*.schema.json
adr/*.md
```

## Sprint 1 scope

Create the initial backend foundation only.

Do not implement real trading.
Do not implement live exchange execution.
Do not add private API key support.

## Required deliverables

```text
backend/
  app/
    main.py
    core/
      config.py
      logging.py
      errors.py
    schemas/
      health.py
      errors.py
    api/
      routes/
        health.py
    tests/
      test_health.py
  pyproject.toml
  README.md
  .env.example
```

## Backend requirements

- Python 3.13+
- FastAPI
- Pydantic
- Pytest
- structured logging
- `/health` endpoint
- `/api/v1/status` endpoint
- system mode defaults to PAPER
- no real execution capability

## Expected endpoints

### GET `/health`

Returns basic service health.

### GET `/api/v1/status`

Returns system mode and component status.

## Security rules

- Do not create LIVE mode activation.
- Do not create exchange execution adapters.
- Do not store secrets.
- Do not include API keys.

## Testing requirements

Add tests for:

- health endpoint returns 200;
- status endpoint returns PAPER mode;
- response format is stable.

## Definition of Done

Sprint 1 is complete only when:

- app starts;
- tests pass;
- no real trading code exists;
- documentation assumptions are followed;
- code structure matches the architecture.

## Output expected from Claude

1. Summary of documents read.
2. Implementation plan.
3. Files created/changed.
4. How to run locally.
5. How to run tests.
6. Known limitations.
