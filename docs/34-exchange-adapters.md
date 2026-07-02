# 34 — Exchange Adapters

## Objetivo

Definir a arquitetura futura dos adaptadores de corretora para execução de ordens.

Este documento é preparatório. A Fase 1 não permite execução real.

## Princípio central

Execução real deve ser isolada em adapters específicos, protegida por Risk Management, System State e autorização explícita.

Nenhum agente analítico pode chamar diretamente um Exchange Adapter.

## Diferença entre adapters

### Market Data Adapter

Recebe dados públicos.

### Exchange Adapter

Interage com conta, ordens, posições e saldos.

Na Fase 1, Exchange Adapter real não deve ser implementado.

## Arquitetura futura

```text
Risk Approved Decision
  ↓
Execution Director
  ↓
Exchange Adapter Interface
  ↓
BinanceExchangeAdapter / BybitExchangeAdapter
  ↓
Exchange API
  ↓
Order Status Monitor
  ↓
Audit
```

## Interface conceitual

```python
class ExchangeAdapter:
    async def get_account_status(self) -> AccountStatus: ...
    async def get_balances(self) -> list[Balance]: ...
    async def place_order(self, order: OrderRequest) -> OrderResponse: ...
    async def cancel_order(self, order_id: str) -> CancelResponse: ...
    async def get_order_status(self, order_id: str) -> OrderStatus: ...
    async def close_position(self, position_id: str) -> ClosePositionResponse: ...
```

## Modos permitidos por fase

### Fase 1

Permitido:

- nenhum adapter real de execução;
- adapter simulado;
- paper trading.

Proibido:

- API key privada;
- ordem real;
- leitura de saldo real;
- alavancagem real.

### Fase futura LIVE_LOCKED

Permitido:

- read-only;
- validação de conexão;
- consulta de saldo com chave restrita.

Proibido por padrão:

- enviar ordens.

### Fase futura LIVE

Permitido apenas após autorização formal:

- ordens reais;
- cancelamento;
- fechamento de posição.

## Order Request interno

```json
{
  "order_id": "uuid",
  "decision_id": "uuid",
  "risk_check_id": "uuid",
  "correlation_id": "uuid",
  "exchange": "BYBIT",
  "symbol": "BTCUSDT",
  "side": "BUY",
  "order_type": "MARKET",
  "quantity": 0.001,
  "reduce_only": false,
  "stop_loss": 99500,
  "take_profit": 101800
}
```

## Order Response interno

```json
{
  "exchange_order_id": "abc123",
  "status": "ACCEPTED",
  "filled_quantity": 0,
  "avg_price": null,
  "raw_response_ref": "audit_ref"
}
```

## Estados de ordem

```text
CREATED
SUBMITTED
ACCEPTED
PARTIALLY_FILLED
FILLED
CANCELLED
REJECTED
FAILED
CLOSED
```

## Segurança

Exchange Adapters devem validar:

- system_mode;
- kill switch;
- risk_check aprovado;
- permissões de chave;
- limites de capital;
- idempotency key;
- estado da corretora.

## Idempotência

Toda ordem real futura deve possuir chave de idempotência.

Objetivo: evitar ordem duplicada em retry ou falha de rede.

## Rate limits

Cada exchange possui limites próprios.

Adapters devem:

- controlar frequência;
- registrar rate limit;
- recuar quando necessário;
- não bloquear o sistema inteiro.

## Auditoria

Toda interação futura com exchange deve registrar:

- request sanitizado;
- response sanitizada;
- timestamp;
- latency;
- status;
- erro, se houver.

Nunca registrar secrets.

## Proibições

- Não implementar execução real na Fase 1.
- Não permitir agente chamar adapter.
- Não enviar ordem sem risk_check.
- Não enviar ordem com kill switch ativo.
- Não armazenar API key no banco comum.
- Não expor adapter ao frontend.

## Critério de sucesso futuro

Exchange Adapters estarão corretos quando for possível trocar Binance por Bybit sem alterar Decision Engine, Risk Management ou Dashboard.
