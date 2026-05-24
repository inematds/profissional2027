# Módulo 1 — Atendimento WhatsApp 24/7

**Versão:** 1.0 — maio/2026
**Categoria:** P1 — Módulo de Atendimento
**Pré-requisitos AIOS:** stack canônica funcional, WABA aprovada pela Meta no número do cliente.

---

## O que faz

Responde clientes finais no WhatsApp Business do cliente PME, **24/7**, em PT-BR, com tom institucional do negócio.

Cobre três famílias de demanda: **dúvidas frequentes** (preço, prazo, horário, disponibilidade), **triagem** (entender o que o cliente quer e rotear) e **escalonamento humano** (passar para o operador interno quando há gatilho de risco, dúvida, ou pedido grande). Resposta em até 1 minuto em horário comercial, mensagem-padrão fora dele com retomada às 6h.

---

## Quando usar

- Cliente PME com **WhatsApp como canal primário** de relacionamento (≥ 60% das interações comerciais e de suporte passam pelo WhatsApp).
- Volume diário de mensagens entre **30 e 800** (abaixo de 30 não justifica; acima de 800 exige stack reforçada e estudo de capacidade).
- Tempo médio de resposta atual é **dor declarada** pelo dono no diagnóstico DPIA (típico: 30 min a 4 h, contra meta de 5 min).
- Existe operador humano hoje atendendo o número — vamos co-pilotar com ele, não substituir.
- Catálogo, tabela de preços, e regras estão minimamente estruturados em ERP (Bling, Omie, Conta Azul) ou planilha.
- Verticais ideais: **distribuidoras, clínicas, varejo de médio volume, prestador de serviço com agenda**.

---

## Quando NÃO usar

- Cliente quer "agente que faz tudo" sem diagnóstico DPIA fechado. Recuse e refaça a fase D.
- Volume diário < 30 mensagens. Custo de WABA + LLM + manutenção não fecha. Sugira playbook humano + templates.
- Negócio cujo **canal primário não é WhatsApp** (B2B enterprise por e-mail, indústria pesada por telefone). Outro módulo.
- Equipe interna **não tem ninguém** disponível para ser champion. Sem champion, módulo morre em 60 dias.
- Cliente quer atender **em inglês ou espanhol** simultaneamente. V1 não trata; ajustar manualmente.
- Atendimento envolve **diagnóstico clínico, jurídico ou financeiro complexo** que demanda profissional habilitado. O agente faz triagem, não consulta.
- Cliente PME se recusa a publicar **política clara de uso de IA** no atendimento (LGPD/transparência).

---

## Resultado esperado

Benchmarks observados em casos Nautis e equivalentes (Pesquisa A §6.1, Pesquisa D, Pesquisa F caso distribuidora):

| Métrica | Baseline típico | Após 90 dias | Após 180 dias |
|---|---|---|---|
| Tempo médio de resposta | 30–240 min | 3–8 min | 1–5 min |
| % atendimentos resolvidos sem humano | 0% | 50–65% | 65–80% |
| Pedidos/leads perdidos por demora | 2–5/dia | 0,5–1,5/dia | < 1/dia |
| NPS do atendimento | sem medida | +15 a +25 pontos | +20 a +35 |
| Custo médio por atendimento | R$ 8–15 (hora-pessoa) | R$ 1,50–3 | R$ 1–2,50 |

**Payback típico:** 2,5 a 4 meses (alinhado a Pesquisa A §2.5 — média 3,2 meses).

---

## Stack utilizada

- **WhatsApp Cloud API direto da Meta** (não BSP) — cf. `01-stack-canonica.md`.
- **n8n self-host na Hetzner** — webhook de entrada, workflow de roteamento, painel de aprovação.
- **Claude Haiku 4.5** para classificação de intenção e respostas simples.
- **Claude Sonnet 4.6** para respostas com tool use múltiplo, casos complexos, raciocínio.
- **Supabase (Postgres + pgvector)** para memória de sessão por número + RAG sobre base de conhecimento do cliente.
- **ERP do cliente** (Bling/Omie/Conta Azul) via REST para consultar tabela, estoque, pedido.

---

## Arquitetura

