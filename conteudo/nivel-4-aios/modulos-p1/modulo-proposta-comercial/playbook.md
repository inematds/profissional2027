# Playbook — Agente de Proposta Comercial — {{NOME_EMPRESA}}

**Versão:** 1.0 — preencher
**Próxima revisão:** {{DATA}}
**Champion:** {{NOME_HEAD_COMERCIAL}} · {{WHATSAPP_CHAMPION}}
**Vendedores ativos:** {{LISTA_VENDEDORES}}
**Implementador:** {{NOME_IMPLEMENTADOR}} · {{WHATSAPP_IMPLEMENTADOR}}
**Canal de incidente:** {{CANAL_INCIDENTE}}

---

## 1. Por que este playbook existe

Este módulo é de **uso interno** — quem usa é o vendedor. O playbook responde:

1. **Como o vendedor dá briefing certo?**
2. **O que o agente entrega?**
3. **Como o vendedor revisa antes de enviar?**
4. **Como o time corrige o agente quando ele erra?**

---

## 2. O fluxo de uso pelo vendedor

### 2.1 Dar briefing

Vendedor abre formulário n8n (ou WhatsApp do bot interno) e preenche/manda:

**Campos mínimos:**
- Nome do contato + empresa + setor + cidade.
- Dor declarada (frase do cliente, não interpretação).
- Contexto (o que está acontecendo hoje).
- Objetivo (com número se mencionado).
- Prazo desejado.
- Orçamento aproximado (se mencionado).

**Campos opcionais:**
- Restrições.
- Stakeholders mencionados.
- Notas livres.

### 2.2 Aguardar processamento

O agente roda os 4 elos em paralelo lógico (~30-90 segundos). Vendedor recebe PDF no painel n8n com botões:

- `[Baixar PDF]`
- `[Editar Markdown antes de gerar novo PDF]`
- `[Aprovar e enviar por e-mail ao cliente]` (apenas se HITL nível 2 ativo)
- `[Descartar e refazer briefing]`

### 2.3 Revisar antes de enviar

Vendedor SEMPRE revisa:

- Nome do cliente está correto?
- Dor capturada corretamente?
- Escopo cobre o que conversamos?
- Preço bate com a tabela atual?
- Validade está correta?
- Exclusões fazem sentido?
- ROI é defensável?

**Tempo médio de revisão:** 3-8 minutos. Se gastar mais que 15 min editando, há problema no briefing ou no prompt — reportar.

### 2.4 Enviar

- Nível 1 (assistir): vendedor baixa PDF e envia ele mesmo por e-mail ou WhatsApp profissional.
- Nível 2 (co-piloto): vendedor clica `[Aprovar e enviar]` → agente dispara e-mail automaticamente para o contato do cliente.

---

## 3. Quando o agente me chama (vendedor)

### 3.1 Briefing incompleto

Se dor, contexto ou objetivo estiverem vazios, o Elo 1 bloqueia e devolve:

> "Briefing incompleto. Por favor adicione: [campos faltantes]. Sem essas informações não consigo desenhar escopo coerente."

Vendedor completa e re-submete.

### 3.2 Evaluator reprovou após 3 iterações

Se o avaliador rejeita a proposta 3 vezes seguidas, o agente entrega o rascunho mais recente com nota:

> "Proposta gerada com observações pendentes: [feedback]. Por favor revisar manualmente."

### 3.3 Tabela de preço retornou inconsistência

Se `consultar_tabela_servicos` retorna ambiguidade ou item não encontrado, agente alerta o vendedor para esclarecer.

---

## 4. Procedimentos do champion (head comercial)

### 4.1 Validação mensal de qualidade

1. Amostrar 10 propostas geradas no mês.
2. Conferir: tom, preço, escopo, ROI.
3. Marcar problemas no painel de auditoria.
4. Reportar ao implementador na manutenção.

### 4.2 Acompanhar taxa de fechamento

