# Template — Baseline Assinado (Exit-gate da Fase D)

> **Como usar.** Este é o documento mais importante de todo o método DPIA. Sem ele assinado, a fase D não acaba e a fase P não começa. É curto de propósito — uma página A4 frente e verso, idealmente. Vai ser referenciado em todo relatório de payback até 12 meses depois. Imprima em duas vias, ambas assinadas — uma fica com o cliente, outra com o implementador. Aceita-se assinatura eletrônica (Clicksign, D4Sign, ou similar), desde que com carimbo de tempo e identificação por e-mail. Não aceite "depois eu assino" — sem assinatura, sem fase P. Este é o teste de seriedade do projeto.

---

# BASELINE — Diagnóstico DPIA

**Projeto:** [Nome do projeto] — [Cliente]
**Implementador:** [Nome / razão social do aluno]
**Data de medição:** [período]
**Data de assinatura:** [data]
**Validade do baseline:** 12 meses a partir da assinatura

---

## 1. Identificação do processo

**Processo medido:** [Nome — ex: "Atendimento e tomada de pedido via WhatsApp Business"]

**Escopo:** [O que entra no processo, o que não entra. Ex: "Inclui interações de venda e dúvida de pedido. Não inclui cobrança ou pós-venda."]

**Atores atuais:** [Nomes — ex: "Cristiane (responsável principal), Felipe (apoio das 14h–18h)"]

---

## 2. Métricas medidas

| # | Métrica | Unidade | Valor medido | Método | Período |
|---|---------|---------|---------------|--------|---------|
| 1 | Tempo médio de resposta | minutos | [valor] | [cronômetro / exportação WhatsApp / observação direta] | [datas] |
| 2 | Volume mensal de interações | unidades/mês | [valor] | [exportação] | [mês] |
| 3 | Taxa de erro / retrabalho | % | [valor] | [ERP / amostragem manual] | [datas] |
| 4 | Custo de hora-pessoa envolvida | R$/hora | [valor] | [folha + encargos sociais] | atual |
| 5 | Receita perdida estimada por gargalo | R$/mês | [valor] | [pedidos perdidos × ticket médio] | atual |
| 6 | Outro KPI específico do setor | [...] | [...] | [...] | [...] |

**Pontos não medidos (declarados):**
- [Métrica X — motivo: dado não disponível em tempo hábil. Será re-medido na fase P.]

---

## 3. KPIs futuros — metas acordadas

| # | KPI | Baseline (item correspondente) | Meta em 90 dias | Meta em 180 dias | Responsável pelo acompanhamento |
|---|-----|-------------------------------|------------------|-------------------|----------------------------------|
| 1 | Tempo médio de resposta | [valor] min | [valor] min | [valor] min | [Champion + Dono] |
| 2 | Volume processado pelo agente | 0% | [valor] % | [valor] % | [Champion] |
| 3 | Taxa de erro | [valor] % | [valor] % | [valor] % | [Champion + Implementador] |
| 4 | Receita capturada / custo evitado | R$ 0 | R$ [valor]/mês | R$ [valor]/mês | [Dono] |

**Marcos de medição contratados:** 30, 60 e 90 dias após entrada em produção, com relatório formal apresentado pelo implementador em cada marco.

---

## 4. Critérios objetivos de sucesso do projeto

O projeto será considerado bem-sucedido se, no marco de 90 dias após entrada em produção:

- [ ] **Pelo menos [X] dos [Y] KPIs acima** atingirem ao menos 80% da meta de 90 dias.
- [ ] **Adoção do agente** (% de interações reais processadas pelo agente vs. canal antigo) ≥ [Z] %.
- [ ] **Taxa de override humano** ≤ [W] % no mês de referência.
- [ ] **Champion interno** segue ativo na função.

Em caso de não atingimento, o implementador apresenta plano de recuperação em até 14 dias, sem custo adicional, dentro do contrato de manutenção vigente.

---

## 5. Premissas registradas

- O baseline acima foi medido pelo implementador no período declarado, com colaboração das pessoas nominadas.
- As metas de 90 e 180 dias pressupõem manutenção do volume de operação dentro de ±20% em relação ao baseline. Variações maiores serão objeto de re-pactuação.
- Mudanças de ERP, troca de número WhatsApp, mudança de regulação setorial ou mudança de mais de 30% do time operacional invalidam o baseline e exigem nova fase D parcial.
- O champion interno [Nome] permanece nas funções definidas pelo menos durante o ramp-up de 90 dias.
- O cliente garante acesso de leitura/escrita aos sistemas listados no relatório de diagnóstico.

---

## 6. Assinaturas

**Pelo cliente:**

Nome: __________________________________________
Cargo: _________________________________________
CPF: ___________________________________________
Data: __________________________________________
Assinatura: ____________________________________

**Pelo implementador:**

Nome: __________________________________________
CPF/CNPJ: ______________________________________
Data: __________________________________________
Assinatura: ____________________________________

**Testemunha (opcional, recomendado para contratos acima de R$ 30k):**

Nome: __________________________________________
CPF: ___________________________________________
Assinatura: ____________________________________

---

# Anexo — Calculadora de ROI vinculada ao baseline

**Cálculo do ganho mensal esperado**