```
                  ┌────────────────────────────────────────┐
                  │  Cliente final no WhatsApp do cliente  │
                  └──────────────────┬─────────────────────┘
                                     │ mensagem entra
                                     ▼
                          ┌────────────────────┐
                          │ Meta Cloud API     │
                          │ webhook → n8n      │
                          └────────┬───────────┘
                                   ▼
                  ┌────────────────────────────────────┐
                  │ Workflow "atendimento" no n8n      │
                  │                                    │
                  │  1. Hidratar contexto (Supabase)   │
                  │     sessão + histórico últimas 20  │
                  │                                    │
                  │  2. Classificador Haiku            │
                  │     → tipo da mensagem             │
                  │                                    │
                  │  3. Roteador                       │
                  │     ├── saudação → resposta fixa   │
                  │     ├── preço → Haiku + tool       │
                  │     ├── pedido → escala módulo 2   │
                  │     ├── reclamação → escala humano │
                  │     ├── geral → Sonnet + RAG       │
                  │     └── fora_escopo → escala       │
                  │                                    │
                  │  4. Gate HITL (nível atual)        │
                  │     ├── nível 2: pausa, aprova     │
                  │     └── nível 3: gates de exceção  │
                  │                                    │
                  │  5. Send Message (Meta)            │
                  │                                    │
                  │  6. Persistir sessão (Supabase)    │
                  │  7. Log evento + métricas          │
                  └────────────────────────────────────┘
                                   │
                                   ▼
                  ┌────────────────────────────────────┐
                  │ Painel do champion (n8n Chat)      │
                  │ - aprovações pendentes             │
                  │ - escalonamentos                   │
                  │ - histórico                        │
                  └────────────────────────────────────┘
```

Padrão dominante: **Routing** (cf. `02-padroes-de-design.md` §2).

---

## Nível HITL recomendado

**Início:** Nível 2 (Co-piloto) em **todos os turnos** por 2 a 4 semanas.

**Após 4 semanas com ≥ 90% qualidade + zero incidente:** subir para Nível 3 (Autopiloto com exceções) com os seguintes gates de exceção sempre ativos:

- Pedido > R$ {{TETO_PEDIDO}} (default R$ 5.000)
- Palavras-chave: "cancelar", "Procon", "advogado", "vencido", "estragado", "reclamar", "jurídico"
- Confidence < 70%
- Cliente da lista VIP
- Fora do horário 22h–6h
- Cliente novo (primeira interação)
- Idioma diferente de PT-BR

**Nível 4 nunca** para conversa com cliente externo na V1. Apenas para mensagens-padrão (saudação, despedida, fora de horário).

Cf. `03-niveis-de-aprovacao-humana.md` para procedimentos.

---

## Custo operacional estimado (R$/mês)

**Cliente pequeno — 1.500 mensagens entrantes/mês, ~3.500 turnos LLM:**

| Item | Mensal |
|---|---|
| Meta WABA (templates marketing/utilidade) | R$ 80–250 |
| Claude API (Haiku + Sonnet com caching) | R$ 100–180 |
| Rateio n8n VPS + Supabase | R$ 50–100 |
| **Total cliente pequeno** | **R$ 230–530** |

**Cliente médio — 6.000 mensagens entrantes/mês:**

| Item | Mensal |
|---|---|
| Meta WABA | R$ 250–600 |
| Claude API | R$ 300–500 |
| Rateio infra | R$ 80–150 |
| **Total cliente médio** | **R$ 630–1.250** |

Implementador cobra **manutenção mensal acima** desses custos (tiers R$ 800–7.000/mês — cf. DPIA Canônico §Acompanhamento).

---

## Tempo de implementação (do diagnóstico ao go-live)

| Fase DPIA | Dias úteis |
|---|---|
| D — Diagnóstico | 5–10 |
| P — Pacote de Contexto | 5–10 |
| I — System prompt + playbook + testes | 7–12 |
| A — Deploy, job shadowing, primeiras métricas | 7–14 |
| **Total até go-live + 30 dias** | **24–46 dias úteis** |

Em cliente bem estruturado (Bling em dia, operador colaborativo, dono presente), fecha em **5 semanas**. Em cliente desorganizado, **9 semanas**.

---

## Métricas obrigatórias

A serem coletadas e apresentadas mensalmente:

1. **Tempo médio de resposta** (minutos) — comparado ao baseline assinado em D.
2. **Volume processado pelo agente** (mensagens/dia, % do total).
3. **% de turnos resolvidos sem humano** (sinal de qualidade).
4. **% de override humano** (humano corrigiu depois — alvo ≤ 15% em nível 3).
5. **Taxa de escalonamento por motivo** (reclamação, valor, confidence, VIP) — distribuição saudável.
6. **Pedidos/leads perdidos por demora** (comparado ao baseline em D).
7. **NPS do atendimento** (pesquisa pós-interação trimestral).
8. **Custo total mensal** (WABA + LLM + infra) — para acompanhar payback.

