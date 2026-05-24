# Módulo 3 — Agendamento Inteligente

**Versão:** 1.0 — maio/2026
**Categoria:** P1 — Módulo de Operação
**Pré-requisitos AIOS:** stack canônica + integração ativa com sistema de agenda (Trinks, Google Calendar, Calendly, ou similar).

---

## O que faz

Gerencia o ciclo completo de **agendamento por WhatsApp** para PMEs com agenda profissional (clínicas, consultórios, salões, serviços técnicos, escritórios): consulta horários disponíveis, agenda, confirma 24h antes, lembra no dia, remarca, registra no-show, oferece encaixe quando há cancelamento.

Cobre as 6 operações típicas:

1. **Consulta de horários disponíveis** (autônomo, autopiloto).
2. **Agendamento de novo encaixe** (co-piloto na V1, autopiloto após maturidade).
3. **Confirmação D-1** (autopiloto com auditoria — clássico nível 4).
4. **Lembrete no dia** (autônomo).
5. **Remarcação** (co-piloto sempre — exige aprovação humana).
6. **Registro de no-show + cobrança de taxa** (co-piloto).

---

## Quando usar

- Cliente PME com **agenda profissional como produto principal** (clínica, consultório, salão, fisioterapia, psicologia, serviço técnico com visita marcada).
- Volume de **40 a 600 atendimentos agendados/mês**.
- Sistema de agenda integrável (Trinks tem API, Google Calendar tem API, Calendly tem API). Agendas em papel ou planilha desestruturada: refazer fase P antes.
- Taxa de no-show atual ≥ 8% (dor concreta). Abaixo disso, ROI fica apertado.
- Atendimento atual gasta ≥ 1,5 h/dia em confirmação manual + remarcação por WhatsApp.
- Verticais ideais: **clínicas (fisioterapia, odontologia, psicologia), salões de beleza, escritórios de serviço, manutenção técnica residencial**.

---

## Quando NÃO usar

- Agendamento exige **diagnóstico clínico/técnico** prévio (consulta de retorno especializada, primeira consulta com triagem por enfermeiro). Agente apenas marca, não tria.
- Volume < 40 atendimentos/mês. Manutenção não fecha.
- Sistema de agenda fechado sem API pública (alguns sistemas legados de clínicas). Fase P precisa endereçar.
- Profissional atende fora de horário fixo e a agenda muda dinamicamente sem padrão (autônomo sem grade). Outro padrão de solução.
- Cliente PME não tem **política clara de cancelamento e no-show** documentada. Antes, definir.
- Recepcionista atual recusa o reposicionamento. Sabotagem garantida.

---

## Resultado esperado

| Métrica | Baseline típico | Após 90 dias | Após 180 dias |
|---|---|---|---|
| Taxa de no-show | 12–25% | 6–10% | 4–8% |
| Tempo da recepção em agenda (h/dia) | 3–6 h | 0,5–1,5 h | 0,3–1 h |
| Confirmações enviadas com sucesso | 60–80% | 92–98% | 95–99% |
| Encaixes preenchidos por cancelamento | < 10% | 30–60% | 50–75% |
| Satisfação do paciente (NPS) | sem medida | +10 a +25 | +15 a +35 |

**Payback típico:** 2 a 3,5 meses (no-show evitado costuma ser o maior componente).

---

## Stack utilizada

- **WhatsApp Cloud API**.
- **n8n** — workflows separados por operação (consulta, agendar, confirmar, lembrar, remarcar).
- **Claude Haiku 4.5** para classificação de intenção e respostas simples.
- **Claude Sonnet 4.6** para remarcação com múltiplas opções e tratamento de exceção.
- **Sistema de agenda do cliente**: Trinks (clínicas), Google Calendar (consultório individual), ou Conta Azul/Omie quando agenda + financeiro estão integrados.
- **Supabase** para sessão, log e métricas.

---

## Arquitetura

```
                ┌────────────────────────────────────┐
                │ Cliente final no WhatsApp          │
                └───────────────┬────────────────────┘
                                ▼
                ┌────────────────────────────────────┐
                │ Roteador de intenção (Haiku)       │
                │  → agendar / consultar / remarcar  │
                │    / cancelar / dúvida geral       │
                └─────────────┬──────────────────────┘
                              ▼
        ┌─────────────────────┼────────────────────────┐
        ▼                     ▼                        ▼
  Workflow                Workflow                 Workflow
  "agendar"               "remarcar"               "dúvida_geral"
   (Sonnet)               (Sonnet,                 (Atendimento
   tool: agenda           HITL 2)                  módulo 1)
        │                     │
        ▼                     ▼
  Cria horário        Champion aprova
  no sistema          remarcação
        │                     │
        └─────────┬───────────┘
                  ▼
        ┌────────────────────┐
        │ Workflows agendados│
        │  (cron no n8n)     │
        │                    │
        │ D-1 18h: confirmar │
        │ D+0 manhã: lembrar │
        │ D+0 final: no-show │
        └────────────────────┘
```

Padrão dominante: **Routing** + crons agendados.

---

## Nível HITL recomendado

| Operação | Nível inicial | Pode subir a |
|---|---|---|
| Consulta de horário disponível | 3 (autopiloto) | 4 após 4 semanas |
| Confirmação D-1 (envia template + capta resposta) | 4 desde o início (auditoria semanal) | — |
| Lembrete no dia | 4 desde o início | — |
| Agendamento de novo paciente | 2 (co-piloto) | 3 após 6 semanas + ≥ 90% qualidade |
| Agendamento de paciente recorrente | 3 (autopiloto, gates) | 4 nunca em V1 |
| **Remarcação** | 2 (co-piloto sempre) | **nunca** sobe além de 3 |
| Registro de no-show + cobrança | 2 (co-piloto) | 3 após 90 dias |
| Pergunta clínica do paciente | 1 (assistir — escala obrigatório) | nunca |

