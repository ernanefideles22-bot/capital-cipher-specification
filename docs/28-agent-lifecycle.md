# 28 — Agent Lifecycle

## Objetivo

Definir o ciclo de vida dos agentes no Capital Cipher AI.

Agentes devem ser registrados, inicializados, monitorados, executados, avaliados e desativados de forma controlada.

## Princípio central

Agente não é uma função solta. Agente é um componente com contrato, estado, responsabilidade, métricas e governança.

## Estados do agente

```text
REGISTERED
INITIALIZING
READY
RUNNING
COMPLETED
FAILED
TIMEOUT
DISABLED
DEGRADED
REMOVED
```

## REGISTERED

Agente cadastrado no sistema, mas ainda não inicializado.

## INITIALIZING

Agente carregando configurações e dependências.

## READY

Agente pronto para receber trabalho.

## RUNNING

Agente processando uma solicitação.

## COMPLETED

Agente concluiu execução com sucesso.

## FAILED

Agente falhou durante execução.

## TIMEOUT

Agente excedeu tempo máximo de resposta.

## DISABLED

Agente desabilitado manualmente ou por configuração.

## DEGRADED

Agente ainda opera, mas com problemas detectados.

## REMOVED

Agente removido do sistema.

## Registro de agente

Cada agente deve declarar:

```json
{
  "agent_name": "QuantAgent",
  "version": "1.0.0",
  "description": "Performs quantitative technical analysis",
  "required_inputs": [],
  "output_contract": "AgentOutputV1",
  "critical": true,
  "timeout_ms": 5000,
  "enabled": true
}
```

## Inicialização

Durante inicialização, o sistema deve validar:

- contrato;
- dependências;
- configurações;
- timeout;
- status enabled/disabled;
- versão.

## Execução

Toda execução deve receber:

- request_id;
- correlation_id;
- input validado;
- configuração ativa;
- timestamp.

Toda execução deve retornar output padronizado.

## Timeout

Cada agente deve possuir timeout configurável.

Se timeout ocorrer:

- registrar evento AGENT_TIMEOUT;
- marcar execução como TIMEOUT;
- notificar Orchestrator;
- avaliar se decisão pode continuar.

Agentes críticos em timeout normalmente devem bloquear decisão.

## Falha

Se agente falhar:

- registrar stack/error sanitizado;
- preservar correlation_id;
- retornar status FAILED;
- não derrubar sistema inteiro;
- acionar alerta se for agente crítico.

## Desabilitação

Agente pode ser desabilitado por:

- configuração;
- falhas recorrentes;
- decisão operacional;
- fase do projeto;
- incompatibilidade de contrato.

## Health check de agente

Cada agente deve expor:

- status;
- última execução;
- última falha;
- latência média;
- taxa de erro;
- versão.

## Versionamento

Agentes devem ser versionados.

Exemplo:

```text
QuantAgent v1.0.0
QuantAgent v1.1.0
```

Mudança de contrato exige nova versão.

## Agentes críticos da Fase 1

```text
MarketDataAgent
QuantAgent
TrendAgent
RiskAgent
AuditAgent
```

## Agentes não críticos da Fase 1

```text
PaperTradingAgent
DashboardStreamAgent
```

## Promoção de agente

Um agente novo deve passar por:

1. implementação isolada;
2. testes de contrato;
3. backtest;
4. paper trading;
5. avaliação;
6. ativação gradual.

## Remoção de agente

Ao remover agente:

- manter histórico;
- registrar motivo;
- não apagar outputs antigos;
- atualizar documentação.

## Proibições

- Não executar agente sem contrato.
- Não ignorar timeout de agente crítico.
- Não permitir agente escrever ordem real.
- Não permitir agente alterar próprio peso sem governança.
- Não apagar histórico do agente.

## Critério de sucesso

O lifecycle estará correto quando qualquer agente puder ser adicionado, desabilitado, monitorado ou substituído sem quebrar o sistema.
