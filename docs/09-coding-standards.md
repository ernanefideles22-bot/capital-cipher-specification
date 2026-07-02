# 09 — Coding Standards

## Objetivo

Definir padrões obrigatórios de implementação para evitar código inconsistente, acoplado ou difícil de testar.

Este documento deve ser lido antes de qualquer implementação.

## Linguagens principais

### Backend

- Python 3.13+
- Tipagem obrigatória
- Pydantic para schemas
- FastAPI para API
- SQLAlchemy para persistência
- Pytest para testes

### Frontend

- TypeScript
- React
- Vite
- Tailwind CSS
- Componentes pequenos e reutilizáveis

## Princípios obrigatórios

### Responsabilidade única

Cada módulo, classe, função e agente deve ter uma responsabilidade clara.

### Baixo acoplamento

Componentes não devem depender diretamente de implementações concretas quando uma interface ou contrato for suficiente.

### Alta coesão

Arquivos devem agrupar comportamentos relacionados.

### Testabilidade

Toda regra crítica deve poder ser testada sem depender de API externa real.

## Padrões de backend

### Estrutura sugerida

```text
backend/
  app/
    main.py
    core/
      config.py
      logging.py
      errors.py
    agents/
      base.py
      market_data.py
      quant.py
      trend.py
      risk.py
      paper_trading.py
      audit.py
    orchestrator/
      service.py
      decision_engine.py
    schemas/
      events.py
      agents.py
      decisions.py
      risk.py
    database/
      models.py
      session.py
      repositories/
    services/
    tests/
```

## Nomeação

### Python

- arquivos: `snake_case.py`
- funções: `snake_case`
- variáveis: `snake_case`
- classes: `PascalCase`
- constantes: `UPPER_SNAKE_CASE`

### TypeScript

- componentes: `PascalCase.tsx`
- hooks: `useSomething.ts`
- serviços: `camelCase.ts`
- tipos: `PascalCase`

## Logs

Logs devem ser estruturados.

Campos mínimos:

- timestamp;
- level;
- service;
- event_type;
- correlation_id;
- message;
- metadata.

## Tratamento de erro

Erros não devem ser escondidos.

Proibido:

```python
try:
    do_something()
except Exception:
    pass
```

Obrigatório:

- registrar erro;
- preservar contexto;
- retornar estado seguro;
- bloquear execução se o erro afetar risco ou dados.

## Testes

Devem existir testes para:

- contratos dos agentes;
- cálculo de indicadores;
- regras de risco;
- orquestração;
- paper trading;
- persistência crítica.

## Commits

Usar padrão Conventional Commits:

```text
feat: add quant agent
fix: correct risk validation
refactor: isolate market data adapter
docs: update agent contracts
test: add risk manager tests
```

## Proibições

- Não colocar segredo no código.
- Não criar execução real na Fase 1.
- Não fazer agente acessar outro agente diretamente.
- Não misturar regra de negócio com frontend.
- Não ignorar falha de auditoria.
- Não usar valores mágicos espalhados pelo código.

## Definição mínima de pronto

Uma tarefa só está pronta quando:

- compila;
- tem teste básico;
- segue contratos;
- registra logs necessários;
- não quebra documentação;
- não viola regras de risco.
