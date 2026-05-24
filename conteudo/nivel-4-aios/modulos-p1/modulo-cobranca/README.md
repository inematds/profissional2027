# Módulo 4 — Cobrança Ativa por WhatsApp

**Versão:** 1.0 — maio/2026
**Categoria:** P1 — Módulo Financeiro / Relacional
**Pré-requisitos AIOS:** stack canônica + integração com financeiro do cliente (Asaas, Conta Azul, Omie, Bling Financeiro).

---

## O que faz

Conduz a **régua de cobrança ativa** de inadimplência por WhatsApp do cliente PME, com tom adequado ao perfil do devedor, registro de promessas de pagamento, geração de Pix, e escalonamento humano para casos graves ou de relacionamento sensível.

Cobre:

- Identificação dos inadimplentes do mês (importação do ERP financeiro).
- Classificação de perfil do devedor (cooperativo, esquivo, hostil — voting de 3 chamadas Haiku).
- Composição de mensagem por perfil (cordial, formal, firme).
- Disparo agendado da mensagem (co-piloto sempre na V1).
- Captura de resposta: promessa, recusa, contestação, silêncio.
- Geração de Pix Asaas ou link de boleto após resposta positiva.
- Registro de promessa de pagamento com prazo.
- Follow-up agendado (D+3, D+7, D+15).
- Escalonamento para humano em casos definidos.

Aplica padrão **Prompt Chaining** + **Voting** (cf. `02-padroes-de-design.md` §1 e §3).

---

## Quando usar

- Cliente PME com **carteira de devedores ≥ 15 inadimplentes/mês**.
- Cobrança atual feita por humano gasta ≥ 4 h/semana e tem **inconsistência de tom** (ora cordial, ora agressivo) ou cobertura incompleta (não chega a todos).
- Existe **integração financeira ativa** (Asaas, Conta Azul, Omie ou Bling Financeiro) com lista atualizada de inadimplentes.
- Política de cobrança documentada (régua: quando enviar 1ª, 2ª, 3ª cobrança; quando suspender serviço).
- Cliente PME aceita **co-piloto humano** na cobrança — automação total não é recomendada em V1.
- Verticais ideais: **contábil, jurídico, agência, prestador de serviço recorrente, distribuidora com fatura, clínica com pacote**.

---

## Quando NÃO usar

- Cobrança envolve **alto valor por inadimplente** (> R$ 20.000) ou litígio judicial em curso. Caminho é jurídico, não automação.
- Cliente PME sem **política de cobrança definida**. Antes, fase P precisa formalizar a régua.
- Inadimplência se origina de **falha do próprio cliente PME** (entrega atrasada, serviço com defeito). Resolver causa raiz antes de cobrar.
- Volume < 15 devedores/mês. Manutenção não fecha.
- Cliente quer cobrar de forma "automática e firme" sem revisão humana. Risco reputacional/regulatório (CDC) alto. Recuse.
- Carteira com perfil predominantemente **B2C de baixíssima renda**. Cobrança via WhatsApp sem cuidado pode virar caso de CDC.

---

## Resultado esperado

| Métrica | Baseline típico | Após 90 dias | Após 180 dias |
|---|---|---|---|
| % inadimplentes contactados no ciclo | 50–80% | 95–100% | 95–100% |
| Tempo da equipe em cobrança (h/semana) | 4–10 h | 1–2 h | 0,5–1,5 h |
| Taxa de recuperação no D+30 | 25–40% | 45–65% | 55–75% |
| Tempo médio de recuperação (dias) | 25–45 | 12–22 | 8–18 |
| Reclamação por tom inadequado | 1–3/mês | < 1/mês | 0–1/trimestre |

**Payback típico:** 1,5 a 3 meses (cobrança ativa eficaz tem ROI alto e rápido).

---

## Stack utilizada

