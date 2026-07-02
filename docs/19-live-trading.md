# 19 — Live Trading

## Objetivo

Definir critérios, restrições e arquitetura futura para live trading.

Este documento não autoriza implementação de execução real na Fase 1.

## Princípio central

Live trading é uma fase futura, bloqueada por padrão.

Nenhum código de execução real deve ser implementado antes de:

- backtesting validado;
- paper trading robusto;
- auditoria confiável;
- kill switch testado;
- gerenciamento de secrets;
- autorização explícita na documentação.

## Modos relacionados

```text
PAPER
LIVE_LOCKED
LIVE
```

### PAPER

Simulação apenas.

### LIVE_LOCKED

Integração real pode existir, mas ordens reais ficam bloqueadas por padrão.

### LIVE

Execução real permitida apenas com limites severos e autorização formal.

## Critérios obrigatórios antes de LIVE_LOCKED

- paper trading funcionando por período prolongado;
- logs auditáveis;
- banco estável;
- métricas confiáveis;
- risk management testado;
- kill switch testado;
- autenticação ativa;
- secrets protegidos;
- dashboard operacional;
- plano de rollback.

## Critérios obrigatórios antes de LIVE

- mínimo de semanas ou meses em paper trading;
- estratégia validada fora da amostra;
- drawdown dentro do limite;
- falhas operacionais corrigidas;
- API keys com permissões mínimas;
- saque desabilitado nas chaves;
- restrição de IP quando possível;
- capital inicial pequeno;
- monitoramento ativo;
- plano de emergência.

## Arquitetura futura de execução

```text
Risk Approved Decision
  ↓
Execution Director
  ↓
Exchange Adapter
  ↓
Order Router
  ↓
Exchange API
  ↓
Order Status Monitor
  ↓
Audit
```

## Execution Director

Responsável por:

- receber decisão aprovada;
- validar modo operacional;
- validar permissões;
- selecionar corretora;
- enviar ordem via adapter;
- monitorar resposta;
- registrar auditoria.

## Exchange Adapter

Cada corretora deve ter adapter próprio:

- BinanceAdapter;
- BybitAdapter;
- outras futuras.

Adapters devem implementar interface comum.

## Permissões mínimas

Chaves de API devem iniciar com:

```text
read-only
```

Depois, em fase controlada:

```text
trade enabled
withdrawal disabled
IP restricted
```

## Limites iniciais para live futuro

```text
risk_per_trade <= 0.25% a 0.50%
max_daily_drawdown <= 1.0% a 2.0%
max_open_positions <= 1 ou 2
max_consecutive_losses <= 2
leverage inicial = 1x
```

Esses limites devem ser conservadores.

## Condições de bloqueio absoluto

O sistema não deve operar live se:

- kill switch estiver ativo;
- auditoria falhar;
- risk check falhar;
- dados de mercado estiverem instáveis;
- latência estiver acima do limite;
- ocorrer divergência de saldo;
- houver falha no adapter da corretora;
- houver erro de autenticação;
- modo não for LIVE;
- limites não estiverem configurados.

## Auditoria obrigatória

Toda ordem real futura deve registrar:

- decisão original;
- risk check;
- payload enviado;
- resposta da corretora;
- status da ordem;
- execução parcial ou total;
- fees;
- PnL;
- motivo de saída;
- erros.

## Regra de capital

O sistema deve começar com capital muito pequeno em fase LIVE.

Aumentar capital somente após evidência operacional, não por confiança subjetiva.

## Proibição crítica

Qualquer implementação que envie ordens reais antes da liberação formal da fase LIVE deve ser considerada violação de segurança e arquitetura.

## Conclusão

Live trading é consequência de validação técnica e estatística. Não é objetivo da Fase 1.
