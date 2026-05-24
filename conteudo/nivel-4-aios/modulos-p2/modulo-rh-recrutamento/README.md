# Módulo 11 — RH / Triagem de Currículos + Agendamento de Entrevista

**Versão:** 0.1 (especificação P2) — maio/2026
**Categoria:** P2 — especificação para V2 da biblioteca
**Pré-requisitos AIOS:** stack canônica + Módulo 3 (Agendamento) dominado.

---

## O que faz

Conduz a **triagem inicial de candidatos** para vagas em PME e o **agendamento da entrevista** com gestor humano. Sem decidir admissão — apenas filtra e organiza.

Cobre:

1. **Recepção de currículo** (formulário site, WhatsApp da vaga, upload PDF).
2. **Extração estruturada** de dados do currículo (experiência, formação, localização, pretensão salarial declarada).
3. **Matching** com critérios da vaga (must-have, nice-to-have).
4. **Classificação** (apto / não apto / parcial).
5. **Agendamento** da entrevista com aptos.
6. **Resposta cordial** para não aptos.
7. **Relatório consolidado** para o gestor.

---

## Quando usar

- PME que abre **vagas com regularidade** (≥ 2/mês) ou recebe **fluxo de candidatura espontânea** alto.
- Gestor/dono gasta ≥ 4h/semana lendo currículo — dor real.
- Existem **critérios objetivos** para a vaga (formação, experiência, localização, disponibilidade).
- Cliente aceita HITL nível 2 — humano confirma agendamento e responde rejeições.
- Verticais ideais: **varejo com rotatividade, prestador de serviço, distribuidora com vendedores externos, agência**.

---

## Stack utilizada

- WhatsApp Cloud API + formulário site.
- n8n.
- Claude Sonnet 4.6 para extração + matching (escolha sensível, requer raciocínio).
- Supabase para banco de candidatos + histórico.
- Integração com sistema de agenda (Trinks/Google Calendar) para entrevistas.

---

## Arquitetura sumária

```
Candidato envia CV (PDF, foto, texto)
       │
       ▼
[Agente Sonnet] extrai dados estruturados
       │
       ▼
[Agente Sonnet] avalia contra critérios da vaga
   Output: classificação + justificativa
       │
       ├── Apto → propõe horários de entrevista
       │           GATE HITL nível 2: gestor aprova convite
       │           agenda entrevista no calendário do gestor
       │
       ├── Parcial → fila para gestor revisar manualmente
       │
       └── Não apto → resposta cordial padrão
                      GATE HITL nível 2 para envio
       │
       ▼
Relatório semanal ao gestor com candidatos no funil
```

---

## Nível HITL recomendado

| Operação | Nível inicial |
|---|---|
| Extração estruturada | 4 |
| Matching com vaga | 4 |
| Resposta cordial não-apto | 2 (humano aprova) |
| Convite para entrevista | 2 |
| Agendamento da entrevista | 3 (após aprovação) |
| **Decisão de contratação** | **nunca pelo agente** |

---

## Tempo estimado de construção (para V2)

| Fase | Dias úteis |
|---|---|
| D | 5–8 |
| P (critérios da vaga + RIPD reforçado + templates de resposta) | 10–14 |
| I | 7–10 |
| A | 7–14 |
| **Total** | **29–46 dias** |

---

## Pré-requisitos

- **Módulo 3 (Agendamento) dominado** — reaproveita tools.
- Critérios da vaga claros (não "alguém bom").
- Templates de comunicação prontos (apto, não apto, parcial).
- **RIPD reforçado** — dados de candidato têm LGPD sensível, especialmente categoria de "dados sensíveis" (raça, religião, orientação).
- Política anti-discriminação clara (agente não filtra por nome, idade, gênero, foto).

---

## Notas para V2

- **Auditoria de viés** semanal — verificar se classificações têm padrão discriminatório.
- Considerar Lei Geral de Proteção de Dados + Lei nº 14.973/2024 (uso de IA em decisões automatizadas).
- Resposta cordial não-apto sempre revisada por humano — relação reputacional.
- Banco de talentos: candidatos não selecionados ficam no Supabase com consentimento para vagas futuras.

---

*Especificação Módulo RH/Recrutamento v0.1 — AIOS Profissional 2027.*
