---
nivel: 2
trilha: D
modulo: 4
titulo: Baseline Numérico
tempo_leitura: 18
exit_criteria: aluno produz baseline completo (tempo, volume, taxa de erro, custos, KPIs de 90 dias com responsável) para o cliente do exercício, com método de medição documentado.
---

# Baseline Numérico

## O que você vai aprender
- O que é baseline e por que ele é o exit-gate inegociável da fase D.
- Como medir tempo, volume, taxa de erro e custo com cronômetro e exportação real, não com chute.
- Como calcular custo-hora de uma pessoa em PME, considerando salário + encargos + benefícios.
- Como definir KPIs de 90 dias com número, prazo e responsável.
- Como calcular ROI esperado e payback com a fórmula do método.

## Por que isso importa
Princípio 2 do DPIA: sem baseline, sem projeto. 67% das PMEs nunca mediram o processo atual antes de "automatizar" (Nautis, Pesquisa A §2.5). Sem número de partida, payback é infinito — o cliente não tem como reconhecer melhoria e o implementador não tem como defender preço. O baseline assinado é o slide 1 da apresentação trimestral; é o documento que o implementador exibe 90 dias depois para mostrar que entregou. Em retorno: o slide 1 é o que diferencia o implementador sério do amador (manual canônico, "Indicadores e medição").

Baseline também é a defesa do implementador. Sem ele, qualquer cliente insatisfeito pode dizer "não melhorou" — e o implementador não tem como provar o contrário.

## O passo a passo

**1. Definir as 4 dimensões obrigatórias.** Para cada processo selecionado no módulo 03, mede-se:
- **Tempo:** tempo médio por ocorrência (cronômetro real, em pelo menos 10-15 amostras).
- **Volume:** ocorrências por dia/semana/mês (exportação do ERP ou contagem direta).
- **Taxa de erro/retrabalho:** % de ocorrências que precisam ser refeitas, corrigidas ou que geram devolução.
- **Custo:** custo unitário × volume + custo de erro + custo de oportunidade.

Quando o cliente diz "isso é difícil de medir", o implementador mede ele mesmo. Sem chutar para preencher (manual canônico, D.3).

**2. Como medir tempo (cronômetro real).** Sentar ao lado do operador. Cronometrar do momento que a mensagem chega ao momento que o pedido fica encerrado no ERP. Mínimo 10 amostras (idealmente 15-20) ao longo de pelo menos 2 períodos diferentes (manhã e tarde). Anotar média, mediana e dispersão. Mediana é mais robusta que média para PME (outliers de pedido grande distorcem).

**3. Como medir volume.** Três caminhos:
- *Exportação do ERP*: Bling, Omie e Conta Azul exportam lista de pedidos por período em CSV. Pega-se 30 a 90 dias.
- *Exportação do WhatsApp Business*: chats podem ser exportados; conta-se interações.
- *Contagem manual*: em PMEs muito pequenas, o operador conta no celular durante uma semana com check de cada interação.

Mínimo 30 dias de janela; ideal 90 dias para captar sazonalidade.

**4. Como medir taxa de erro/retrabalho.** Em distribuidora, devolução por SKU errado é o indicador limpo (registro no Bling). Em clínica, no-show é o indicador. Em contábil, retorno de documento mal arquivado é o indicador. Cada nicho tem o seu — listar com o operador na entrevista.

**5. Como calcular custo-hora real.** A pessoa que opera o processo tem custo-hora maior que o "salário ÷ 220 horas". Cálculo realista:
- Salário bruto + encargos (≈ 70% adicionais em CLT no Brasil) + benefícios (VR, plano de saúde, etc.).
- Total mensal ÷ 176 horas úteis efetivamente trabalhadas (descontando feriados, férias, faltas).
- Resultado: custo-hora real do operador em PME costuma ser 1,8 a 2,3 vezes o "salário ÷ 220".

Exemplo: vendedor interno com salário bruto R$ 3.200 → custo total ≈ R$ 5.700/mês → custo-hora ≈ R$ 32,40/h. Não R$ 14,50 como o cálculo ingênuo sugeriria.

**6. Calcular custo agregado do processo.** Tempo agregado × custo-hora + custo de erro (quanto custa cada retrabalho) + custo de oportunidade (quanto se deixa de faturar quando o processo falha).

**7. Definir KPIs de 90 dias.** Para cada baseline, um KPI futuro com:
- *Número* (não "melhorar", mas "de 47min para 5min").
- *Prazo* (90 dias é o padrão; 30 e 60 dias são marcos intermediários).
- *Responsável* (champion interno do cliente, nomeado).
- *Forma de medição* (mesma ferramenta usada no baseline, mesma janela, mesma metodologia).

KPI sem número, prazo e responsável é decorativo. Pesquisa A §2.5 e D §4: 80% das falhas em IA têm origem em "falta de clareza no caso de uso" — KPI vago é manifestação clara disso.

**8. Calcular ROI esperado (calculadora do método).**

```
Ganho mensal = (horas/mês economizadas × custo/hora-pessoa) 
             + (receita adicional capturada por velocidade ou conversão) 
             - (custo de operação do agente: LLM + n8n + manutenção)

Payback (meses) = Investimento de projeto ÷ Ganho mensal
```

Modelo completo no anexo de cálculo do `templates/template-baseline-assinado.md`.

Payback médio defensável: 3,2 meses (Pesquisa A §2.5). Promessa abaixo disso ataca a credibilidade. Promessa acima de 6 meses mata a venda. Sweet spot: projetar entre 3 e 5 meses com folga de erro.

