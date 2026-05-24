# Playbook — Agente de Pedidos via WhatsApp — {{NOME_EMPRESA}}

**Versão:** 1.0 — preencher
**Próxima revisão:** {{DATA}}
**Champion:** {{NOME_CHAMPION}} · {{WHATSAPP_CHAMPION}}
**Implementador:** {{NOME_IMPLEMENTADOR}} · {{WHATSAPP_IMPLEMENTADOR}}
**Canal de incidente:** {{CANAL_INCIDENTE}}

---

## 1. Por que este playbook existe

Quatro perguntas operacionais:

1. **O que o agente registra sozinho?** (rascunho de pedido)
2. **Quando o agente me chama?** (gates de exceção)
3. **O que eu faço quando ele me chama?** (aprovar/editar/recusar rascunho)
4. **Como eu corrijo o agente quando ele erra?**

Se você não consegue responder em 5 minutos, avise o implementador.

---

## 2. O que o agente faz sozinho

| # | Ação | Nível | Condição |
|---|---|---|---|
| 1 | Interpretar texto livre do pedido | 4 | sempre |
| 2 | Consultar catálogo, estoque, frete | 4 | sempre |
| 3 | Aplicar regras formais de desconto (RN-02) | 3 | dentro da regra |
| 4 | Aplicar regras tácitas RT-01 (clientes específicos) | 3 | CNPJ na lista |
| 5 | Pedir esclarecimento sobre item ambíguo | 4 | sempre |
| 6 | Repetir itens em texto para confirmação do cliente | 4 | sempre |
| 7 | Criar pedido RASCUNHO no ERP | 3 | cliente confirmou itens em texto |
| 8 | Aprovar e firmar pedido | **nunca** | sempre exige champion |
| 9 | Gerar cobrança Pix | **nunca** | sempre exige champion |
| 10 | Aplicar desconto > 5% (fora RT-01) | **nunca** | sempre escala |

Esta tabela espelha a Matriz HITL aprovada na fase I.3.

---

## 3. Quando o agente me chama

### 3.1 Gatilhos automáticos

- **Valor > R$ {{TETO_PEDIDO}}** → {{NOME_DECISOR}}.
- **Cliente novo** (não cadastrado no ERP) → champion para qualificação.
- **Cliente com pendência financeira** → financeiro/dono.
- **Desconto > 5% fora RT-01** → dono.
- **Item fora do catálogo + cliente insiste** → champion.
- **Pedido com 15+ itens diferentes** → champion (complexidade).
- **Prazo de pagamento fora da política** → financeiro.
- **CEP fora de área** → champion para avaliar entrega especial.
- **Palavras de risco** ("cancelar", "Procon", etc.) → dono/responsável.

### 3.2 Painel de aprovação

Cada rascunho de pedido criado pelo agente aparece no painel n8n com:

- Cliente (nome, CNPJ, região, histórico curto).
- Itens (SKU, nome oficial, quantidade, preço unitário, subtotal).
- Regras aplicadas (RN-02, RT-01 se aplicável).
- Desconto e total.
- Forma de pagamento (Pix/boleto).
- Prazo de entrega.
- Botões: `[Aprovar e firmar]` `[Editar rascunho]` `[Recusar e responder eu]` `[Pedir mais info ao cliente]`.

**Aprovação firma o pedido no ERP + dispara cobrança (Pix/boleto) + envia confirmação final ao cliente automaticamente.**

---

## 4. Procedimentos do champion

### 4.1 Aprovar rascunho

1. Conferir cliente e histórico.
2. Conferir itens, preço, desconto.
3. Se tudo OK → `[Aprovar e firmar]`.
4. Painel automaticamente cria pedido firme no ERP, dispara cobrança e responde ao cliente.

### 4.2 Editar rascunho

1. Clicar `[Editar rascunho]`.
2. Ajustar item, quantidade ou desconto.
3. Confirmar com cliente final na thread se houve alteração relevante.
4. Aprovar.

### 4.3 Cliente com pendência financeira

1. **Nunca informar a pendência no WhatsApp** (LGPD + relação).
2. Conferir no ERP qual o débito e a margem.
3. Negociar: regularização parcial antes de novo pedido, ou pedido apenas à vista.
4. Conversa por telefone ou WhatsApp pessoal, não pelo número público.

### 4.4 Item fora do catálogo

1. Avaliar se vale a pena pedir do fornecedor / fazer compra pontual.
2. Se sim, atualizar catálogo no ERP + base de conhecimento.
3. Se não, recusar gentilmente.

### 4.5 Quando o cliente cancela depois da aprovação

1. Cancelar pedido no ERP (status "cancelado pelo cliente").
2. Cancelar cobrança Pix se ainda não paga.
3. Se já paga: registrar estorno (procedimento bancário).
4. Registrar no Diário de aprendizado.

---

## 5. Correções e feedback

### 5.1 Erro de interpretação do agente

- Marcar a interação como `[Incorreta]` no painel.
- Escrever o que o agente deveria ter entendido.
- Implementador ajusta vocabulário (Bloco 3) ou few-shot (Bloco 6) na manutenção.

### 5.2 Mudança de regra/preço

