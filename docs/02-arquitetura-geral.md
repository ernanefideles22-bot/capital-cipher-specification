# 02 — Arquitetura Geral

## Visão macro

O Capital Cipher AI será dividido em cinco áreas principais:

1. Coleta de dados.
2. Inteligência multiagente.
3. Gerenciamento de risco.
4. Execução.
5. Auditoria.

## Fluxo principal

```text
Mercado
  ↓
Market Data Agent
  ↓
Orchestrator
  ↓
Agentes especializados
  ↓
Decision Engine
  ↓
Risk Management
  ↓
Paper Trading / Executor
  ↓
Audit Log
  ↓
Database
```

## Regra de isolamento

Nenhum agente deve acessar diretamente outro agente.

Toda comunicação deve passar pelo **Orchestrator**.

Isso evita acoplamento, reduz comportamento imprevisível e permite que novos agentes sejam adicionados sem quebrar o sistema.

## Componentes principais

### Market Data Layer

Responsável por coletar dados de mercado em tempo real.

Fontes iniciais:

- Binance WebSocket público.
- Bybit WebSocket público.

Dados esperados:

- candles OHLCV;
- trades recentes;
- volume;
- spread;
- order book;
- timestamp;
- latência.

### Agent Layer

Camada responsável por executar análises especializadas.

Cada agente deve receber uma entrada padronizada e retornar uma saída padronizada.

### Orchestration Layer

Responsável por:

- receber eventos de mercado;
- chamar agentes;
- consolidar respostas;
- gerar uma decisão candidata;
- enviar a decisão para o Risk Management.

### Risk Layer

Responsável por aprovar, reduzir ou vetar uma operação.

O risco tem poder superior ao orquestrador e aos agentes analíticos.

### Execution Layer

Na primeira fase, a execução será apenas simulada.

Nenhuma ordem real deve ser enviada.

### Audit Layer

Responsável por registrar:

- entrada recebida;
- decisão de cada agente;
- decisão consolidada;
- validação de risco;
- latência;
- erros;
- resultado da operação simulada.

## Tecnologias recomendadas

### Backend

- Python 3.13+
- FastAPI
- AsyncIO
- Pydantic
- SQLAlchemy
- PostgreSQL ou Supabase
- Redis em fase posterior

### Frontend

- React
- Vite
- TypeScript
- Tailwind CSS
- Lightweight Charts

### Infraestrutura

- Docker
- GitHub
- Vercel para frontend
- VPS para backend contínuo

## Decisão arquitetural crítica

O frontend pode estar na Vercel, mas o backend de trading 24/7 não deve depender exclusivamente de ambiente serverless.

Processos contínuos, WebSockets persistentes e agentes rodando em tempo real devem ficar em VPS, container ou infraestrutura dedicada.
