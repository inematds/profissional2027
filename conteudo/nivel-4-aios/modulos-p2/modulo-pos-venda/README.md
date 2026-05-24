# Módulo 7 — Pós-venda Automatizado (NPS + Feedback + Retenção)

**Versão:** 0.1 (especificação P2) — maio/2026
**Categoria:** P2 — especificação para V2 da biblioteca
**Pré-requisitos AIOS:** Módulo 1 (Atendimento WhatsApp) em produção ≥ 60 dias.

---

## O que faz

Conduz a **jornada de pós-venda** por WhatsApp: envia pesquisa NPS em momento certo, captura feedback estruturado e não-estruturado, identifica clientes em risco de churn, dispara ações de retenção (oferta personalizada, contato humano), e alimenta o CRM com tags acionáveis.

Cobre cinco operações:

1. **Disparo de NPS** em D+7 após compra/serviço.
2. **Captura de resposta** (nota 0-10 + comentário aberto).
3. **Classificação automática** do feedback (detrator, neutro, promotor; tema do comentário).
4. **Ação de retenção** para detratores (escalonamento humano em até 24h).
5. **Push para CRM** com tag e score atualizados.

---

## Quando usar

- Cliente PME com **fluxo de compra ou serviço recorrente** (≥ 50 transações/mês).
- Já tem o **Módulo 1 (Atendimento WhatsApp)** em produção — pós-venda extende a relação existente.
- CRM ativo (RD Station, Pipedrive, HubSpot) com possibilidade de receber tags por integração.
- Cliente PME se compromete a **agir sobre detratores** em até 24h. Sem ação, NPS vira teatro.
- Verticais ideais: **e-commerce, serviço recorrente (clínica, consultoria, agência), varejo médio, distribuidora B2B**.

---

## Stack utilizada

- WhatsApp Cloud API.
- n8n (cron diário identifica clientes elegíveis para NPS).
- Claude Haiku 4.5 para classificação de comentário.
- CRM via API.
- Supabase para histórico de NPS e cohort analysis.

---

## Arquitetura sumária

```
Cron diário identifica compras de D-7
       │
       ▼
Disparar template Meta NPS (1 mensagem por cliente)
       │
       ▼
Capturar resposta (nota 0-10 + comentário opcional)
       │
       ▼
[Classificador Haiku] detrator/neutro/promotor + tema do comentário
       │
       ├── Detrator → escalar humano em 24h + tag no CRM
       ├── Neutro → tag no CRM, sem ação imediata
       └── Promotor → pedir referral/depoimento + tag no CRM
       │
       ▼
Push para CRM + Supabase
```

---

## Nível HITL recomendado

| Operação | Nível inicial |
|---|---|
| Disparo de pesquisa NPS | 4 (template Meta padronizado) |
| Captura e classificação | 4 |
| Ação automática promotor (pedido de referral) | 3 (com gates) |
| **Ação detrator** | **2 — humano sempre contata** |
| Update CRM | 4 |

---

## Tempo estimado de construção (para V2)

| Fase | Dias úteis |
|---|---|
| D | 5–8 |
| P (templates Meta + integração CRM + RIPD) | 8–12 |
| I | 7–10 |
| A | 7–14 |
| **Total** | **27–44 dias** |

---

## Pré-requisitos

- **Módulo 1 (Atendimento WhatsApp)** em produção — compartilha número e infra.
- **Integração CRM** estável (RD Station, Pipedrive ou HubSpot).
- **Política de retenção definida** (o que fazer com detratores — desconto? Visita do dono? Pedido de desculpa formal?).
- **Champion no comercial/sucesso do cliente** disponível para tratar detratores em 24h.

---

## Notas para V2

- Definir templates Meta diferenciados por vertical (varejo curto, B2B mais formal).
- Adicionar **detecção de risco de churn** com base em padrão de uso (não apenas NPS) — exige tracking adicional.
- Considerar integração com Asaas/Conta Azul para detectar redução de uso recorrente como sinal de churn.

---

*Especificação Módulo Pós-venda v0.1 — AIOS Profissional 2027.*
