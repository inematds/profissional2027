---
nivel: 2
trilha: D
modulo: 3
titulo: Mapeamento de Dor Real
tempo_leitura: 16
exit_criteria: aluno produz, para um cliente (real ou role-play), tabela comparativa dor declarada × dor real × volume × custo estimado, com 3 a 5 dores priorizadas e uma escolhida para projeto.
---

# Mapeamento de Dor Real

## O que você vai aprender
- A diferença entre dor declarada (o que o dono diz que dói) e dor real (o que de fato consome tempo, dinheiro e moral da equipe).
- Como o walk-the-floor revela dor real em 4 horas de observação.
- A regra "frequência diária × tempo agregado × custo unitário" para selecionar gargalo automatizável.
- Como descartar dor que parece grande mas tem volume insuficiente.
- Como apresentar a dor priorizada ao dono sem confrontá-lo.

## Por que isso importa
Toda PME tem dores. Nem toda dor vira projeto. O implementador junior aceita qualquer dor que o dono mencionar; o sênior filtra com critério de volume. A diferença prática: o junior automatiza um processo de baixa frequência e cobra R$ 30 mil por algo que economiza R$ 800/mês — payback 38 meses, cliente furioso. O sênior automatiza o processo de frequência diária e cobra R$ 50 mil por algo que economiza R$ 18 mil/mês — payback 2,8 meses, cliente que vira indicação. Pesquisa A §2.5 e F §1.4 indicam que payback de 3,2 meses é o ponto-base defensável; menos que isso, o aluno atira no pé.

A sub-etapa D.2 do manual canônico — "identificar gargalos com volume, não com gosto" — é a regra central deste módulo.

## O passo a passo

**1. Listar todas as dores declaradas na entrevista.** Saída da entrevista com o dono (módulo 02) costuma ter 5 a 12 dores mencionadas. Listar todas, sem filtrar ainda. Em geral, o dono prioriza por urgência subjetiva (o que doeu ontem) — não por volume.

**2. Mapear o processo real no walk-the-floor.** Walk-the-floor é meio expediente ao lado do operador, com cronômetro, caderno aberto e celular guardado. Roteiro:
- Sentar onde o operador senta.
- Observar o ciclo completo de pelo menos 8 a 12 interações reais.
- Cronometrar o início e o fim de cada bloco do processo (chegada da mensagem, primeira resposta, confirmação, lançamento no ERP, encerramento).
- Anotar interrupções (telefone, outra atendente perguntando, cliente físico chegando).
- Anotar regras tácitas em ação ("aqui ela abriu uma planilha que ninguém mencionou na entrevista").
- Anotar emoções do operador: irritação, hesitação, alívio. Emoção marca onde o atrito é real.

**3. Para cada dor declarada, calcular três variáveis:**
- *Frequência:* quantas vezes por dia/semana ocorre?
- *Tempo agregado:* tempo médio por ocorrência × frequência mensal.
- *Custo unitário:* tempo agregado × custo-hora da pessoa envolvida + custo de erro/retrabalho/oportunidade perdida.

Dor com frequência menor que 3 vezes/semana raramente justifica projeto. Vira "observação para o relatório" — pode entrar como melhoria menor numa fase futura.

**4. Cruzar com dor observada no walk-the-floor.** Quase sempre, o walk-the-floor revela 1 ou 2 dores que o dono não mencionou (porque ele não está lá), e mostra que uma das dores mencionadas pelo dono não é tão grande quanto ele acha. Essa comparação dor-declarada × dor-real é o coração do diagnóstico.

**5. Construir tabela priorizada.** Formato sugerido:

| # | Dor declarada (dono) | Dor observada (operador) | Frequência | Tempo agregado/mês | Custo R$/mês | Score |
|---|----------------------|---------------------------|------------|----------------------|----------------|-------|
| 1 | "respondemos lento no WhatsApp" | confirmado + bairro/SKU ausente | 480 pedidos/mês | 376h | R$ 14.300 | A |
| 2 | "perdemos pedido para concorrente" | parcialmente — 7-8% perdidos | 36 pedidos/mês perdidos | 0h direto, ~R$ 9k receita | R$ 9.000 | B |
| 3 | "demora para emitir NF-e" | não confirmado — operadora resolve em <2min | 480/mês | 16h | R$ 600 | D |

Score A/B/C/D combina volume + custo + viabilidade técnica. Critério:
- **A:** alta frequência + alto custo + tecnologia madura disponível (ex.: atendimento WhatsApp, agendamento, FAQ).
- **B:** alta frequência + custo médio OU custo alto + frequência média.
- **C:** baixa frequência + alto custo (ex.: contrato grande raro). Pode ir para fase futura.
- **D:** baixa frequência + baixo custo. Não automatizar.

**6. Selecionar UMA dor para o projeto.** Em PME, sempre uma. O Princípio 1 do método e a anti-armadilha #10 do manual canônico ("tentar atravessar várias frentes ao mesmo tempo no primeiro cliente") são consistentes: um DPIA por processo. Outro processo é outro DPIA.

