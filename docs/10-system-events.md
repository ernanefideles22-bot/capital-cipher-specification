# 10 — System Events

## Objetivo

Definir os eventos padronizados usados pelo Capital Cipher AI.

Eventos são a base da comunicação entre coleta de dados, orquestrador, agentes, risco, execução simulada e auditoria.

## Princípios

- Todo evento deve ter `event_id`.
- Todo evento deve ter `correlation_id`.
- Todo evento deve ter `timestamp`.
- Todo evento deve ser auditável.
- Todo evento crítico deve ser persistido.

## Campos comuns

```json
{
  "event_id": "uuid",
  "correlation_id": "uuid",
  "event_type": "CANDLE_CLOSED",
  "source": "MarketDataAgent",
  "timestamp": "2026-07-01T12:00:00Z",
  "version": "1.0",
  "payload": {}
}
```

## Eventos de mercado

### MARKET_CONNECTED

Emitido quando uma conexão com exchange é estabelecida.

### MARKET_DISCONNECTED

Emitido quando uma conexão é perdida.

### CANDLE_CLOSED

Emitido quando um candle fecha.

Payload mínimo:

```json
{
  "exchange": "BINANCE",
  "symbol": "BTCUSDT",
  "timeframe": "15m",
  "open": 100000,
  "high": 101000,
  "low": 99500,
  "close": 100700,
  "volume": 1234.56
}
```

### TRADE_RECEIVED

Emitido quando um trade individual é recebido.

### ORDERBOOK_UPDATED

Emitido quando o book é atualizado.

## Eventos de agentes

### AGENT_STARTED

Agente iniciou processamento.

### AGENT_COMPLETED

Agente concluiu análise.

### AGENT_FAILED

Agente falhou.

### AGENT_TIMEOUT

Agente não respondeu dentro do limite.

## Eventos de decisão

### DECISION_CANDIDATE_CREATED

Orchestrator criou uma decisão candidata.

### DECISION_SENT_TO_RISK

Decisão enviada ao Risk Management.

### DECISION_APPROVED

Risco aprovou a decisão.

### DECISION_REDUCED

Risco reduziu a decisão.

### DECISION_BLOCKED

Risco bloqueou a decisão.

## Eventos de risco

### RISK_CHECK_STARTED

Validação de risco iniciada.

### RISK_CHECK_COMPLETED

Validação de risco concluída.

### KILL_SWITCH_TRIGGERED

Kill switch acionado.

## Eventos de paper trading

### PAPER_ORDER_CREATED

Ordem simulada criada.

### PAPER_ORDER_FILLED

Ordem simulada preenchida.

### PAPER_ORDER_CLOSED

Ordem simulada encerrada.

### PAPER_ORDER_CANCELLED

Ordem simulada cancelada.

## Eventos de auditoria

### AUDIT_LOG_CREATED

Registro de auditoria criado.

### AUDIT_LOG_FAILED

Falha ao registrar auditoria.

Regra crítica: se a auditoria falhar em evento crítico, a operação deve ser bloqueada.

## Eventos de sistema

### SYSTEM_STARTED

Sistema iniciado.

### SYSTEM_STOPPED

Sistema parado.

### SYSTEM_MODE_CHANGED

Modo do sistema alterado.

Modos permitidos:

```text
OFFLINE
PAPER
LIVE_LOCKED
LIVE
```

Na Fase 1, o único modo operacional permitido é:

```text
PAPER
```

## Correlação

Todos os eventos gerados a partir do mesmo candle ou decisão devem compartilhar o mesmo `correlation_id`.

Isso permite reconstruir a cadeia completa:

```text
CANDLE_CLOSED → AGENT_COMPLETED → DECISION_CANDIDATE_CREATED → RISK_CHECK_COMPLETED → PAPER_ORDER_CREATED → AUDIT_LOG_CREATED
```
