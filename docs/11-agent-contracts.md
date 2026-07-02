# 11 — Agent Contracts

## Objetivo

Definir contratos padronizados de entrada e saída para os agentes do Capital Cipher AI.

Nenhum agente deve retornar resposta livre sem estrutura.

## Contrato base de entrada

```json
{
  "request_id": "uuid",
  "correlation_id": "uuid",
  "agent_name": "QuantAgent",
  "timestamp": "2026-07-01T12:00:00Z",
  "symbol": "BTCUSDT",
  "timeframe": "15m",
  "market_context": {},
  "config": {}
}
```

## Contrato base de saída

```json
{
  "agent_name": "QuantAgent",
  "status": "COMPLETED",
  "signal": "BUY",
  "confidence": 78,
  "reason": "Price above VWAP and EMA alignment bullish",
  "evidence": {},
  "warnings": [],
  "latency_ms": 42,
  "created_at": "2026-07-01T12:00:01Z"
}
```

## Status permitidos

```text
COMPLETED
FAILED
TIMEOUT
SKIPPED
BLOCKED
```

## Sinais permitidos

```text
BUY
SELL
HOLD
WAIT
BLOCK
NEUTRAL
```

## Regras de confiança

`confidence` deve ser um número inteiro entre 0 e 100.

Interpretação:

```text
0-39   baixa confiança
40-59  neutro/fraco
60-74  razoável
75-89  forte
90-100 muito forte
```

Confiança alta não autoriza execução sozinha.

## Market Data Agent

### Entrada

```json
{
  "exchange": "BINANCE",
  "symbol": "BTCUSDT",
  "timeframe": "15m",
  "data_type": "CANDLES"
}
```

### Saída

```json
{
  "agent_name": "MarketDataAgent",
  "status": "COMPLETED",
  "connection_status": "CONNECTED",
  "symbol": "BTCUSDT",
  "timeframe": "15m",
  "candles": [],
  "latency_ms": 35
}
```

## Quant Agent

### Entrada

Recebe candles OHLCV e configurações de indicadores.

### Saída

```json
{
  "agent_name": "QuantAgent",
  "status": "COMPLETED",
  "signal": "BUY",
  "confidence": 82,
  "reason": "Bullish EMA alignment, price above VWAP, RSI neutral",
  "evidence": {
    "ema_9": 100750,
    "ema_21": 100200,
    "ema_50": 99500,
    "rsi": 56.2,
    "atr": 420.5,
    "vwap": 100100
  },
  "warnings": []
}
```

## Trend Agent

### Saída

```json
{
  "agent_name": "TrendAgent",
  "status": "COMPLETED",
  "regime": "BULL_TREND",
  "signal": "BUY",
  "confidence": 76,
  "reason": "Higher highs and higher lows with EMA slope positive",
  "evidence": {
    "market_regime": "BULL_TREND",
    "volatility_state": "NORMAL"
  }
}
```

## Risk Agent

### Entrada

Recebe decisão candidata, saldo simulado, limites e estado de risco.

### Saída

```json
{
  "agent_name": "RiskAgent",
  "status": "COMPLETED",
  "risk_status": "APPROVED",
  "approved": true,
  "position_size": 100.0,
  "risk_percent": 1.0,
  "stop_loss": 99500,
  "take_profit": 101800,
  "risk_reward": 2.0,
  "reason": "Risk within configured limits",
  "warnings": []
}
```

## Paper Trading Agent

### Saída

```json
{
  "agent_name": "PaperTradingAgent",
  "status": "COMPLETED",
  "paper_order_id": "uuid",
  "side": "BUY",
  "entry_price": 100700,
  "stop_loss": 99500,
  "take_profit": 101800,
  "position_size": 100.0,
  "fees_estimated": 0.08,
  "slippage_estimated": 0.02
}
```

## Audit Agent

### Saída

```json
{
  "agent_name": "AuditAgent",
  "status": "COMPLETED",
  "audit_id": "uuid",
  "stored": true,
  "reason": "Decision chain stored successfully"
}
```

## Regra de compatibilidade

Qualquer mudança de contrato exige:

1. Atualizar este documento.
2. Atualizar schemas Pydantic.
3. Atualizar testes.
4. Validar consumidores do contrato.
