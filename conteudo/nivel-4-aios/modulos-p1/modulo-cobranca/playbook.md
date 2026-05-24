# Playbook — Agente de Cobrança Ativa — {{NOME_EMPRESA}}

**Versão:** 1.0 — preencher
**Próxima revisão:** {{DATA}}
**Champion:** {{NOME_CHAMPION}} (financeiro) · {{WHATSAPP_CHAMPION}}
**Decisor sênior:** {{NOME_DONO_OU_SOCIO}}
**Implementador:** {{NOME_IMPLEMENTADOR}} · {{WHATSAPP_IMPLEMENTADOR}}
**Canal de incidente:** {{CANAL_INCIDENTE}}

---

## 1. Por que este playbook existe

Cobrança é o módulo de **maior risco reputacional e regulatório** do AIOS. Quatro perguntas:

1. **O que o agente compõe sozinho? (composição, não envio)**
2. **Quando o agente me chama?**
3. **O que eu faço quando ele me chama?**
4. **Como eu corrijo o agente quando ele erra?**

Se você não consegue responder em 5 minutos, **bloqueie envios** e avise o implementador.

---

## 2. O que o agente faz sozinho

| # | Ação | Nível | Condição |
|---|---|---|---|
| 1 | Importar inadimplentes do ERP financeiro (cron diário) | 4 | sempre |
| 2 | Verificar pagamento recente (não cobrar quem pagou) | 4 | sempre |
| 3 | Classificar perfil (voting 3x) | 4 | sempre (auditoria semanal) |
| 4 | Compor mensagem por perfil + etapa da régua | 4 | sempre (em rascunho) |
| 5 | **Disparar mensagem ao devedor** | **2 (co-piloto)** | sempre champion aprova |
| 6 | Registrar promessa de pagamento | 4 | resposta com data clara |
| 7 | Registrar pausa (vulnerabilidade temporária) | 3 | escala se for sinal de vulnerabilidade real |
| 8 | Gerar Pix após promessa | 2 | sempre champion aprova |
| 9 | Atualizar status no ERP financeiro | 3 | após pagamento confirmado |
| 10 | Tratar contestação/hostilidade | **nunca** | sempre escala |

---

## 3. Quando o agente me chama

### 3.1 Gatilhos jurídicos (escala obrigatória, sem envio)

- "Procon", "advogado", "consumidor", "processar", "abusivo", "indevido".
- Histórico de litígio.
- Cliente em recuperação judicial.

### 3.2 Gatilhos de vulnerabilidade (escala obrigatória)

- Idoso, deficiente, doença grave declarada, luto, sinal de baixa renda.

### 3.3 Gatilhos de relação (escala obrigatória)

- Cliente VIP ou estratégico.
- Valor > R$ {{TETO}}.
- Cliente menciona sair da carteira.

### 3.4 Gatilhos operacionais

- Verificar_pagamento_recente positivo (cliente já pagou).
- Cliente solicita parcelamento.
- Cliente solicita desconto fora da política.
- Confidence < 70%.
- Cliente em pausa registrada.

### 3.5 Painel diário

Toda manhã o champion abre o painel n8n e vê:

- **Lote do dia** (lista de cobranças propostas pelo agente).
- Para cada item: cliente, valor, etapa da régua, perfil, mensagem proposta.
- Botões por item: `[Aprovar e enviar]` `[Editar antes de enviar]` `[Pular hoje]` `[Pausar 7 dias]`.
- Botão geral: `[Aprovar lote inteiro]` (apenas se todos forem perfil cooperativo e valor < R$ 1.500).

---

## 4. Procedimentos do champion

### 4.1 Aprovar lote diário

1. Abrir painel pela manhã (9h-10h).
2. Conferir lista, especialmente flag de vulnerabilidade/hostilidade (esses já vêm separados acima).
3. Para cooperativos pequenos: aprovar em lote.
4. Para esquivos: ler cada mensagem antes de aprovar.
5. Para hostis e vulneráveis: assumir manualmente (não passar pelo agente).

### 4.2 Contestação hostil

1. **Ler todo o histórico antes de responder.**
2. Ligar para o cliente (não responder por WhatsApp — escalada).
3. Ouvir contestação completa, registrar.
4. Se cliente tem razão, regularizar e pedir desculpa.
5. Se não tem, comunicar com firmeza educada e direcionar ao jurídico se aplicar.

### 4.3 Vulnerabilidade

1. Ligar (não WhatsApp).
2. Entender situação real.
3. Oferecer pausa ampla (30+ dias), parcelamento estendido, ou negociação.
4. Documentar a decisão com clareza para futuras manutenções.

### 4.4 Parcelamento solicitado

1. Verificar política vigente.
2. Aprovar até teto pré-definido sem consultar dono.
3. Acima do teto, consultar dono.
4. Registrar parcelamento no ERP financeiro.
5. Comunicar ao cliente com data e valor das parcelas.

### 4.5 Cliente que já pagou (verificar_pagamento_recente positivo)

1. O agente já vai pedir desculpa automaticamente.
2. Você confirma no sistema.
3. Se for repetitivo, ajustar tool ou frequência da importação.

### 4.6 "Estou falando com robô?"