**7. Apresentar ao dono sem confrontar.** O dono costuma ter colocado a dor errada no topo. Apresentação:
- *"Pelo que medi com a Maria na quarta, o ponto onde mais se perde tempo é X — não Y como tínhamos conversado na segunda."*
- Mostra a tabela com os números.
- *"O Y existe e vale tratar, mas em um segundo momento. Para o primeiro projeto, X gera payback estimado em N meses."*

O dono quase sempre concorda quando o número está claro. Quando insiste em Y, é sinal de que o problema dele com Y é pessoal (cliente importante que reclamou), não operacional. Aí o implementador documenta a preferência do dono no relatório como "decisão do cliente, com risco de payback estendido".

## Exemplo aplicado — Polaris Bebidas (Joinville/SC)

Saída do walk-the-floor da quarta-feira (3 horas ao lado da Maria + 1 hora ao lado do João):

**Dores declaradas pelo dono (segunda-feira):**
1. "Respondemos lento no WhatsApp."
2. "Perdemos pedido para concorrente quando demoramos."
3. "Demora para emitir NF-e."
4. "Erro de SKU gera devolução cara."
5. "Eu mesmo respondo até 21h, é insustentável."

**Dores observadas no walk-the-floor:**
- Tempo médio para fechar pedido: 47 min (medido em 12 pedidos).
- Tempo gasto perguntando bairro/CEP quando cliente esquece: 4-6 min por pedido (em ~40% dos pedidos).
- Tempo gasto pesquisando SKU no Bling com nomenclatura inconsistente: 3-5 min (em ~25% dos pedidos).
- Cliente VIP (Bar do Zé) entrou 2 vezes no expediente — Maria sempre escalou para Sr. Roberto, com fila de espera de 18 min.
- NF-e: emitida em 90 segundos pela Maria; não é gargalo.
- Áudios longos: 5 pedidos no expediente; cada áudio levou ~3 min para ser transcrito.

**Tabela priorizada produzida:**

| # | Dor declarada | Dor observada | Frequência/mês | Tempo agregado | Custo R$/mês | Score |
|---|----|----|----|----|----|----|
| 1 | "lentos no WhatsApp" | confirmado: 47min/pedido | 480 pedidos | 376h | R$ 14.288 | A |
| 2 | "perdemos pedido" | parcial: 7% pedidos perdidos | 36 perdidos | — | R$ 9.000 receita | B |
| 3 | "demora NF-e" | refutado: 90s | 480 | 12h | R$ 456 | D |
| 4 | "erro SKU" | confirmado: ~11% retrabalho | 53 devoluções | 18h | R$ 1.954 (logística) | B |
| 5 | "dono atende 21h" | confirmado: 14 atendimentos/sem | 60/mês | 30h dono | R$ — qualitativo | A* |

Dor selecionada para o projeto: **#1 (atendimento de pedido no WhatsApp)**, que naturalmente carrega parte da #2, #4 e #5 ao ser resolvida. NF-e fora do escopo. Dor declarada #3 fica como observação no relatório.

Conversa com o dono na sexta: *"Sr. Roberto, a NF-e leva 90 segundos. Não é gargalo. O gargalo é o pedido em si — 47 minutos por pedido, 376 horas/mês. Resolvendo isso, o senhor para de atender 21h, e a Maria fica livre para a função nova que conversamos. NF-e ficamos olhando depois."* — Resposta do dono: aceito.

## Erros comuns
- **Aceitar a priorização do dono sem medir.** Implementador junior trata a dor declarada como verdade. O dono fala por sensação. *Corrige:* tabela com número antes de propor escopo.
- **Selecionar múltiplas dores no primeiro projeto.** "Vamos automatizar atendimento + cobrança + agendamento." Resultado: nada fica pronto. *Corrige:* uma dor, um projeto, um DPIA.
- **Ignorar regra tácita observada no walk-the-floor.** Implementador vê a planilha lateral da Maria, não anota, e o agente sai sem o dado da promo da semana. *Corrige:* tudo que aparece no walk-the-floor entra no caderno e na tabela.
- **Confrontar o dono publicamente.** "O senhor estava errado, a NF-e não é problema" desmoraliza. *Corrige:* apresentar com número, deixar o dono concluir sozinho.
- **Tentar resolver dor de tipo C (baixa frequência) "porque dá pena do cliente".** Score D nunca vira projeto. *Corrige:* disciplina de score; emoção fora.

## Checklist de saída
- [ ] Lista completa de dores declaradas (8 a 12 itens).
- [ ] Walk-the-floor realizado, com cronômetro, em pelo menos 8 ciclos completos do processo.
- [ ] Tabela priorizada produzida com score A/B/C/D em cada dor.
- [ ] Uma dor selecionada para o projeto, com justificativa numérica.
- [ ] Dores não selecionadas registradas como observação para fases futuras.
- [ ] Conversa de validação com o dono concluída, com decisão registrada.

## Vai além
- Manual canônico, sub-etapa D.2 — "identificar gargalos com volume, não com gosto".
- Pesquisa D §4 — razões #1 e #8 de falha em projetos de IA (escopo difuso, ausência de métricas).
- Síntese transversal §4.1 — sweet spot do ICP cliente (volume mínimo viável).
- Exercício prático: pegar uma empresa do círculo do aluno, listar 10 dores declaradas pelo dono e fazer walk-the-floor de 2 horas. Produzir tabela priorizada e selecionar 1 dor.
