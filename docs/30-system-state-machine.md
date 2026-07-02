# 30 — System State Machine

## Objetivo

Definir a máquina de estados do Capital Cipher AI.

O sistema deve operar sempre em um estado explícito. Transições precisam ser controladas, auditadas e seguras.

## Estados principais

```text
OFFLINE
INITIALIZING
PAPER
LIVE_LOCKED
LIVE
DEGRADED
ERROR
MAINTENANCE
```

## OFFLINE

Sistema parado ou sem processamento operacional.

Permite:

- leitura de configuração;
- health check básico;
- inicialização.

Não permite:

- avaliação de mercado;
- paper orders;
- execução real.

## INITIALIZING

Sistema carregando dependências.

Valida:

- banco;
- configuração;
- agentes;
- market data;
- logs;
- auditoria.

Transição permitida:

```text
INITIALIZING → PAPER
INITIALIZING → ERROR
```

## PAPER

Modo operacional da Fase 1.

Permite:

- market data;
- agentes;
- decisão candidata;
- risk check;
- paper trading;
- auditoria;
- dashboard.

Não permite:

- execução real;
- API keys privadas;
- live orders.

## LIVE_LOCKED

Modo futuro.

Integrações reais podem estar configuradas, mas execução real fica bloqueada por padrão.

Transição para LIVE exige autorização explícita.

## LIVE

Modo futuro de execução real.

Não permitido na Fase 1.

## DEGRADED

Sistema parcialmente funcional com falhas não fatais.

Exemplos:

- agente não crítico falhando;
- dashboard com atraso;
- fonte secundária indisponível.

Em DEGRADED, o sistema pode continuar em PAPER se risco e auditoria estiverem funcionando.

## ERROR

Falha crítica.

Exemplos:

- auditoria indisponível;
- banco indisponível;
- risk check falhando;
- dados de mercado corrompidos;
- kill switch acionado por falha.

Em ERROR, novas operações devem ser bloqueadas.

## MAINTENANCE

Modo de manutenção.

Permite ajustes e inspeções. Não permite operação.

## Transições permitidas

```text
OFFLINE → INITIALIZING
INITIALIZING → PAPER
INITIALIZING → ERROR
PAPER → DEGRADED
DEGRADED → PAPER
PAPER → ERROR
DEGRADED → ERROR
ERROR → MAINTENANCE
MAINTENANCE → OFFLINE
PAPER → OFFLINE
```

Transições futuras:

```text
PAPER → LIVE_LOCKED
LIVE_LOCKED → LIVE
LIVE → LIVE_LOCKED
LIVE_LOCKED → PAPER
```

## Transições proibidas na Fase 1

```text
PAPER → LIVE_LOCKED
PAPER → LIVE
OFFLINE → LIVE
ERROR → LIVE
```

## Eventos que alteram estado

```text
SYSTEM_STARTED
SYSTEM_READY
SYSTEM_MODE_CHANGED
KILL_SWITCH_TRIGGERED
AUDIT_FAILED
DATABASE_UNAVAILABLE
MARKET_DATA_UNAVAILABLE
RISK_ENGINE_FAILED
SYSTEM_STOPPED
```

## Kill Switch

Kill switch deve forçar:

```text
PAPER → ERROR
LIVE_LOCKED → ERROR
LIVE → ERROR
```

Na Fase 1, kill switch bloqueia paper orders e novas decisões.

## Regras de auditoria

Toda transição de estado deve registrar:

- estado anterior;
- novo estado;
- motivo;
- ator;
- timestamp;
- correlation_id quando aplicável.

## Recuperação

Após ERROR:

```text
ERROR → MAINTENANCE → OFFLINE → INITIALIZING → PAPER
```

Não permitir retorno direto:

```text
ERROR → PAPER
```

sem validação.

## Critério de sucesso

A máquina de estados estará correta quando impedir estados inseguros e permitir reconstruir todas as transições por auditoria.
