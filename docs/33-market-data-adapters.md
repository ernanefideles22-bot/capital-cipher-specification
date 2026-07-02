# 33 — Market Data Adapters

## Objetivo

Definir a arquitetura dos adaptadores de dados de mercado.

Market Data Adapters normalizam dados vindos de exchanges diferentes para um formato único usado pelo Capital Cipher AI.

## Princípio central

O restante do sistema não deve depender do formato específico da Binance, Bybit ou qualquer outra exchange.

Todos os dados externos devem ser convertidos para contratos internos.

## Responsabilidades

Um Market Data Adapter deve:

- conectar à fonte de dados;
- receber dados brutos;
- validar payload básico;
- normalizar formato;
- aplicar timestamp;
- emitir eventos internos;
- registrar falhas;
- reconectar quando necessário.

## Fontes iniciais

```text
Binance public WebSocket
Bybit public WebSocket
CSV historical data
Database replay
```

## Interface conceitual

```python
class MarketDataAdapter:
    async def connect(self) -> None: ...
    async def disconnect(self) -> None: ...
    async def subscribe_candles(self, symbol: str, timeframe: str) -> None: ...
    async def subscribe_trades(self, symbol: str) -> None: ...
    async def subscribe_orderbook(self, symbol: str) -> None: ...
```

## Contrato interno de candle

```json
{
  "exchange": "BINANCE",
  "symbol": "BTCUSDT",
  "timeframe": "15m",
  "open": 100000,
  "high": 101000,
  "low": 99500,
  "close": 100700,
  "volume": 1234.56,
  "closed_at": "2026-07-01T12:00:00Z",
  "received_at": "2026-07-01T12:00:01Z"
}
```

## Normalização

Cada exchange pode usar nomes, escalas e estruturas diferentes.

O adapter deve converter tudo para o formato interno.

Exemplos:

- `s` → `symbol`;
- `o` → `open`;
- `h` → `high`;
- `l` → `low`;
- `c` → `close`;
- `v` → `volume`.

## Reconexão

O adapter deve suportar:

- reconexão automática;
- backoff exponencial;
- limite de tentativas;
- evento MARKET_DISCONNECTED;
- evento MARKET_CONNECTED.

## Rate limits

Adapters devem respeitar limites de cada fonte.

Não implementar polling agressivo sem necessidade.

## Data Quality

Todo dado normalizado deve passar pelo módulo de qualidade antes de virar evento confiável.

Fluxo:

```text
Raw Exchange Payload
  ↓
Adapter Normalization
  ↓
Data Quality Validation
  ↓
Internal Market Event
```

## Adapters iniciais

### BinanceMarketDataAdapter

Responsável por dados públicos da Binance.

### BybitMarketDataAdapter

Responsável por dados públicos da Bybit.

### CsvMarketDataAdapter

Responsável por carregar dados históricos para backtest.

### ReplayMarketDataAdapter

Responsável por reprocessar dados já salvos no banco.

## Proibições

- Não enviar payload bruto diretamente aos agentes.
- Não deixar formato da exchange vazar para o domínio.
- Não ignorar desconexão.
- Não usar API key privada para market data público na Fase 1.
- Não misturar adapter de dados com execução de ordens.

## Critério de sucesso

Market Data Adapters estarão corretos quando o resto do sistema conseguir trocar Binance por Bybit sem alterar agentes, orquestrador ou risk management.
