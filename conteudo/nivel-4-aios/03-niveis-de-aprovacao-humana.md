# Aprovação Humana por Nível de Risco (HITL) nos Módulos AIOS

> Referência canônica: seção "Aprovação humana por nível de risco" do `metodo/dpia-canonico.md`. Este documento operacionaliza os 4 níveis dentro dos módulos AIOS.

---

## Os 4 níveis em uma frase cada

| Nível | Nome | Em uma frase |
|---|---|---|
| 1 | **Assistir** | Agente lê e sugere internamente; humano escreve e age. |
| 2 | **Co-piloto** | Agente prepara a ação externa; humano aprova clique-a-clique antes de qualquer efeito. |
| 3 | **Autopiloto com exceções** | Agente executa no fluxo normal; pausa e escala em gatilhos de risco. |
| 4 | **Autônomo com auditoria** | Agente executa tudo; humano audita por amostragem em retrospectiva. |

---

## Tabela operacional — nível inicial recomendado por módulo

A regra: **começar no nível mais conservador, subir por evidência (≥ 90% qualidade por 30 dias + zero incidente + champion concorda).**

| Módulo | Nível inicial recomendado | Pode subir a |
|---|---|---|
| Atendimento WhatsApp | 2 (Co-piloto) | 3 após 4 semanas estáveis |
| Pedidos via WhatsApp | 2 (Co-piloto) | 3 para cliente recorrente conhecido; valor sempre tem gate |
| Agendamento (confirmar/lembrar) | 3 (Autopiloto com exceções) | 4 só para lembrete puro |
| Agendamento (remarcar) | 2 (Co-piloto) | 3 após 6 semanas |
| Cobrança (1ª mensagem) | 2 (Co-piloto) | 3 após 30 dias |
| Cobrança (escalonamento) | 1 (Assistir) | 2 só após retro completa |
| Proposta Comercial | 1 (Assistir, agente gera, humano envia) | 2 nunca para envio sem revisão humana |
| Resumo de Reunião | 4 desde início (uso interno) | — |

**Regra de ouro:** quando o agente fala com cliente externo ou cria registro financeiro/fiscal, **piso é nível 2**. Quando o agente trabalha em rascunho interno, pode iniciar em 3 ou 4.

---

## Como operacionalizar cada nível dentro do n8n

### Nível 1 — Assistir

**Setup técnico no n8n:**
- Workflow termina com node **n8n Chat / Slack / WhatsApp do champion** mostrando a sugestão.
- **Não há node que envie mensagem externa, crie registro em ERP, ou dispare cobrança.**
- O humano lê, copia ou descarta.

**Indicadores no playbook:**
- Métrica: tempo entre sugestão do agente e ação humana.
- Métrica: % de sugestões aproveitadas (sinal de qualidade do prompt).

**Exemplo:** módulo Cobrança em primeira semana — agente identifica inadimplentes, sugere mensagem por perfil, mas humano envia a mensagem manualmente.

---

### Nível 2 — Co-piloto

**Setup técnico no n8n:**
- Workflow tem **gate de aprovação** explícito antes de qualquer ação externa.
- Implementação típica: node "Send and Wait for Response" (n8n nativo) ou webhook customizado para painel do champion.
- O humano vê: contexto da conversa + ação proposta + botões `[Aprovar] [Editar] [Recusar] [Assumir]`.

**Indicadores no playbook:**
- Métrica: % de aprovações sem edição (alta = agente bem treinado).
- Métrica: latência humana (tempo entre proposta e clique).

**Exemplo:** módulo Atendimento WhatsApp em primeiras 4 semanas — agente responde a cada turno via co-piloto. Cliente final percebe resposta como humana, mas todo turno passa por aprovação da Cristiane.

**Cuidado:** nível 2 cansa rápido. Se a equipe interna aprova 200 mensagens/dia, ela vai começar a clicar "Aprovar" no automático e perder a qualidade do controle. Sinal de que está na hora de subir para nível 3 com gates de risco.

---

### Nível 3 — Autopiloto com exceções

**Setup técnico no n8n:**
- Fluxo normal: agente executa direto (envio, criação de registro, etc.).
- **Gates de exceção** desviam para co-piloto:
  - Valor > R$ X (configurável por cliente).
  - Lista de palavras-chave de risco (reclamação, jurídico, urgência).
  - Confidence score do LLM abaixo de 70%.
  - Cliente VIP (lista configurável).
  - Fora do horário comercial.
- Gate dispara node `escalar_humano(motivo, contexto)` — mesmo node usado no nível 2.

**Indicadores no playbook:**
- Métrica: % de interações que disparam exceção (≤ 15% saudável).
- Métrica: % de override humano (humano corrige depois) — alvo ≤ 10%.

**Exemplo:** módulo Atendimento WhatsApp maduro — agente responde sozinho perguntas de preço, prazo, status. Mas escala para a Cristiane se cliente diz "cancelar", se pedido > R$ 5.000, ou se confidence < 70%.

