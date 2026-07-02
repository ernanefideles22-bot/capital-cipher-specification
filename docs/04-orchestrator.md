# 04 — Orchestrator

## Função

O Orchestrator é o centro de coordenação do Capital Cipher AI.

Ele não deve ser tratado como um agente comum. Ele é o componente responsável por organizar o fluxo de decisão.

## Responsabilidades

O Orchestrator deve:

- receber eventos de mercado;
- validar o formato dos dados;
- acionar os agentes necessários;
- aguardar respostas dentro de um tempo limite;
- consolidar os pareceres;
- gerar uma decisão candidata;
- enviar a decisão para validação de risco;
- registrar a decisão no sistema de auditoria.

## O que o Orchestrator não pode fazer

O Orchestrator não pode:

- executar ordens diretamente;
- ignorar veto do Risk Director;
- alterar saldo;
- modificar histórico;
- tomar decisão sem registrar evidências.

## Entrada padrão

Exemplo conceitual:

```json
{
  "event_type": "CANDLE_CLOSED",
  "exchange": "BINANCE",
  "symbol": "BTCUSDT",
  "timeframe": "15m",
  "timestamp": "2026-07-01T12:00:00Z",
  "market_data": {
    "open": 100000,
    "high": 101000,
    "low": 99500,
    "close": 100700,
    "volume": 1234.56
  }
}
```

## Saída consolidada

```json
{
  "decision_id": "uuid",
  "symbol": "BTCUSDT",
  "timeframe": "15m",
  "candidate_action": "BUY",
  "confidence": 82,
  "agents": [
    {
      "name": "QuantAgent",
      "signal": "BUY",
      "confidence": 84,
      "reason": "EMA alignment and VWAP support"
    }
  ],
  "risk_status": "PENDING",
  "created_at": "2026-07-01T12:00:02Z"
}
```

## Estratégia de tempo limite

Cada agente deve ter tempo máximo de resposta.

Se um agente não responder dentro do prazo, o Orchestrator deve registrar falha e decidir se continua ou bloqueia a análise.

Agentes críticos, como Risk Agent, não podem ser ignorados.

## Estado mínimo do Orchestrator

O Orchestrator deve manter:

- status dos agentes;
- última decisão;
- latência média;
- falhas recentes;
- fila de eventos pendentes;
- modo do sistema: OFFLINE, PAPER, LIVE_LOCKED ou LIVE.

## Modo inicial obrigatório

A Fase 1 deve operar exclusivamente em modo:

```text
PAPER
```

Nenhuma ordem real deve ser enviada.
