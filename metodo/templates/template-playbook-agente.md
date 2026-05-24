# Template — Playbook do Agente (Saída de P/I)

> **Como usar.** O Playbook do Agente é o **instrumento operacional** do champion interno. Não é manual técnico nem documento institucional — é o que o operador abre durante o expediente para decidir "o agente fez certo? eu preciso intervir? como?". Diferente do System Prompt (que é instrução para a máquina), o Playbook é instrução para o humano. Mora num lugar acessível ao champion (Notion, Google Doc, ou markdown sincronizado), com permissão de edição. **Atualizado em toda manutenção mensal** — playbook congelado é o sinal mais claro de abandono iminente do contrato (Princípio 6 do DPIA Canônico). Tamanho: 8 a 25 páginas em PME típica. Linguagem: direta, em PT-BR, com exemplos reais e screenshots quando possível.

---

# Playbook — Agente [NOME] — [CLIENTE]

**Versão:** [n] — [data]
**Próxima revisão:** [data — em geral todo mês]
**Champion interno:** [nome, e-mail, WhatsApp]
**Implementador responsável:** [nome, e-mail, WhatsApp]
**Canal de incidente:** [WhatsApp do implementador / e-mail / Slack — qual é o caminho oficial]

---

## 1. Por que este playbook existe

Este documento responde quatro perguntas no dia a dia:

1. **O que o agente faz sozinho?** (lista de ações autônomas)
2. **Quando o agente me chama?** (gatilhos de escalonamento)
3. **O que eu faço quando ele me chama?** (decisão + ação)
4. **Como eu corrijo o agente quando ele erra?** (procedimento de feedback)

**Se você (champion) abrir este playbook e não conseguir responder essas 4 perguntas em 5 minutos, avise o implementador — o playbook está desatualizado.**

---

## 2. O que o agente faz sozinho

| # | Ação | Nível HITL | Condição |
|---|------|------------|----------|
| 1 | Responder dúvidas de preço de tabela | 3 (Autopiloto com exceções) | item está na tabela vigente |
| 2 | Confirmar status de pedido pelo número | 4 (Autônomo, auditoria semanal) | cliente conhecido, pedido < 90d |
| 3 | [...] | [...] | [...] |

[Esta tabela espelha a Matriz HITL aprovada na fase I.3 — quando muda lá, muda aqui.]

---

## 3. Quando o agente me chama (gatilhos de escalonamento)

### 3.1 Gatilhos automáticos (regras fixas)

- **Pedido acima de R$ [X]** → escalado para [pessoa].
- **Palavra-chave detectada:** "cancelar", "reclamação", "Procon", "advogado", "jurídico", "vencido", "estragado" → escalado para [pessoa].
- **Cliente VIP** (lista anexa) → toda interação escalada após primeira resposta de cortesia.
- **Fora do horário comercial** (22h–6h) → mensagem automática + fila para 6h.
- **Cliente novo** (primeira interação) → resposta-padrão de boas-vindas + escalado para qualificação humana.

### 3.2 Gatilhos por confidence (quando o agente "não sabe")

- Quando o agente avalia confiança < 70% na resposta, ele responde "vou checar com a equipe e te retorno" e escala.
- Quando o agente identifica pergunta que não está na base de conhecimento, escala.
- Quando há ambiguidade no pedido (ex: "Stella" — pode ser Artois 330, 600, ou long neck) e a pergunta de esclarecimento já foi feita uma vez sem sucesso, escala.

### 3.3 O que aparece para o champion quando há escalonamento

[Descrever a interface: notificação em qual canal, com qual contexto. Exemplo: "Notificação no n8n Chat / WhatsApp do champion com: histórico da conversa, classificação do gatilho, sugestão de resposta do agente, botões 'aprovar', 'editar', 'tomar controle'."]

---

## 4. Procedimentos do champion por situação

### 4.1 Quando o agente escala "pedido grande"
1. Abrir Bling, conferir histórico do cliente.
2. Verificar margem do pedido (se aplicável).
3. Se aprovação adicional necessária, chamar [dono / financeiro].
4. Retornar ao cliente final pela mesma thread, mantendo tom do agente.
5. Marcar a interação como "resolvida — aprovação humana" no painel.

### 4.2 Quando o agente escala "palavra-chave reclamação"
1. Ler toda a thread anterior (não responder antes de ler).
2. Identificar se é reclamação real ou figura de linguagem ("ah meu Deus, vou reclamar!").
3. Se real: assumir o atendimento humano até resolução completa. Nunca devolver ao agente no mesmo dia.
4. Documentar a reclamação no [sistema de tickets / planilha / Bling Observação].
5. Reportar ao dono em 24h se valor envolvido > R$ [X] ou se cliente é VIP.

