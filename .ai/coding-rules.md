# Coding Rules

## Python

- Usar Python 3.13+.
- Usar type hints obrigatórios.
- Usar Pydantic para validação.
- Usar funções pequenas.
- Separar domínio, serviços, adapters e infraestrutura.

## Nomeação Python

```text
files: snake_case.py
functions: snake_case
variables: snake_case
classes: PascalCase
constants: UPPER_SNAKE_CASE
```

## TypeScript

- Usar strict mode.
- Evitar `any`.
- Criar tipos a partir dos contratos.
- Componentes pequenos e explícitos.

## Nomeação TypeScript

```text
components: PascalCase.tsx
hooks: useSomething.ts
types: PascalCase
variables: camelCase
```

## Estrutura backend sugerida

```text
backend/app
  core
  schemas
  agents
  orchestrator
  risk
  market_data
  paper_trading
  audit
  database
  api
  tests
```

## Regras de função

Uma função deve:

- fazer uma coisa;
- ter nome claro;
- receber dependências explicitamente;
- ser testável.

## Regras de classe

Uma classe deve:

- ter responsabilidade única;
- não conhecer detalhes desnecessários;
- depender de interfaces quando possível.

## Proibições de código

- Não usar variável global para estado crítico.
- Não usar sleep arbitrário para sincronização.
- Não fazer retry infinito.
- Não capturar exceção sem log.
- Não misturar adapter com regra de domínio.
- Não usar payload bruto da exchange diretamente nos agentes.

## Testes

Nomear testes de forma descritiva:

```text
test_risk_blocks_when_drawdown_exceeded
test_orchestrator_preserves_correlation_id
test_paper_order_requires_risk_check
```

## Configuração

Usar `.env.example` para documentar variáveis.

Nunca versionar `.env` real.