---

### Nível 4 — Autônomo com auditoria

**Setup técnico no n8n:**
- Fluxo executa sem interrupção.
- **Job de auditoria** roda em paralelo: amostra 5–10% das interações, expõe ao champion em painel semanal.
- Champion marca interações erradas → vira insumo para ajuste de prompt na manutenção.

**Indicadores no playbook:**
- Métrica: % de interações revisadas (mínimo 5% amostral).
- Métrica: taxa de erro detectado na auditoria — alvo ≤ 2%.
- Métrica: tempo entre erro detectado e correção do prompt.

**Exemplo:** módulo Agendamento (lembrete de consulta) na Clínica MoveBem — agente envia lembrete 24h antes para 80 pacientes/dia. Auditoria semanal: 10 amostras revisadas pela champion. Nenhum humano no loop em tempo real.

**Limite firme:** nível 4 nunca para criar pedido, cobrar, emitir NF-e, dar desconto, ou conversar com VIP. Nível 4 é para ações **reversíveis, padronizadas, alto volume, baixo risco financeiro**.

---

## Matriz HITL por ação (modelo para o cliente assinar)

Toda matriz HITL aprovada na fase I.3 é uma versão preenchida desta tabela:

| # | Ação do agente | Nível aprovado | Gatilho de exceção (se nível 3) | Quem aprova |
|---|---|---|---|---|
| 1 | Responder dúvida de preço | 3 | confidence < 70% | Champion |
| 2 | Confirmar pedido no ERP | 2 | sempre humano | Champion |
| 3 | Enviar cobrança Pix | 2 | sempre humano | Financeiro |
| 4 | Aplicar desconto até X% | 3 | acima de X% escala | Dono |
| 5 | Resposta a "obrigado/tchau" | 4 | — | (auditoria) |
| 6 | Lembrar consulta D-1 | 4 | — | (auditoria) |
| 7 | Remarcar consulta | 2 | sempre humano | Champion |
| 8 | Confirmar consulta D+0 | 3 | confidence < 70% | Champion |

A matriz preenchida vira anexo do contrato. Mudança de nível **só por escrito**, na revisão mensal.

---

## Quando subir de nível

Critérios cumulativos. Todos os três precisam estar presentes:

1. **Métrica de qualidade ≥ 90% por 30 dias consecutivos.** Mede-se com base na taxa de override humano + taxa de marcação "incorreta" na auditoria amostral.
2. **Zero incidente crítico no período.** Incidente crítico = qualquer erro que (a) causou reclamação formal do cliente final, (b) gerou perda financeira detectável, ou (c) violou regra de conformidade (LGPD, fiscal).
3. **Champion concorda explicitamente.** Conversa de 15 minutos com pergunta direta: "você está confortável em deixar X funcionando sozinho com auditoria semanal em vez de aprovar uma a uma?"

Se um dos três falha, fica no nível atual. Não há "vamos tentar e ver" — esse é o atalho que vira incidente público.

---

## Quando descer de nível (recuperação)

Critérios para descer:

- Métrica de qualidade caiu abaixo de 80% por 14 dias.
- Incidente crítico ocorreu (descer imediatamente, sem discussão).
- Champion deixou a empresa e o substituto ainda não foi treinado.
- Cliente final reclamou formalmente.
- Implementador detectou regressão (mudança de política do cliente não refletida no prompt).

Descer de nível **não é fracasso** — é o desenho funcionando. O playbook do agente deve documentar a descida como evento operacional rotineiro.

---

## Sinais de matriz HITL mal desenhada

- **Todas as ações no mesmo nível.** Provável: ninguém pensou. Cada ação tem risco distinto.
- **Nível 3 ou 4 desde o dia 1.** Provável: cliente impaciente e implementador cedendo. Reverter.
- **Lista de gatilhos com menos de 5 itens.** Provável: pesquisa insuficiente da fase D. Voltar.
- **"O agente decide sozinho quando escalar."** Não. Gatilhos são regras explícitas + confidence. Decisão sobre autonomia é decisão de negócio, não decisão do LLM.

---

## Como apresentar HITL ao cliente

Linguagem para conversa de aprovação na fase I:

> "Cada ação do nosso assistente tem um nível de autonomia. Comecemos com o seguinte: ele responde dúvida de preço sozinho, mas para criar pedido ele te chama. Em 30 dias, com dados, a gente revê. Se ele tiver acertado em pelo menos 9 de cada 10 vezes, sobe um degrau. Se errar, desce. Isso está aprovado por escrito antes de irmos ao ar."

Frase para evitar: "ele já é inteligente, pode ficar autônomo". O cliente vai te cobrar essa frase no primeiro erro.

---

*Aprovação Humana por Nível AIOS v1.0 — operacionalização dos 4 níveis em produção PME.*
