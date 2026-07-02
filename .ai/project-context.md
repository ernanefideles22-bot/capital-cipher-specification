# Project Context — Capital Cipher AI

## Projeto

Capital Cipher AI é uma plataforma institucional de trading algorítmico baseada em arquitetura multiagente.

O objetivo não é criar um bot simples. O objetivo é construir uma organização digital de análise, risco, simulação, auditoria e execução futura controlada.

## Estado atual

A documentação oficial está em:

```text
/docs
/contracts
.ai
```

A implementação ainda não deve começar antes de a IA ler a documentação relevante.

## Princípios centrais

- Risk Management tem autoridade máxima.
- Nenhum agente executa ordem.
- Toda decisão precisa de correlation_id.
- Toda decisão precisa ser auditável.
- Fase 1 opera apenas em PAPER.
- Execução real é proibida na Fase 1.
- Contratos JSON devem orientar schemas Pydantic e TypeScript.

## Arquitetura conceitual

```text
Market Data
  ↓
Message Bus
  ↓
Orchestrator
  ↓
Agents
  ↓
Decision Engine
  ↓
Risk Management
  ↓
Paper Trading
  ↓
Audit
  ↓
Dashboard
```

## Agentes iniciais

- MarketDataAgent
- QuantAgent
- TrendAgent
- RiskAgent
- PaperTradingAgent
- AuditAgent

## Ativos iniciais

- BTCUSDT
- ETHUSDT
- SOLUSDT

## Timeframes iniciais

- 15m para scalp
- 1h para day trade
- 4h para swing

## Stack sugerida

Backend:

- Python 3.13+
- FastAPI
- Pydantic
- SQLAlchemy
- PostgreSQL / Supabase
- AsyncIO

Frontend:

- React
- Vite
- TypeScript
- Tailwind CSS
- Lightweight Charts

Infra:

- GitHub
- Docker
- Vercel para frontend
- VPS para backend 24/7

## Regra para IAs

Antes de escrever código, leia:

1. `README.md`
2. `docs/01-visao-do-projeto.md`
3. `docs/02-arquitetura-geral.md`
4. `docs/09-coding-standards.md`
5. `docs/11-agent-contracts.md`
6. `docs/13-api-specification.md`
7. `docs/16-security-rules.md`
8. `contracts/*.schema.json`

## Objetivo da Fase 1

Criar um MVP técnico com:

- market data público;
- agentes básicos;
- orquestrador;
- decisão candidata;
- risk check;
- paper trading;
- auditoria;
- dashboard;
- zero execução real.
