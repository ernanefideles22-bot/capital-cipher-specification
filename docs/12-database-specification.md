# 12 — Database Specification

## Objetivo

Definir o modelo inicial de banco de dados para a Fase 1 do Capital Cipher AI.

O banco deve permitir auditoria, replay, análise de performance e rastreabilidade completa das decisões.

## Banco recomendado

- PostgreSQL
- Supabase pode ser usado como opção inicial gerenciada

## Princípios

- Toda decisão deve ser rastreável.
- Todo evento crítico deve ser persistido.
- Nenhuma operação simulada deve existir sem decisão e validação de risco associadas.
- `correlation_id` deve conectar eventos da mesma cadeia.

## Tabelas iniciais

### system_events

Armazena eventos gerais do sistema.

Campos:

```text
id UUID PK
event_type TEXT NOT NULL
source TEXT NOT NULL
correlation_id UUID NOT NULL
payload JSONB NOT NULL
created_at TIMESTAMPTZ NOT NULL
```

Índices:

```text
idx_system_events_correlation_id
idx_system_events_event_type
idx_system_events_created_at
```

---

### market_candles

Armazena candles recebidos das exchanges.

Campos:

```text
id UUID PK
exchange TEXT NOT NULL
symbol TEXT NOT NULL
timeframe TEXT NOT NULL
open NUMERIC NOT NULL
high NUMERIC NOT NULL
low NUMERIC NOT NULL
close NUMERIC NOT NULL
volume NUMERIC NOT NULL
closed_at TIMESTAMPTZ NOT NULL
created_at TIMESTAMPTZ NOT NULL
```

Índices:

```text
idx_market_candles_symbol_timeframe_closed_at
```

---

### agent_outputs

Armazena resposta de cada agente.

Campos:

```text
id UUID PK
correlation_id UUID NOT NULL
agent_name TEXT NOT NULL
status TEXT NOT NULL
signal TEXT
confidence INTEGER
reason TEXT
evidence JSONB
warnings JSONB
latency_ms INTEGER
created_at TIMESTAMPTZ NOT NULL
```

Índices:

```text
idx_agent_outputs_correlation_id
idx_agent_outputs_agent_name
idx_agent_outputs_created_at
```

---

### decisions

Armazena decisões candidatas do Orchestrator.

Campos:

```text
id UUID PK
correlation_id UUID NOT NULL
symbol TEXT NOT NULL
timeframe TEXT NOT NULL
candidate_action TEXT NOT NULL
confidence INTEGER NOT NULL
reason TEXT
agent_summary JSONB NOT NULL
risk_status TEXT DEFAULT 'PENDING'
created_at TIMESTAMPTZ NOT NULL
```

---

### risk_checks

Armazena validações de risco.

Campos:

```text
id UUID PK
decision_id UUID NOT NULL
correlation_id UUID NOT NULL
risk_status TEXT NOT NULL
approved BOOLEAN NOT NULL
position_size NUMERIC
risk_percent NUMERIC
stop_loss NUMERIC
take_profit NUMERIC
risk_reward NUMERIC
reason TEXT
warnings JSONB
created_at TIMESTAMPTZ NOT NULL
```

---

### paper_orders

Armazena ordens simuladas.

Campos:

```text
id UUID PK
decision_id UUID NOT NULL
risk_check_id UUID NOT NULL
correlation_id UUID NOT NULL
exchange TEXT NOT NULL
symbol TEXT NOT NULL
side TEXT NOT NULL
entry_price NUMERIC NOT NULL
stop_loss NUMERIC
take_profit NUMERIC
position_size NUMERIC NOT NULL
status TEXT NOT NULL
fees_estimated NUMERIC
slippage_estimated NUMERIC
opened_at TIMESTAMPTZ
closed_at TIMESTAMPTZ
pnl NUMERIC
created_at TIMESTAMPTZ NOT NULL
```

---

### audit_logs

Armazena auditoria completa.

Campos:

```text
id UUID PK
correlation_id UUID NOT NULL
audit_type TEXT NOT NULL
entity_type TEXT NOT NULL
entity_id UUID
payload JSONB NOT NULL
created_at TIMESTAMPTZ NOT NULL
```

## Relações lógicas

```text
system_events.correlation_id
  → agent_outputs.correlation_id
  → decisions.correlation_id
  → risk_checks.correlation_id
  → paper_orders.correlation_id
  → audit_logs.correlation_id
```

## Regra crítica

Se uma decisão não puder ser registrada, ela não pode avançar para paper trading.

Se uma validação de risco não puder ser registrada, a operação deve ser bloqueada.

Se a auditoria falhar, o sistema deve entrar em modo seguro.

## Dados sensíveis

Na Fase 1, o banco não deve armazenar API keys privadas de corretora.

Quando houver necessidade futura de secrets, eles devem ser armazenados em cofre apropriado, não em tabela comum.
