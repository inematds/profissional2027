---
nivel: 2
trilha: A
modulo: 4
titulo: Medição e Resultado
tempo_leitura: 18
exit_criteria: aluno produz relatórios de 30, 60 e 90 dias contra baseline para o cliente do exercício, apresenta em 4 slides e calcula payback efetivo.
---

# O que você vai aprender
- Como medir contra baseline assinado, usando exatamente a mesma metodologia da fase D.
- A apresentação trimestral em 4 slides — o slide 1 que separa o implementador sério do amador.
- Como calcular payback efetivo mês a mês e payback acumulado.
- Como apresentar resultado quando o KPI foi atingido e quando não foi.
- Como usar o marco 30 dias para fechar o contrato de manutenção.

## Por que isso importa
Princípio 2 do DPIA: sem baseline, sem projeto. A consequência prática é o marco 30 dias. O baseline assinado em D só vira instrumento de valor se o implementador volta com o número novo, na mesma unidade, com a mesma metodologia. Pesquisa A §2.5 e G §8.3: clientes que recebem relatório formal de payback no marco 30/60/90 renovam manutenção em 87% dos casos; clientes que não recebem renovam em 31%.

Marco 30 dias também é o evento comercial mais importante do projeto. É quando o implementador apresenta o ganho concreto e propõe — com proposta na mesa — o tier de manutenção. Não é venda nova; é continuação do contrato vendido em D.

## O passo a passo

**1. Reaplicar exatamente a metodologia do baseline.** A medição do marco precisa replicar o método de D para ser comparável:
- Mesma unidade (minutos, %, R$).
- Mesma janela (30 dias completos, 30 dias úteis, etc.).
- Mesma fonte (Bling, log do agente, WhatsApp export).
- Mesma exclusão (outliers, fim de semana, feriado).

Sem essa disciplina, comparação fica enviesada e cliente desconfia.

**2. Extrair métricas do log estruturado.** Do Supabase, contar:
- Volume total de interações.
- Tempo médio do agente (início da conversa → confirmação do pedido).
- Tempo médio do humano (quando escalou).
- Taxa de escalonamento.
- Taxa de override (quando humano corrige resposta do agente antes de enviar).
- Taxa de devolução (cruzar com Bling).
- Pedidos por cliente.
- Cobertura horária (interações fora do horário comercial agora atendidas).

**3. Calcular ganho mensal efetivo.**

```
Ganho mensal medido = 
  (horas economizadas medidas × custo-hora real)
  + (receita adicional capturada — pedidos atendidos fora do horário, conversão maior)
  - (custo operação agente — LLM + n8n + manutenção mensal projetada)
```

Comparar com a projeção feita em D. Em geral:
- *Mês 1:* 50-70% do ganho projetado (curva de aprendizado, regras ajustando, operador adaptando).
- *Mês 2:* 80-95% do ganho projetado.
- *Mês 3:* 95-105% do ganho projetado.

Payback acumulado: somar ganho até o mês N, comparar com investimento total. Payback efetivo costuma ficar entre 3,2 e 4,5 meses em projetos bem feitos.

**4. Apresentação trimestral em 4 slides.** Manual canônico, "Como apresentar resultado ao cliente":

- *Slide 1 — Onde estávamos.* Baseline assinado em D, com data. Tabela com tempo, volume, erro, custo.
- *Slide 2 — Onde estamos.* Métrica do mês atual, mesma unidade. Tabela paralela à do slide 1.
- *Slide 3 — ROI acumulado.* Linha do tempo: investimento de projeto, ganho mensal, payback acumulado, mês em que cruza o ponto de equilíbrio.
- *Slide 4 — Próximos 90 dias.* O que vai melhorar, com número-meta (taxa de override caindo, subida de nível HITL programada, novos KPIs).

Slide 1 é o que separa o implementador sério do amador. Quem não tem slide 1 não tem nada.

**5. Apresentar o relatório.** Presencial sempre que possível. Marco 30 dias dura 60-90 min:
- 5 min: contexto, agradecer Maria pelo trabalho operacional.
- 20 min: passar pelos 4 slides com calma.
- 20 min: discussão, perguntas, objeções.
- 15 min: proposta de manutenção (módulo 05).
- 15 min: assinatura do contrato + agenda de marcos 60 e 90 dias.

**6. Quando o KPI é atingido.** Apresentar com naturalidade técnica. Sem euforia, sem auto-elogio excessivo. Dado fala por si.

Trecho-padrão: *"O KPI principal — tempo médio de resposta — saiu de 47 minutos para 9 minutos, dentro da meta. A taxa de devolução caiu de 11% para 4%. Sr. Roberto não respondeu WhatsApp depois das 18h em todo o período. Payback acumulado em 30 dias: 24% do projeto pago."*

**7. Quando o KPI não é atingido.** Acontece. Tratamento:
- Apresentar mesmo assim, sem omitir.
- Identificar a causa: técnica? humana? de escopo? de baseline?
- Apresentar plano de ajuste para os próximos 30 dias.
- Reafirmar compromisso de payback até o marco 90 dias.

Cliente respeita transparência. Cliente que descobre depois que o número foi maquiado vira inimigo.

**8. Plano de ajuste se a meta está fora.**

