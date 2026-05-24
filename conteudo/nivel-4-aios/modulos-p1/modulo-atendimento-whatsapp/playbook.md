# Playbook — Agente de Atendimento WhatsApp — {{NOME_EMPRESA}}

**Versão:** 1.0 — preencher com data
**Próxima revisão:** {{DATA_PROX_MANUTENCAO}}
**Champion interno:** {{NOME_CHAMPION}} · {{EMAIL_CHAMPION}} · {{WHATSAPP_CHAMPION}}
**Implementador responsável:** {{NOME_IMPLEMENTADOR}} · {{WHATSAPP_IMPLEMENTADOR}}
**Canal oficial de incidente:** {{CANAL_INCIDENTE}}

---

## 1. Por que este playbook existe

Quatro perguntas que este documento responde no dia a dia:

1. **O que o agente faz sozinho?**
2. **Quando o agente me chama?**
3. **O que eu faço quando ele me chama?**
4. **Como eu corrijo o agente quando ele erra?**

Se você (champion) não consegue responder essas 4 perguntas em 5 minutos lendo este playbook, **avise o implementador** — o playbook está desatualizado.

---

## 2. O que o agente faz sozinho

| # | Ação | Nível HITL | Condição |
|---|---|---|---|
| 1 | Responder saudação / despedida | 4 (autônomo, auditoria) | qualquer |
| 2 | Responder dúvida de preço de tabela | 3 (autopiloto com exceção) | item está na tabela vigente, confidence ≥ 70% |
| 3 | Responder dúvida de prazo de pedido existente | 3 | pedido < 90d, cliente identificado |
| 4 | Informar disponibilidade de produto/serviço | 3 | confidence ≥ 70% |
| 5 | Esclarecer dúvida de FAQ (RAG) | 3 | RAG retornou fonte única |
| 6 | Iniciar registro de pedido novo | 2 (co-piloto) | sempre passa pelo champion antes do envio final ao cliente |
| 7 | Aplicar desconto até {{DESCONTO_MAX_AGENTE}}% | 3 | dentro da regra RN-02 |
| 8 | Resposta padrão "estou falando com robô?" | 4 | resposta aprovada (4.4) |
| 9 | Resposta padrão fora de horário | 4 | 22h–6h ou feriado configurado |

Esta tabela espelha a **Matriz HITL aprovada na fase I.3**. Quando muda lá, muda aqui — em até 24h.

---

## 3. Quando o agente me chama (gatilhos de escalonamento)

### 3.1 Gatilhos automáticos (regras fixas)

- **Valor > R$ {{TETO_PEDIDO}}** → {{NOME_DECISOR_VALOR}}.
- **Palavras-chave detectadas:** "cancelar", "Procon", "advogado", "jurídico", "vencido", "estragado", "rua errada", "reclamar", "processar", "danificado" → {{NOME_DECISOR_RECLAMACAO}}.
- **Cliente VIP** (lista anexa em base de conhecimento) → toda interação alerta o champion mesmo se respondida pelo agente.
- **Fora do horário comercial:** 22h–6h e feriados configurados → mensagem-padrão + fila para 6h.
- **Cliente novo** (primeira interação) → resposta saudação + escala para qualificação humana.
- **Idioma diferente de PT-BR** → escala imediato com frase-padrão.

### 3.2 Gatilhos por confidence

- **Confiança < 70%** na resposta → escala com "vou checar e te retorno".
- **Pergunta fora da base de conhecimento** → escala.
- **Ambiguidade após 1 esclarecimento** → escala.
- **Reformulação da mesma pergunta 2x pelo cliente** → escala (sinal de incompreensão).

### 3.3 O que aparece para você quando há escalonamento

Notificação no **{{CANAL_ESCALONAMENTO}}** (n8n Chat ou WhatsApp do champion) com:

- Histórico completo da thread (últimas 20 mensagens).
- Classificação do gatilho (qual motivo disparou).
- Sugestão de resposta do agente (você pode aprovar, editar, ou tomar controle).
- Botões: `[Aprovar e enviar]` `[Editar antes de enviar]` `[Assumir conversa]` `[Recusar e responder eu]`.

---

## 4. Procedimentos do champion por situação

### 4.1 Quando o agente escala "valor grande"

1. Abrir {{ERP}} e conferir histórico do cliente (CNPJ/CPF).
2. Verificar limite de crédito e pendência (se aplicável).
3. Se aprovação adicional necessária, chamar {{NOME_DONO}}.
4. Retornar ao cliente final pela mesma thread, mantendo o tom do agente.
5. Marcar a interação como "resolvida — aprovação humana" no painel n8n.

### 4.2 Quando o agente escala "palavra-chave de reclamação"

