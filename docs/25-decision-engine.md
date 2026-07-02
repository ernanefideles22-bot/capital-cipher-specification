# 25 — Decision Engine

## Objetivo

Definir o motor responsável por transformar outputs de agentes em uma decisão candidata.

O Decision Engine não executa ordens. Ele apenas consolida evidências e gera uma proposta para o Risk Management.

## Princípio central

Decisão não deve ser baseada em votação simples.

O sistema deve usar hierarquia, pesos, contexto, qualidade histórica dos agentes e veto de risco.

## Entrada

O Decision Engine recebe:

- evento de mercado;
- outputs dos agentes;
- regime de mercado;
- memória operacional;
- configurações de estratégia;
- estado do sistema.

## Saída

```json
{
  "decision_id": "uuid",
  "correlation_id": "uuid",
  "symbol": "BTCUSDT",
  "timeframe": "15m",
  "candidate_action": "BUY",
  "confidence": 82,
  "strategy": "SCALP_15M",
  "reason": "Quant, trend and volume aligned under acceptable volatility",
  "agent_summary": [],
  "warnings": [],
  "risk_status": "PENDING"
}
```

## Ações candidatas

```text
BUY
SELL
HOLD
WAIT
BLOCK
```

## Etapas de decisão

```text
1. Validar inputs
2. Remover outputs inválidos
3. Identificar agentes críticos ausentes
4. Classificar regime de mercado
5. Aplicar estratégia ativa
6. Consolidar sinais
7. Calcular confiança
8. Gerar warnings
9. Criar decisão candidata
10. Enviar para Risk Management
```

## Agentes críticos

Na Fase 1:

- Market Data Agent;
- Quant Agent;
- Trend Agent;
- Risk Agent.

O Risk Agent não participa da decisão candidata inicial. Ele valida depois.

## Pesos iniciais sugeridos

```text
Quant Agent: 40%
Trend Agent: 30%
Market Data Quality: 20%
Operational Context: 10%
```

Em fases futuras:

```text
Visual Agent
Liquidity Agent
Volume Agent
News Agent
Macro Agent
Sentiment Agent
```

## Regras de bloqueio antes do risco

Decision Engine pode gerar BLOCK quando:

- dados estão incompletos;
- agentes críticos falharam;
- sinais são contraditórios demais;
- mercado está indefinido;
- volatilidade está extrema;
- confidence está abaixo do mínimo.

## Confidence

Confidence deve ser calculada com base em:

- alinhamento de sinais;
- qualidade dos dados;
- força dos indicadores;
- regime de mercado;
- desempenho histórico do agente;
- quantidade de warnings.

## Confidence mínima

Configuração inicial sugerida:

```text
minimum_candidate_confidence: 70
minimum_execution_confidence_after_risk: 75
```

## Sinais contraditórios

Exemplo:

```text
Quant: BUY 85
Trend: SELL 80
Market Data: OK
```

Resultado esperado:

```text
WAIT ou BLOCK
```

Não transformar conflito forte em média neutra sem explicação.

## Warnings

Warnings possíveis:

```text
LOW_CONFIDENCE
AGENT_TIMEOUT
CONFLICTING_SIGNALS
HIGH_VOLATILITY
LOW_VOLUME
DATA_QUALITY_ISSUE
REGIME_UNCLEAR
```

## Auditoria

Toda decisão candidata deve registrar:

- inputs usados;
- agentes considerados;
- agentes ignorados;
- pesos aplicados;
- confidence final;
- motivo;
- warnings.

## Proibições

- Não executar ordem.
- Não ignorar falha crítica.
- Não transformar voto simples em decisão final.
- Não aceitar agente sem contrato válido.
- Não criar decisão sem correlation_id.

## Critério de sucesso

O Decision Engine estará aceitável quando produzir decisões candidatas coerentes, auditáveis e reproduzíveis a partir dos mesmos inputs.
