# Implementation Rules

## Antes de codar

Sempre:

1. Ler documentos relevantes em `/docs`.
2. Verificar contratos em `/contracts`.
3. Identificar fase do projeto.
4. Confirmar que a tarefa não viola segurança.
5. Planejar mudança pequena.

## Regra de escopo

Implementar em pequenos incrementos.

Não criar várias camadas grandes sem testes.

## Backend

Usar:

- FastAPI;
- Pydantic;
- SQLAlchemy;
- AsyncIO;
- Pytest.

## Frontend

Usar:

- React;
- TypeScript;
- Vite;
- Tailwind;
- componentes pequenos.

## Schemas

Criar modelos Pydantic a partir dos JSON Schemas.

Evitar estruturas paralelas inconsistentes.

## Logs

Todo serviço crítico deve registrar logs estruturados.

Campos mínimos:

- timestamp;
- level;
- service;
- event_type;
- correlation_id;
- message.

## Erros

Erros críticos devem bloquear operação.

Não usar tratamento silencioso.

## Testes

Para cada módulo crítico, criar testes.

Prioridade:

1. Risk Management
2. Contracts
3. Orchestrator
4. Decision Engine
5. Paper Trading
6. Market Data

## Commits

Usar Conventional Commits:

```text
feat:
fix:
docs:
test:
refactor:
chore:
```

## Proibido

- Implementar live trading.
- Colocar secrets no código.
- Criar agente sem contrato.
- Criar decisão sem audit trail.
- Criar paper order sem risk_check.
- Fazer endpoint sensível sem autenticação futura prevista.
- Misturar dados da exchange diretamente no domínio.
