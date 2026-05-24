# Playbook — Agente de Resumo de Reunião — {{NOME_EMPRESA}}

**Versão:** 1.0 — preencher
**Próxima revisão:** {{DATA}}
**Champion:** {{NOME_CHAMPION}} (ops ou lead do time) · {{WHATSAPP_CHAMPION}}
**Time alvo:** {{LISTA_TIME}}
**Implementador:** {{NOME_IMPLEMENTADOR}} · {{WHATSAPP_IMPLEMENTADOR}}
**Canal de incidente:** {{CANAL_INCIDENTE}}

---

## 1. Por que este playbook existe

Quatro perguntas:

1. **Como configuro a reunião para o agente processar?**
2. **O que o agente faz sozinho?**
3. **Como reviso e corrijo?**
4. **Quando o agente NÃO deve processar?**

---

## 2. Como configurar uma reunião

### 2.1 Sistema de transcrição

- Otter.ai (recomendado para PT-BR de qualidade) — convidar `notetaker@otter.ai` para a sala.
- Fireflies (alternativa) — `notetaker@fireflies.ai`.
- Gravação local + upload manual para o n8n via formulário (fallback).

### 2.2 Tag de sensibilidade

No agendamento (Google Calendar / Outlook), adicionar tag no título:

- `#hitl4` (default — agente distribui automaticamente).
- `#hitl2` (champion revisa antes de distribuir — uso obrigatório para reuniões abaixo).

**Reuniões SEMPRE com `#hitl2`:**
- C-level discutindo demissão, fusão, decisão financeira sensível.
- 1:1 com feedback delicado.
- Reunião com cliente externo cujo conteúdo será repassado a ele.
- Entrevista de candidato.
- Reunião com dado pessoal sensível.

### 2.3 Tipo de reunião

Tag adicional no título:
- `#kickoff`, `#weekly`, `#1on1`, `#cliente`, `#retro`, `#planning`.

Define template específico aplicado pela agregação.

---

## 3. O que o agente faz sozinho

| # | Ação | Nível | Condição |
|---|---|---|---|
| 1 | Receber transcrição via webhook | 4 | sempre |
| 2 | Extrair decisões, ações, riscos, próximos passos (4 paralelos) | 4 | sempre |
| 3 | Agregar em ata estruturada | 4 | sempre |
| 4 | Distribuir por e-mail aos participantes | 4 | tag #hitl4 |
| 5 | Distribuir ao grupo WhatsApp interno | 4 | tag #hitl4 |
| 6 | Criar cards no PM (Trello/Asana) para cada ação | 4 | tag #hitl4 |
| 7 | Aguardar champion antes de distribuir | 2 | tag #hitl2 |
| 8 | Distribuir ata a quem não estava na reunião | nunca | sempre |

---

## 4. Quando o agente me chama (champion)

### 4.1 Tag #hitl2 detectada

Painel n8n mostra:
- Ata gerada.
- Botões: `[Aprovar e distribuir]` `[Editar antes de distribuir]` `[Reter (não distribuir)]`.

Champion revisa em até 1h após o fim da reunião.

### 4.2 Inconsistência no JSON intermediário

Se algum dos 4 paralelos retorna formato inválido ou conteúdo problemático, o agregador alerta o implementador. Reunião fica pendente.

### 4.3 Transcrição com qualidade ruim

Se transcrição tem < 70% de qualidade detectada (muitos `[inaudível]`, frases truncadas), o agente alerta:

> "Transcrição com qualidade insuficiente. Ata gerada pode ter falhas. Recomenda-se revisão humana antes de distribuir."

Champion decide se aprova ou pede gravação manual de quem participou.

---

## 5. Procedimentos do champion

### 5.1 Auditoria semanal (não-negociável em nível 4)

1. Sexta-feira, abrir 5 atas geradas na semana (sorteio).
2. Conferir contra a memória dos participantes:
   - Decisões capturadas corretamente?
   - Ações com responsável + prazo certos?
   - Algo crítico não capturado?
3. Marcar problemas no painel.
4. Reportar ao implementador na manutenção mensal.

### 5.2 Reunião #hitl2 — revisão

1. Abrir ata no painel.
2. Conferir nominação correta (especialmente em conteúdo delicado).
3. Editar se necessário (campo Markdown editável).
4. Decidir distribuição:
   - Distribuir aos participantes apenas.
   - Distribuir a subgrupo (champion + dono).
   - Não distribuir (apenas arquivar).

