# 03 — Organograma Multiagente

## Conceito

O Capital Cipher AI deve funcionar como uma organização digital, não como um único robô.

Cada agente representa uma função especializada dentro de uma mesa institucional de trading.

## Organograma conceitual

```text
Capital Cipher AI

CEO / Master AI
│
├── COO / Orchestrator
│
├── Trading Director
│   ├── Market Data Agent
│   ├── Quant Agent
│   ├── Trend Agent
│   ├── Volume Agent
│   ├── Liquidity Agent
│   └── Visual Chart Agent
│
├── Risk Director
│   ├── Position Risk Agent
│   ├── Drawdown Agent
│   ├── Exposure Agent
│   └── Kill Switch Agent
│
├── Execution Director
│   ├── Paper Trading Agent
│   ├── Binance Executor
│   └── Bybit Executor
│
└── Audit Director
    ├── Logger Agent
    ├── Replay Agent
    └── Performance Agent
```

## Hierarquia de autoridade

A autoridade do sistema deve seguir esta ordem:

1. Risk Director.
2. Orchestrator.
3. Execution Director.
4. Agentes analíticos.
5. Dashboard.

## Regras de autoridade

### Agentes analíticos

Podem sugerir:

- BUY;
- SELL;
- HOLD;
- WAIT;
- BLOCK.

Não podem executar ordens.

### Orchestrator

Pode consolidar análises e gerar uma decisão candidata.

Não pode ignorar o risco.

### Risk Director

Pode:

- aprovar;
- reduzir;
- vetar;
- acionar kill switch.

### Execution Director

Pode executar apenas decisões previamente aprovadas pelo risco.

### Audit Director

Registra tudo. Não altera decisões.

## Regra central

Mesmo que todos os agentes analíticos aprovem uma operação, se o Risk Director vetar, a operação não acontece.

## Antipadrão proibido

Não usar votação simples entre agentes.

Exemplo proibido:

```text
5 agentes compram, 2 vendem, então compra.
```

Esse modelo é frágil e ignora hierarquia de risco.

## Modelo correto

```text
Agentes analisam → Orchestrator consolida → Risk valida → Executor simula/executa → Auditor registra
```
