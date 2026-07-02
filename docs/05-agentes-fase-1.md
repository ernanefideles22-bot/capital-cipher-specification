# 05 — Agentes da Fase 1

## Objetivo da Fase 1

Criar os agentes essenciais para validar arquitetura, dados, análise, risco, simulação e auditoria.

A Fase 1 não busca lucro real. Busca provar que o sistema funciona de maneira organizada, rastreável e segura.

## Agentes obrigatórios

### 1. Market Data Agent

Responsável por coletar dados de mercado.

#### Entradas

- exchange;
- symbol;
- timeframe;
- tipo de dado solicitado.

#### Saídas

- candles OHLCV;
- trades;
- volume;
- spread;
- timestamp;
- status de conexão.

#### Regras

- Deve usar dados públicos na Fase 1.
- Não deve usar API key de conta real.
- Deve registrar falhas de conexão.

---

### 2. Quant Agent

Responsável por análise técnica quantitativa.

#### Indicadores iniciais

- EMA 9;
- EMA 21;
- EMA 50;
- RSI;
- ATR;
- VWAP;
- MACD;
- volume ratio.

#### Saída esperada

```json
{
  "agent": "QuantAgent",
  "signal": "BUY",
  "confidence": 78,
  "reason": "EMA9 above EMA21, price above VWAP, RSI neutral",
  "metrics": {
    "rsi": 54.2,
    "atr": 120.5
  }
}
```

---

### 3. Trend Agent

Responsável por classificar o regime de mercado.

#### Classificações possíveis

- BULL_TREND;
- BEAR_TREND;
- RANGE;
- HIGH_VOLATILITY;
- LOW_VOLATILITY;
- UNDEFINED.

---

### 4. Risk Agent

Responsável por validar risco.

Na Fase 1, não aprova dinheiro real. Apenas simula aprovação ou bloqueio.

#### Pode retornar

- APPROVED;
- REDUCED;
- BLOCKED;
- KILL_SWITCH.

---

### 5. Paper Trading Agent

Responsável por simular ordens.

#### Deve registrar

- preço de entrada;
- stop loss;
- take profit;
- tamanho simulado;
- resultado;
- slippage estimado;
- taxas estimadas.

---

### 6. Audit Agent

Responsável por registrar absolutamente tudo.

#### Deve salvar

- evento original;
- respostas dos agentes;
- decisão consolidada;
- validação de risco;
- operação simulada;
- erros;
- latência;
- timestamps.

## Agentes fora da Fase 1

Os seguintes agentes ficam para fases posteriores:

- Visual Chart Agent;
- News Agent;
- Macro Agent;
- Sentiment Agent;
- Portfolio Agent;
- Learning Agent;
- Live Execution Agent.

## Regra técnica

Cada agente deve possuir contrato de entrada e saída estável.

Alterações no contrato exigem atualização da documentação.