### 4.3 Quando o agente erra mas não escalou (você percebe sozinho)
1. Tomar controle da conversa imediatamente — usar comando [/assumir ou equivalente].
2. Corrigir com o cliente final, pedindo desculpa breve sem citar "o agente".
3. Registrar o erro no diário de aprendizado (seção 6).
4. Se erro grave (informou preço errado, prometeu prazo impossível, deu dado de outro cliente), avisar implementador no mesmo dia.

### 4.4 Quando o cliente final pergunta "estou falando com robô?"
- Resposta padrão aprovada: "Sou a Polaris — uso um sistema com IA para responder mais rápido, mas tem time humano sempre por perto. Posso te ajudar com [pergunta original]?"
- Nunca negar; nunca enrolar. Honestidade ganha confiança.

---

## 5. Como corrigir o agente (procedimento de feedback)

### 5.1 Correção de resposta pontual
1. No painel [...], localizar a interação com erro.
2. Marcar como "incorreta" e escrever a resposta certa no campo de feedback.
3. O implementador revisa essas marcações semanalmente e ajusta o system prompt / base de conhecimento.

### 5.2 Correção de regra
1. Se o erro decorre de regra desatualizada (preço mudou, política mudou), abrir o Mapa de Processos (seção 3 — Regras formais) e atualizar a regra.
2. Marcar a regra atualizada com data e nome.
3. Avisar o implementador — ele propaga para o system prompt na manutenção seguinte (ou no mesmo dia se urgente).

### 5.3 Caso novo nunca visto
1. Registrar o caso no Mapa de Processos (seção 6 — Casos).
2. Se for tipo recorrente, propor adição de regra (RT-XX) ou ajuste de fluxo.
3. Discutir na próxima reunião de manutenção.

---

## 6. Diário de aprendizado (atualize toda semana)

Tabela curta com 1 linha por incidente ou aprendizado. Vira insumo da manutenção mensal.

| Data | Caso | O que aconteceu | Ação tomada | Ajuste proposto |
|------|------|------------------|--------------|------------------|
| [dd/mm] | [...] | [...] | [...] | [...] |

---

## 7. Plano de rollback (como desligar o agente em 15 min)

Se algo der muito errado:

1. **Pausar fluxo no n8n:** abrir [URL], workflow "[nome]", clicar em "Deactivate". Confirmação visível.
2. **Reativar mensagem automática humana:** ativar workflow "Fallback humano" (mensagem de "estamos com mais demanda, retornamos em até X min").
3. **Avisar o time interno:** mensagem fixa no grupo "[nome]" sobre rollback.
4. **Avisar implementador:** WhatsApp direto + e-mail.
5. **Re-engajamento:** o implementador apura a causa em até 2h, propõe correção, e o reativar só ocorre com aprovação explícita do champion + dono.

**Quem tem permissão de acionar rollback:** [champion, dono]. Outros funcionários: avisam o champion.

---

## 8. Métricas do mês (resumo para a reunião com o dono)

Atualizado pelo champion ou implementador todo mês, antes da visita.

| Métrica | Valor mês atual | Valor mês anterior | Baseline (fase D) | Meta 90d |
|---------|------------------|--------------------|-----------------------|-----------|
| Tempo médio de resposta | [...] | [...] | [...] | [...] |
| Volume processado | [...] | [...] | — | [...] |
| % adoção | [...] | [...] | — | [...] |
| % override humano | [...] | [...] | — | [...] |
| Pedidos perdidos | [...] | [...] | [...] | [...] |
| Receita capturada (vs perda do baseline) | [...] | [...] | [...] | [...] |

---

# Exemplo preenchido — Playbook do Agente Polaris Bebidas

**Versão:** 1.3 — atualizada em 22/junho/2026 (alteração: incorpora desconto sazonal de inverno).
**Champion:** Cristiane Bertoldi · cristiane@polaris.com.br · (47) 9 9999-XXXX.
**Implementador:** [nome] · (47) 9 8888-XXXX.
**Canal de incidente oficial:** WhatsApp do implementador.

## 2. O que o agente faz sozinho

| # | Ação | Nível | Condição |
|---|------|-------|----------|
| 1 | Responder pergunta de preço de produto na tabela | 3 | produto está na tabela ativa do Bling |
| 2 | Informar status de pedido pelo número | 4 | pedido < 90 dias, cliente identificado pelo número WhatsApp cadastrado |
| 3 | Confirmar disponibilidade de produto | 3 | confiança ≥ 70% na resposta do estoque |
| 4 | Criar pedido novo no Bling | 2 (co-piloto) | sempre passa pela Cristiane antes do envio final |
| 5 | Gerar cobrança Pix via Asaas | 2 | sempre confirmação humana |
| 6 | Aplicar desconto até 5% à vista | 3 | até 5% automático; acima escala |
| 7 | Aplicar desconto fixo Casa do Carlos (7%) | 4 | CNPJ na lista RT-01 |
| 8 | Resposta a "obrigado", saudação, despedida | 4 | qualquer |

