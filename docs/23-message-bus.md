# 23 — Message Bus

## Objetivo

Definir a camada de comunicação assíncrona entre componentes do Capital Cipher AI.

O Message Bus permite que eventos de mercado, agentes, orquestrador, risco, paper trading e auditoria se comuniquem de forma desacoplada.

## Princípio central

Componentes não devem chamar uns aos outros diretamente quando a comunicação puder ser feita por eventos.

Na Fase 1, a implementação pode começar simples. A arquitetura deve, porém, estar preparada para Redis Streams, RabbitMQ, NATS ou Kafka em fases futuras.

## Responsabilidades

O Message Bus deve:

- distribuir eventos;
- preservar correlation_id;
- permitir consumidores independentes;
- reduzir acoplamento;
- suportar retries;
- registrar falhas;
- permitir replay em fases futuras.

## Implementação por fase

### Fase 1

Pode usar:

```text
in-memory event dispatcher
asyncio.Queue
Redis simples em fase posterior
```

### Fase 2+

Avaliar:

```text
Redis Streams
RabbitMQ
NATS
Kafka
```

## Tópicos conceituais

```text
market.events
agent.requests
agent.outputs
decision.events
risk.events
paper.orders
audit.events
system.events
```

## Formato padrão de mensagem

```json
{
  "message_id": "uuid",
  "event_id": "uuid",
  "correlation_id": "uuid",
  "topic": "market.events",
  "event_type": "CANDLE_CLOSED",
  "source": "MarketDataAgent",
  "timestamp": "2026-07-01T12:00:00Z",
  "version": "1.0",
  "payload": {}
}
```

## Regras de roteamento

### Market events

Devem ser consumidos por:

- Orchestrator;
- Audit Agent;
- Dashboard stream.

### Agent requests

Devem ser consumidos pelo agente específico solicitado.

### Agent outputs

Devem ser consumidos por:

- Orchestrator;
- Audit Agent;
- Observability.

### Risk events

Devem ser consumidos por:

- Orchestrator;
- Paper Trading Agent;
- Audit Agent;
- Dashboard.

## Garantias mínimas

Na Fase 1:

- entrega best-effort;
- logs de falha;
- correlation_id obrigatório.

Em fases futuras:

- persistência;
- retry;
- dead-letter queue;
- idempotência;
- replay.

## Idempotência

Eventos críticos devem possuir identificador único para evitar processamento duplicado.

Exemplo:

```text
paper_order_created não pode gerar duas ordens para o mesmo decision_id + risk_check_id.
```

## Dead Letter Queue futura

Eventos que falharem repetidamente devem ser enviados para uma fila de erro.

Tópico sugerido:

```text
dead_letter.events
```

## Replay futuro

O sistema deve ser desenhado para permitir reprocessar eventos históricos e reconstruir decisões.

Isso será essencial para:

- auditoria;
- debugging;
- backtesting;
- avaliação de agentes;
- investigação de falhas.

## Proibições

- Não perder correlation_id.
- Não usar evento sem versionamento.
- Não enviar secrets em payload.
- Não criar dependência circular entre agentes.
- Não permitir que agente publique ordem real.

## Critério de sucesso

O Message Bus estará aceitável quando o fluxo abaixo for rastreável:

```text
CANDLE_CLOSED → AGENT_REQUESTED → AGENT_COMPLETED → DECISION_CREATED → RISK_CHECKED → PAPER_ORDER_CREATED → AUDIT_LOGGED
```
