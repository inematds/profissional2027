# Módulo 6 — Resumo Automático de Reunião

**Versão:** 1.0 — maio/2026
**Categoria:** P1 — Módulo de Produtividade Interna
**Pré-requisitos AIOS:** stack canônica + sistema de gravação/transcrição (Otter, Fireflies, Read.ai, ou Whisper local).

---

## O que faz

Recebe **transcrição** ou **áudio bruto** de reunião e produz **ata estruturada** com:

- Resumo executivo (3-5 frases).
- Decisões tomadas (lista numerada).
- Ações com responsável e prazo (formato acionável).
- Riscos e bloqueios identificados.
- Próximos passos com prazo.
- Trechos-chave (citações relevantes).

Distribui a ata por e-mail, WhatsApp do grupo, ou registro em CRM/PM (Pipedrive, Trello, Asana).

Aplica padrão **Parallelization (sectioning)** — 4 chamadas Sonnet em paralelo, cada uma cobrindo uma seção da ata, com agregação final.

---

## Quando usar

- Equipe interna do cliente PME tem **≥ 3 reuniões/semana** com duração média ≥ 30 min.
- Equipe gasta ≥ 2 h/semana escrevendo ata manualmente (e a maioria das atas fica incompleta).
- Decisões e ações se perdem entre reuniões (sintoma: "a gente já tinha falado sobre isso").
- Cliente PME tem rotina formal de **kickoff, weekly, 1:1, reunião com cliente externo**.
- Uso interno (não externo) — agente trabalha em dado da própria empresa, baixo risco.
- Verticais ideais: **qualquer PME com gestão semi-profissionalizada** (consultoria, agência, escritório técnico, software house pequena, time de vendas de qualquer setor).

---

## Quando NÃO usar

- Reuniões **muito curtas** (< 15 min) ou **muito esporádicas** (< 2/semana). ROI inviável.
- Reunião com **dado clínico/financeiro/jurídico sensível** sem cuidado adicional de LGPD. Antes, fase P precisa endereçar.
- Cliente PME **não grava reuniões** e não vai começar. Sem áudio, sem módulo.
- Reunião em ambiente com áudio péssimo (chão de fábrica, restaurante barulhento). Transcrição inviável.

---

## Resultado esperado

| Métrica | Baseline típico | Após 90 dias | Após 180 dias |
|---|---|---|---|
| Tempo gasto em ata/reunião | 25–45 min | 3–8 min (revisão) | 2–6 min |
| % reuniões com ata completa | 30–60% | 90–100% | 95–100% |
| % ações com responsável + prazo claros | 40–70% | 85–95% | 90–98% |
| Tempo champion produzindo ata semanal | 4–8 h | 30 min–1 h | 15-45 min |
| Reuniões "redundantes" (re-discutir o que já foi decidido) | 1–3/semana | 0,3-1/semana | < 0,5/semana |

**Payback típico:** 1,5 a 3 meses (custo baixíssimo, ganho de produtividade alto).

---

## Stack utilizada

- **Sistema de transcrição** — Otter.ai, Fireflies, Read.ai (planos comerciais com PT-BR a partir de US$ 8-20/mês por usuário), OU Whisper local (gratuito mas exige setup).
- **n8n** — recebe webhook do sistema de transcrição (ou arquivo de upload), dispara 4 chamadas Sonnet em paralelo, agrega.
- **Claude Sonnet 4.6 com prompt caching** — caching crítico aqui: transcrição longa cacheada barateia chamadas paralelas.
- **Supabase** — armazena atas para busca histórica e RAG sobre "o que decidimos sobre X?".
- **Saída**: e-mail (SendGrid, AWS SES), WhatsApp (Cloud API), CRM/PM via API.

---

## Arquitetura

```
   Reunião acontece → gravação automática (Otter/Fireflies)
        │
        ▼
   Sistema de transcrição entrega .txt + metadados via webhook
        │
        ▼
═══════════════════════════════════════════════════
  Workflow "resumo-reuniao" no n8n
═══════════════════════════════════════════════════

  Entrada: transcrição + participantes + tipo de reunião
        │
        ▼
  Cache a transcrição no Claude (prompt caching)
        │
        ▼
  ╔════════════════════════════════════════╗
  ║ 4 chamadas Sonnet EM PARALELO          ║
  ║                                        ║
  ║  P1: extrair decisões                  ║
  ║  P2: extrair ações (resp + prazo)      ║
  ║  P3: extrair riscos/bloqueios          ║
  ║  P4: extrair próximos passos           ║
  ╚════════════════════════════════════════╝
        │
        ▼
  Agregação (Haiku): compor ata estruturada
  + resumo executivo de 3-5 frases
        │
        ▼
  GATE HITL: opcionalmente, champion revisa antes
  de distribuir (default: distribui direto, nível 4)
        │
        ▼
  Distribuir:
  ├── E-mail aos participantes
  ├── Mensagem ao grupo WhatsApp interno
  ├── Card no Trello/Asana com ações
  └── Registro no Supabase (RAG futuro)
═══════════════════════════════════════════════════
```

---

## Nível HITL recomendado

**Nível 4 desde o início** (uso interno, baixo risco) — ata é gerada e distribuída automaticamente, com **auditoria semanal** pelo champion.