### 5.3 Ata com [INDEFINIDO]

1. Conferir contexto.
2. Preencher manualmente o que ficou indefinido (responsável, prazo).
3. Re-distribuir versão atualizada.

### 5.4 "A ata diz que decidi X, mas eu não decidi"

1. Avisar implementador no mesmo dia.
2. Marcar `[Erro de decisão]` no painel.
3. Implementador analisa transcrição vs. ata, ajusta prompt ou few-shot.

---

## 6. Correção do agente

### 6.1 Decisão capturada errada

- Ajustar Bloco "Regras" da chamada paralela P1 (Decisões).
- Adicionar few-shot do que é decisão vs. sugestão.

### 6.2 Ação sem responsável claro

- Reforçar regra P2: ação sem responsável → [INDEFINIDO], não chutar.

### 6.3 Tom genérico no resumo executivo

- Reforçar regra do agregador: resumo executivo precisa de decisão específica.

### 6.4 Distribuição errada (ata foi a quem não devia)

- **Bloqueador crítico.** Pausar imediatamente.
- Revisar lógica de extração de participantes.
- Auditar últimas 30 atas distribuídas.

---

## 7. Diário de aprendizado

| Data | Reunião | Tag | O que aconteceu | Ação | Ajuste |
|---|---|---|---|---|---|
| | | | | | |

---

## 8. Plano de rollback (15 min)

1. n8n: workflow "resumo-reuniao" → Deactivate.
2. Time volta a escrever ata manualmente (caminho conhecido).
3. Avisar implementador.
4. Sem urgência crítica.

---

## 9. Métricas do mês

| Métrica | Mês atual | Mês anterior | Baseline (D) | Meta 90d |
|---|---|---|---|---|
| Tempo médio em ata/reunião (min) | | | | ≤ 8 |
| % reuniões com ata produzida | | | | ≥ 95% |
| % ações com resp+prazo claros | | | | ≥ 90% |
| Tempo transcrição→ata (min) | | | — | ≤ 5 |
| Taxa de erro na auditoria | | | — | < 5% |
| Reuniões redundantes/semana | | | | < 1 |
| Custo total | | | — | |

---

# Exemplo preenchido — Time INEMA.CLUB

**Versão:** 1.0 — uso interno.
**Champion:** Marina Reis (head ops).
**Time:** Nei (curador), Marina (ops), Felipe (dev), Bruna (editorial).
**Implementador:** Felipe Saito.

## Diário (extrato)

| Data | Reunião | Tag | Aconteceu | Ação | Ajuste |
|---|---|---|---|---|---|
| 03/jul | Weekly programa | #hitl4 | Ata perfeita | OK | — |
| 09/jul | 1:1 Nei x Marina | #hitl2 | Conversa sobre carga; agente captou bem | Marina aprovou distribuição apenas para os 2 | OK |
| 15/jul | Reunião com cliente vertical | #hitl2 #cliente | Ata sugeriu "Marina vai enviar contrato" sem Marina ter falado isso | Marina marcou erro | Implementador ajustou P2 — não atribuir ação se nome não foi citado |

## Métricas julho

| Métrica | Jul | Jun | Baseline | Meta |
|---|---|---|---|---|
| Tempo em ata/reunião | 4 min | 6 min | 35 min | ≤ 8 ✓ |
| % com ata | 100% | 100% | 55% | ≥ 95% ✓ |
| Ações com resp+prazo | 93% | 89% | 62% | ≥ 90% ✓ |
| Transcrição→ata | 3 min | 4 min | — | ≤ 5 ✓ |
| Erro na auditoria | 3,1% | 4,8% | — | < 5% ✓ |
| Custo | R$ 410 | R$ 395 | — | < R$ 600 ✓ |

---

## Dicas

- **Otter.ai com PT-BR Pro é melhor escolha em 2026** — qualidade superior a Zoom puro.
- **Tag #hitl2 é responsabilidade do organizador da reunião**, não do agente.
- **Auditoria semanal é não-negociável**, mesmo em nível 4.
- **Reunião com cliente externo SEMPRE #hitl2.**
- **Plano de rollback testado uma vez no primeiro mês.**

---

*Playbook do Módulo Resumo de Reunião v1.0 — AIOS Profissional 2027.*