```
Horas/mês economizadas      = [horas atual] − [horas projetada]   = [valor] horas
Custo de hora-pessoa        = R$ [hora] (folha + encargos)
Economia operacional        = [horas economizadas] × R$ [hora]    = R$ [valor]

Receita adicional capturada = [pedidos recuperados/mês] × ticket médio = R$ [valor]

Custo operacional do agente (mensal):
  - LLM (Claude Sonnet + Haiku)           R$ [valor]
  - n8n self-host (Hetzner)               R$ [valor]
  - Supabase / pgvector                   R$ [valor]
  - WABA (Cloud API Meta)                 R$ [valor — usage-based]
  - Manutenção mensal contratada          R$ [valor]
                                          ─────────────────
                          subtotal:        R$ [valor]

Ganho mensal líquido = (Economia operacional + Receita adicional) − Custo operacional
                     = R$ [valor]/mês

Payback (meses) = Investimento de projeto ÷ Ganho mensal líquido
               = R$ [valor] ÷ R$ [valor]/mês
               = [X,Y] meses
```

**Sensibilidade:**

| Cenário | Ganho mensal | Payback |
|---------|---------------|---------|
| Conservador (60% da meta) | R$ [valor] | [meses] |
| Base (100% da meta) | R$ [valor] | [meses] |
| Otimista (130% da meta) | R$ [valor] | [meses] |

---

# Exemplo preenchido — Polaris Bebidas Ltda (Joinville/SC)

**Processo medido:** Atendimento e tomada de pedido via WhatsApp Business.

**Escopo:** Interações de venda, consulta de preço, status de pedido e renegociação de prazo. Não inclui cobrança de inadimplentes (será ciclo DPIA seguinte).

**Atores atuais:** Cristiane Bertoldi (atendente principal, 8h–18h), Felipe Schmitz (apoio, 14h–18h).

| # | Métrica | Unidade | Valor medido | Método | Período |
|---|---------|---------|---------------|--------|---------|
| 1 | Tempo médio de resposta (08h–16h) | min | 11 | cronômetro presencial 4h + amostra exportação | 12–16/maio/2026 |
| 2 | Tempo médio de resposta (16h–21h) | min | 47 | cronômetro + exportação WABA | 12–16/maio/2026 |
| 3 | Volume mensal | mensagens | 6.160 | exportação WABA 30 dias | abr/2026 |
| 4 | Pedidos perdidos por demora | pedidos/dia | 4 (estimativa do dono) | observação + ERP | abr/2026 |
| 5 | Taxa de erro de pedido (correção pós-criação) | % | 4,2 | relatório Bling | abr/2026 |
| 6 | Custo hora-pessoa atendimento | R$/hora | 38 | folha + encargos | atual |
| 7 | Receita perdida estimada | R$/mês | 33.440 | 4 pedidos × R$ 380 × 22 dias úteis | abr/2026 |

**KPIs futuros:**

| # | KPI | Baseline | 90 dias | 180 dias | Responsável |
|---|-----|----------|---------|----------|-------------|
| 1 | Tempo médio de resposta (qualquer horário) | 47 / 11 min | ≤ 5 min | ≤ 3 min | Cristiane + Reinaldo |
| 2 | Volume processado pelo agente | 0% | ≥ 70% | ≥ 85% | Cristiane |
| 3 | Taxa de erro de pedido | 4,2% | ≤ 2,5% | ≤ 1,5% | Cristiane + Implementador |
| 4 | Receita recuperada | R$ 0 | R$ 22.000/mês | R$ 28.000/mês | Reinaldo |

**Critério de sucesso aos 90 dias:** ao menos 3 dos 4 KPIs com 80% da meta + adoção ≥ 65% + override ≤ 15% + Cristiane ativa.

**Cálculo ROI base:**

```
Horas/mês economizadas    = 80 horas (8 atendentes-equivalente × 10h/mês)
Custo de hora-pessoa      = R$ 38
Economia operacional      = 80 × 38 = R$ 3.040/mês

Receita adicional         = 3 pedidos/dia × R$ 380 × 22 dias = R$ 25.080/mês

Custo operacional agente  = R$ 350 (LLM) + R$ 120 (n8n) + R$ 180 (Supabase)
                          + R$ 250 (WABA) + R$ 2.400 (manutenção)
                          = R$ 3.300/mês

Ganho líquido             = (3.040 + 25.080) − 3.300 = R$ 24.820/mês
Payback                   = 42.000 / 24.820 = 1,7 mês

Sensibilidade:
  Conservador (60%)       = R$ 14.892/mês → payback 2,8 meses
  Base (100%)             = R$ 24.820/mês → payback 1,7 mês
  Otimista (130%)         = R$ 32.266/mês → payback 1,3 mês
```

Assinado: Reinaldo K. Salvi (dono) e [nome do implementador] em 19/maio/2026.

---

## Dicas de uso

- **Imprima e assine fisicamente sempre que possível.** Documento físico tem peso simbólico que e-mail não tem. PDF assinado em Clicksign é o segundo melhor.
- **Não enfeite os números do baseline para parecer mais dramático.** O cliente vai conferir 90 dias depois. Mentira aqui mata o contrato de manutenção.
- **Declare honestamente o que você não conseguiu medir.** "Não medido — será re-medido em P" é resposta profissional. Chute é tiro no pé.
- **Sensibilidade conservador/base/otimista** evita que o cliente cobre o cenário otimista como meta única. Use sempre os três.
- **Validade de 12 meses** é cláusula. Depois desse prazo, baseline antigo não vale para defender bônus por resultado.
