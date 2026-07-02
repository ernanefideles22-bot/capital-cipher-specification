# 15 — Development Workflow

## Objetivo

Definir o processo oficial de desenvolvimento do Capital Cipher AI.

Este workflow deve ser seguido por humanos, Claude Coworker, GPT, Codex ou qualquer outra ferramenta de IA envolvida no projeto.

## Regra principal

Nenhuma implementação deve começar sem leitura prévia da documentação relevante.

## Fluxo oficial

```text
1. Ler documentação
2. Definir escopo pequeno
3. Criar plano técnico
4. Implementar
5. Testar
6. Atualizar documentação
7. Registrar decisões arquiteturais
8. Abrir PR ou commit controlado
```

## Branches

### `main`

Branch estável.

Deve conter apenas documentação ou código validado.

### `docs/*`

Branches para documentação.

### `feat/*`

Branches para novas funcionalidades.

### `fix/*`

Correções.

### `refactor/*`

Refatorações sem mudança funcional.

## Commits

Usar Conventional Commits:

```text
docs: add API specification
feat: add market data agent
fix: correct risk validation
refactor: isolate orchestrator service
test: add quant agent tests
chore: update dependencies
```

## Sprints iniciais

### Sprint 0 — Documentação

Objetivo: criar base arquitetural.

Status: em andamento.

### Sprint 1 — Estrutura do backend

Entregáveis:

- FastAPI;
- estrutura de pastas;
- configuração;
- logging;
- health check;
- schemas base;
- testes iniciais.

### Sprint 2 — Market Data

Entregáveis:

- conexão WebSocket pública;
- candles;
- status de conexão;
- armazenamento inicial.

### Sprint 3 — Agentes base

Entregáveis:

- BaseAgent;
- QuantAgent;
- TrendAgent;
- RiskAgent;
- AuditAgent.

### Sprint 4 — Orchestrator

Entregáveis:

- roteamento de eventos;
- execução dos agentes;
- consolidação de decisão;
- correlação de eventos.

### Sprint 5 — Paper Trading

Entregáveis:

- simulação de ordens;
- taxas estimadas;
- slippage estimado;
- PnL simulado.

### Sprint 6 — Dashboard

Entregáveis:

- status do sistema;
- gráfico;
- agentes;
- decisões;
- risco;
- paper trading;
- auditoria.

## Definition of Ready

Uma tarefa só pode começar se tiver:

- objetivo claro;
- documento relacionado;
- entrada e saída esperadas;
- critério de sucesso;
- riscos conhecidos.

## Definition of Done

Uma tarefa só está concluída quando:

- código compila;
- testes passam;
- logs existem;
- erros são tratados;
- documentação está atualizada;
- não viola arquitetura;
- não cria execução real indevida.

## Revisão de código

Toda revisão deve verificar:

- acoplamento;
- contratos;
- risco;
- logs;
- tratamento de erro;
- testes;
- segurança;
- aderência à documentação.

## Uso de IA no desenvolvimento

A IA deve:

- ler `/docs` antes de implementar;
- declarar plano antes de alterar muitos arquivos;
- preferir mudanças pequenas;
- não improvisar arquitetura;
- não criar execução real sem autorização documental;
- atualizar docs se mudar comportamento.

## Antipadrões proibidos

- Implementar tudo em um único arquivo.
- Misturar frontend com regra de trading.
- Usar API key real em desenvolvimento inicial.
- Ignorar falha de auditoria.
- Fazer agente chamar agente diretamente.
- Criar decisão sem correlation_id.
- Criar ordem simulada sem risk_check.

## Estratégia de releases

### v0.1

Documentação e arquitetura.

### v0.2

Backend básico.

### v0.3

Market data + agentes.

### v0.4

Orchestrator + risk.

### v0.5

Paper trading.

### v0.6

Dashboard operacional.

### v1.0

Paper trading robusto e auditável.