- **WhatsApp Cloud API** (templates de cobrança aprovados pela Meta — categoria utilidade).
- **n8n** — orquestração com cron diário para identificar inadimplentes do dia.
- **Claude Haiku 4.5** para classificação de perfil (voting 3x) e mensagens curtas.
- **Claude Sonnet 4.6** para composição de mensagens sensíveis e tratamento de respostas complexas.
- **Asaas** ou **Conta Azul** para geração de Pix/boleto e atualização de status.
- **Supabase** para histórico de cobrança, promessas, log.

---

## Arquitetura

```
   Cron diário 9h
        │
        ▼
[Importar inadimplentes do ERP financeiro]
        │
        ▼
[Filtrar pela régua: 1ª cobrança = D+3 após vencimento;
 2ª = D+10; 3ª = D+20; jurídico = D+30+]
        │
        ▼
[Voting Haiku 3x] classifica perfil do devedor
   (cooperativo, esquivo, hostil) baseado em
   histórico de pagamento e respostas anteriores
        │
        ▼
[Compor mensagem por perfil + posição na régua]
   (Sonnet)
        │
        ▼
[GATE HITL nível 2] champion vê lote diário,
   aprova / edita / pula mensagens
        │
        ▼
[Disparar via WhatsApp Meta]
        │
        ▼
[Aguardar resposta — janela 72h]
        │
        ▼
   ┌────────┬────────────┬─────────────┐
   ▼        ▼            ▼             ▼
"pago"   "vou pagar"   "não pago"   silêncio
   │        │            │             │
   │        ▼            ▼             ▼
   │   [registrar     [escalar    [aguardar
   │    promessa +     champion]   próximo passo
   │    gerar Pix]                  da régua]
   │
   ▼
[atualizar ERP financeiro]
```

---

## Nível HITL recomendado

| Operação | Nível inicial | Pode subir a |
|---|---|---|
| Classificação de perfil | 4 (auditoria semanal) | — |
| Composição de mensagem | 4 | — (apenas a composição é autônoma) |
| **Envio da mensagem ao devedor** | **2 (co-piloto sempre nos primeiros 30 dias)** | 3 após 60 dias estáveis, com gates rígidos |
| Geração de Pix após promessa | 2 | 3 após 30 dias |
| Atualização de status no ERP | 3 | 4 após 60 dias |
| Escalonamento em hostilidade | 1 (assistir, escala imediato) | — |

**Gates de exceção sempre ativos (mesmo em nível 3):**
- Valor devido > R$ {{TETO_COBRANCA}} (default R$ 5.000).
- Devedor é cliente VIP ou estratégico.
- Histórico de litígio anterior.
- Resposta contém: "advogado", "Procon", "consumidor", "processar", "indevido", "abusivo".
- Conta vencida há mais de 60 dias.
- Devedor pessoa física com sinais de vulnerabilidade (idoso, deficiente, baixa renda).

**Nível 4 NUNCA** para envio ao devedor. Cobrança autônoma é risco regulatório (CDC, LGPD) e relacional.

---

## Custo operacional estimado (R$/mês)

**Cliente pequeno — 20 inadimplentes/mês, ~80 mensagens incluindo follow-ups:**

| Item | Mensal |
|---|---|
| Meta WABA (templates utilidade) | R$ 30–80 |
| Claude API (Haiku voting + Sonnet ocasional) | R$ 40–90 |
| Asaas (Pix e boleto) | R$ variável (cliente paga) |
| Rateio infra | R$ 30–80 |
| **Total** | **R$ 100–250** |

**Cliente médio — 100 inadimplentes/mês:**

| Item | Mensal |
|---|---|
| Meta WABA | R$ 150–400 |
| Claude API | R$ 120–280 |
| Rateio infra | R$ 50–100 |
| **Total** | **R$ 320–780** |

---

## Tempo de implementação

| Fase | Dias úteis |
|---|---|
| D | 7–10 (política de cobrança precisa estar clara) |
| P | 7–14 (templates Meta + régua + integração financeira) |
| I | 10–14 |
| A | 10–14 |
| **Total** | **34–52 dias úteis** |

Gargalo: aprovação Meta dos templates de cobrança (categoria "utilidade") leva 3–7 dias.

---

