# 18 — Paper Trading

## Objetivo

Definir o sistema de paper trading do Capital Cipher AI.

Paper trading é a fase em que o sistema simula operações com dados reais de mercado, sem enviar ordens reais para corretoras.

## Princípio central

Paper trading deve ser tratado como ambiente de validação operacional, não como prova definitiva de lucratividade.

## Responsabilidades

O Paper Trading Engine deve:

- receber decisões aprovadas pelo Risk Management;
- criar ordens simuladas;
- simular preenchimento;
- aplicar taxas estimadas;
- aplicar slippage estimado;
- acompanhar stop loss;
- acompanhar take profit;
- encerrar posições;
- calcular PnL;
- registrar auditoria completa.

## Fluxo

```text
Decision Approved
  ↓
Risk Check
  ↓
Paper Order Created
  ↓
Simulated Fill
  ↓
Position Monitoring
  ↓
Exit Condition
  ↓
PnL Calculation
  ↓
Audit
```

## Tipos de ordem simulada

Na Fase 1:

- MARKET_SIMULATED;
- LIMIT_SIMULATED em fase posterior.

## Estados da ordem

```text
CREATED
FILLED
PARTIALLY_FILLED
CANCELLED
CLOSED
EXPIRED
FAILED
```

## Campos mínimos de uma ordem simulada

```json
{
  "paper_order_id": "uuid",
  "decision_id": "uuid",
  "risk_check_id": "uuid",
  "correlation_id": "uuid",
  "exchange": "BINANCE",
  "symbol": "BTCUSDT",
  "side": "BUY",
  "entry_price": 100700,
  "stop_loss": 99500,
  "take_profit": 101800,
  "position_size": 100.0,
  "status": "FILLED",
  "fees_estimated": 0.08,
  "slippage_estimated": 0.02
}
```

## Regras de preenchimento

### Market simulated

A entrada simulada deve considerar:

- preço atual;
- spread estimado;
- slippage estimado;
- taxa da corretora.

### Stop loss

Para posição BUY:

```text
se preço <= stop_loss → fechar posição
```

Para posição SELL:

```text
se preço >= stop_loss → fechar posição
```

### Take profit

Para posição BUY:

```text
se preço >= take_profit → fechar posição
```

Para posição SELL:

```text
se preço <= take_profit → fechar posição
```

## Taxas e slippage

Mesmo em simulação, o sistema deve descontar:

- taxa maker/taker estimada;
- slippage estimado;
- spread estimado.

Ignorar custos transforma o paper trading em ilusão estatística.

## Métricas obrigatórias

- trades abertos;
- trades fechados;
- win rate;
- PnL bruto;
- PnL líquido;
- taxas estimadas;
- slippage estimado;
- drawdown;
- perdas consecutivas;
- performance por ativo;
- performance por timeframe;
- performance por agente/estratégia.

## Integração com Risk Management

Paper order só pode ser criada se existir:

- decision_id;
- risk_check_id;
- risk_status APPROVED ou REDUCED.

Se risk_status for BLOCKED ou KILL_SWITCH, nenhuma ordem simulada deve ser criada.

## Auditoria

Cada ordem simulada deve registrar:

- motivo da entrada;
- agentes envolvidos;
- validação de risco;
- preços;
- horários;
- motivo da saída;
- resultado;
- métricas.

## Proibições

Na Fase 1, o Paper Trading Engine não pode:

- usar API key privada;
- enviar ordem real;
- alterar saldo real;
- simular sem risk_check;
- apagar histórico de trades.

## Critério de sucesso

O paper trading estará validado quando:

- operar por semanas sem falhas críticas;
- registrar todas as decisões;
- calcular PnL com custos;
- permitir replay de decisões;
- demonstrar estabilidade técnica;
- demonstrar comportamento coerente em diferentes regimes de mercado.
