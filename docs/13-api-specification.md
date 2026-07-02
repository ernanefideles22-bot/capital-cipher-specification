# 13 — API Specification

## Objetivo

Definir a API inicial do Capital Cipher AI para backend, dashboard, agentes, auditoria e paper trading.

A API deve ser projetada para monitoramento, controle operacional e integração futura, não para expor execução real na Fase 1.

## Princípios

- Toda API deve ser versionada.
- Toda resposta deve seguir formato consistente.
- Toda operação crítica deve gerar evento de auditoria.
- Nenhum endpoint da Fase 1 pode executar ordem real.
- Endpoints administrativos devem exigir autenticação.

## Base URL

```text
/api/v1
```

## Formato padrão de resposta

### Sucesso

```json
{
  "success": true,
  "data": {},
  "error": null,
  "meta": {
    "request_id": "uuid",
    "timestamp": "2026-07-01T12:00:00Z"
  }
}
```

### Erro

```json
{
  "success": false,
  "data": null,
  "error": {
    "code": "RISK_BLOCKED",
    "message": "Operation blocked by risk management",
    "details": {}
  },
  "meta": {
    "request_id": "uuid",
    "timestamp": "2026-07-01T12:00:00Z"
  }
}
```

## Health Check

### GET `/health`

Verifica se o backend está online.

Resposta:

```json
{
  "status": "ok",
  "service": "capital-cipher-api",
  "version": "1.0.0"
}
```

### GET `/status`

Retorna estado operacional do sistema.

```json
{
  "mode": "PAPER",
  "market_data": "CONNECTED",
  "orchestrator": "RUNNING",
  "risk": "ACTIVE",
  "database": "CONNECTED"
}
```

## Market Data

### GET `/market/symbols`

Retorna ativos permitidos.

### GET `/market/candles`

Parâmetros:

```text
exchange=BINANCE
symbol=BTCUSDT
timeframe=15m
limit=100
```

### GET `/market/latency`

Retorna latência das conexões com exchanges.

## Agents

### GET `/agents`

Lista agentes registrados.

### GET `/agents/status`

Retorna status de cada agente.

```json
[
  {
    "name": "QuantAgent",
    "status": "READY",
    "last_run_at": "2026-07-01T12:00:00Z",
    "avg_latency_ms": 42
  }
]
```

### GET `/agents/{agent_name}/last-output`

Retorna último output de um agente.

## Orchestrator

### GET `/orchestrator/status`

Retorna estado do orquestrador.

### POST `/orchestrator/evaluate`

Executa avaliação manual em modo PAPER.

Obrigatório: este endpoint não pode enviar ordem real.

Body:

```json
{
  "exchange": "BINANCE",
  "symbol": "BTCUSDT",
  "timeframe": "15m"
}
```

## Decisions

### GET `/decisions`

Lista decisões recentes.

### GET `/decisions/{decision_id}`

Detalha uma decisão específica.

## Risk

### GET `/risk/status`

Retorna estado atual de risco.

### GET `/risk/limits`

Retorna limites configurados.

### POST `/risk/kill-switch`

Aciona kill switch manual.

Body:

```json
{
  "reason": "Manual emergency stop"
}
```

## Paper Trading

### GET `/paper/orders`

Lista ordens simuladas.

### GET `/paper/orders/{order_id}`

Detalha ordem simulada.

### GET `/paper/performance`

Retorna performance do paper trading.

Métricas iniciais:

- total de trades;
- win rate;
- PnL simulado;
- drawdown;
- sequência de perdas;
- taxa estimada;
- slippage estimado.

## Audit

### GET `/audit/events`

Lista eventos auditados.

### GET `/audit/correlation/{correlation_id}`

Reconstrói toda a cadeia de eventos relacionada a uma decisão.

## WebSocket

### `/ws/system`

Stream de estado do sistema.

Eventos enviados:

- SYSTEM_STATUS;
- AGENT_STATUS;
- MARKET_STATUS;
- RISK_STATUS.

### `/ws/market`

Stream de candles e preços.

### `/ws/logs`

Stream de logs operacionais.

### `/ws/decisions`

Stream de decisões do Orchestrator.

## Códigos de erro

```text
VALIDATION_ERROR
NOT_FOUND
AGENT_TIMEOUT
AGENT_FAILED
RISK_BLOCKED
KILL_SWITCH_ACTIVE
AUDIT_FAILED
MARKET_DATA_UNAVAILABLE
SYSTEM_NOT_READY
UNAUTHORIZED
FORBIDDEN
```

## Segurança da API

Na Fase 1, endpoints sensíveis devem exigir autenticação.

Endpoints públicos permitidos apenas para:

- health check;
- status básico, se não expuser dado sensível.

## Proibição crítica

Nenhum endpoint da Fase 1 pode executar ordem real em corretora.

Qualquer implementação de execução real antes da fase autorizada viola a arquitetura oficial do projeto.
