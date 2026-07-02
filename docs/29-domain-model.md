# 29 — Domain Model

## Objetivo

Definir o modelo de domínio do Capital Cipher AI.

Este documento descreve as entidades centrais, agregados, relações e invariantes que devem orientar a implementação.

## Princípio central

O sistema deve ser modelado em torno de decisões auditáveis, não apenas em torno de ordens.

Uma ordem simulada ou real é consequência de uma cadeia de decisão validada.

## Entidades principais

```text
MarketEvent
Candle
Agent
AgentOutput
Decision
RiskCheck
Strategy
PaperOrder
AuditLog
SystemEvent
SystemState
```

## MarketEvent

Representa um evento recebido do mercado.

Exemplos:

- candle fechado;
- trade recebido;
- orderbook atualizado;
- conexão estabelecida;
- conexão perdida.

Invariantes:

- deve possuir event_id;
- deve possuir timestamp;
- deve possuir source;
- deve possuir payload validado.

## Candle

Representa um candle OHLCV.

Campos:

```text
exchange
symbol
timeframe
open
high
low
close
volume
closed_at
```

Invariantes:

- high >= open, close, low;
- low <= open, close, high;
- volume >= 0;
- closed_at obrigatório;
- symbol obrigatório.

## Agent

Representa um agente registrado no sistema.

Atributos:

```text
name
version
status
critical
timeout_ms
enabled
```

Invariantes:

- nome único por versão;
- contrato obrigatório;
- status conhecido;
- timeout definido.

## AgentOutput

Representa a resposta de um agente.

Invariantes:

- deve possuir correlation_id;
- deve respeitar contrato;
- confidence entre 0 e 100;
- status permitido;
- signal permitido.

## Decision

Representa uma decisão candidata criada pelo Orchestrator ou Decision Engine.

Invariantes:

- não pode existir sem correlation_id;
- não pode existir sem agent_summary;
- não pode ser executada sem RiskCheck;
- deve possuir action candidata;
- deve ser auditável.

## RiskCheck

Representa validação de risco sobre uma decisão.

Invariantes:

- deve estar associado a uma Decision;
- deve retornar APPROVED, REDUCED, BLOCKED ou KILL_SWITCH;
- deve registrar motivo;
- operação não pode seguir se risk_status for BLOCKED ou KILL_SWITCH.

## Strategy

Representa um conjunto versionado de regras de decisão.

Invariantes:

- deve possuir strategy_id;
- deve possuir versão;
- deve definir ativos e timeframes permitidos;
- deve definir perfil de risco;
- deve registrar alterações por versão.

## PaperOrder

Representa uma ordem simulada.

Invariantes:

- não pode existir sem Decision;
- não pode existir sem RiskCheck aprovado ou reduzido;
- deve possuir correlation_id;
- deve registrar entrada, saída e resultado;
- deve descontar taxas e slippage estimados.

## AuditLog

Representa registro de auditoria.

Invariantes:

- deve possuir correlation_id;
- deve registrar payload;
- não deve armazenar secrets;
- eventos críticos não devem ser apagados.

## SystemState

Representa o estado operacional do sistema.

Estados possíveis:

```text
OFFLINE
PAPER
LIVE_LOCKED
LIVE
ERROR
MAINTENANCE
```

## Agregados principais

### Decision Aggregate

Inclui:

- Decision;
- AgentOutputs;
- RiskCheck;
- PaperOrder;
- AuditLogs.

Regra: toda cadeia de decisão deve ser reconstruível por correlation_id.

### Strategy Aggregate

Inclui:

- Strategy;
- parâmetros;
- versões;
- backtests;
- paper performance.

### Agent Aggregate

Inclui:

- Agent;
- AgentOutputs;
- métricas;
- status;
- ranking.

## Invariantes globais

- Nenhuma ordem sem RiskCheck.
- Nenhuma decisão sem auditabilidade.
- Nenhum agente sem contrato.
- Nenhum evento crítico sem correlation_id.
- Nenhum modo LIVE na Fase 1.
- Nenhum secret no frontend.

## Linguagem ubíqua

Usar os termos abaixo consistentemente:

```text
Decision
Candidate Action
Risk Check
Paper Order
Agent Output
Correlation ID
System Mode
Strategy Version
Market Regime
Kill Switch
```

## Critério de sucesso

O modelo de domínio estará correto quando desenvolvedores e IAs conseguirem implementar entidades, bancos, eventos e APIs usando os mesmos conceitos sem ambiguidade.
