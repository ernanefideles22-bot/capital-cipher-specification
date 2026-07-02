# 32 — Data Quality

## Objetivo

Definir regras de qualidade de dados para o Capital Cipher AI.

Decisões de trading baseadas em dados ruins devem ser bloqueadas.

## Princípio central

Dados de mercado são insumo crítico. Se a qualidade dos dados for duvidosa, o sistema deve reduzir confiança ou bloquear decisão.

## Dados avaliados

```text
candles
trades
orderbook
volume
spread
timestamps
latency
exchange status
```

## Validação de candle

Um candle é válido quando:

- possui exchange;
- possui symbol;
- possui timeframe;
- possui open, high, low, close;
- possui volume;
- possui closed_at;
- high >= open, close e low;
- low <= open, close e high;
- volume >= 0;
- timestamp é coerente com timeframe.

## Candle inválido

Exemplos:

```text
high menor que close
low maior que open
volume negativo
timestamp ausente
symbol vazio
close nulo
```

Ação:

- rejeitar candle;
- registrar DataQualityError;
- bloquear decisão se candle for necessário.

## Dados atrasados

Dados são atrasados quando o timestamp recebido está além do limite aceitável.

Exemplo inicial:

```text
max_market_data_delay_ms: 5000
```

Se exceder:

- marcar warning;
- reduzir confiança;
- bloquear se atraso for crítico.

## Gaps

Gap ocorre quando candles esperados não chegam.

Ação:

- detectar intervalo ausente;
- solicitar preenchimento se possível;
- bloquear decisão se gap afetar indicadores.

## Duplicação

Eventos duplicados devem ser identificados por:

- event_id;
- exchange + symbol + timeframe + closed_at;
- correlation_id quando aplicável.

Ação:

- ignorar duplicado idempotente;
- registrar warning se duplicação for recorrente.

## Ordem temporal

Candles devem ser processados em ordem.

Proibido:

- calcular indicador com candles fora de ordem;
- processar candle futuro antes do anterior;
- ignorar buracos sem marcação.

## Outliers

Outliers devem ser detectados, mas não necessariamente rejeitados.

Exemplos:

- candle muito grande;
- volume extremo;
- spread anormal;
- variação incompatível com histórico recente.

Ação:

- marcar HIGH_VOLATILITY ou DATA_ANOMALY;
- enviar para Risk Management;
- reduzir confiança ou bloquear conforme severidade.

## Latência

Monitorar:

- latência da exchange;
- latência do processamento;
- latência dos agentes;
- latência do WebSocket interno.

Latência excessiva deve gerar warning ou bloqueio.

## Qualidade do orderbook

Quando usado, validar:

- bids presentes;
- asks presentes;
- bid menor que ask;
- spread calculável;
- profundidade mínima.

## Data Quality Score

Criar score de 0 a 100.

Exemplo:

```text
100 = dados íntegros
80 = pequenos warnings
60 = dados utilizáveis com cautela
<60 = bloquear decisão
```

## Saída padrão

```json
{
  "data_quality_score": 92,
  "status": "VALID",
  "warnings": [],
  "errors": []
}
```

## Estados possíveis

```text
VALID
WARNING
SUSPECT
INVALID
```

## Regras de bloqueio

Bloquear quando:

- candle inválido;
- gap crítico;
- dados muito atrasados;
- orderbook corrompido;
- timestamp inconsistente;
- fonte desconectada;
- score abaixo do limite mínimo.

## Proibições

- Não preencher dado faltante sem marcar.
- Não suavizar outlier sem registrar.
- Não calcular indicador com candle inválido.
- Não operar com fonte desconectada.
- Não ignorar timestamp.

## Critério de sucesso

O módulo de qualidade de dados estará correto quando impedir que dados ruins virem decisões silenciosamente.
