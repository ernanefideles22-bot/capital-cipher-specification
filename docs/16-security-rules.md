# 16 — Security Rules

## Objetivo

Definir regras de segurança obrigatórias para o Capital Cipher AI.

Este projeto lida com lógica de trading, dados financeiros e, em fases futuras, poderá interagir com corretoras. Por isso, segurança deve ser tratada como requisito central, não como detalhe posterior.

## Princípio central

A Fase 1 não pode conter execução real de ordens nem chaves privadas de corretora.

## Modos de operação

```text
OFFLINE
PAPER
LIVE_LOCKED
LIVE
```

### OFFLINE

Sistema parado ou sem processamento operacional.

### PAPER

Modo permitido na Fase 1.

Permite:

- receber dados públicos;
- simular decisões;
- simular ordens;
- registrar auditoria.

Não permite:

- execução real;
- alavancagem real;
- uso de API key privada.

### LIVE_LOCKED

Modo futuro em que integrações reais podem existir, mas ordens ficam bloqueadas por padrão.

### LIVE

Modo futuro. Só pode existir após documentação, testes, auditoria e autorização explícita.

## Secrets

Proibido armazenar secrets em:

- código-fonte;
- README;
- documentação pública;
- logs;
- banco de dados comum;
- frontend;
- commits.

Secrets devem ser armazenados em:

- variáveis de ambiente;
- secret manager;
- cofre apropriado;
- ambiente controlado de deploy.

## API Keys de corretora

Na Fase 1:

```text
API keys privadas são proibidas.
```

Quando forem permitidas em fase futura:

- usar permissões mínimas;
- começar com read-only;
- separar ambiente de teste;
- limitar saque/desativar withdrawal;
- restringir IP quando possível;
- rotacionar chaves;
- registrar uso;
- nunca expor no frontend.

## Autenticação

Endpoints sensíveis devem exigir autenticação.

Áreas sensíveis:

- configurações;
- risk;
- kill switch;
- paper trading;
- auditoria;
- agentes;
- modo operacional.

## Autorização

No futuro, permissões devem separar:

- viewer;
- operator;
- risk_manager;
- admin.

Apenas perfil autorizado pode alterar risco ou modo operacional.

## Kill Switch

O kill switch deve:

- estar disponível via dashboard;
- estar disponível via API protegida;
- bloquear novas decisões;
- bloquear paper orders;
- registrar auditoria;
- exibir estado no dashboard.

## Logs

Logs não podem conter:

- API keys;
- tokens;
- senhas;
- dados sensíveis de conta;
- payloads privados sem sanitização.

Logs devem conter:

- correlation_id;
- event_type;
- service;
- severity;
- timestamp;
- mensagem objetiva.

## Auditoria

Eventos críticos devem ser auditados:

- mudança de modo;
- kill switch;
- alteração de limites;
- decisão candidata;
- risk check;
- criação de paper order;
- falha de agente;
- falha de dados;
- falha de banco;
- erro de autenticação.

## Falha segura

O sistema deve preferir bloquear operação em caso de dúvida.

Condições que exigem bloqueio:

- falha de auditoria;
- falha de risk check;
- dados incompletos;
- latência excessiva;
- exchange desconectada;
- agente crítico em falha;
- modo incompatível;
- kill switch ativo.

## Segurança do frontend

O frontend não deve conter:

- secrets;
- lógica crítica de risco;
- autorização apenas visual;
- endpoints privados expostos sem autenticação.

O frontend apenas solicita e exibe informações. Decisões críticas pertencem ao backend.

## Segurança de banco

Regras:

- usar migrations;
- evitar permissões amplas;
- aplicar backups;
- auditar alterações críticas;
- proteger conexão com SSL quando possível.

## Deploy

Ambientes separados:

```text
local
dev
staging
production
```

A Fase 1 pode usar local/dev/staging.

Produção com capacidade real exige documentação própria.

## Regra final

Qualquer implementação que permita execução financeira real antes de autorização formal deve ser considerada falha crítica de segurança e deve ser revertida.
