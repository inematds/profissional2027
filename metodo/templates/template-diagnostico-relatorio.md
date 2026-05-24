# Template — Relatório de Diagnóstico DPIA (Entregável da Fase D)

> **Como usar.** Este é o documento principal entregue ao cliente ao final da fase D. Tem três funções: (1) provar ao cliente que ele tem um diagnóstico sério em mãos — não uma proposta comercial disfarçada; (2) servir de referência para as fases P, I e A; (3) sustentar o contrato de manutenção meses depois. Tamanho típico: 8 a 20 páginas + apresentação de 10–15 slides separada para a reunião de fechamento. Use placeholders entre colchetes (`[NOME DO CLIENTE]`) e substitua antes da entrega. Nunca entregue com a marca d'água "DRAFT" — o relatório precisa parecer pronto. Linguagem: português brasileiro, direto, sem jargão. O dono precisa entender sem precisar do implementador para traduzir.

---

# Diagnóstico DPIA — [NOME DO CLIENTE]

**Cliente:** [Razão social]
**Setor:** [setor]
**Porte:** [funcionários] colaboradores, faturamento aprox. [faixa]
**Período do diagnóstico:** [data início] a [data fim]
**Implementador:** [nome]
**Versão:** 1.0
**Confidencialidade:** Documento interno — não compartilhar fora da empresa sem autorização.

---

## 1. Sumário executivo (1 página)

Em até 5 parágrafos, descreva:

- O que motivou o diagnóstico.
- O processo selecionado para a primeira intervenção e por quê.
- O ganho esperado em número absoluto e em % sobre o baseline.
- O prazo do projeto.
- O investimento total estimado (projeto + 12 meses de manutenção).

**Regra:** o dono lê só essa página em 60% dos casos. Se ela não fizer o trabalho de venda interno do projeto na empresa, o resto do relatório não importa.

---

## 2. Contexto do negócio

### 2.1 O que a empresa faz
[Descrição em 1 parágrafo, do jeito que o próprio cliente descreveu na sessão 1.]

### 2.2 Time e estrutura
[Organograma simplificado + nº de funcionários por área. Inclua nominalmente quem foi entrevistado.]

### 2.3 Ferramentas atuais em uso
| Camada | Ferramenta | Uso real |
|--------|------------|----------|
| ERP | [Bling/Omie/Conta Azul/Tiny/outro] | [completo/parcial/abandonado] |
| WhatsApp | [Business/Pessoal] | [1 número/n números] |
| CRM | [se houver] | [usado/parado] |
| Financeiro | [bancos, Asaas, Pix] | [...] |
| Planilhas críticas | [...] | [...] |

### 2.4 Histórico com IA
[O que o cliente já tentou. Por que abandonou. O que aprendeu. Importante para calibrar expectativa.]

---

## 3. Processos avaliados

### 3.1 Lista dos processos candidatos
[3 a 5 processos considerados, com nota qualitativa de cada — volume, dor, complexidade.]

### 3.2 Processo selecionado para o primeiro ciclo DPIA
**Processo:** [Nome — ex: "Atendimento e tomada de pedido via WhatsApp"]
**Justificativa da seleção:** [combinação de volume, dor mensurável, viabilidade técnica e custo de não-fazer].
**Processos postergados:** [outros candidatos, com nota "fica para o próximo ciclo DPIA"].

### 3.3 Mapa do processo atual
[Texto + diagrama simples. Inclui: gatilho de entrada, atores, decisões, ferramentas tocadas, saída. Identifica pontos de gargalo com cor/marca.]

### 3.4 Regras explícitas
[Lista numerada das regras que o dono articulou.]

### 3.5 Regras tácitas capturadas no walk-the-floor
[Lista das regras que o operador segue sem manual. Esta seção é o diferencial do relatório.]

---

## 4. Baseline numérico

| Métrica | Valor atual medido | Método de medição | Período medido |
|---------|---------------------|---------------------|----------------|
| Tempo médio por interação | [min] | [cronômetro / exportação] | [datas] |
| Volume mensal | [n] | [exportação WABA / ERP] | [mês] |
| Taxa de erro/retrabalho | [%] | [ERP / amostragem] | [datas] |
| Custo de hora-pessoa envolvida | R$ [valor] | [folha + encargos] | — |
| Custo de oportunidade estimado | R$ [valor]/mês | [cálculo explicado abaixo] | — |

**Cálculo do custo de oportunidade:** [explicar a conta, com fonte de cada número].

**Pontos onde não foi possível medir:** [se houver — declarar honestamente, não chutar].

---

## 5. KPIs futuros propostos

| KPI | Baseline atual | Meta em 90 dias | Meta em 180 dias | Responsável |
|-----|----------------|------------------|------------------|-------------|
| [Tempo médio] | [...] | [...] | [...] | [Champion + Dono] |
| [Volume processado pelo agente] | 0 | [...] | [...] | [Champion] |
| [Taxa de erro] | [...] | [...] | [...] | [Champion + Implementador] |
| [Receita capturada / custo evitado] | [...] | [...] | [...] | [Dono] |

**Cada KPI:** medido na mesma unidade do baseline, com responsável nominal, com prazo.

---

## 6. Pessoas — decisão de reposicionamento

| Pessoa | Cargo atual | Papel pós-implantação | Status da decisão |
|--------|-------------|------------------------|---------------------|
| [Nome] | [...] | [Continua / Reposicionada para X / Champion / Desligamento programado] | Confirmado por [dono] em [data] |

**Champion interno nomeado:** [Nome, cargo, e-mail, telefone].
**Disponibilidade do champion para o projeto:** [horas/semana garantidas].

---

## 7. Riscos LGPD e regulatórios