- Atualizar no ERP primeiro.
- Validar que a tool `consultar_tabela_precos` retorna o novo valor.
- Se mudança estrutural (nova regra), atualizar Mapa de Processos + avisar implementador.

### 5.3 Novo padrão de pedido (vocabulário novo)

- Registrar no Diário.
- Implementador adiciona ao Bloco 3 na manutenção.

---

## 6. Diário de aprendizado

| Data | Cliente | Caso | O que aconteceu | Ação | Ajuste |
|---|---|---|---|---|---|
| | | | | | |

---

## 7. Plano de rollback (15 min)

1. **n8n:** `{{URL_N8N}}` → workflow "{{WORKFLOW_PEDIDOS}}" → Deactivate.
2. **Reativar fallback humano** (pedidos voltam a ser digitados manualmente pelo operador).
3. **Aviso no grupo interno.**
4. **Avisar implementador.**
5. **Retorno só com OK escrito** de champion + dono.

Permissão de rollback: {{NOMES_AUTORIZADOS}}.

---

## 8. Métricas do mês

| Métrica | Mês atual | Mês anterior | Baseline (D) | Meta 90d |
|---|---|---|---|---|
| Pedidos registrados pelo agente | | | — | |
| % do total de pedidos | | | — | |
| Tempo médio mensagem→pedido firme (min) | | | | |
| Taxa de aprovação sem edição | | | — | ≥ 80% |
| Erros de digitação no ERP | | | | < 2% |
| Pedidos perdidos por demora | | | | < 1/dia |
| Inadimplência dos pedidos do agente | | | | ≤ baseline geral |
| Receita capturada vs perdida do baseline | | | | |
| Custo total | | | | |

---

# Exemplo preenchido — Playbook Polaris Bebidas (extrato)

**Versão:** 1.2 — atualizado em 25/junho/2026.
**Champion:** Cristiane Bertoldi.
**Decisor de valor:** Reinaldo Cavalcanti.
**Implementador:** Nei Maldaner.

## 2. O que o agente Polaris faz sozinho

| # | Ação | Nível | Condição |
|---|---|---|---|
| 1 | Interpretar pedido em texto livre | 4 | sempre |
| 2 | Consultar Bling (tabela, estoque, cliente) | 4 | sempre |
| 3 | Aplicar 3% à vista (RN-02) | 3 | automático |
| 4 | Aplicar 7% Casa do Carlos (RT-01) | 3 | CNPJ exato |
| 5 | Pedir esclarecimento (Stella → qual formato) | 4 | sempre |
| 6 | Criar rascunho no Bling | 3 | cliente confirmou itens |
| 7 | Aprovar e firmar pedido | nunca | Cristiane sempre |
| 8 | Gerar Pix via Asaas | nunca | Cristiane sempre após firmar |
| 9 | Desconto > 5% | nunca | Reinaldo via escala |

## 3.1 Gatilhos Polaris

- Pedido > R$ 5.000 → Reinaldo.
- Cliente novo → Cristiane.
- Pendência financeira → Reinaldo (sem mencionar ao cliente).
- Desconto > 5% fora RT-01 → Reinaldo.
- Cerveja sem álcool sem marca → Cristiane via escala.

## 6. Diário de aprendizado Polaris (extrato)

| Data | Cliente | Caso | O que aconteceu | Ação | Ajuste |
|---|---|---|---|---|---|
| 12/jun | Bar do Zé | Pediu "uma de Stella" | Agente perguntou formato 3x sem resolver | Cristiane assumiu, era unidade não caixa | Adicionar exemplo de "uma" = unidade, fora do mínimo |
| 18/jun | Comercial Pinheiros | 50cx Brahma + 30cx Skol | Aprovado, pedido fechou em 2 min | — | Modelo de cliente ideal |
| 21/jun | Mercado Bom Preço | Tem pendência R$ 1.800 | Agente escalou silencioso | Reinaldo cobrou regularização | Conferido — procedimento correto |

## 8. Métricas Polaris — junho 2026

| Métrica | Junho | Maio | Baseline (D) | Meta 90d |
|---|---|---|---|---|
| Pedidos pelo agente/dia | 22 | 16 | — | 25 ✓ |
| % do total | 73% | 58% | — | 70% ✓ |
| Tempo médio mensagem→firme | 2,8 min | 4,2 min | 18 min | ≤ 4 min ✓ |
| Taxa de aprovação sem edição | 86% | 79% | — | ≥ 80% ✓ |
| Erros no Bling | 1,1% | 1,8% | 5% | < 2% ✓ |
| Pedidos perdidos por demora | 0,6/dia | 1,2/dia | 3/dia | < 1/dia ✓ |
| Receita capturada | +R$ 19.800 | +R$ 14.500 | (perda R$ 24k) | +R$ 18k ✓ |

---

## Dicas de uso

- Painel de aprovação precisa estar SEMPRE aberto durante o expediente. Cristiane com o painel em uma aba do navegador.
- Recusar rascunho não é falha do agente — é o sistema funcionando.
- Toda manutenção mensal revisa as 3 maiores categorias de override (regras desatualizadas geralmente).
- Plano de rollback testado uma vez no primeiro mês.

---

*Playbook do Módulo Pedidos via WhatsApp v1.0 — AIOS Profissional 2027.*