## 3.1 Gatilhos automáticos

- **Pedido acima de R$ 5.000** → Cristiane.
- **Desconto pedido > 5%** → Reinaldo.
- **Palavras-chave:** "cancelar", "Procon", "advogado", "vencido", "estragado", "rua errada" → Reinaldo (Cristiane se Reinaldo offline).
- **Cliente da lista VIP** (15 nomes, anexo): toda interação alertada à Cristiane mesmo se respondida pelo agente.
- **22h–6h:** mensagem "estamos fora do horário, retornamos a partir das 6h" + fila.
- **Cerveja sem álcool sem confirmação de marca:** escala (RN-04).

## 4.1 Procedimento — pedido grande
1. Cristiane abre Bling, vê histórico do CNPJ.
2. Confere limite de crédito (Bling > Cliente > Aba Financeiro).
3. Se cliente novo, pede prazo + adianta 50% Pix.
4. Cristiane responde mantendo o "tom Polaris" — direto, cordial, sem floreios.
5. Marca a interação como "resolvida humano" no n8n Chat.

## 4.4 Resposta padrão "estou falando com robô?"
"Oi! Sou a Polaris Bebidas — uso um sistema com IA para responder mais rápido, mas a Cristiane e o time estão por perto sempre que precisar de algo mais específico. Posso te ajudar com [pergunta original]?"

## 6. Diário de aprendizado (extrato)

| Data | Caso | O que aconteceu | Ação | Ajuste |
|------|------|------------------|------|--------|
| 17/jun | Cliente pediu "stout artesanal" | Agente disse que não tinha. Tinha — Schornstein Stout. | Cristiane assumiu e enviou foto. | Adicionar "stout" + "stout artesanal" como sinônimo de Schornstein Stout 600. **Feito em 18/jun na base de conhecimento.** |
| 19/jun | Cliente novo pediu prazo 30 dias | Agente respondeu "padrão é 14 dias", cliente insistiu, agente escalou. | Reinaldo aprovou 21 dias dado o volume. | Sem ajuste — escalonamento funcionou. |
| 22/jun | Promoção de inverno entrou | Agente continuou cobrando preço cheio Heineken 0,0 | Cristiane corrigiu manualmente 4 vezes em 1 dia. | **Atualizar regra RN-02 com tabela sazonal. Manutenção do dia 22/jun fez.** |

## 7. Plano de rollback Polaris
1. n8n: app.polaris.ne8n.run → workflow "polaris-atendimento" → Deactivate.
2. Reativar workflow "polaris-fallback-humano".
3. Mensagem no grupo "Polaris Equipe": "Agente pausado, atendimento 100% humano por instabilidade."
4. WhatsApp do implementador.
5. Retorno só com OK escrito de Cristiane + Reinaldo.

Permissão de rollback: Cristiane, Reinaldo, implementador.

## 8. Métricas — Junho 2026

| Métrica | Junho | Maio | Baseline (D) | Meta 90d |
|---------|-------|------|----------------|-----------|
| Tempo médio resposta | 3,1 min | 4,8 min | 47 / 11 min | ≤ 5 min ✓ |
| Volume processado pelo agente | 78% | 64% | — | ≥ 70% ✓ |
| % override humano | 11% | 18% | — | ≤ 15% ✓ |
| Pedidos perdidos | 0,8/dia | 1,6/dia | 4/dia | ≤ 1/dia ✓ |
| Receita capturada | R$ 26.400 | R$ 18.900 | R$ 0 (perda R$ 33.440) | R$ 22.000 ✓ |

---

## Dicas de uso

- **Co-escreva com o champion.** Se a Cristiane não leu cada parágrafo, o playbook está fraco.
- **Versione e date sempre.** Versão antiga acessível por histórico do Git/Notion é prova de manutenção viva.
- **Diário de aprendizado é insumo da manutenção.** Quem chega na reunião mensal com o diário em mãos sempre tem a pauta certa.
- **Plano de rollback deve ser testado uma vez.** Combine com o cliente fazer um teste cego de desligamento em horário de baixa, no primeiro mês. Vale como exit-gate de A.
- **Não terceirize o playbook para a IA gerar.** A IA escreve um rascunho; o operador refina. Playbook gerado por IA sem revisão humana é fantasia.
