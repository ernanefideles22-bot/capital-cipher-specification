# 26 — Strategy Engine

## Objetivo

Definir o motor de estratégias do Capital Cipher AI.

O Strategy Engine determina quais regras, timeframes, agentes e parâmetros devem ser aplicados em cada contexto de mercado.

## Princípio central

Estratégia não é apenas um conjunto de indicadores. Estratégia é um conjunto de regras com contexto, risco, entrada, saída, invalidação e critérios de não operação.

## Estratégias iniciais

### SCALP_15M

Foco em operações curtas no timeframe de 15 minutos.

Ativos iniciais:

- BTCUSDT;
- ETHUSDT;
- SOLUSDT.

Agentes principais:

- Quant Agent;
- Trend Agent;
- Risk Agent;
- Paper Trading Agent.

Indicadores:

- EMA 9;
- EMA 21;
- EMA 50;
- RSI;
- ATR;
- VWAP;
- volume ratio.

### DAY_1H

Foco em operações intraday no timeframe de 1 hora.

### SWING_4H

Foco em operações mais longas no timeframe de 4 horas.

## Estrutura de estratégia

```json
{
  "strategy_id": "SCALP_15M",
  "enabled": true,
  "symbols": ["BTCUSDT", "ETHUSDT", "SOLUSDT"],
  "timeframe": "15m",
  "minimum_confidence": 75,
  "risk_profile": "MODERATE",
  "required_agents": ["QuantAgent", "TrendAgent"],
  "optional_agents": ["VolumeAgent", "LiquidityAgent"],
  "entry_rules": [],
  "exit_rules": [],
  "block_rules": []
}
```

## Perfis de risco

### CONSERVATIVE

```text
risk_per_trade: 0.5%
max_open_positions: 3
risk_reward_min: 1.8
```

### MODERATE

```text
risk_per_trade: 1.0%
max_open_positions: 8
risk_reward_min: 2.2
```

### AGGRESSIVE

```text
risk_per_trade: 1.5%
max_open_positions: 15
risk_reward_min: 2.8
```

Na Fase 1, todos são apenas simulação.

## Regras de entrada

Uma estratégia deve definir:

- contexto permitido;
- indicadores mínimos;
- agentes obrigatórios;
- confiança mínima;
- volatilidade aceitável;
- volume mínimo;
- stop lógico;
- take lógico;
- critério de invalidação.

## Regras de saída

Uma estratégia deve definir:

- stop loss;
- take profit;
- saída por tempo;
- saída por reversão;
- saída por risco;
- saída por kill switch.

## Regras de bloqueio

Operação deve ser bloqueada quando:

- mercado estiver fora do regime da estratégia;
- volume for insuficiente;
- volatilidade exceder limite;
- agentes críticos falharem;
- dados estiverem incompletos;
- risk/reward for inadequado;
- drawdown estiver no limite.

## Configuração por regime

Estratégias devem considerar regime de mercado:

```text
BULL_TREND
BEAR_TREND
RANGE
HIGH_VOLATILITY
LOW_VOLATILITY
UNDEFINED
```

Exemplo:

```text
SCALP_15M pode operar em BULL_TREND e BEAR_TREND.
SCALP_15M deve reduzir agressividade em RANGE.
SCALP_15M deve bloquear em HIGH_VOLATILITY extrema.
```

## Estratégia e backtest

Toda estratégia deve possuir:

- versão;
- parâmetros;
- histórico de backtests;
- histórico de paper trading;
- métricas por período;
- status de aprovação.

## Versionamento de estratégia

Exemplo:

```text
SCALP_15M_v1
SCALP_15M_v2
```

Nunca alterar estratégia em produção sem nova versão.

## Proibições

- Não alterar estratégia sem registrar versão.
- Não operar sem risk profile.
- Não executar estratégia sem agentes obrigatórios.
- Não misturar parâmetros de backtest com live sem validação.

## Critério de sucesso

O Strategy Engine estará aceitável quando permitir ligar/desligar estratégias, aplicar parâmetros por regime e gerar decisões rastreáveis por versão de estratégia.