1. **Ler toda a thread anterior antes de responder.**
2. Identificar se é reclamação real ou figura de linguagem ("ah meu Deus, vou reclamar!").
3. Se real: **assumir o atendimento humano** até resolução completa. Nunca devolver ao agente no mesmo dia.
4. Documentar a reclamação no {{SISTEMA_DE_TICKETS}}.
5. Reportar a {{NOME_DONO}} em 24h se valor envolvido > R$ {{TETO_REPORTE_DONO}} ou se cliente é VIP.

### 4.3 Quando o agente escala "cliente VIP"

1. Resposta em até 5 minutos sempre.
2. Tratamento por nome.
3. Não há resposta-padrão — cada VIP tem histórico próprio, consultar antes.

### 4.4 Quando o cliente final pergunta "estou falando com robô?"

Resposta padrão aprovada (já no system prompt, Bloco 6 exemplo 8):

> "Oi! Aqui é o assistente da {{NOME_EMPRESA}} — uso um sistema com IA para responder mais rápido, mas {{NOME_CHAMPION}} e o time estão por perto sempre que precisar de algo mais específico. Posso te ajudar com [pergunta original]?"

**Nunca negue. Nunca enrole. Honestidade ganha confiança.**

### 4.5 Quando o agente erra mas não escalou (você percebe sozinho)

1. **Tomar controle da conversa imediatamente** — botão `[Assumir]` no painel.
2. Corrigir com o cliente final, pedindo desculpa breve sem citar "o agente".
3. Registrar o erro na seção 6 (Diário de aprendizado).
4. Se erro grave (preço errado, prazo impossível, dado de outro cliente vazado), **avisar o implementador no mesmo dia**.

---

## 5. Como corrigir o agente (procedimento de feedback)

### 5.1 Correção de resposta pontual

1. No painel n8n Chat, localizar a interação errada.
2. Marcar como `[Incorreta]` e escrever a resposta certa no campo de feedback.
3. O implementador revisa essas marcações semanalmente e ajusta system prompt / base de conhecimento.

### 5.2 Correção de regra

1. Se o erro decorre de regra desatualizada (preço mudou, política mudou), abrir o **Mapa de Processos** (saída da fase P) e atualizar a regra.
2. Marcar a regra atualizada com data e nome do champion.
3. Avisar o implementador — propagação para o system prompt vira tarefa de manutenção (ou no mesmo dia se urgente).

### 5.3 Caso novo nunca visto

1. Registrar o caso no Mapa de Processos, seção "Casos".
2. Se recorrente, propor adição de regra (RT-XX) ou ajuste de fluxo.
3. Discutir na próxima reunião de manutenção mensal.

---

## 6. Diário de aprendizado (atualize toda semana)

| Data | Caso | O que aconteceu | Ação tomada | Ajuste proposto |
|---|---|---|---|---|
| {{DD/MM}} | | | | |

Vira insumo da reunião de manutenção mensal. Champion que chega com diário tem pauta certa.

---

## 7. Plano de rollback (como desligar o agente em 15 min)

Se algo der muito errado:

1. **Pausar fluxo no n8n:** abrir {{URL_N8N}}, workflow "{{NOME_WORKFLOW_PRINCIPAL}}", clicar `[Deactivate]`. Confirmação visível.
2. **Reativar fallback humano:** ativar workflow "{{NOME_WORKFLOW_FALLBACK}}" — mensagem "estamos com mais demanda, retornamos em até X min" + fila para humano.
3. **Avisar o time interno:** mensagem fixa no grupo "{{GRUPO_INTERNO}}" sobre rollback.
4. **Avisar implementador:** WhatsApp direto + e-mail.
5. **Re-engajamento:** o implementador apura a causa em até 2h, propõe correção, e o reativar **só ocorre com aprovação explícita** do champion + dono.

**Permissão de acionar rollback:** {{NOMES_PERMITIDOS_ROLLBACK}}. Outros funcionários: avisam o champion.

**Plano de rollback deve ser testado uma vez no primeiro mês** (combinar horário de baixa com cliente). Vale como exit-gate da fase A.

---

## 8. Métricas do mês (resumo para a reunião com o dono)

Atualizado mensalmente antes da visita do implementador.

| Métrica | Mês atual | Mês anterior | Baseline (fase D) | Meta 90d |
|---|---|---|---|---|
| Tempo médio de resposta (min) | | | | |
| Volume processado pelo agente | | | — | |
| % atendimentos sem humano | | | — | |
| % override humano | | | — | ≤ 15% |
| Pedidos/leads perdidos por demora | | | | |
| NPS do atendimento | | | | |
| Custo total (WABA + LLM + infra) | | | — | |

---

# Exemplo preenchido — Playbook Polaris Bebidas

**Versão:** 1.3 — atualizado em 22/junho/2026 (incorpora desconto sazonal de inverno).
**Champion:** Cristiane Bertoldi · cristiane@polaris.com.br · (47) 9 9999-XXXX.
**Implementador:** Nei Maldaner · (47) 9 8888-XXXX.
**Canal oficial:** WhatsApp do implementador.

