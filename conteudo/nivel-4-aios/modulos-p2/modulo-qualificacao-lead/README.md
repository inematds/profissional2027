# Módulo 8 — Qualificação de Lead Inbound

**Versão:** 0.1 (especificação P2) — maio/2026
**Categoria:** P2 — especificação para V2 da biblioteca
**Pré-requisitos AIOS:** stack canônica + Módulo 1 (Atendimento WhatsApp) dominado.

---

## O que faz

Recebe **leads inbound** (formulário do site, anúncio Meta/Google, indicação WhatsApp) e conduz **qualificação inicial** antes de passar para o vendedor humano. Aplica framework BANT/SPIN simplificado adaptado a PME brasileira.

Cobre:

1. **Recepção do lead** (multi-canal).
2. **Saudação personalizada** com tom da empresa.
3. **3-5 perguntas-chave** para qualificar (dor, contexto, prazo, orçamento aproximado, decisor).
4. **Classificação** (lead frio, morno, quente) com score numérico.
5. **Roteamento** para vendedor certo (por nicho, território, ou rotatividade).
6. **Resgate de leads frios** com cadência educativa em 30/60/90 dias.

---

## Quando usar

- Cliente PME recebe **≥ 20 leads inbound/mês** com qualidade variável.
- Vendedores gastam tempo significativo em leads que não fecham nunca (sintoma: "tô cansado de atender quem nunca compra").
- Existe **definição clara de ICP** (perfil de cliente ideal) — sem ICP, não há qualificação possível.
- CRM ativo para receber leads classificados.
- Verticais ideais: **imobiliária, B2B com ticket médio R$ 5k-100k, serviço profissional (jurídico, contábil, agência)**.

---

## Stack utilizada

- WhatsApp Cloud API + integração com formulário do site (Webflow, WordPress, Lovable).
- n8n com routing inicial.
- Claude Sonnet 4.6 para condução de qualificação (Sonnet, não Haiku — a qualidade da pergunta-resposta determina conversão).
- CRM (RD Station, Pipedrive).
- Supabase para histórico de leads + cadência de resgate.

---

## Arquitetura sumária

```
Lead chega (formulário, anúncio, WhatsApp direto)
       │
       ▼
[Detectar canal de origem] → atribuição
       │
       ▼
[Agente Sonnet] conduz qualificação em 3-5 turnos
   - Saudação + identificação
   - Pergunta 1: dor declarada
   - Pergunta 2: contexto (porte, setor)
   - Pergunta 3: prazo + orçamento
   - Pergunta 4: decisor presente
       │
       ▼
[Classificador] score 0-100 + tag (frio/morno/quente)
       │
       ├── Quente (≥70) → push CRM + notifica vendedor em 5 min
       ├── Morno (40-69) → push CRM + agendar contato em 24h
       └── Frio (<40) → cadência educativa 30/60/90d
```

---

## Nível HITL recomendado

| Operação | Nível inicial |
|---|---|
| Saudação e perguntas iniciais | 3 (gates: pergunta confusa → escala) |
| Classificação | 4 |
| Roteamento ao vendedor | 4 |
| Resgate de lead frio | 4 (cadência automatizada) |
| **Conversa com lead quente >R$ 50k** | **2 — vendedor entra antes** |

---

## Tempo estimado de construção (para V2)

| Fase | Dias úteis |
|---|---|
| D | 7–10 (ICP precisa estar fechado!) |
| P | 10–14 (templates + integração CRM + cadência de resgate) |
| I | 10–14 |
| A | 7–14 |
| **Total** | **34–52 dias** |

---

## Pré-requisitos

- **ICP documentado pelo cliente PME** — sem ICP, módulo é palpite.
- **Módulo 1 dominado** pelo implementador (compartilha lógica de WhatsApp + tools).
- **CRM operante** com pipeline definido.
- **Política de SLA** por classificação (quente em 5min, morno em 24h, frio em 30/60/90d).

---

## Notas para V2

- Definir cadência de resgate por vertical (imobiliária tem ciclo longo; serviço tem ciclo curto).
- Considerar análise de fonte do lead (qualidade média por canal) para feedback ao marketing.
- Detectar lead duplicado (mesmo CPF/CNPJ via canais diferentes).

---

*Especificação Módulo Qualificação de Lead v0.1 — AIOS Profissional 2027.*
