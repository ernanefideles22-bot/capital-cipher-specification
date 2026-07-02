# 22 — Testing Strategy

## Objetivo

Definir a estratégia de testes do Capital Cipher AI.

Testes são obrigatórios porque o sistema tomará decisões financeiras simuladas e, no futuro, poderá interagir com capital real.

## Princípio central

Código sem teste não pode controlar risco, orquestração ou execução.

## Tipos de teste

```text
Unit tests
Integration tests
Contract tests
Backtest validation
Simulation tests
Security tests
End-to-end tests
Regression tests
```

## Testes unitários

Devem cobrir:

- indicadores;
- regras de risco;
- cálculo de posição;
- cálculo de PnL;
- validação de schemas;
- decisão do Orchestrator;
- parsing de eventos.

## Testes de integração

Devem cobrir:

- API + banco;
- Orchestrator + agentes;
- Market Data + persistência;
- Risk + Paper Trading;
- Audit + decisões.

## Contract tests

Obrigatórios para agentes.

Cada agente deve respeitar:

- entrada definida;
- saída definida;
- status permitidos;
- sinais permitidos;
- confidence 0–100;
- campos obrigatórios.

## Testes de risco

Cenários obrigatórios:

- operação aprovada;
- operação reduzida;
- operação bloqueada;
- kill switch ativo;
- drawdown diário excedido;
- perdas consecutivas excedidas;
- dados ausentes;
- latência excessiva;
- falha de auditoria.

## Testes de paper trading

Cenários obrigatórios:

- criar ordem simulada;
- preencher ordem simulada;
- acionar stop loss;
- acionar take profit;
- calcular taxas;
- calcular slippage;
- calcular PnL;
- impedir ordem sem risk_check.

## Testes de backtesting

Devem verificar:

- ausência de lookahead bias;
- candles processados em ordem;
- indicadores calculados apenas com dados disponíveis;
- taxas aplicadas;
- slippage aplicado;
- métricas corretas.

## Testes de API

Endpoints devem testar:

- sucesso;
- validação;
- não autorizado;
- não encontrado;
- erro interno;
- resposta padronizada.

## Testes de segurança

Devem verificar:

- ausência de secrets no código;
- endpoints sensíveis protegidos;
- logs sem API keys;
- modo LIVE indisponível na Fase 1;
- execução real impossível na Fase 1.

## Testes end-to-end

Fluxo mínimo:

```text
Candle recebido
  → agentes analisam
  → decisão candidata criada
  → risco aprova
  → paper order criada
  → auditoria registrada
  → dashboard exibe resultado
```

## Testes de regressão

Toda correção de bug deve gerar teste que impeça retorno do erro.

## Ferramentas recomendadas

Backend:

- pytest;
- pytest-asyncio;
- httpx;
- factory-boy ou fixtures próprias.

Frontend:

- Vitest;
- React Testing Library;
- Playwright em fase futura.

## Cobertura mínima

Fase inicial:

```text
Core risk logic: 90%+
Agent contracts: 90%+
Orchestrator: 80%+
Paper trading: 80%+
API básica: 70%+
```

Cobertura alta não garante qualidade, mas cobertura baixa em módulo crítico é inaceitável.

## Dados de teste

Criar fixtures para:

- candles normais;
- candles de alta volatilidade;
- mercado lateral;
- tendência de alta;
- tendência de baixa;
- gaps;
- volume anormal;
- dados incompletos.

## Critério de merge

Nenhuma alteração crítica deve ser aceita se:

- testes falharem;
- contratos quebrarem;
- risk tests falharem;
- auditoria for removida;
- execução real for introduzida sem autorização.

## Regra final

O sistema deve ser construído para falhar de forma segura. Testes devem provar isso.
