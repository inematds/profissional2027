# Módulo 2 — Pedidos via WhatsApp

**Versão:** 1.0 — maio/2026
**Categoria:** P1 — Módulo de Operação Comercial
**Pré-requisitos AIOS:** Módulo 1 (Atendimento WhatsApp) dominado; integração ativa com ERP (Bling/Omie/Tiny).

---

## O que faz

Recebe, interpreta, valida e roteia **pedidos de venda** enviados por WhatsApp Business do cliente PME, criando o pedido em **rascunho no ERP** para confirmação humana (em níveis HITL 2-3).

Cobre o ciclo: cliente final manda texto livre → agente extrai itens e quantidades → valida contra catálogo e estoque → calcula preço com regras de desconto → confirma com o cliente final repetindo itens → cria rascunho de pedido no ERP → escala para champion confirmar e disparar emissão de NF-e/cobrança.

Aplica padrão **Prompt Chaining** (cf. `02-padroes-de-design.md` §1), com nós de validação programática entre os elos.

---

## Quando usar

- Cliente PME com **fluxo de pedidos por WhatsApp recorrente** (≥ 15 pedidos/dia).
- Catálogo estruturado em **ERP integrável** (Bling, Omie, Tiny) com tabela de preços vigente.
- Histórico mostra padrão recorrente de itens e clientes (≥ 60% dos pedidos vêm de clientes já cadastrados).
- Existe regra clara de **pedido mínimo, condição de pagamento e prazo de entrega**.
- Operador atual gasta ≥ 3 horas/dia digitando pedidos no ERP a partir de WhatsApp — dor mensurável.
- Verticais ideais: **distribuidoras, atacado, varejo B2B, e-commerce conversacional**.

---

## Quando NÃO usar

- Pedidos com **alto grau de customização técnica** (móveis sob medida, projetos de engenharia). Texto livre não captura especificação.
- Volume < 15 pedidos/dia. Custo de implementação não tem payback.
- Cliente PME **sem catálogo no ERP** ou com tabela em planilha caótica. Antes, fase P precisa estruturar.
- Pedidos exigem aprovação prévia do dono caso a caso. Não há rotina automatizável.
- Cliente recusa cláusula de confirmação humana antes do envio para cliente final (nível 2 não-negociável em V1 do AIOS).
- Negócio com **alto índice de inadimplência** sem régua de crédito documentada. Antes, definir política.

---

## Resultado esperado

| Métrica | Baseline típico | Após 90 dias | Após 180 dias |
|---|---|---|---|
| Tempo médio para registrar pedido | 8–25 min | 1,5–4 min | 1–3 min |
| Pedidos perdidos por demora | 2–5/dia | 0,5–1/dia | < 0,5/dia |
| Erros de digitação no ERP | 4–8% dos pedidos | 1–2% | < 1% |
| Receita capturada que era perdida | — | +8 a +15% | +12 a +20% |
| Tempo livre do operador | — | 2–4 h/dia | 3–5 h/dia (reposicionado) |

**Payback típico:** 2,5 a 4 meses.

---

## Stack utilizada

- **WhatsApp Cloud API** (Meta direto).
- **n8n** — workflow com 4 elos (extrair → validar → calcular → confirmar).
- **Claude Sonnet 4.6** para extração estruturada e raciocínio sobre regras.
- **Claude Haiku 4.5** para confirmação simples e formatação de mensagem final.
- **ERP via REST**: Bling (mais comum em distribuidora), Omie ou Tiny.
- **Supabase** para sessão por número, log de pedidos rascunho, métricas.

---

## Arquitetura

```
Mensagem entra no WhatsApp
       │
       ▼
[Roteador do Módulo 1] → detecta intenção "pedido"
       │
       ▼
═══════════════════════════════════════════════════
  Workflow "pedidos" no n8n — chaining 4 elos
═══════════════════════════════════════════════════

  [Elo 1] Extrair itens (Sonnet → JSON)
    saída: [{ produto, qtde, observacao }, ...]
       │
       ▼  validação: itens estruturados? cliente identificado?
       │
  [Elo 2] Validar contra catálogo (tool: consultar_tabela)
    para cada item: existe? está em estoque?
       │
       ▼  se algum item ambíguo → pergunta de esclarecimento
       │
  [Elo 3] Aplicar regras (Sonnet)
    pedido mínimo, desconto cliente, prazo, frete
       │
       ▼
  [Elo 4] Compor resposta de confirmação (Haiku)
    "Anotando: 30cx Brahma + 20cx Skol = R$ X,
     entrega quinta. Confirma?"
       │
       ▼
  Cliente final responde "confirma"
       │
       ▼
  GATE HITL nível 2: champion vê rascunho, aprova
       │
       ▼
  Tool: criar_pedido_bling(rascunho) → número de pedido
       │
       ▼
  Mensagem ao cliente final: "Pedido #1234 confirmado.
  Entrega quinta. Pix R$ X ou boleto 14 dias?"
       │
       ▼
  Tool: gerar_cobranca_pix() OU agendar_boleto()
═══════════════════════════════════════════════════
```

---

## Nível HITL recomendado

**Início e default permanente para criação no ERP:** Nível 2 (Co-piloto). Champion sempre aprova o rascunho antes de o pedido virar firme no Bling.

**Pode subir para Nível 3 (autopiloto com exceção) em:**

- Clientes **recorrentes conhecidos** (consultar_cliente retorna "≥ 5 pedidos nos últimos 6 meses, zero inadimplência").
- **Valor abaixo do teto** (default R$ 2.000).
- **Sem desconto especial** (apenas RN-02 padrão aplicado).

