# Módulo 5 — Proposta Comercial a partir de Briefing

**Versão:** 1.0 — maio/2026
**Categoria:** P1 — Módulo Comercial Interno
**Pré-requisitos AIOS:** stack canônica + biblioteca de propostas anteriores do cliente (ou do implementador) para few-shot.

---

## O que faz

Transforma um **briefing curto** (texto livre, áudio transcrito, ou notas de reunião) numa **proposta comercial estruturada** em PT-BR no template editorial do cliente, contendo:

- Diagnóstico da dor declarada.
- Escopo proposto (entregáveis claros, fases, prazo).
- ROI estimado (com base em premissas declaradas).
- Preço (a partir da tabela do implementador/cliente).
- Próximos passos e validade.

Aplica padrão **Prompt Chaining** com 4 elos + **Evaluator-Optimizer** opcional para polimento (cf. `02-padroes-de-design.md` §1 e §5).

**Uso interno** do cliente PME — quem usa é o vendedor/consultor, não o cliente final. Por isso pode iniciar em nível HITL 1 ou 2 (vendedor sempre revisa antes de enviar).

---

## Quando usar

- Cliente PME tem **time comercial que escreve 5+ propostas/semana** com estrutura repetitiva.
- Tempo médio para escrever proposta atual é ≥ 1 hora — dor mensurável.
- Existe **template editorial** ou pelo menos um padrão informal nas propostas atuais.
- Catálogo de produtos/serviços e tabela de preço acessíveis.
- 10+ propostas anteriores disponíveis para servir de **few-shot real**.
- Verticais ideais: **consultoria, agência de marketing, escritório técnico, implementador de IA (uso próprio!), TI corporativo, integrador**.

---

## Quando NÃO usar

- Volume < 5 propostas/semana. ROI apertado.
- Propostas exigem **anexos visuais complexos** (renders, plantas, mockups) — agente faz texto, não diagrama.
- Cliente quer "proposta autônoma enviada direto ao prospect". Recuse — sempre humano revisa.
- Não há **template editorial nem padrão informal** consistente. Antes, fase P precisa codificar o template.
- Vendas com **alta variabilidade de preço por negociação caso a caso**. Agente compõe; precificação sempre humana.

---

## Resultado esperado

| Métrica | Baseline típico | Após 90 dias | Após 180 dias |
|---|---|---|---|
| Tempo para escrever proposta | 60–120 min | 12–25 min | 8–20 min |
| Propostas enviadas/semana | 5–8 | 12–18 | 15–25 |
| Taxa de fechamento | x% | +3 a +8 pontos | +5 a +12 pontos |
| Erros de preço/escopo nas propostas | 8–15% | 2–4% | < 2% |
| Tempo de cliente PME na resposta | sem medida | sem medida ou -15% | -20% |

Ganho de **velocidade** + **consistência** + **qualidade do texto** = **taxa de fechamento sobe**, especialmente em proposta enviada no mesmo dia da reunião (velocidade vence concorrência).

**Payback típico:** 2 a 4 meses.

---

## Stack utilizada

- **n8n** — workflow com 4 elos de chaining + interface de input.
- **Claude Sonnet 4.6** — todos os elos (extração, escopo, redação, validação). Tarefa de escrita longa demanda Sonnet.
- **Supabase + pgvector** — biblioteca de propostas anteriores em RAG para few-shot dinâmico.
- **API de PDF (Carbone, DocRaptor, ou Puppeteer no n8n)** — geração do PDF final no template editorial.
- **CRM do cliente** (RD Station, Pipedrive, HubSpot) — push da proposta gerada para o estágio "proposta enviada".

Não usa WhatsApp diretamente — saída é PDF + e-mail. Vendedor decide canal.

---

## Arquitetura

```
   Vendedor preenche briefing
   (formulário n8n ou WhatsApp do vendedor)
        │
        ▼
═══════════════════════════════════════════════════
  Chaining 4 elos
═══════════════════════════════════════════════════
  [Elo 1] Extrair (Sonnet → JSON)
   { dor, contexto, objetivo, restricoes,
     prazo_desejado, orcamento_aproximado }
        │
        ▼  validação: campos obrigatórios preenchidos?
        │
  [Elo 2] Gerar escopo (Sonnet)
   - Fases (DPIA se aplicar, ou outro framework)
   - Entregáveis por fase
   - Prazo total
        │
        ▼
  [Elo 3] Calcular preço (Sonnet + tool)
   tool: consultar_tabela_servicos(itens_escopo)
   aplica regras de desconto da empresa
        │
        ▼
  [Elo 4] Redigir proposta (Sonnet)
   no template editorial + tom oficial
   busca 3 propostas similares no RAG para few-shot
        │
        ▼  (opcional) Evaluator-Optimizer
        │
  Geração de PDF
        │
        ▼
  GATE HITL nível 1 ou 2:
   vendedor revisa, edita, aprova, envia
═══════════════════════════════════════════════════
```

---

## Nível HITL recomendado

- **Sempre nível 1 ou 2** para envio. Proposta é documento que define contrato; revisão humana é não-negociável.
- **Nível 1 (Assistir)** — agente entrega PDF como rascunho; vendedor abre, edita, envia. Default permanente para propostas acima de R$ 50.000.
- **Nível 2 (Co-piloto)** — vendedor revisa no painel n8n, aprova, agente envia por e-mail automaticamente. Permitido para propostas até R$ 50.000 com cliente recorrente.