**Regra firme:** o agente nunca aconselha sobre tratamento, sintoma, dor. Toda pergunta clínica vira escalonamento ao profissional habilitado. Documente isso explicitamente na matriz HITL e no contrato (responsabilidade técnica).

---

## Custo operacional estimado (R$/mês)

**Clínica pequena — 200 atendimentos/mês, ~600 interações WhatsApp:**

| Item | Mensal |
|---|---|
| Meta WABA (templates D-1 e lembrete D+0) | R$ 60–180 |
| Claude API (Haiku predominante) | R$ 30–80 |
| Sistema de agenda (Trinks, etc.) | já existente |
| Rateio infra | R$ 30–80 |
| **Total** | **R$ 120–340** |

**Clínica média — 600 atendimentos/mês:**

| Item | Mensal |
|---|---|
| Meta WABA | R$ 250–500 |
| Claude API | R$ 80–180 |
| Rateio infra | R$ 50–100 |
| **Total** | **R$ 380–780** |

---

## Tempo de implementação

| Fase | Dias úteis |
|---|---|
| D | 5–8 |
| P (agenda + política de no-show + templates Meta) | 7–14 (aprovação Meta dos templates é gargalo) |
| I | 7–10 |
| A | 7–14 |
| **Total** | **26–46 dias úteis** |

Gargalo comum: **aprovação Meta dos templates de confirmação e lembrete** (3–7 dias úteis sozinho). Iniciar templates na fase P, paralelo às outras tarefas.

---

## Métricas obrigatórias

1. **Taxa de no-show** — contra baseline.
2. **Tempo de resposta a solicitação de horário** (minutos).
3. **% de confirmações D-1 respondidas** pelo paciente.
4. **% de encaixes preenchidos** quando há cancelamento.
5. **Tempo da recepção em agenda** (horas/dia, autodeclarado ou rastreado).
6. **NPS pós-consulta** (pesquisa por WhatsApp 1 dia depois).
7. **% de escalonamentos para profissional** (saudável < 20%).
8. **Custo total mensal.**

---

## Erros comuns e como evitar

- **Agente confirma consulta sem perguntar.** Resultado: paciente que esqueceu fica irritado por mensagem repetida. *Corrige:* template Meta começa com "Olá {{nome}}, sua consulta com {{profissional}} amanhã às {{hora}}. Posso confirmar?".
- **Agente aceita remarcação sem revisar conflito.** Resultado: dois pacientes no mesmo horário. *Corrige:* nível 2 sempre para remarcação. Tool `consultar_agenda` antes de propor novo horário.
- **Agente trata dor/sintoma como pergunta administrativa.** Resultado: paciente em quadro grave esperando resposta automatizada. *Corrige:* prompt explícito + lista de palavras-gatilho clínicas (dor, urgente, sangrar, desmaio, febre, etc.) → escala imediato.
- **Templates Meta não aprovados a tempo.** Resultado: deploy atrasa. *Corrige:* submeter templates no início da fase P, não em I.
- **Esquecer LGPD em prontuário.** Resultado: agente acessa dado sensível sem necessidade. *Corrige:* tool `consultar_prontuario_metadados` retorna apenas dados administrativos (nome, telefone, profissional, horário), nunca dado clínico.
- **Cobrar no-show automaticamente.** Resultado: paciente bravo com relação rompida. *Corrige:* sempre co-piloto na cobrança de taxa; champion humano avalia caso a caso.

---

## Variações por vertical

### Clínica de fisioterapia (MoveBem)
- Tool central: `consultar_agenda_trinks`, `agendar_trinks`, `remarcar_trinks`.
- Regras frequentes: sessões em pacote (10 sessões pré-pagas), encaixe de mesma especialidade, retorno em 48h tem desconto.
- Gatilhos clínicos: "tô com dor que tá piorando", "não consigo mexer", "tô com febre" → escala fisioterapeuta de plantão.

### Salão de beleza
- Tool: Google Calendar + planilha de profissionais.
- Regras: serviço por profissional específico (cliente quer Maria, não outra), tempo variável por procedimento, política de no-show com taxa fixa.
- Sem gatilhos clínicos; gatilho é alergia/reação a produto → escala.

### Serviço técnico residencial (manutenção)
- Tool: Google Calendar + roteirização (CEP).
- Regras: janela de chegada (manhã/tarde, não horário exato), confirmação prévia ao deslocamento, taxa de visita.
- Gatilho: emergência ("não tenho água/luz") → escala imediato com prioridade.

---

## Mapeamento para DPIA

| Fase | Pré-requisitos |
|---|---|
| D | Baseline de no-show, tempo da recepção em agenda, encaixes perdidos. Política de cancelamento clara. KPIs assinados. Champion (recepcionista) decidido. |
| P | Agenda integrada e validada com chamadas reais. Templates Meta submetidos e em aprovação. Regras de no-show e taxa. Lista de palavras-gatilho clínicas. Glossário (sinônimos de procedimentos). |
| I | system-prompt.md + playbook.md. Matriz HITL com restrição firme sobre conselho clínico. Homologação com 30 casos reais (≥ 90% acerto — barra mais alta por sensibilidade). |
| A | Deploy faseado: primeiro só lembretes (nível 4); depois agendamento simples (nível 2); depois remarcação (nível 2 permanente). Job shadowing 1 semana com recepcionista. Micro-vídeos sobre encaixe e no-show. |

---

*Módulo Agendamento v1.0 — AIOS Profissional 2027.*