**Exceções que devem subir para nível 2 (co-piloto):**
- Reunião com cliente externo cujo conteúdo vai ser repassado a ele.
- Reunião com C-level discutindo decisão sensível (estratégia, demissão, fusão).
- Reunião com dados pessoais relevantes (entrevista de candidato, conversa com cliente VIP).

A definição "esta reunião é HITL 2 ou 4" é feita no momento do agendamento (tag na sala de reunião).

---

## Custo operacional estimado (R$/mês)

**Time pequeno — 12 reuniões/semana (~50/mês), duração média 35 min:**

| Item | Mensal |
|---|---|
| Sistema de transcrição (Otter Business 5 usuários) | R$ 200-400 |
| Claude API (Sonnet com caching agressivo) | R$ 80-150 |
| Envio de e-mail (SendGrid free tier) | R$ 0-30 |
| Rateio infra | R$ 30-80 |
| **Total** | **R$ 310-660** |

**Time médio — 30 reuniões/semana:**

| Item | Mensal |
|---|---|
| Transcrição (Otter Team) | R$ 500-900 |
| Claude API com caching | R$ 200-400 |
| Rateio infra | R$ 50-100 |
| **Total** | **R$ 750-1.400** |

---

## Tempo de implementação

| Fase | Dias úteis |
|---|---|
| D | 3-5 (mais simples — baseline é tempo de ata e taxa de cobertura) |
| P | 5-8 (definir tipos de reunião, template de ata, lista de canais de distribuição) |
| I | 5-8 |
| A | 5-10 |
| **Total** | **18-31 dias úteis** |

Módulo de **menor complexidade** entre os P1. Bom segundo módulo para implementador iniciante (depois do Atendimento WhatsApp).

---

## Métricas obrigatórias

1. **Tempo gasto em ata por reunião** (de 30+ min para < 10 min — revisão apenas).
2. **% reuniões com ata produzida.**
3. **% ações com responsável + prazo claros.**
4. **Tempo de processamento** (transcrição → ata distribuída): alvo < 5 min após fim da reunião.
5. **Taxa de erro detectado na auditoria semanal** (ação atribuída a pessoa errada, decisão mal capturada): alvo < 5%.
6. **Custo total mensal.**
7. **NPS interno do time** sobre a ata.

---

## Erros comuns e como evitar

- **Transcrição com baixa qualidade.** Resultado: ata cheia de imprecisões. *Corrige:* validar qualidade do mic, escolher sistema de transcrição com bom PT-BR (Otter, Fireflies; evitar transcrição de Zoom puro que erra demais).
- **Não cachear a transcrição.** Resultado: 4 chamadas paralelas custam 4x. *Corrige:* prompt caching obrigatório (Anthropic dá 90% de desconto em cache hit).
- **Ação sem responsável.** Resultado: ata bonita, nada acontece. *Corrige:* P2 (extrai ações) tem regra explícita "se ação não tem responsável claro, listar com `RESPONSAVEL: indefinido`" para o champion humano definir.
- **Resumo executivo genérico.** Resultado: ninguém lê. *Corrige:* prompt do agregador exige resumo com decisões específicas, não "discutiu-se sobre X".
- **Ata distribuída a quem não estava na reunião.** Risco de vazamento. *Corrige:* lista de distribuição vem dos participantes da reunião, não de listas estáticas.
- **Reunião sensível indo para nível 4.** Risco LGPD/relacional. *Corrige:* tag explícita no agendamento + default conservador quando há dúvida.
- **Não revisar amostral semanal.** Resultado: agente desvia e ninguém percebe. *Corrige:* champion revisa 5 atas/semana mínimo.

---

## Variações por vertical

### Consultoria (uso típico do implementador)
- Reuniões com cliente: weekly de projeto, status, kickoff.
- Tags: cliente externo → HITL 2.
- Distribuição: cliente + time interno + Pipedrive.
- Templates específicos por tipo (kickoff tem seção "premissas", status tem "marco/meta").

### Agência
- Reuniões: planning semanal, daily, retrospectiva, briefing com cliente.
- Distribuição: time interno via Slack + cliente via e-mail (para reuniões externas).

### Escritório técnico
- Reuniões: levantamento técnico (vai pra documento de projeto), revisão de obra/projeto.
- Distribuição: time + cliente + sistema de gestão de projetos.

### Software house / dev team
- Reuniões: planning, retro, daily, revisão técnica.
- Distribuição: Slack + Linear/Jira (criar issues automaticamente das ações).

---

## Mapeamento para DPIA

| Fase | Pré-requisitos |
|---|---|
| D | Baseline: tempo gasto em ata, % reuniões com ata, % ações claras, reuniões redundantes/semana. KPIs assinados. Champion (líder de time ou ops) decidido. |
| P | Inventário dos tipos de reunião (kickoff, weekly, 1:1, cliente externo). Template de ata por tipo. Lista de distribuição por tipo. Sistema de transcrição escolhido e testado. RIPD sobre dados em reuniões (RH, financeiro, cliente externo). |
| I | system-prompt.md (4 paralelos + agregador) + playbook.md. Matriz HITL com tags de reunião. Homologação com 10 reuniões reais — comparar ata gerada vs. ata humana. |
| A | Deploy direto em nível 4 para reuniões internas. Job shadowing 2 semanas: champion revisa todas as atas e calibra. Micro-vídeo "como tagear reunião sensível". |

---

*Módulo Resumo de Reunião v1.0 — AIOS Profissional 2027.*