## 2. O que o agente Polaris faz sozinho

| # | Ação | Nível | Condição |
|---|---|---|---|
| 1 | Saudação, despedida, agradecimento | 4 | qualquer |
| 2 | Preço produto da tabela | 3 | produto na tabela ativa do Bling, confidence ≥ 70% |
| 3 | Status de pedido | 3 | pedido < 90d, cliente identificado por número WhatsApp |
| 4 | Disponibilidade | 3 | confidence ≥ 70% |
| 5 | FAQ (forma pagamento, área de entrega, horário) | 3 | RAG fonte única |
| 6 | Iniciar pedido no Bling (rascunho) | 2 | sempre Cristiane confirma antes |
| 7 | Aplicar desconto 3% à vista (RN-02) | 3 | automático |
| 8 | Aplicar desconto Casa do Carlos 7% (RT-01) | 3 | CNPJ na lista |
| 9 | Acima de 5% de desconto | 2 | Reinaldo aprova |

## 3.1 Gatilhos automáticos Polaris

- **Pedido > R$ 5.000** → Reinaldo.
- **Desconto pedido > 5%** (fora RT-01) → Reinaldo.
- **Palavras:** "cancelar", "Procon", "advogado", "vencido", "estragado", "rua errada" → Reinaldo (Cristiane se Reinaldo offline).
- **Cliente da lista VIP** (15 nomes, anexo): toda interação alertada à Cristiane mesmo se respondida.
- **22h–6h:** mensagem-padrão + fila para 6h.
- **Cerveja sem álcool sem confirmação de marca:** escala (RN-04).

## 4.1 Procedimento — pedido grande Polaris

1. Cristiane abre Bling, vê histórico do CNPJ.
2. Confere limite de crédito (Bling > Cliente > Aba Financeiro).
3. Cliente novo: pedir prazo + adiantar 50% Pix.
4. Responder mantendo o "tom Polaris" — direto, cordial.
5. Marcar interação "resolvida humano" no n8n Chat.

## 4.4 Resposta padrão "estou falando com robô?" — Polaris

"Oi! Aqui é a Polaris Bebidas — uso um sistema com IA para responder mais rápido, mas a Cristiane e o time estão por perto sempre que precisar. Posso te ajudar com [pergunta original]?"

## 6. Diário de aprendizado (extrato Polaris)

| Data | Caso | O que aconteceu | Ação | Ajuste |
|---|---|---|---|---|
| 17/jun | Cliente pediu "stout artesanal" | Agente disse que não tinha. Tinha — Schornstein Stout. | Cristiane assumiu e enviou foto. | Adicionar sinônimo "stout artesanal" → Schornstein Stout 600. **Feito em 18/jun na base.** |
| 19/jun | Cliente novo pediu prazo 30d | Agente respondeu "padrão 14d", cliente insistiu, escalou. | Reinaldo aprovou 21d dado o volume. | Sem ajuste — escalonamento funcionou. |
| 22/jun | Promoção de inverno entrou | Agente continuou cobrando preço cheio Heineken 0,0 | Cristiane corrigiu 4 vezes em 1 dia. | **Atualizar RN-02 com tabela sazonal. Manutenção do dia 22/jun fez.** |

## 7. Plano de rollback Polaris

1. n8n: app.polaris.n8n.run → workflow "polaris-atendimento" → Deactivate.
2. Reativar "polaris-fallback-humano".
3. Mensagem no grupo "Polaris Equipe": "Agente pausado, atendimento 100% humano por instabilidade."
4. WhatsApp do implementador.
5. Retorno só com OK escrito de Cristiane + Reinaldo.

Permissão: Cristiane, Reinaldo, implementador.

## 8. Métricas Polaris — junho 2026

| Métrica | Junho | Maio | Baseline (D) | Meta 90d |
|---|---|---|---|---|
| Tempo médio resposta | 3,1 min | 4,8 min | 47 min | ≤ 5 min ✓ |
| Volume processado pelo agente | 78% | 64% | — | ≥ 70% ✓ |
| % override humano | 11% | 18% | — | ≤ 15% ✓ |
| Pedidos perdidos | 0,8/dia | 1,6/dia | 4/dia | ≤ 1/dia ✓ |
| NPS atendimento | +32 | +27 | -5 | +20 ✓ |
| Custo total | R$ 480 | R$ 510 | — | < R$ 600 ✓ |

---

## Dicas de uso

- **Co-escreva com o champion.** Se a Cristiane não leu cada parágrafo, o playbook está fraco.
- **Versione e date sempre.**
- **Diário é insumo da manutenção.**
- **Plano de rollback testado uma vez no primeiro mês.**
- **Não terceirize o playbook para a IA gerar.** IA escreve rascunho; operador refina. Playbook gerado por IA sem revisão humana é fantasia.

---

*Playbook do Módulo Atendimento WhatsApp v1.0 — AIOS Profissional 2027.*
