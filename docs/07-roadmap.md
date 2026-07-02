# 07 — Roadmap

## Fase 0 — Documentação

Objetivo: criar a fonte oficial de verdade do projeto.

Entregáveis:

- README;
- visão do projeto;
- arquitetura geral;
- organograma multiagente;
- especificação do Orchestrator;
- agentes da Fase 1;
- gerenciamento de risco;
- roadmap.

Status: em andamento.

---

## Fase 1 — MVP técnico sem dinheiro real

Objetivo: validar arquitetura e funcionamento básico.

Entregáveis:

- backend FastAPI;
- conexão WebSocket pública com Binance/Bybit;
- Market Data Agent;
- Quant Agent;
- Trend Agent;
- Risk Agent;
- Paper Trading Agent;
- Audit Agent;
- banco de dados;
- dashboard simples;
- logs estruturados;
- testes básicos.

Critério de sucesso:

- sistema rodando sem dinheiro real;
- recebendo candles em tempo real;
- gerando decisões simuladas;
- registrando auditoria completa;
- sem execução real.

---

## Fase 2 — Paper trading robusto

Objetivo: simular operação com mais realismo.

Entregáveis:

- slippage estimado;
- taxas simuladas;
- múltiplos ativos;
- múltiplos timeframes;
- replay de mercado;
- relatórios de performance;
- controle de drawdown;
- dashboard operacional.

Critério de sucesso:

- operação simulada por semanas;
- logs consistentes;
- métricas confiáveis;
- estratégias avaliadas fora da amostra.

---

## Fase 3 — Agentes avançados

Objetivo: adicionar inteligência contextual.

Entregáveis:

- Visual Chart Agent;
- Liquidity Agent;
- News Agent;
- Macro Agent;
- Sentiment Agent;
- Portfolio Agent.

Critério de sucesso:

- agentes com contratos estáveis;
- respostas auditáveis;
- melhoria comprovada sobre baseline quantitativo.

---

## Fase 4 — Live locked

Objetivo: preparar execução real sem liberar autonomia plena.

Entregáveis:

- integração com API privada;
- cofre de secrets;
- permissões reduzidas;
- ordens bloqueadas por padrão;
- confirmação humana;
- limites rígidos de capital.

Critério de sucesso:

- sistema tecnicamente pronto para operar;
- sem autonomia total;
- execução real apenas com autorização explícita.

---

## Fase 5 — Live controlado

Objetivo: operar com capital pequeno sob limites severos.

Condições obrigatórias:

- meses de paper trading positivo;
- auditoria confiável;
- kill switch testado;
- latência aceitável;
- risco conservador;
- capital inicial baixo.

## Regra estratégica

O sistema só deve avançar de fase quando a fase anterior estiver validada por evidências, não por impressão subjetiva.