**9. Documentar método de medição.** Cada número no baseline carrega uma nota de rodapé: "medido com cronômetro em 14 pedidos entre 09/05 e 16/05, manhã e tarde" ou "exportação do Bling, período 01/02 a 30/04". O cliente assina sabendo como o número foi feito; em 90 dias, refaz-se a medição da mesma forma.

**10. Levar o baseline para assinatura.** Documento separado do relatório de diagnóstico, em geral 2 a 4 páginas. Formato no `template-baseline-assinado.md`. Assinatura física ou eletrônica do dono. Cópia para o implementador, cópia para o cliente.

## Exemplo aplicado — Polaris Bebidas (Joinville/SC)

Após walk-the-floor da quarta + exportação do Bling dos últimos 90 dias + cronometragem de 14 pedidos, o aluno produz:

**Baseline da Polaris Bebidas — atendimento de pedido WhatsApp**

| Dimensão | Valor medido | Método |
|----------|--------------|--------|
| Tempo médio por pedido | 47 min (mediana), 52 min (média), desvio 14 min | Cronômetro, 14 amostras, quarta 09h-12h e 14h-17h |
| Volume mensal | 480 pedidos (média dos últimos 3 meses) | Exportação Bling, fev-abr |
| Taxa de retrabalho (devolução SKU) | 11,2% | Bling, registros de devolução fev-abr |
| Pedidos perdidos por demora (cliente cancelou) | ≈ 7% (estimativa) | Análise de conversas WhatsApp + relato Maria/João |
| Custo-hora operador interno | R$ 32,40/h | Salário R$ 3.200 + encargos 78% ÷ 176h |
| Tempo agregado mensal | 376 horas | 47min × 480 pedidos |
| Custo agregado operador | R$ 12.182/mês | 376h × R$ 32,40 |
| Custo de devolução | R$ 1.954/mês | 53,8 dev. × R$ 36,30 custo médio logístico |
| Custo de oportunidade (pedidos perdidos) | ≈ R$ 9.000/mês | 33,6 ped. × ticket médio R$ 268 |
| Custo total mensal do gargalo | R$ 23.136/mês | soma |

**KPIs de 90 dias:**

| KPI | Valor atual | Meta 90d | Responsável | Medição |
|-----|-------------|----------|-------------|---------|
| Tempo médio resposta inicial | 18 min (estimado) | < 90 segundos | Maria (champion) | Webhook n8n + log |
| Tempo médio para fechar pedido | 47 min | < 8 min em 80% dos casos | Maria | Webhook n8n + Bling |
| Taxa de devolução por SKU errado | 11,2% | < 3% | Maria | Bling |
| Pedidos atendidos após 18h | 14/sem (Sr. Roberto) | 0 — agente cobre até 22h | Sr. Roberto | Log do agente |

**ROI esperado:**

- Investimento de projeto: R$ 58.000 (fase D R$ 5.500 já paga + R$ 52.500 fase P+I+A).
- Ganho mensal projetado: economia de ≈ 280h/mês × R$ 32,40 = R$ 9.072 + redução devolução R$ 1.250 + recuperação pedidos perdidos R$ 6.300 − custo operação agente R$ 950 = **R$ 15.672/mês**.
- Payback projetado: 58.000 ÷ 15.672 = **3,7 meses**.

**Documento de baseline impresso, assinado pelo Sr. Roberto na sexta da semana seguinte. Cópia arquivada digitalmente.**

## Erros comuns
- **Aceitar "depois te mando os números" do cliente.** Nunca chega. *Corrige:* implementador mede, no walk-the-floor.
- **Calcular custo-hora pelo salário bruto ÷ 220.** Subestima em ~80%. *Corrige:* salário + encargos + benefícios ÷ 176h.
- **Usar média e ignorar mediana.** Outliers (pedido grande de fim de mês) distorcem média. *Corrige:* registrar média + mediana + desvio.
- **KPI sem número, prazo e responsável.** "Melhorar o atendimento" não é KPI. *Corrige:* "de 47min para <8min em 90 dias, responsável Maria".
- **Não documentar método de medição.** 90 dias depois, ninguém lembra como mediu o baseline. *Corrige:* nota de rodapé em cada número.
- **Prometer payback de 1 mês.** Ataca credibilidade e dá motivo para o cliente recusar o projeto. *Corrige:* 3 a 5 meses, com folga.

## Checklist de saída
- [ ] As 4 dimensões medidas com método explícito.
- [ ] Mínimo de 10-15 amostras de tempo com cronômetro.
- [ ] Exportação de volume (30-90 dias) anexada.
- [ ] Custo-hora calculado realisticamente (encargos + benefícios).
- [ ] KPIs de 90 dias com número, prazo e responsável.
- [ ] ROI e payback projetados pela fórmula do método.
- [ ] Baseline impresso, assinado pelo dono, arquivado.

## Vai além
- `metodo/templates/template-baseline-assinado.md` — formulário oficial + anexo de cálculo de ROI.
- Manual canônico, sub-etapa D.3 e seção "Calculadora de ROI".
- Pesquisa A §2.5 — número de Nautis sobre payback médio de 3,2 meses.
- Exercício prático: medir baseline real (tempo + volume + erro + custo) de um processo de uma empresa do círculo do aluno, em pelo menos 10 amostras de tempo, e produzir documento estilo "Baseline Assinado" mesmo sem cliente formal.