Todas as métricas plotadas na seção 8 do playbook (cf. `playbook.md`).

---

## Erros comuns e como evitar

- **Subir para nível 3 sem 4 semanas estáveis em nível 2.** Origem: pressa do cliente ou cansaço da champion aprovando 200 msgs/dia. *Corrige:* exit-gate documentado, métricas formais, sem atalho.
- **Few-shot sintético no system prompt.** Origem: implementador pulou a exportação do histórico WhatsApp na fase P. *Corrige:* exigir mínimo 7 exemplos reais por tipo de interação.
- **Esquecer cláusula "estou falando com robô?"** Origem: prompt copiado de tutorial. *Corrige:* Bloco 8 do system prompt sempre traz a frase aprovada (Playbook 4.4).
- **Não persistir sessão.** Origem: implementador esqueceu Supabase. Resultado: agente repete pergunta a cada mensagem. *Corrige:* hidratar contexto é primeiro passo do workflow, sempre.
- **Catálogo embutido no prompt.** Origem: tentativa de economizar tool call. Resultado: prompt > 10k tokens, custo explode, atualização dói. *Corrige:* catálogo via tool `consultar_tabela_precos` ou RAG.
- **Agente respondendo em horário "humano" sem aviso ao cliente sobre IA.** Risco LGPD/transparência. *Corrige:* política de uso de IA pública no canal + frase de resposta aprovada quando perguntado.
- **Cliente recusa o módulo após 60 dias.** Causa quase sempre: champion sem treinamento real, playbook congelado. *Corrige:* manutenção mensal **com sessão de revisão obrigatória do playbook**.

---

## Variações por vertical

### Distribuidoras / atacado (Polaris Bebidas)

- **Tom:** direto, comercial, foco em prazo de entrega e roteiro.
- **Tools-chave:** `consultar_tabela_precos`, `consultar_estoque`, `consultar_pedido`, `criar_pedido_bling` (rascunho).
- **Regras tácitas frequentes:** descontos por cliente (RT-01 estilo Casa do Carlos), roteiros de entrega, bonificação.
- **Gatilhos extras:** pedido fora do mínimo da região, produto com validade próxima.

### Clínicas / consultórios (MoveBem)

- **Tom:** acolhedor, sem jargão clínico, cordial.
- **Tools-chave:** `consultar_agenda`, `propor_horarios`, `confirmar_consulta` (co-piloto), `consultar_prontuario_metadados` (LGPD — sem dado sensível).
- **Regras tácitas:** convênios aceitos, primeira consulta tem cadastro específico, retorno tem encaixe.
- **Gatilhos extras:** **pergunta clínica** → escala obrigatório para profissional (nunca o agente diagnostica).

### Contábil / jurídico (Verbo)

- **Tom:** profissional, técnico-mas-acessível, registra tudo.
- **Tools-chave:** `consultar_cliente_no_dominio`, `criar_ticket_atendimento`, `consultar_calendario_obrigacoes`.
- **Regras tácitas:** prazos fiscais críticos (sempre escalar), reclamação sobre multa (sempre escalar), dúvida sobre regime tributário (escalar a sócio).
- **Gatilhos extras:** menção a fiscalização/auditoria → escalar imediato.

---

## Mapeamento para DPIA

| Fase | Pré-requisitos para este módulo |
|---|---|
| **D** | Baseline assinado com tempo médio atual, volume diário, pedidos perdidos, custo de hora-pessoa de atendimento. KPIs futuros definidos. Reposicionamento do operador atual decidido. Champion nomeado. |
| **P** | Histórico exportado de 60–90 dias do WhatsApp. Glossário do cliente (vocabulário, sinônimos). Regras formais (RN) e tácitas (RT) numeradas. Plano de integração com ERP. RIPD enxuto sobre dados pessoais que o agente vai tocar. |
| **I** | Este módulo (system-prompt.md + playbook.md). Matriz HITL aprovada por escrito. Testes de homologação com ≥ 85% acerto em 30–50 casos reais. |
| **A** | Deploy em nível 2 + job shadowing 3 dias + champion treinado + 5–10 micro-vídeos + relatórios 30/60/90 dias. |

---

*Módulo Atendimento WhatsApp v1.0 — AIOS Profissional 2027.*
