# 20 — Observability

## Objetivo

Definir logs, métricas, tracing e monitoramento do Capital Cipher AI.

Observabilidade é obrigatória porque o sistema precisa explicar o que aconteceu antes, durante e depois de cada decisão.

## Princípios

- Todo evento crítico deve ser observável.
- Toda decisão deve ser rastreável por `correlation_id`.
- Falhas não devem ser silenciosas.
- Métricas devem revelar risco operacional, não apenas performance financeira.

## Camadas de observabilidade

```text
Logs
Metrics
Tracing
Audit
Alerts
Dashboards
```

## Logs estruturados

Formato mínimo:

```json
{
  "timestamp": "2026-07-01T12:00:00Z",
  "level": "INFO",
  "service": "orchestrator",
  "event_type": "DECISION_CANDIDATE_CREATED",
  "correlation_id": "uuid",
  "message": "Decision candidate created",
  "metadata": {}
}
```

## Níveis de log

```text
DEBUG
INFO
WARNING
ERROR
CRITICAL
```

## Logs obrigatórios

- conexão com exchange;
- perda de conexão;
- candle recebido;
- início e fim de agente;
- timeout de agente;
- decisão candidata;
- risk check;
- operação simulada;
- kill switch;
- falha de auditoria;
- mudança de modo;
- erro de banco.

## Métricas técnicas

- latência por agente;
- latência do market data;
- tempo de ciclo do Orchestrator;
- eventos por minuto;
- erros por serviço;
- timeouts por agente;
- uso de CPU;
- uso de memória;
- status do banco;
- status do WebSocket.

## Métricas operacionais

- decisões por dia;
- operações simuladas;
- operações bloqueadas;
- bloqueios por risco;
- acionamentos do kill switch;
- PnL simulado;
- drawdown simulado;
- perdas consecutivas.

## Métricas por agente

Cada agente deve expor:

- total de execuções;
- taxa de falha;
- latência média;
- último status;
- confidence média;
- distribuição de sinais;
- quantidade de warnings.

## Tracing

Cada cadeia de decisão deve ser rastreável por `correlation_id`.

Exemplo:

```text
CANDLE_CLOSED
  → AGENT_STARTED
  → AGENT_COMPLETED
  → DECISION_CANDIDATE_CREATED
  → RISK_CHECK_COMPLETED
  → PAPER_ORDER_CREATED
  → AUDIT_LOG_CREATED
```

## Alertas

Alertas obrigatórios:

- exchange desconectada;
- agente crítico falhou;
- risk check falhou;
- auditoria falhou;
- drawdown acima do limite;
- kill switch acionado;
- banco indisponível;
- latência excessiva;
- modo alterado.

## Ferramentas possíveis

Fase inicial:

- logs JSON em arquivo;
- banco de auditoria;
- dashboard interno.

Fase posterior:

- Sentry;
- Grafana;
- Prometheus;
- Loki;
- OpenTelemetry.

## Dashboard de observabilidade

Deve mostrar:

- status geral;
- serviços ativos;
- latência;
- erros recentes;
- eventos por minuto;
- agentes falhando;
- últimas decisões;
- estado do risco.

## Regra de segurança

Logs nunca devem conter:

- secrets;
- API keys;
- tokens;
- senhas;
- dados privados sensíveis.

## Critério de sucesso

A observabilidade estará aceitável quando for possível responder:

- por que o sistema entrou em uma operação;
- por que o sistema bloqueou uma operação;
- qual agente falhou;
- qual evento causou uma decisão;
- qual foi a latência;
- quais dados estavam disponíveis no momento.