### 7.1 Dados pessoais tocados pelo agente
[Lista — CPF, dados de saúde, financeiro, etc.]

### 7.2 Base legal aplicável
[Para cada categoria — execução de contrato, consentimento, legítimo interesse.]

### 7.3 Regime aplicável
[Microempresa? EPP? Aplica regime simplificado ANPD Resolução 2/2022? Dispensa de DPO?]

### 7.4 RIPD enxuto (Relatório de Impacto à Proteção de Dados)
[Versão simplificada — 1 a 2 páginas — descrevendo a operação, os dados, os riscos e as mitigações.]

### 7.5 Outros riscos regulatórios
[Setoriais — CFM para clínicas, OAB para escritórios, CFC para contábeis, etc.]

---

## 8. Solução proposta — visão técnica de alto nível

### 8.1 Arquitetura proposta
[Diagrama textual: canal de entrada → orquestração n8n → LLM (Claude Sonnet/Haiku) → integrações (ERP, base de conhecimento, Pix) → saída.]

### 8.2 Stack
- LLM: Claude Sonnet para raciocínio + Haiku para volume
- Orquestração: n8n self-host na Hetzner
- Memória/RAG: Supabase com pgvector
- Canal: WhatsApp Cloud API
- Integrações: [lista nominal com endpoints]

### 8.3 Matriz HITL inicial proposta
[Tabela com cada ação do agente → nível 1/2/3/4 — sujeita a aprovação final na fase I.]

### 8.4 Plano de rollback
[Como desligar o agente em 15 min se algo der errado.]

---

## 9. Plano de projeto

### 9.1 Cronograma
| Semana | Fase | Atividades-chave | Entregável |
|--------|------|------------------|-------------|
| 1–2 | D | (já concluída) | Este relatório |
| 3–4 | P | Pacote de contexto | Pacote |
| 5–6 | I | Agente + playbook | Agente em homologação |
| 7–8 | A | Deploy + ramp-up | Sistema em produção |
| +30/60/90 dias | A | Marcos de medição | Relatórios contra baseline |

### 9.2 Investimento
| Item | Valor | Forma de pagamento |
|------|-------|---------------------|
| Diagnóstico (fase D) | R$ [valor] | Já liquidado |
| Projeto (fases P, I, A) | R$ [valor] | 40% início P + 30% início I + 30% entrega A |
| Manutenção mensal | R$ [valor]/mês | Tier [básico/intermediário/avançado] — contrato 12 meses |

### 9.3 ROI projetado
[Cálculo simplificado. Ganho mensal esperado / investimento = payback em meses.]

---

## 10. Anexos

- **Anexo A:** Baseline assinado (documento separado — ver `template-baseline-assinado.md`).
- **Anexo B:** Exportações do WhatsApp e ERP usadas como evidência.
- **Anexo C:** Notas de walk-the-floor com tempos cronometrados.
- **Anexo D:** Proposta comercial detalhada com cláusulas.
- **Anexo E:** Modelo de RIPD enxuto.

---

## 11. Próximos passos

1. **[Data]:** Assinatura da proposta de projeto.
2. **[Data]:** Início da fase P (acesso a sistemas confirmado, sessões com operador agendadas).
3. **[Data]:** Reunião de kick-off com o time.

---

*Diagnóstico elaborado por [Nome do implementador] — Profissional 2027 — INEMA.CLUB. Documento entregue em [data]. Válido por 60 dias para fins comerciais.*

---

# Exemplo preenchido — Resumo executivo de caso fictício

> **Cliente:** Polaris Bebidas Ltda (distribuidora de bebidas, Joinville/SC, 14 funcionários, R$ 8M/ano).
>
> **Processo selecionado:** Atendimento e tomada de pedido via WhatsApp Business. Volume confirmado de 6.160 interações/mês, com tempo de resposta médio saltando de 11 min antes das 16h para 47 min depois — gargalo crítico das 16h às 21h gera estimativamente 4 pedidos perdidos/dia (R$ 33.440/mês de receita não capturada). Outros dois processos avaliados (cobrança e relatório gerencial) ficam para um segundo ciclo DPIA em 2026 S2.
>
> **Ganho esperado:** tempo médio de resposta < 5 min em qualquer horário, pedidos perdidos < 1/dia, taxa de erro de pedido < 2% (baseline 4,2%). Em receita: recuperação de R$ 25 a 30 mil/mês a partir do 90º dia de operação.
>
> **Prazo:** 6 semanas para entrada em produção, contado a partir da assinatura desta proposta. Marcos de medição em 30, 60 e 90 dias.
>
> **Investimento total:** R$ 42.000 (projeto, dividido 40/30/30) + R$ 2.400/mês (manutenção intermediária, contrato 12 meses). Payback projetado: 3,3 meses considerando ganho conservador de R$ 18 mil/mês de receita recuperada.

---

## Dicas de uso

- **Sumário executivo de 1 página é não-negociável.** Se passar de 1 página, está pesando o dono. Refaça.
- **Use unidades consistentes.** Se o baseline é "minutos", o KPI futuro é "minutos". Não troque para "segundos" no relatório.
- **Diagrama simples é melhor que diagrama bonito.** ASCII art ou imagem retangular cumpre. Não perca dois dias no Figma.
- **Anexo de evidências é a defesa do método.** Em algum momento o cliente vai duvidar dos números — você abre o anexo C e mostra cronômetro registrado.
- **Apresente em reunião presencial ou videochamada com tela compartilhada.** Nunca por e-mail anexo "qualquer coisa me liga".
- **Reaproveite as seções entre projetos** — estrutura é estável, conteúdo muda. Em 5 projetos do mesmo setor o relatório fica fluido em 2 dias.
