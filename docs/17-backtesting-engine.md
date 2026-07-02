# 17 — Backtesting Engine

## Objetivo

Definir o motor de backtesting do Capital Cipher AI.

O Backtesting Engine deve permitir testar estratégias, agentes e regras de risco usando dados históricos antes de qualquer execução real.

## Princípio central

Nenhuma estratégia deve avançar para paper trading robusto ou live trading sem passar por backtests documentados.

Backtest não prova lucratividade futura. Ele apenas elimina estratégias claramente ruins, inconsistentes ou excessivamente ajustadas ao passado.

## Responsabilidades

O Backtesting Engine deve:

- carregar dados históricos;
- simular candles sequencialmente;
- executar agentes como se fosse tempo real;
- gerar decisões candidatas;
- aplicar regras de risco;
- simular entradas e saídas;
- calcular taxas e slippage estimados;
- gerar métricas de performance;
- registrar resultados;
- permitir replay de decisões.

## Fontes de dados

Fontes iniciais possíveis:

- Binance historical klines;
- Bybit historical data;
- arquivos CSV;
- banco interno de candles coletados.

## Dados mínimos necessários

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

## Fluxo de backtest

```text
Historical Data
  ↓
Candle Replay
  ↓
Orchestrator
  ↓
Agents
  ↓
Decision Engine
  ↓
Risk Management
  ↓
Simulated Execution
  ↓
Metrics
  ↓
Report
```

## Tipos de backtest

### Backtest simples

Executa uma estratégia em um intervalo histórico.

### Backtest por walk-forward

Divide dados em períodos de treino e validação.

### Out-of-sample test

Testa em dados que não foram usados para ajustar regras.

### Stress test

Testa em períodos de alta volatilidade, quedas fortes e lateralização.

## Métricas obrigatórias

- total de trades;
- win rate;
- loss rate;
- profit factor;
- expectancy;
- max drawdown;
- average win;
- average loss;
- risk/reward médio;
- sequência máxima de perdas;
- retorno acumulado;
- retorno por ativo;
- retorno por timeframe;
- exposição média;
- slippage estimado;
- fees estimadas.

## Métricas avançadas futuras

- Sharpe ratio;
- Sortino ratio;
- Calmar ratio;
- Ulcer index;
- Kelly fraction;
- Monte Carlo simulation;
- regime performance.

## Regras de simulação

O backtest deve evitar lookahead bias.

Proibido:

- usar preço futuro para decidir entrada passada;
- calcular indicador com dados ainda não disponíveis;
- usar fechamento de candle antes de ele existir;
- ignorar taxas e slippage;
- otimizar parâmetros sem validação fora da amostra.

## Saída padrão de relatório

```json
{
  "backtest_id": "uuid",
  "symbol": "BTCUSDT",
  "timeframe": "15m",
  "start_date": "2025-01-01",
  "end_date": "2025-06-01",
  "total_trades": 120,
  "win_rate": 54.2,
  "profit_factor": 1.42,
  "max_drawdown": 8.5,
  "net_pnl": 12.4,
  "fees": 1.1,
  "slippage": 0.6
}
```

## Persistência

Resultados de backtest devem ser salvos em tabelas próprias em fase futura:

```text
backtest_runs
backtest_trades
backtest_metrics
backtest_agent_outputs
```

## Critério mínimo para avançar

Uma estratégia só pode avançar para paper trading robusto se:

- tiver backtest positivo em mais de um período;
- não depender de apenas poucos trades;
- sobreviver a taxas e slippage;
- tiver drawdown aceitável;
- tiver desempenho fora da amostra;
- possuir relatório auditável.

## Regra final

Backtest bom não autoriza live trading. Apenas autoriza investigação em paper trading.
