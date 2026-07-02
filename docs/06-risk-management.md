# 06 — Risk Management

## Princípio central

O gerenciamento de risco tem autoridade máxima sobre qualquer decisão operacional.

Nenhum agente analítico, orquestrador ou executor pode ignorar o Risk Management.

## Objetivo

Evitar que o sistema opere quando as condições de risco forem inadequadas.

A função do risco não é maximizar lucro. A função do risco é preservar capital e controlar exposição.

## Responsabilidades

O Risk Management deve validar:

- risco por operação;
- drawdown diário;
- drawdown total;
- exposição por ativo;
- exposição por direção;
- alavancagem máxima;
- quantidade de operações simultâneas;
- perda consecutiva;
- volatilidade anormal;
- falhas de dados;
- latência excessiva;
- modo operacional do sistema.

## Estados possíveis

### APPROVED

Operação aprovada para paper trading ou, em fases futuras, execução real controlada.

### REDUCED

Operação permitida, mas com tamanho reduzido.

### BLOCKED

Operação bloqueada.

### KILL_SWITCH

Todas as operações devem ser interrompidas imediatamente.

## Condições de bloqueio obrigatório

O sistema deve bloquear operação quando:

- dados de mercado estiverem incompletos;
- o Market Data Agent estiver instável;
- houver divergência grave entre agentes críticos;
- o drawdown diário atingir o limite;
- houver sequência máxima de perdas;
- a latência estiver acima do limite definido;
- o modo do sistema não permitir operação;
- o ativo estiver fora da lista permitida.

## Limites iniciais sugeridos

Para paper trading:

```text
risk_per_trade: 1.0%
max_daily_drawdown: 5.0%
max_consecutive_losses: 3
max_open_positions: 3
default_leverage: 1x
max_leverage: 5x em simulação
```

Para operação real futura, esses números devem ser revisados e reduzidos.

## Saída padrão

```json
{
  "risk_status": "APPROVED",
  "position_size": 100.0,
  "risk_percent": 1.0,
  "stop_loss": 99500,
  "take_profit": 101800,
  "risk_reward": 2.0,
  "reason": "Risk within configured limits"
}
```

## Regra crítica

Toda decisão de risco deve ser registrada antes da simulação ou execução.

Se o registro falhar, a operação deve ser bloqueada.