1. CRM marca quais propostas viraram contrato.
2. Cruzar com propostas geradas pelo agente vs. propostas manuais.
3. Reportar diferenças trimestralmente.

### 4.3 Treinar vendedor novo

1. Vendedor novo gera 5 propostas-piloto com supervisão.
2. Champion revisa cada uma antes de enviar.
3. Após 5 propostas aprovadas, vendedor opera autônomo no nível HITL definido.

---

## 5. Correção do agente

### 5.1 Tom inadequado (linguagem genérica de marketing)

- Marcar no painel.
- Implementador ajusta Bloco "Regras" do Elo 4 + amplia lista de frases proibidas.

### 5.2 Preço errado

- Verificar tabela de preços (foi atualizada?).
- Verificar tool consultar_tabela_servicos retornando valor certo.
- Reportar ao implementador.

### 5.3 Escopo fora de padrão (entregável vago, falta exclusão)

- Implementador adiciona few-shot real do que é entregável bom.
- Reforça regra no Elo 2.

### 5.4 ROI inflado

- Implementador reforça regra de conservadorismo no Elo 4.
- Adiciona few-shot de ROI bem escrito.

---

## 6. Diário de aprendizado

| Data | Vendedor | Cliente | Caso | O que aconteceu | Ajuste |
|---|---|---|---|---|---|
| | | | | | |

---

## 7. Plano de rollback (15 min)

1. n8n: workflow "proposta-chaining" → Deactivate.
2. Vendedores voltam a escrever propostas manualmente do template Word/Google Docs.
3. Avisar implementador.
4. Sem urgência crítica — proposta manual é caminho conhecido.

---

## 8. Métricas do mês

| Métrica | Mês atual | Mês anterior | Baseline (D) | Meta 90d |
|---|---|---|---|---|
| Tempo médio briefing→PDF (min) | | | | ≤ 25 |
| Propostas/semana enviadas | | | | |
| Taxa de edição pré-envio | | | — | ≤ 30% |
| Taxa de fechamento | | | | |
| Erros de preço/escopo reportados | | | | < 2% |
| Tempo briefing→envio ao prospect | | | | |
| NPS interno | | | — | ≥ +20 |
| Custo total | | | — | |

---

# Exemplo preenchido — Time comercial INEMA.CLUB (uso próprio)

**Versão:** 1.0 — uso interno do programa Profissional 2027.
**Champion:** Nei Maldaner (curador).
**Vendedores:** alunos certificados do Profissional 2027 usando para suas próprias consultorias.

## Diário (extrato)

| Data | Vendedor | Cliente | Caso | Aconteceu | Ajuste |
|---|---|---|---|---|---|
| 12/jul | aluno A | Distribuidora SC | Briefing dizia "automatizar tudo" sem dor concreta | Elo 1 bloqueou pedindo dor específica | OK — funcionando |
| 18/jul | aluno B | Clínica PR | Escopo gerou fase "alinhamento estratégico" sem entregável | Marcado, implementador ajustou Elo 2 | Adicionou exemplo de fase sem entregável proibido |
| 25/jul | aluno C | Contábil SP | Preço veio R$ 18k para projeto de 8 semanas | Vendedor identificou que tabela estava desatualizada | Atualizar tabela mensal |

---

## Dicas

- **Briefing curto e específico vence briefing longo e vago.** Treinar vendedor a capturar a dor real.
- **Revisão de 5 minutos é o sweet spot.** Mais que isso, briefing foi pobre.
- **Nunca envie sem ler — não importa o quanto o agente está bom.**
- **Atualizar tabela de preços mensalmente é não-negociável.**
- **Time comercial precisa ver o agente como ferramenta, não substituto.** Reposicionamento na fase D: "o agente escreve o rascunho, você revisa, edita e fecha a venda."

---

*Playbook do Módulo Proposta Comercial v1.0 — AIOS Profissional 2027.*
