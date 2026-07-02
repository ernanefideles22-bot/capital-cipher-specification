# Engineering Principles

## Prioridade absoluta

A ordem de prioridade do Capital Cipher AI é:

```text
1. Segurança
2. Correção
3. Auditoria
4. Arquitetura
5. Testabilidade
6. Performance
7. Velocidade
```

Nunca inverter essa ordem.

## Regra de risco

Risk Management manda em todos.

Nenhum agente, endpoint, dashboard ou executor pode ignorar o risco.

## Regra de execução

Na Fase 1:

```text
execução real é proibida
```

Apenas paper trading é permitido.

## Contratos primeiro

Antes de implementar uma estrutura de dados, verificar se existe contrato em `/contracts`.

Se não existir, propor contrato antes de implementar.

## Documentação primeiro

Se uma decisão arquitetural nova for tomada, atualizar `/docs` ou criar ADR.

## Baixo acoplamento

Agentes não devem chamar outros agentes diretamente.

Comunicação deve passar por Orchestrator, Message Bus ou serviços explícitos.

## Fail-safe

Em caso de dúvida:

- bloquear decisão;
- registrar auditoria;
- alertar sistema;
- não prosseguir silenciosamente.

## Observabilidade obrigatória

Toda decisão crítica deve ter:

- logs;
- correlation_id;
- audit trail;
- timestamps;
- razão explícita.

## Testes obrigatórios

Módulos críticos exigem testes:

- risk;
- orchestrator;
- decision engine;
- paper trading;
- contracts;
- market data.

## Não fazer

- Não criar monolito sem fronteiras.
- Não misturar frontend com regra de trading.
- Não colocar secrets no código.
- Não criar LIVE na Fase 1.
- Não esconder erros.
- Não criar lógica financeira sem teste.