> "Oi! Aqui é da {{NOME_EMPRESA}} — uso um sistema com IA para organizar as cobranças, mas {{NOME_CHAMPION}} está por perto sempre que precisar de algo mais específico. Como posso ajudar?"

---

## 5. Correção do agente

### 5.1 Tom inadequado detectado

1. **Pause o envio do dia** se for sistêmico.
2. Avise o implementador no mesmo dia.
3. Implementador revisa Bloco 4 (templates) e Bloco 8 (proibições) na manutenção.

### 5.2 Classificação de perfil errada

1. Marque `[Perfil errado]` no painel.
2. Implementador ajusta voting prompt e few-shot.

### 5.3 Régua errada (cobrou no dia errado)

1. Verifique configuração do cron.
2. Verifique importação do ERP financeiro.
3. Reportar ao implementador.

---

## 6. Diário de aprendizado

| Data | Cliente | Etapa | Perfil | O que aconteceu | Ação | Ajuste |
|---|---|---|---|---|---|---|
| | | | | | | |

Cobrança sem diário = abandono iminente.

---

## 7. Plano de rollback (15 min)

1. n8n: `{{URL_N8N}}` → workflow "cobranca-diaria" → Deactivate.
2. **Confirmar que nenhuma mensagem está em fila.**
3. Avisar implementador.
4. Cobrança volta a ser manual pelo financeiro até nova ordem.
5. Retorno só com OK escrito champion + dono.

**Em caso de incidente regulatório (cliente reclamou no Procon):** rollback imediato.

---

## 8. Métricas do mês

| Métrica | Mês atual | Mês anterior | Baseline (D) | Meta 90d |
|---|---|---|---|---|
| % inadimplentes contactados na régua | | | | ≥ 95% |
| Tempo champion em cobrança (h/sem) | | | | ≤ 2 h |
| Taxa de recuperação D+30 | | | | ≥ 50% |
| Tempo médio de recuperação (dias) | | | | ≤ 18 |
| Promessas cumpridas (%) | | | — | ≥ 70% |
| Reclamações por tom | | | | 0 |
| % escalonamento por hostilidade | | | — | < 8% |
| Incidentes CDC/regulatórios | | | 0 | 0 (não-negociável) |
| Custo total | | | — | |

---

# Exemplo preenchido — Verbo Contabilidade (extrato)

**Versão:** 1.2 — atualizado em 28/jul/2026.
**Champion:** Patricia Souza (financeiro) · (11) 9 1234-XXXX.
**Sócia:** Cláudia Verbo · (11) 9 4321-XXXX.
**Implementador:** Nei Maldaner.

## 3.1 Gatilhos jurídicos Verbo
- "Procon", "advogado", "consumidor", "abusivo" → Cláudia.
- Histórico de litígio → jurídico (Dra. Marina, escritório parceiro).
- Recuperação judicial → Cláudia + jurídico.

## 6. Diário Patricia (extrato)

| Data | Cliente | Etapa | Perfil | O que aconteceu | Ação | Ajuste |
|---|---|---|---|---|---|---|
| 03/jul | Padaria Sol | 1ª | coop. | Pagou na hora ao receber Pix | OK | Modelo. |
| 09/jul | Sr. Antonio (PF) | 2ª | esquivo | "tô internado" | Pausei 30 dias | Ajustar prompt para reconhecer "internado" como vulnerab. |
| 14/jul | Auto Posto AB | 2ª | hostil | "Vão pro Procon" | Cláudia ligou, regularizou — havia erro nosso | Verificar tool consultar_pagamento_recente; cliente havia pagado mas não conciliado. |
| 22/jul | Petshop K9 | 3ª | esquivo | Pediu 5x | Aprovei 3x conforme política | OK |

## 8. Métricas Verbo — julho 2026

| Métrica | Jul | Jun | Baseline | Meta |
|---|---|---|---|---|
| % contactados | 100% | 87% | 62% | ≥ 95% ✓ |
| Tempo Patricia em cobrança | 1,4 h/sem | 2,2 h/sem | 6 h/sem | ≤ 2 h ✓ |
| Recuperação D+30 | 58% | 51% | 32% | ≥ 50% ✓ |
| Tempo médio recuperação | 14 dias | 17 dias | 32 dias | ≤ 18 ✓ |
| Promessas cumpridas | 76% | 71% | — | ≥ 70% ✓ |
| Reclamações por tom | 0 | 0 | 2/mês | 0 ✓ |
| Escalonamento hostilidade | 5,2% | 6,1% | — | < 8% ✓ |
| Incidentes CDC | 0 | 0 | 0 | 0 ✓ |
| Custo | R$ 180 | R$ 195 | — | < R$ 300 ✓ |

---

## Dicas

- **Painel diário aprovado em até 30 min** — bloco fixo na agenda da Patricia.
- **Nunca aprovar em lote quando há perfis esquivo/hostil misturados.**
- **Cobrança é o módulo com maior risco — manutenção mensal precisa revisar 5 casos de cada perfil pelo menos.**
- **Plano de rollback testado nas duas primeiras semanas.**

---

*Playbook do Módulo Cobrança v1.0 — AIOS Profissional 2027.*
