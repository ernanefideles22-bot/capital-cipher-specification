# 24 — Agent Memory

## Objetivo

Definir o sistema de memória dos agentes do Capital Cipher AI.

A memória permite que agentes consultem histórico, desempenho, decisões anteriores e contexto operacional sem depender de estado interno frágil.

## Princípio central

Agentes não devem manter memória crítica apenas em RAM.

Qualquer informação necessária para auditoria, aprendizado, replay ou análise posterior deve ser persistida.

## Tipos de memória

```text
Short-term memory
Operational memory
Performance memory
Audit memory
Strategic memory
```

## Short-term memory

Memória temporária usada durante um ciclo de decisão.

Exemplo:

- candles recentes;
- outputs de agentes;
- decisão candidata;
- risk check.

Pode existir em RAM durante o ciclo, mas deve ser registrada se for relevante.

## Operational memory

Histórico recente do sistema.

Exemplos:

- status dos agentes;
- últimas decisões;
- falhas recentes;
- latência média;
- símbolos ativos;
- modo operacional.

## Performance memory

Memória de desempenho dos agentes e estratégias.

Exemplos:

- win rate por agente;
- acurácia por regime de mercado;
- drawdown por estratégia;
- erros por timeframe;
- confiança média versus resultado real.

## Audit memory

Registro imutável dos eventos importantes.

Deve responder:

- quem decidiu;
- quando decidiu;
- com quais dados;
- por qual motivo;
- qual foi o resultado;
- qual agente falhou.

## Strategic memory

Memória futura para aprendizado de longo prazo.

Exemplos:

- estratégias com melhor desempenho por regime;
- parâmetros que funcionam melhor em alta volatilidade;
- agentes que devem ter menor peso em certos contextos.

## Regra de acesso

Agentes podem consultar memória através de interfaces controladas.

Agentes não devem acessar tabelas diretamente de forma livre.

Acesso deve ocorrer por serviços/repositórios:

```text
MemoryService
PerformanceRepository
AuditRepository
DecisionRepository
```

## Memória proibida

Não armazenar:

- API keys;
- senhas;
- tokens;
- secrets;
- dados sensíveis sem necessidade.

## Exemplo de consulta de memória

```json
{
  "agent_name": "QuantAgent",
  "query_type": "RECENT_PERFORMANCE",
  "symbol": "BTCUSDT",
  "timeframe": "15m",
  "lookback_days": 30
}
```

## Exemplo de resposta

```json
{
  "agent_name": "QuantAgent",
  "symbol": "BTCUSDT",
  "timeframe": "15m",
  "trades": 120,
  "win_rate": 54.2,
  "avg_confidence": 76,
  "profit_factor": 1.32,
  "max_drawdown": 7.4
}
```

## Memória e aprendizado

A memória deve servir como base para aprendizado futuro, mas a Fase 1 não deve permitir aprendizado autônomo que altere risco ou execução sem validação.

## Retenção de dados

Política inicial sugerida:

```text
system_events: manter indefinidamente durante MVP
agent_outputs: manter indefinidamente durante MVP
decisions: manter indefinidamente durante MVP
paper_orders: manter indefinidamente durante MVP
logs técnicos: rotação futura
```

## Risco de memória contaminada

Dados ruins geram decisões ruins.

O sistema deve marcar eventos suspeitos:

- candle incompleto;
- falha de conexão;
- agente em timeout;
- decisão sem consenso;
- risco bloqueado;
- dados fora do padrão.

## Critério de sucesso

A memória estará aceitável quando permitir:

- reconstruir decisões;
- avaliar agentes;
- gerar relatórios;
- alimentar backtests;
- comparar estratégias;
- identificar falhas recorrentes.