| Causa | Tratamento |
|-------|-----------|
| Taxa de override alta (Maria refazendo muita resposta) | Iterar system prompt + RAG; talvez voltar para P em um aspecto específico |
| Cliente final não adotou (continua mandando WhatsApp para o número pessoal do dono) | Comunicação ao cliente final + Sr. Roberto deixar de responder no pessoal |
| Volume real menor que o projetado | Discutir se baseline subestimou ou se mês foi atípico |
| Integração com Bling intermitente | Investigar; pode ser cache, rate limit, ou problema do Bling |

**9. Marco 60 dias.** Repete a estrutura do marco 30, com dois acréscimos:
- Discussão de subida de nível HITL (se métricas permitem).
- Discussão de expansão de escopo (segundo processo, próximo módulo) — sem fechar venda agora, só plantar.

**10. Marco 90 dias.** Fechamento formal do projeto inicial. Apresentação em 4 slides; ROI acumulado próximo do payback total ou já passando. Acordo de continuidade no tier de manutenção em vigor.

## Exemplo aplicado — Polaris Bebidas (Joinville/SC)

**Marco 30 dias — apresentação em 28/06.**

Slide 1 — Onde estávamos (baseline assinado 24/04):
- Tempo médio: 47 min/pedido.
- Volume: 480/mês.
- Devolução SKU: 11,2%.
- Atendimento após 18h: 14/sem (pelo Sr. Roberto).
- Custo agregado: R$ 23.136/mês.

Slide 2 — Onde estamos (28/05 a 27/06):
- Tempo médio: 9,2 min/pedido (80% dos casos).
- Volume: 516/mês (8% maior, picado por sazonalidade).
- Devolução SKU: 4,1%.
- Atendimento após 18h: 0.
- Custo agregado: R$ 6.840/mês.
- Economia mensal: R$ 16.296.

Slide 3 — ROI acumulado:
- Investimento: R$ 58.000 (fase D + projeto).
- Ganho mês 1: R$ 16.296 - R$ 950 (custo operação) - R$ 2.400 (manutenção tier intermediário) = R$ 12.946.
- Payback acumulado: 22,3%.
- Projeção: payback total no mês 4-5.

Slide 4 — Próximos 90 dias:
- Métrica de qualidade hoje: 91% (acima de 90% por 18 dias). Subida HITL de A1, A3, A4 do nível 2 para nível 3 a partir do dia 35 — aprovação do Sr. Roberto + Maria.
- Meta de tempo médio para marco 60: 7,5 min em 85% dos casos.
- Meta de devolução SKU para marco 90: < 3%.
- Discussão preliminar de novo módulo: emissão NF-e automática condicional (entra em fase D simplificada se decidido).

Discussão (20 min): Sr. Roberto pergunta sobre o tempo médio de 9,2 min — esperava 5-8 min. Aluno explica: 9,2 min inclui escalonamentos (que ainda são 22%). Para pedidos que o agente fecha sozinho, tempo médio é 4,1 min. À medida que escalonamento cai (com a subida de nível HITL), tempo médio agregado cai também.

Sr. Cláudio pergunta sobre o ROI projetado. Aluno apresenta planilha. Concordância.

Proposta de manutenção apresentada (módulo 05). Tier intermediário R$ 2.400/mês. Assinatura na própria reunião.

**Marco 60 dias — 28/07:** tempo médio 7,1 min, devolução 3,0%, payback acumulado 45%. Sr. Roberto aceita subir A4 (lembrete) para nível 4 (autônomo com auditoria) a partir do dia 70.

**Marco 90 dias — 27/08:** tempo médio 6,8 min, devolução 2,8%, payback acumulado 70%. Projeto se paga no mês 4 (estimativa). Polaris pede orçamento de segundo módulo: cobrança automática de fatura vencida. Aluno propõe nova fase D simplificada (aproveitando Pacote de Contexto existente em ~60%).

## Erros comuns
- **Mudar metodologia entre baseline e marco.** Não compara. *Corrige:* exatamente a mesma medição.
- **Apresentar por e-mail.** Cliente lê pela metade, não internaliza. *Corrige:* reunião presencial.
- **Esconder métrica ruim.** Cliente descobre depois. *Corrige:* transparência total.
- **Não usar slide 1.** Sem âncora, dado parece sem contexto. *Corrige:* baseline original sempre presente.
- **Não combinar marco 60 e 90 na reunião do 30.** Atrasa renovação. *Corrige:* agenda combinada.
- **Pular marco 90 porque "está tudo bem".** Cliente esquece o ganho. *Corrige:* os 3 marcos são obrigatórios.

## Checklist de saída
- [ ] Metodologia de medição idêntica à do baseline.
- [ ] Relatório de marco 30 produzido com 4 slides.
- [ ] Apresentação presencial com dono + champion.
- [ ] Cálculo de payback acumulado claro.
- [ ] Plano para 90 dias seguintes documentado.
- [ ] Marcos 60 e 90 agendados.

## Vai além
- Manual canônico, seção "Indicadores e medição".
- Pesquisa A §2.5 — payback médio 3,2 meses.
- Pesquisa F §1.2 — modelo tripartite.
- Exercício prático: simular relatório de marco 30 dias completo para o cliente do exercício, com os 4 slides e a estrutura de apresentação.
