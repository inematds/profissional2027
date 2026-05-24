# Módulo 10 — Financeiro Básico (Conciliação + Categorização + Alerta de Caixa)

**Versão:** 0.1 (especificação P2) — maio/2026
**Categoria:** P2 — especificação para V2 da biblioteca
**Pré-requisitos AIOS:** stack canônica + integração com financeiro do cliente (Conta Azul, Omie, Bling Financeiro).

---

## O que faz

Auxilia o **financeiro do cliente PME** em 3 tarefas recorrentes que consomem tempo:

1. **Conciliação básica** — cruzar extrato bancário (OFX/CSV) com lançamentos no ERP, sugerir match para itens pendentes.
2. **Categorização de despesa** — classificar despesas avulsas em centros de custo / categorias do plano de contas, a partir de descrição e histórico.
3. **Alerta de fluxo de caixa** — projeção semanal de saldo com base em recebíveis e pagáveis; alerta proativo quando saldo projetado em D+7 fica abaixo do limite seguro.

Aplica nível 1 (assistir) para conciliação e categorização — agente sugere, humano confirma.

---

## Quando usar

- PME com **financeiro próprio interno** (não terceirizado totalmente para contábil).
- Volume mensal: 50-500 lançamentos bancários, 100-800 despesas a categorizar.
- ERP financeiro estruturado: Conta Azul, Omie, Bling Financeiro com plano de contas configurado.
- Dono ou financeiro gasta ≥ 8 h/semana com conciliação + categorização — dor real.
- Verticais ideais: **comércio com volume médio, prestador de serviço com muitos lançamentos, escritório com regime de competência**.

---

## Stack utilizada

- n8n com cron diário (importação OFX/CSV).
- Claude Haiku 4.5 para categorização (alta confiança em padrões repetitivos).
- Claude Sonnet 4.6 para conciliação ambígua e projeção de caixa.
- ERP financeiro via API (Conta Azul tem boa API; Omie idem).
- Supabase para histórico de classificação (treina contexto).

---

## Arquitetura sumária

```
Cron diário 7h
       │
       ▼
[Importar extrato bancário do dia] (OFX automático ou upload)
       │
       ▼
[Agente Haiku] sugere match com lançamento no ERP
   confidence alta → marca como "sugestão"
   confidence baixa → marca "pendência humana"
       │
       ▼
GATE HITL nível 1: financeiro confirma sugestões
       │
       ▼
[Categorização de despesas avulsas]
   por descrição + histórico do mesmo fornecedor
       │
       ▼
[Projeção de caixa] D+7 e D+30
   alerta se saldo projetado < limite seguro
       │
       ▼
Relatório diário enviado ao dono (WhatsApp/e-mail)
```

---

## Nível HITL recomendado

| Operação | Nível inicial |
|---|---|
| Importação de extrato | 4 |
| Sugestão de match (conciliação) | 1 (humano confirma) |
| Categorização de despesa nova | 1 |
| Categorização de despesa recorrente (mesmo fornecedor) | 3 |
| Alerta de fluxo de caixa | 4 |
| **Lançamento direto no ERP** | **nunca em V1** |

---

## Tempo estimado de construção (para V2)

| Fase | Dias úteis |
|---|---|
| D | 7–10 |
| P (plano de contas + integração ERP + parsing OFX) | 14–21 |
| I | 10–14 |
| A | 10–14 |
| **Total** | **41–59 dias** |

Módulo de **construção mais demorada** da lista P2 — financeiro tem rigor e variabilidade de conta.

---

## Pré-requisitos

- Plano de contas do cliente estruturado e estável.
- ERP financeiro com 6+ meses de histórico para treinar categorização.
- Política de revisão definida (quem confirma sugestões, em que prazo).
- Cliente aceita HITL nível 1 permanente para lançamentos.

---

## Notas para V2

- Detecção de transação anômala (potencial fraude/erro).
- Integração com NF-e para auto-conciliação de notas.
- Cuidado adicional LGPD/sigilo financeiro — RIPD reforçado.

---

*Especificação Módulo Financeiro Básico v0.1 — AIOS Profissional 2027.*