**Gates de exceção sempre ativos (mesmo em nível 3):**
- Valor > R$ {{TETO_PEDIDO}}.
- Cliente novo (sem histórico).
- Desconto > 5%.
- Pedido com item em ruptura de estoque.
- Pagamento fora das condições padrão.
- Cliente em pendência financeira.

**Nível 4 nunca** para criação de pedido firme. Erro fiscal/financeiro é passivo.

---

## Custo operacional estimado (R$/mês)

**Distribuidora pequena — 30 pedidos/dia, ~900/mês:**

| Item | Mensal |
|---|---|
| Meta WABA | R$ 100–250 |
| Claude API (Sonnet para extração) | R$ 150–280 |
| ERP (Bling Plano X) | já existente cliente |
| Rateio infra | R$ 50–100 |
| **Total** | **R$ 300–630** |

**Distribuidora média — 100 pedidos/dia:**

| Item | Mensal |
|---|---|
| Meta WABA | R$ 350–700 |
| Claude API com caching | R$ 400–700 |
| Rateio infra | R$ 80–150 |
| **Total** | **R$ 830–1.550** |

---

## Tempo de implementação

| Fase DPIA | Dias úteis |
|---|---|
| D — Diagnóstico (focado em pedido) | 7–12 |
| P — Pacote de Contexto (catálogo + regras + integração ERP) | 7–14 |
| I — Prompt + playbook + testes ERP | 10–14 |
| A — Deploy nível 2 + job shadowing + métricas 30d | 10–14 |
| **Total** | **34–54 dias úteis** |

Caso o Módulo 1 (Atendimento) já esteja em produção no cliente, **subtrair 30–40%** do tempo total (catálogo, sessão, infra reaproveitados).

---

## Métricas obrigatórias

1. **Pedidos registrados pelo agente** (volume mensal, % do total de pedidos).
2. **Tempo médio entre primeira mensagem do cliente e pedido firme** (minutos).
3. **Taxa de erro do agente** (pedidos que precisaram ser corrigidos antes do envio).
4. **Taxa de aprovação sem edição** (% de rascunhos que o champion aprova sem mexer).
5. **Receita capturada vs. baseline de pedidos perdidos.**
6. **Inadimplência dos pedidos gerados** (controle de qualidade do filtro de crédito).
7. **Custo total mensal** (WABA + LLM + infra).
8. **NPS do cliente final** sobre velocidade de pedido (pesquisa trimestral).

---

## Erros comuns e como evitar

- **Pular validação contra catálogo.** Resultado: pedido criado com SKU errado, devolução, perda de margem. *Corrige:* tool `consultar_tabela` obrigatória no Elo 2; produto não-encontrado vira pergunta de esclarecimento, nunca chute.
- **Confirmar pedido sem repetir itens.** Resultado: cliente final esperando algo diferente, conflito. *Corrige:* RN-03 sempre — agente repete os itens em texto antes do "confirma?".
- **Aplicar desconto além do permitido.** Resultado: margem destruída. *Corrige:* Bloco 4 do prompt + tool de desconto com teto rígido + escalonamento acima.
- **Criar pedido firme antes da aprovação do champion.** Sem nível 2 ativo no início, agente cria pedido fantasma se cliente cancela depois. *Corrige:* gate HITL antes de `criar_pedido_bling` em modo firme — sempre rascunho primeiro.
- **Ignorar inadimplência do cliente.** Resultado: pedido entregue para quem deve. *Corrige:* tool `consultar_cliente` retorna flag de pendência financeira; gate dispara escalonamento.
- **Tratar item ambíguo como pedido fechado.** "Stella" pode ser Artois 330, 600, Long Neck. *Corrige:* pergunta de esclarecimento; após 1 tentativa sem sucesso, escalar.
- **Subir para nível 3 sem 30 dias de dados.** Resultado: pedido errado vira incidente. *Corrige:* exit-gate documentado.

---

## Variações por vertical

### Distribuidoras / atacado (Polaris)
- Tool central: `criar_pedido_bling(cliente, itens, observacao)`.
- Regras frequentes: pedido mínimo por região, bonificação por volume, roteiro de entrega.
- Métricas extras: % pedidos atendidos no roteiro previsto.

### Varejo / e-commerce conversacional
- Tool central: `criar_pedido_tiny` ou similar.
- Regras frequentes: frete por CEP, prazo por região, devolução em 7 dias.
- Métricas extras: % abandono pós-confirmação.

### Padaria / hortifruti / pequeno varejo de bairro
- Volume pode estar abaixo do mínimo (15/dia). Avaliar com cuidado.
- Tool: pode ser planilha + WhatsApp Web nos casos sem ERP. **Stack alternativa**: planilha → Apps Script → n8n.
- Em PMEs sem ERP, **antes** considerar implementar ERP simples (Tiny) na fase P.

---

## Mapeamento para DPIA

| Fase | Pré-requisitos |
|---|---|
| D | Baseline de pedidos/dia, tempo de digitação, taxa de erro, pedidos perdidos, custo de hora-operador. Reposicionamento do digitador atual decidido. KPI futuro assinado. |
| P | Catálogo do ERP exportado e validado, glossário com sinônimos de produto, regras formais (pedido mínimo, desconto, frete), regras tácitas (descontos por cliente, roteiros), credenciais de produção do ERP validadas em chamada de teste. Política de crédito documentada. |
| I | system-prompt.md + playbook.md, matriz HITL com gates de exceção aprovados, teste de homologação com 30 pedidos reais de histórico (≥ 85% acerto). |
| A | Deploy nível 2 + job shadowing por 5 dias no operador atual, micro-vídeo "como aprovar rascunho", relatórios 30/60/90d, contrato de manutenção ativo. |

---

*Módulo Pedidos via WhatsApp v1.0 — AIOS Profissional 2027.*
