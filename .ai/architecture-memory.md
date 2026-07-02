# Architecture Memory

## Decisões fixas

Este arquivo registra as decisões arquiteturais que qualquer IA deve preservar.

## 1. Arquitetura multiagente

O sistema é composto por agentes especializados coordenados por um Orchestrator.

Agentes não são livres para executar ações críticas.

## 2. Orchestrator central

O Orchestrator coordena eventos, agentes e decisões candidatas.

Ele não executa ordens e não ignora Risk Management.

## 3. Risk-first

Risk Management tem poder de veto absoluto.

Mesmo que todos os agentes recomendem BUY ou SELL, o risco pode bloquear.

## 4. Paper-first

A Fase 1 opera exclusivamente em PAPER.

Live trading é fase futura e bloqueada.

## 5. Event-driven

Eventos com correlation_id conectam a cadeia completa.

Fluxo desejado:

```text
CANDLE_CLOSED
  → AGENT_OUTPUT
  → DECISION
  → RISK_CHECK
  → PAPER_ORDER
  → AUDIT_LOG
```

## 6. Contract-first

JSON Schemas em `/contracts` são a base para:

- Pydantic;
- TypeScript types;
- validação;
- testes de contrato.

## 7. Auditoria obrigatória

Toda decisão crítica precisa ser rastreável.

Se auditoria falhar, operações devem ser bloqueadas.

## 8. Backend 24/7 fora da Vercel

Frontend pode usar Vercel.

Backend operacional, WebSocket e agentes 24/7 devem rodar em VPS/container/infra dedicada.

## 9. Market Data separado de Execution

Market Data Adapters coletam dados públicos.

Exchange Adapters futuros lidam com conta/ordens.

Não misturar os dois.

## 10. LIVE proibido na Fase 1

Qualquer implementação que permita ordem real na Fase 1 deve ser rejeitada.
