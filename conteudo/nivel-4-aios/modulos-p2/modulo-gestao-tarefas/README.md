# Módulo 12 — Gestão de Tarefas (Pedidos Soltos → Tarefas Organizadas)

**Versão:** 0.1 (especificação P2) — maio/2026
**Categoria:** P2 — especificação para V2 da biblioteca
**Pré-requisitos AIOS:** stack canônica + Módulo 6 (Resumo de Reunião) dominado (compartilha lógica de extração).

---

## O que faz

Recebe **pedidos soltos** que chegam ao dono ou gestor por múltiplos canais (WhatsApp, e-mail, reunião informal, áudio gravado andando), organiza em **tarefas estruturadas**, prioriza por critério configurável (urgência × impacto), e mantém follow-up.

Cobre:

1. **Captura** de pedido via WhatsApp do dono (texto, áudio, encaminhamento).
2. **Extração estruturada** (o quê, para quem, prazo, contexto).
3. **Priorização** (matriz urgência × impacto + dependências).
4. **Push** para PM (Trello, Asana, Linear, Notion).
5. **Lembrete proativo** de tarefas próximas do prazo.
6. **Resumo diário** ao dono (agenda + tarefas críticas).

Aplica padrão **Prompt Chaining** (extração + priorização).

---

## Quando usar

- Dono/gestor de PME que **gerencia tudo de cabeça** — pedidos chegam por todo lado, atrasos viram regra.
- Time pequeno (3-15 pessoas) onde a organização ainda é informal.
- Existe **algum sistema de PM** ativo (Trello é o mais comum em PME) ou disposição de adotar Trello/Asana.
- Dono aceita usar WhatsApp como input — não vai migrar para sistema próprio.
- Verticais ideais: **agência, consultoria, escritório técnico, dono-operador de varejo, pequena software house**.

---

## Stack utilizada

- WhatsApp Cloud API (input do dono).
- Whisper (áudio → texto) ou serviço cloud equivalente.
- n8n.
- Claude Sonnet 4.6 para extração e priorização.
- API do sistema de PM (Trello, Asana, Linear, Notion).
- Supabase para histórico e métricas.

---

## Arquitetura sumária

```
Dono manda mensagem ou áudio no WhatsApp do bot
       │
       ▼
[Se áudio] → Whisper transcreve
       │
       ▼
[Agente Sonnet] extrai:
   - o quê (descrição da tarefa)
   - quem é responsável (ou [INDEFINIDO] → pergunta ao dono)
   - prazo (data ou descrição relativa)
   - contexto e dependências
       │
       ▼
[Priorização] urgência × impacto (configurável)
       │
       ▼
GATE HITL nível 2 (primeiros 30 dias):
   dono confirma extração e prioridade
       │
       ▼
Push para Trello/Asana com etiquetas
       │
       ▼
Cron diário 8h: enviar ao dono
   - tarefas do dia
   - tarefas em atraso
   - alertas de prazo (D-1, D-3)
```

---

## Nível HITL recomendado

| Operação | Nível inicial |
|---|---|
| Captação e extração | 4 |
| Priorização | 3 (em casos de baixa confiança → escala) |
| Push para PM | 2 (primeiros 30d), depois 3 |
| Lembrete proativo | 4 |
| Resumo diário | 4 |
| **Atribuição a pessoa específica** | **2 — sempre dono confirma** |

---

## Tempo estimado de construção (para V2)

| Fase | Dias úteis |
|---|---|
| D | 5–8 |
| P (matriz de prioridade + integração PM + lista de responsáveis) | 7–12 |
| I | 7–10 |
| A | 5–10 |
| **Total** | **24–40 dias** |

Módulo de complexidade média. Bom para implementador que já dominou Módulo 6.

---

## Pré-requisitos

- **Módulo 6 (Resumo de Reunião) dominado** — reaproveita lógica de extração de ações.
- Sistema de PM ativo ou disposição para adotar.
- **Matriz de prioridade definida pelo cliente** (não pelo implementador).
- Lista de responsáveis com WhatsApp/e-mail para atribuição.
- Dono aceita usar o módulo diariamente — sem uso, módulo morre em 30 dias.

---

## Notas para V2

- Integração com **calendário** para detectar conflitos de prazo.
- Detecção de **tarefas duplicadas** (mesmo pedido vindo por canais diferentes).
- Sugestão proativa: "essa tarefa tem 3 dependências em atraso" — agente reordena.
- Considerar canal Slack como input alternativo ao WhatsApp.

---

*Especificação Módulo Gestão de Tarefas v0.1 — AIOS Profissional 2027.*
