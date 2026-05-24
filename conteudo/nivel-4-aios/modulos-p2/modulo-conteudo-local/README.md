# Módulo 9 — Máquina de Conteúdo Local

**Versão:** 0.1 (especificação P2) — maio/2026
**Categoria:** P2 — especificação para V2 da biblioteca
**Pré-requisitos AIOS:** stack canônica + biblioteca de conteúdo prévio do cliente para tom.

---

## O que faz

Gera **fluxo de conteúdo local recorrente** para PME (varejo, serviço, restaurante, clínica): posts curtos para Instagram + mensagens-status para WhatsApp Business + atualizações para Google Meu Negócio. A partir de **inputs mínimos do dono** (foto da semana, novidade do dia, promoção curta), o agente gera variações editoriais consistentes em tom e identidade da marca.

Cobre:

1. **Captação de input** (formulário WhatsApp do dono — 1 foto + 1 frase).
2. **Geração** de 3-5 variações por canal (Instagram, WhatsApp status, Google).
3. **Sugestão de horários** ideais por canal.
4. **Agendamento** via integrações (Meta Business Suite, Google Business).
5. **Métricas semanais** consolidadas (alcance, engajamento, view de perfil).

---

## Quando usar

- PME com **presença em redes locais** (Instagram + WhatsApp + Google Maps) mas inconsistente — posta uma semana, some duas.
- Dono é o **criador único de conteúdo** e gasta 3-6 h/semana com isso.
- Existe **identidade visual mínima** (cores, logo, tipografia) já definida.
- Cliente PME aceita HITL nível 2 — dono revisa cada peça antes de publicar.

---

## Stack utilizada

- WhatsApp Cloud API (input do dono).
- n8n com integrações Meta Business Suite, Google My Business API.
- Claude Sonnet 4.6 para texto.
- Geração de imagem opcional via Stable Diffusion ou DALL-E (gerar variação de fundo, não pessoa).
- Banco de templates visuais (Canva API ou Figma exportado).
- Supabase para biblioteca + agenda editorial.

---

## Arquitetura sumária

```
Dono manda WhatsApp: foto + frase ("hoje tem mocotó")
       │
       ▼
[Agente Sonnet] gera:
   - 3 variações de legenda IG
   - 1 frase curta para WhatsApp status
   - 1 update para Google Meu Negócio
       │
       ▼
GATE HITL nível 2: dono aprova no painel WhatsApp
       │
       ▼
Agendamento automático:
   - IG: horário ideal por dia da semana
   - WhatsApp status: agora
   - Google: agora
       │
       ▼
Coleta de métricas semanal
```

---

## Nível HITL recomendado

| Operação | Nível inicial |
|---|---|
| Captação do input | 4 |
| Geração de variações | 4 |
| **Publicação em qualquer canal** | **2 — dono sempre aprova** |
| Métricas semanais | 4 |

---

## Tempo estimado de construção (para V2)

| Fase | Dias úteis |
|---|---|
| D | 5–8 |
| P (identidade + templates + integração com Meta e Google) | 10–18 (aprovações Meta + Google são gargalo) |
| I | 7–10 |
| A | 5–10 |
| **Total** | **27–46 dias** |

---

## Pré-requisitos

- Identidade visual mínima documentada.
- Conta Meta Business + Google Business gerenciada pelo cliente.
- Histórico de conteúdo (30+ posts antigos) para tom no few-shot.
- Dono disponível em rotina diária (5-10 min/dia para input + aprovação).

---

## Notas para V2

- Considerar geração visual sintética para PME sem foto profissional — cuidado com qualidade.
- Adicionar análise de concorrentes locais (1x/mês) para inspirar pauta.
- Integração com WhatsApp Status do dono (canal pessoal) — atenção LGPD para mensagens marketing.

---

*Especificação Módulo Conteúdo Local v0.1 — AIOS Profissional 2027.*