## Métricas obrigatórias

1. **Taxa de cobertura da régua** (% inadimplentes contactados no dia certo).
2. **Taxa de recuperação D+30** (% que pagaram em até 30 dias após primeira cobrança).
3. **Tempo médio de recuperação** (dias da primeira cobrança ao pagamento).
4. **Promessas de pagamento cumpridas** (% das promessas que viraram pagamento real).
5. **Reclamações por tom** (formal, via canal próprio do cliente final).
6. **Tempo da equipe em cobrança** (horas/semana).
7. **% de escalonamento por hostilidade** (alvo < 8%).
8. **Custo total mensal.**

---

## Erros comuns e como evitar

- **Mensagem genérica para todos os perfis.** Resultado: cordial parece formal, hostil parece intimidado, esquivo perde gancho. *Corrige:* voting de classificação + 3 templates por nível da régua.
- **Cobrar autonomamente sem revisão.** Resultado: mensagem errada para cliente em pendência por erro do próprio cliente PME (já pagou e sistema não atualizou). *Corrige:* nível 2 nos primeiros 30 dias, sempre.
- **Tom hostil ou intimidador.** Risco CDC. *Corrige:* lista negra de palavras no prompt (Bloco 8); revisão obrigatória do tom no Bloco 4 do system prompt.
- **Cobrar em horário inadequado.** Resultado: irritação. *Corrige:* janela definida (9h-19h dias úteis, nada em domingo, nada em feriado).
- **Ignorar promessa de pagamento.** Resultado: cobrança redundante D+1, devedor abandona. *Corrige:* registro da promessa + supressão de cobrança até a data prometida + 3 dias.
- **Tratar contestação como resistência.** "Já paguei dia 15." Não cobre de novo até verificar com financeiro. *Corrige:* tool `verificar_pagamento_recente` antes da próxima cobrança.
- **Insistir após pedido de pausa.** "Estou doente, te respondo semana que vem." Respeitar. *Corrige:* prompt explícito para registrar pausa e suspender.

---

## Variações por vertical

### Contábil (Verbo)
- Cobrança de honorários mensais. Devedores são empresas, não pessoa física — tom formal, sem dramatização.
- Régua: D+5, D+15, D+25, suspensão de serviço D+40.
- Tool: integração Conta Azul ou Domínio Sistemas.
- Sensibilidade: cliente contábil é parceiro de longo prazo. Tom errado destrói relação.

### Clínica com pacote pré-pago
- Cobrança de parcelas pendentes ou taxa de no-show.
- Régua mais curta: D+3, D+10, D+20.
- Tool: Conta Azul + Asaas.
- Sensibilidade: dor da relação clínica + financeira. Tom acolhedor sempre.

### Distribuidora com faturamento a prazo
- Cobrança de boletos vencidos.
- Régua D+3, D+10, D+20, jurídico D+30.
- Tool: Bling Financeiro ou Asaas.
- Sensibilidade: cliente B2B com fluxo de caixa apertado é regra, não exceção. Aceitar parcelamento.

---

## Mapeamento para DPIA

| Fase | Pré-requisitos |
|---|---|
| D | Baseline de inadimplência, taxa de recuperação atual, tempo da equipe, reclamações por tom. Política de cobrança formal escrita. KPIs assinados. Champion (financeiro) decidido. |
| P | Templates Meta submetidos. Régua documentada por vertical. Integração financeira validada com chamada real. Lista negra de tom. Histórico de cobranças anteriores classificado por perfil. RIPD enxuto LGPD (dado financeiro de cliente final). |
| I | system-prompt.md + playbook.md. Matriz HITL com gates rígidos para CDC e relação. Homologação com 30 casos reais (≥ 85% acerto). Frase aprovada para "estou falando com robô?". |
| A | Deploy faseado: primeiro só classificação + composição em modo assistir (1 semana); depois envio nível 2 (4 semanas); depois nível 3 com gates após 60 dias de operação estável. |

---

*Módulo Cobrança v1.0 — AIOS Profissional 2027.*
