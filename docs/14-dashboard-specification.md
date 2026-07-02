# 14 — Dashboard Specification

## Objetivo

Definir o dashboard operacional do Capital Cipher AI.

O dashboard deve permitir monitorar o sistema, visualizar decisões, acompanhar agentes, revisar risco e observar paper trading em tempo real.

## Princípios de interface

- Interface objetiva.
- Informação operacional acima de estética.
- Métricas críticas sempre visíveis.
- Nenhum botão de execução real na Fase 1.
- Logs e decisões precisam ser rastreáveis.

## Layout principal

```text
Header
  ├── Logo / Nome do sistema
  ├── Modo: OFFLINE / PAPER / LIVE_LOCKED / LIVE
  ├── Status geral
  └── Kill Switch

Main Dashboard
  ├── Market Overview
  ├── Trading Chart
  ├── Agent Status
  ├── Decision Feed
  ├── Risk Panel
  ├── Paper Trading Panel
  └── Audit Logs
```

## Tela 1 — Overview

Objetivo: visão executiva do sistema.

Cards obrigatórios:

- Status do sistema;
- Modo atual;
- Exchange conectada;
- Ativo principal;
- Timeframe;
- Último preço;
- Latência;
- Agentes ativos;
- Decisões hoje;
- Paper PnL;
- Drawdown simulado;
- Kill Switch status.

## Tela 2 — Market

Objetivo: visualizar mercado em tempo real.

Componentes:

- gráfico candlestick;
- seletor de exchange;
- seletor de symbol;
- seletor de timeframe;
- volume;
- VWAP;
- EMAs;
- RSI;
- ATR;
- status da conexão.

Biblioteca recomendada:

```text
TradingView Lightweight Charts
```

## Tela 3 — Agents

Objetivo: monitorar agentes.

Tabela de agentes:

```text
Name | Status | Last Run | Signal | Confidence | Latency | Errors
```

Estados permitidos:

```text
READY
RUNNING
FAILED
TIMEOUT
DISABLED
```

Cada agente deve ter tela de detalhe com:

- último input;
- último output;
- confidence;
- reason;
- evidence;
- warnings;
- histórico recente.

## Tela 4 — Decisions

Objetivo: acompanhar decisões do Orchestrator.

Campos:

- decision_id;
- correlation_id;
- timestamp;
- symbol;
- timeframe;
- candidate_action;
- confidence;
- risk_status;
- final_status.

Cada decisão deve permitir abrir a cadeia completa:

```text
Candle → Agentes → Orchestrator → Risk → Paper Order → Audit
```

## Tela 5 — Risk

Objetivo: acompanhar e controlar risco.

Cards:

- risco por trade;
- drawdown diário;
- drawdown total;
- perdas consecutivas;
- posições simuladas abertas;
- exposição por ativo;
- estado do kill switch;
- operações bloqueadas.

Ações permitidas:

- acionar kill switch;
- alterar modo para OFFLINE ou PAPER;
- visualizar limites.

Ações proibidas na Fase 1:

- liberar LIVE;
- conectar API key real;
- executar ordem real.

## Tela 6 — Paper Trading

Objetivo: acompanhar simulações.

Métricas:

- total de trades;
- win rate;
- PnL simulado;
- PnL por ativo;
- drawdown;
- melhor trade;
- pior trade;
- taxa estimada;
- slippage estimado;
- curva de equity simulada.

Tabela de ordens:

```text
Order ID | Symbol | Side | Entry | Stop | Take | Status | PnL | Opened At | Closed At
```

## Tela 7 — Audit

Objetivo: permitir inspeção técnica.

Filtros:

- correlation_id;
- event_type;
- agent_name;
- symbol;
- date range;
- status.

Cada item deve exibir payload JSON formatado.

## Tela 8 — Settings

Configurações permitidas na Fase 1:

- exchange pública;
- símbolos permitidos;
- timeframes;
- limites de paper trading;
- agentes habilitados;
- tema visual;
- modo PAPER/OFFLINE.

Configurações proibidas na Fase 1:

- chaves privadas de corretora;
- execução real;
- alavancagem real;
- modo LIVE.

## UX de segurança

O botão Kill Switch deve estar sempre visível.

Quando o kill switch estiver ativo, a interface deve mostrar claramente:

```text
SYSTEM LOCKED — KILL SWITCH ACTIVE
```

## Atualização em tempo real

Usar WebSocket para:

- status do sistema;
- preços;
- logs;
- decisões;
- agentes;
- paper orders.

## Regras visuais

- Não esconder falhas críticas.
- Não mascarar risco com interface otimista.
- Não usar linguagem que sugira lucro garantido.
- Sempre diferenciar PAPER de LIVE visualmente.

## Critério de conclusão da V1

O dashboard da V1 estará pronto quando permitir:

- visualizar mercado;
- ver agentes;
- ver decisões;
- ver risco;
- ver ordens simuladas;
- investigar auditoria;
- acionar kill switch.
