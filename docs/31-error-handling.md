# 31 — Error Handling

## Objetivo

Definir como erros devem ser classificados, tratados, registrados e propagados no Capital Cipher AI.

O sistema deve falhar de forma segura.

## Princípio central

Na dúvida, bloquear operação.

É melhor perder uma oportunidade simulada do que permitir uma decisão baseada em dados ruins, risco inválido ou auditoria quebrada.

## Classes de erro

```text
ValidationError
DataQualityError
MarketDataError
AgentError
AgentTimeoutError
RiskError
AuditError
DatabaseError
ConfigurationError
SecurityError
SystemStateError
ExternalServiceError
```

## ValidationError

Erro de formato, schema ou payload inválido.

Ação:

- rejeitar payload;
- registrar warning ou error;
- não continuar cadeia crítica.

## DataQualityError

Dados de mercado incompletos, duplicados, atrasados ou inconsistentes.

Ação:

- bloquear decisão se crítico;
- marcar evento como suspeito;
- registrar auditoria.

## MarketDataError

Falha na conexão ou coleta de dados.

Ação:

- tentar reconectar;
- alertar dashboard;
- bloquear se dados forem necessários para decisão.

## AgentError

Erro durante execução de agente.

Ação:

- registrar output FAILED;
- preservar correlation_id;
- continuar apenas se agente não for crítico.

## AgentTimeoutError

Agente excedeu tempo máximo.

Ação:

- registrar TIMEOUT;
- notificar Orchestrator;
- bloquear se agente for crítico.

## RiskError

Falha no Risk Management.

Ação:

- bloquear decisão;
- acionar alerta;
- impedir paper order;
- considerar estado ERROR se persistente.

## AuditError

Falha em auditoria.

Ação:

- bloquear decisão crítica;
- impedir paper order;
- colocar sistema em estado seguro se necessário.

## DatabaseError

Falha de persistência.

Ação:

- retry limitado;
- registrar em log local se possível;
- bloquear operações críticas.

## ConfigurationError

Configuração ausente ou inválida.

Ação:

- impedir inicialização;
- retornar erro claro;
- não assumir valores perigosos.

## SecurityError

Violação de regra de segurança.

Exemplos:

- secret em local proibido;
- tentativa de LIVE na Fase 1;
- endpoint sensível sem autenticação;
- execução real não autorizada.

Ação:

- bloquear;
- auditar;
- alertar;
- exigir correção manual.

## SystemStateError

Tentativa de ação incompatível com o estado atual.

Exemplo:

```text
Criar paper order em OFFLINE.
```

Ação:

- rejeitar;
- registrar;
- manter estado seguro.

## Estratégias de retry

Usar retry apenas para erros recuperáveis.

Permitido:

- reconexão com exchange;
- falha temporária de rede;
- timeout transitório de serviço externo.

Proibido:

- repetir execução de ordem sem idempotência;
- repetir evento crítico sem controle;
- retry infinito;
- retry silencioso.

## Retry policy inicial

```text
max_retries: 3
backoff: exponential
jitter: true
```

## Fail-fast

Usar fail-fast quando:

- configuração estiver inválida;
- contrato estiver quebrado;
- modo for proibido;
- schema for inválido.

## Fail-safe

Usar fail-safe quando:

- risco falhar;
- auditoria falhar;
- dados forem suspeitos;
- estado estiver indefinido.

Fail-safe significa bloquear operação e preservar capital/simulação.

## Formato padrão de erro

```json
{
  "error_code": "AGENT_TIMEOUT",
  "message": "QuantAgent exceeded timeout",
  "severity": "ERROR",
  "correlation_id": "uuid",
  "recoverable": true,
  "metadata": {}
}
```

## Severidades

```text
INFO
WARNING
ERROR
CRITICAL
```

## Erros críticos

Devem acionar bloqueio:

- RiskError;
- AuditError;
- DatabaseError em evento crítico;
- SecurityError;
- SystemStateError crítico;
- MarketDataError persistente.

## Proibições

- Não usar `except Exception: pass`.
- Não esconder erro crítico.
- Não continuar decisão sem risk check.
- Não criar paper order após falha de auditoria.
- Não fazer retry infinito.
- Não registrar secrets em erro.

## Critério de sucesso

O tratamento de erros estará correto quando toda falha relevante tiver resposta previsível, auditável e segura.
