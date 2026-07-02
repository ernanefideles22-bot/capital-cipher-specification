# Capital Cipher AI

## Software Architecture Specification (SAS)

Repositório oficial de documentação do **Capital Cipher AI**.

O objetivo deste repositório é manter a especificação técnica, arquitetural e operacional de uma plataforma institucional de trading algorítmico baseada em arquitetura multiagente.

## Objetivo do projeto

Construir uma plataforma capaz de:

- consumir dados de mercado em tempo real;
- coordenar agentes especializados;
- combinar análise quantitativa, visual e contextual;
- executar paper trading;
- validar risco antes de qualquer decisão;
- registrar auditoria completa de todas as análises;
- evoluir futuramente para execução real controlada.

## Regra central

> Nenhum agente individual pode executar ordens ou ignorar o sistema de risco.

Toda decisão deve passar por orquestração, validação de risco e auditoria.

## Estrutura da documentação

```text
/docs
  01-visao-do-projeto.md
  02-arquitetura-geral.md
  03-organograma-multiagente.md
  04-orchestrator.md
  05-agentes-fase-1.md
  06-risk-management.md
  07-roadmap.md
```

## Status

Versão inicial da documentação: **SAS v1.0 — Working Draft**.

Este repositório deve ser tratado como a fonte oficial de verdade para Claude Coworker, GPT, desenvolvedores humanos e futuras automações do projeto.