**Nunca níveis 3 ou 4** na V1. Proposta automática enviada ao prospect é receita para incidente.

---

## Custo operacional estimado (R$/mês)

**Time comercial pequeno — 25 propostas/mês:**

| Item | Mensal |
|---|---|
| Claude API (Sonnet, ~10k tokens/proposta) | R$ 80–180 |
| Geração de PDF (API) | R$ 20–50 |
| Rateio infra | R$ 30–80 |
| **Total** | **R$ 130–310** |

**Time médio — 80 propostas/mês:**

| Item | Mensal |
|---|---|
| Claude API com caching | R$ 250–500 |
| PDF | R$ 60–150 |
| Rateio infra | R$ 50–100 |
| **Total** | **R$ 360–750** |

---

## Tempo de implementação

| Fase | Dias úteis |
|---|---|
| D | 5–8 (dor de proposta, baseline tempo, taxa de fechamento) |
| P | 10–15 (template editorial + tabela de preços + biblioteca de propostas históricas indexadas no RAG) |
| I | 8–12 |
| A | 5–10 (uso interno, ramp-up curto) |
| **Total** | **28–45 dias úteis** |

A fase P costuma ser a mais demorada: indexar 50–100 propostas antigas no RAG e definir o template editorial canônico exige sessão dedicada.

---

## Métricas obrigatórias

1. **Tempo médio para escrever proposta** (minutos do briefing ao PDF final).
2. **Propostas/semana enviadas.**
3. **Taxa de fechamento** (% de propostas que viraram contrato).
4. **Taxa de edição pré-envio** (% de propostas que o vendedor mexeu antes de mandar).
5. **Erros de preço/escopo** (relatos pós-envio).
6. **Tempo do briefing à proposta enviada ao prospect** (minutos/horas).
7. **NPS interno do time comercial** sobre a ferramenta.
8. **Custo total mensal.**

---

## Erros comuns e como evitar

- **Briefing curto demais ou genérico.** Resultado: proposta vaga. *Corrige:* formulário de briefing exige campos mínimos (dor, contexto, prazo, orçamento). Validação no Elo 1 bloqueia se algum falta.
- **Preço calculado pelo LLM em vez de tool.** Resultado: número errado. *Corrige:* `consultar_tabela_servicos` obrigatório no Elo 3; LLM não calcula.
- **Few-shot estático (mesmas 3 propostas para tudo).** Resultado: textos repetitivos, descontextualizados. *Corrige:* RAG dinâmico — busca 3 propostas similares por dor/setor/porte.
- **Esquecer prazo de validade da proposta.** Resultado: ambiguidade contratual. *Corrige:* validade fixa no template (15 ou 30 dias) ou variável por porte.
- **Proposta enviada sem revisão humana.** *Corrige:* gate HITL não-negociável.
- **Não atualizar tabela de preços no RAG.** Resultado: proposta com preço antigo. *Corrige:* indexação automática quando ERP/planilha atualiza, ou check manual mensal.
- **Tom genérico de IA (excesso de "vamos transformar seu negócio").** *Corrige:* Bloco 8 do prompt proíbe vocabulário de marketing genérico.

---

## Variações por vertical

### Consultoria de implementação de IA (o próprio implementador!)
- Template baseado em DPIA: fases D/P/I/A + Acompanhamento.
- Tabela: diagnóstico (R$ 3-8k) + projeto (R$ 20-80k) + manutenção (R$ 800-7k/mês).
- Few-shot: propostas Polaris, MoveBem, Verbo (cases anonimizados).
- Saída: PDF + slides (template editorial INEMA.CLUB).

### Agência de marketing digital
- Template por serviço: SEO, Tráfego pago, Conteúdo, Social, Branding.
- Preço por escopo mensal recorrente.
- Few-shot: por nicho atendido.

### Escritório técnico (engenharia, arquitetura, contábil)
- Template com escopo técnico + cronograma + entregáveis regulamentares.
- Preço por hora-homem ou por entregável fixo.
- Validade longa (60–90 dias).

---

## Mapeamento para DPIA

| Fase | Pré-requisitos |
|---|---|
| D | Baseline de tempo por proposta, volume/semana, taxa de fechamento, erros. KPI futuro assinado. Champion (head comercial) decidido. |
| P | Template editorial canônico extraído de propostas anteriores. Tabela de preços e regras de desconto formalizada. 50-100 propostas antigas indexadas no Supabase pgvector. Glossário de vocabulário comercial. |
| I | system-prompt.md (4 prompts encadeados) + playbook.md. Matriz HITL nível 1 ou 2 sempre. Homologação com 10 briefings reais — vendedor compara proposta gerada vs. proposta que ele escreveria. |
| A | Deploy com job shadowing por 1 semana (implementador acompanha vendedor usando). Micro-vídeo "como dar briefing", "como revisar antes de enviar". Métricas semanais nas primeiras 4 semanas. |

---

*Módulo Proposta Comercial v1.0 — AIOS Profissional 2027.*
