# 27 — Agent Ranking

## Objetivo

Definir como agentes serão avaliados, ranqueados e ponderados dentro do Capital Cipher AI.

O objetivo não é criar competição artificial entre agentes, mas medir confiabilidade, utilidade e desempenho por contexto de mercado.

## Princípio central

Nem todos os agentes devem ter o mesmo peso em todos os contextos.

Um agente pode ser bom em tendência e ruim em mercado lateral. O ranking precisa considerar contexto.

## Métricas por agente

Cada agente deve ser avaliado por:

- taxa de acerto direcional;
- contribuição para decisões vencedoras;
- contribuição para decisões perdedoras;
- confidence médio;
- confiança versus resultado;
- latência média;
- taxa de falha;
- taxa de timeout;
- warnings frequentes;
- desempenho por ativo;
- desempenho por timeframe;
- desempenho por regime de mercado.

## Contextos de avaliação

```text
symbol
timeframe
market_regime
strategy
volatility_state
session/time window
```

## Score do agente

Score inicial sugerido:

```text
agent_score = performance_score + reliability_score + context_score - penalty_score
```

## Componentes do score

### Performance score

Mede resultado histórico do agente em decisões passadas.

### Reliability score

Mede estabilidade operacional.

Considera:

- falhas;
- timeouts;
- latência;
- respostas inválidas.

### Context score

Mede desempenho em contextos específicos.

Exemplo:

```text
QuantAgent pode ter score alto em BULL_TREND e score menor em RANGE.
```

### Penalty score

Penaliza:

- overconfidence;
- sinais contraditórios frequentes;
- falhas recorrentes;
- outputs inválidos;
- warnings críticos.

## Overconfidence

Um agente é overconfident quando apresenta confidence alto, mas resultado ruim de forma recorrente.

Exemplo:

```text
confidence médio: 90
resultado: negativo
```

Esse agente deve ter peso reduzido.

## Uso no Decision Engine

O ranking pode influenciar pesos, mas não pode substituir regras de risco.

Exemplo:

```text
QuantAgent base weight: 40%
QuantAgent adjusted weight: 32% após penalidade por baixo desempenho em RANGE
```

## Peso mínimo e máximo

Para evitar comportamento instável, pesos devem possuir limites.

Exemplo:

```text
min_weight: 5%
max_weight: 50%
```

Nenhum agente analítico deve dominar sozinho a decisão.

## Atualização do ranking

Na Fase 1:

- ranking pode ser calculado em relatórios;
- não deve alterar decisão automaticamente sem controle.

Em fase posterior:

- ranking pode ajustar pesos de forma controlada;
- alterações devem ser auditadas.

## Dados necessários

Para ranquear agentes, registrar:

- agent_output;
- decision_id;
- paper_order_id;
- resultado da operação;
- contexto de mercado;
- strategy_id;
- timestamps.

## Métricas proibidas isoladamente

Não ranquear apenas por:

- win rate;
- confidence;
- PnL absoluto;
- quantidade de sinais.

Essas métricas isoladas podem enganar.

## Critério de avaliação mais robusto

Preferir combinação de:

- expectancy;
- drawdown;
- estabilidade;
- contexto;
- amostra suficiente;
- contribuição marginal.

## Amostra mínima

Não alterar peso de agente com poucos dados.

Exemplo inicial:

```text
min_samples_for_weight_adjustment: 100 decisions
```

## Proibições

- Não permitir que ranking ignore Risk Management.
- Não ajustar pesos sem auditoria.
- Não punir agente por decisão que foi alterada depois sem registrar contexto.
- Não promover agente com amostra pequena.

## Critério de sucesso

O Agent Ranking estará aceitável quando permitir identificar quais agentes ajudam, quais atrapalham e em quais contextos isso acontece.
