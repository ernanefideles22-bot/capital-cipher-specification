# 21 — Deployment

## Objetivo

Definir estratégia de implantação do Capital Cipher AI.

O sistema possui componentes com necessidades diferentes. Frontend, backend, workers, banco e observabilidade não devem ser tratados como uma única aplicação simples.

## Princípio central

Frontend pode ser serverless. Backend operacional 24/7 não deve depender exclusivamente de serverless.

## Ambientes

```text
local
dev
staging
production
```

## Local

Ambiente usado para desenvolvimento.

Deve permitir:

- rodar backend;
- rodar frontend;
- rodar testes;
- simular banco;
- simular market data quando necessário.

## Dev

Ambiente remoto para testes iniciais.

Pode usar:

- Vercel para frontend;
- Supabase para banco;
- VPS pequena para backend.

## Staging

Ambiente semelhante ao production, mas sem capital real.

Deve rodar em modo PAPER.

## Production

Ambiente futuro.

Só pode existir após:

- documentação completa;
- autenticação;
- observabilidade;
- backups;
- kill switch;
- plano de rollback;
- validação de risco.

## Componentes

```text
Frontend
Backend API
Market Data Worker
Agent Workers
Orchestrator
Database
Redis / Message Bus
Monitoring
```

## Frontend

Recomendado:

```text
Vercel
```

Responsável por:

- dashboard;
- gráficos;
- status;
- logs;
- controle operacional.

## Backend

Recomendado:

```text
VPS / container / serviço dedicado
```

Responsável por:

- FastAPI;
- WebSocket;
- Orchestrator;
- agentes;
- paper trading;
- integração com banco.

## Banco de dados

Opções iniciais:

- Supabase;
- PostgreSQL em VPS.

Supabase é aceitável para início, desde que segurança e permissões sejam bem configuradas.

## Docker

O projeto deve possuir Docker para padronizar ambiente.

Arquivos esperados na implementação futura:

```text
Dockerfile
Dockerfile.worker
docker-compose.yml
.env.example
```

## Variáveis de ambiente

Exemplo conceitual:

```text
APP_ENV=dev
SYSTEM_MODE=PAPER
DATABASE_URL=
REDIS_URL=
LOG_LEVEL=INFO
ALLOWED_SYMBOLS=BTCUSDT,ETHUSDT,SOLUSDT
DEFAULT_TIMEFRAME=15m
```

Na Fase 1, não incluir chaves privadas de corretora.

## CI/CD

GitHub Actions deve futuramente executar:

- lint;
- type check;
- testes;
- build;
- validação de documentação;
- verificação de secrets acidentais.

## Estratégia de deploy inicial

### Etapa 1

Frontend na Vercel.

### Etapa 2

Backend em VPS com Docker.

### Etapa 3

Banco no Supabase ou PostgreSQL.

### Etapa 4

Logs centralizados.

### Etapa 5

Monitoramento e alertas.

## Regras de produção futura

Produção deve ter:

- HTTPS;
- autenticação;
- backup;
- monitoring;
- rollback;
- secrets protegidos;
- firewall;
- logs;
- plano de incidentes.

## Rollback

Toda versão deve permitir retorno à versão anterior.

Não implantar mudança irreversível sem migration documentada.

## Backups

Banco deve possuir backup periódico em ambiente real.

Backups devem ser testados. Backup não testado é apenas hipótese.

## Proibições

- Não rodar backend 24/7 apenas em função serverless.
- Não expor banco diretamente ao frontend.
- Não colocar secrets no repositório.
- Não implantar LIVE sem plano formal.
- Não misturar ambiente dev com production.

## Critério de sucesso da implantação inicial

A implantação inicial estará aceitável quando:

- frontend estiver acessível;
- backend responder health check;
- banco conectar;
- market data funcionar;
- WebSocket interno operar;
- logs forem gerados;
- sistema estiver em PAPER.
