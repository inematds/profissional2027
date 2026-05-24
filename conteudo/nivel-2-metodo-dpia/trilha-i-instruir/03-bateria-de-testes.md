---
nivel: 2
trilha: I
modulo: 3
titulo: Bateria de Testes
tempo_leitura: 18
exit_criteria: aluno conduz bateria de 30-50 casos reais, calcula taxa de acerto medida e categoriza erros em pelo menos 4 categorias.
---

# O que você vai aprender
- Como construir bateria de homologação com 30 a 50 casos reais retirados do histórico do cliente.
- Como definir o critério objetivo de "acerto" para o caso, validado pelo operador.
- Como calcular a taxa de acerto medida (não estimada) e por que ≥ 85% é o piso.
- Como categorizar erros em famílias para guiar o ajuste no módulo 04.
- Como apresentar resultado do teste para o cliente sem soar defensivo.

## Por que isso importa
Bateria de testes é o portão de qualidade da fase I. Sem ela, o agente entra em produção e descobre seus erros com cliente real — incidente garantido, perda de confiança, possível fim do contrato. Pesquisa G §3.3: projetos com teste estruturado em homologação têm 4 vezes menos incidente na primeira semana de produção.

O número ≥ 85% não é arbitrário. Vem da prática Nautis (Pesquisa A §2.5) e da prática StackAI/NIST citada em Pesquisa B §8. Abaixo de 85% em homologação, o agente vai ter taxa de override humano > 30% em produção, o que mata a economia do projeto (operador acaba refazendo tudo).

## O passo a passo

**1. Selecionar 30 a 50 casos do histórico real.** Da exportação do WhatsApp (90 dias) feita em P:
- Selecionar aleatoriamente 40 a 50 conversas completas (do "olá" ao "pedido confirmado" ou ao "ok, valeu").
- Distribuição esperada: ~60% casos do happy path, ~30% casos com exceção (cliente novo, regra tácita disparada, escalonamento), ~10% casos difíceis (cliente irritado, mensagem ambígua, áudio longo).
- Anonimizar nomes pessoais mas manter nomes de cliente PJ e CNPJ (para validar regras como inadimplência).

**2. Para cada caso, registrar:**
- Input completo (texto/áudio do cliente).
- Contexto: data, horário, dia da semana, cliente cadastrado?, histórico recente.
- Resposta humana real (o que o operador respondeu na época).
- Resposta esperada validada pelo operador hoje (que pode ser diferente da resposta real, se houver erro reconhecido).
- Decisão esperada: continuar fluxo / escalar / pedir esclarecimento.

**3. Definir critério de "acerto" por caso.** Acerto não é "resposta idêntica". Critério:
- *Acerto de decisão:* o agente tomou a decisão certa (responder vs. escalar vs. pedir esclarecimento)?
- *Acerto de regra:* o agente aplicou a regra correta (RT-XX correspondente)?
- *Acerto de dados:* o pedido criado bate com o que o cliente pediu (SKU, qtde, valor, entrega, pagamento)?
- *Acerto de tom:* a resposta está no tom institucional-cordial (sem palavras vetadas, sem informalidade indevida)?

Caso conta como acerto quando os 4 critérios passam. Quando algum falha, registrar qual.

**4. Executar a bateria.** Em ambiente de homologação, alimentar o agente caso a caso (ou em batch, via webhook simulado). Para cada caso, capturar:
- Resposta do agente.
- Tools chamadas.
- Tempo de processamento.
- Confidence.
- Decisão tomada.

Em geral, 40 casos consomem 2 a 4 horas de execução + análise. Aluno acompanha cada caso.

**5. Calcular métricas.**

- *Taxa de acerto geral:* (casos com acerto nos 4 critérios) ÷ total. Alvo ≥ 85%.
- *Taxa por critério:* acerto de decisão / acerto de regra / acerto de dados / acerto de tom — separados, para diagnóstico.
- *Latência média:* tempo até primeira resposta.
- *Taxa de escalonamento:* % de casos onde agente decidiu escalar (deve bater com a expectativa do design — em geral 15-30% no primeiro deploy).
- *Distribuição de confidence:* % de casos com confidence < 0,7 (que devem virar escalonamento via gatilho).

**6. Categorizar erros.** Para cada caso que falhou, atribuir uma categoria de erro:
- *Falha de contexto:* o agente não tinha a informação no Pacote de Contexto. → volta para P.
- *Falha de regra:* o agente tinha a regra mas não aplicou. → ajustar system prompt (módulo 04).
- *Falha de interpretação:* o agente não entendeu o cliente. → mais few-shot, melhor RAG.
- *Falha de tool use:* o agente chamou tool errada ou com parâmetro errado. → revisar schema.
- *Falha de tom:* resposta tecnicamente correta, tom inadequado. → revisar exemplos.
- *Caso fora de escopo:* situação não prevista no playbook. → discutir com cliente se entra no escopo ou fica explicitamente fora.

A distribuição entre essas categorias define o caminho de ajuste.

**7. Tomar decisão sobre o exit-gate.**

- *Taxa ≥ 85% e zero categoria com > 30% dos erros:* aprovado, segue para homologação com cliente (módulo 05).
- *Taxa 70-85%:* iterar no módulo 04, refazer bateria.
- *Taxa < 70%:* problema estrutural. Avaliar se voltar para P (faltou contexto crítico) ou se reescrever I do zero (faltou estrutura).

Não negociar o piso. ≥ 85% é o piso, não meta opcional.

**8. Documentar o relatório de teste.** Para o cliente, relatório curto:
- Tamanho da bateria.
- Taxa de acerto geral e por critério.
- Top 3 categorias de erro.
- Ajustes feitos.
- Decisão (segue para homologação | itera).

Este relatório vira anexo do Pacote de Contexto e parte do contrato de manutenção.

## Exemplo aplicado — Polaris Bebidas (Joinville/SC)

Bateria de 42 casos selecionados de fevereiro-abril 2026. Distribuição:
- 26 casos happy path (cliente cadastrado, SKU claro, bairro informado, pagamento Pix/boleto).
- 11 casos com exceção (cliente novo PJ, bairro Glória same-day, áudio, RT-09 desconto recorrente).
- 5 casos difíceis (cliente VIP, cliente irritado pedindo cancelamento, mensagem com sticker, cliente inadimplente, pedido > R$ 5k).

Execução em ambiente de homologação, segunda-feira da semana 3 de I, das 09h às 13h, com Maria acompanhando.

**Resultado primeira rodada (system prompt v0.4):**

| Métrica | Valor | Alvo |
|---------|-------|------|
| Taxa de acerto geral | 79% (33/42) | ≥ 85% |
| Acerto de decisão | 93% (39/42) | — |
| Acerto de regra | 88% (37/42) | — |
| Acerto de dados | 95% (40/42) | — |
| Acerto de tom | 86% (36/42) | — |
| Latência média | 3,8s | < 5s |
| Taxa de escalonamento | 19% | 20-30% |

**Categorização dos 9 erros:**
- *Falha de regra (3):* o agente esqueceu de aplicar RT-09 desconto recorrente em 3 casos onde cliente havia comprado nos últimos 7 dias. Cause: a regra estava no system prompt mas a tool de consulta de histórico não retornava esse dado consistentemente.
- *Falha de interpretação (3):* o agente não entendeu termos regionais (a Skol "lata" pode ser "lata 350" ou "lata 473"; o cliente diz só "lata"). Few-shot não cobria essas variações.
- *Falha de tom (2):* o agente respondeu com "Vamos lá!" em 2 casos, palavra vetada na lista de restrições.
- *Caso fora de escopo (1):* cliente perguntou sobre "cerveja sem álcool", produto novo lançado em maio, não estava no catálogo Bling de teste. Agente respondeu de forma genérica, deveria ter perguntado.

Decisão: itera (módulo 04) e refaz bateria.

**Segunda rodada (system prompt v0.5):**

| Métrica | Valor |
|---------|-------|
| Taxa de acerto geral | 88% (37/42) |

Erros remanescentes:
- 2 casos de áudio longo com ruído de fundo que o Haiku transcreveu errado.
- 2 casos de SKU ambíguo onde o agente decidiu sozinho em vez de perguntar (decisão correta levaria a perguntar, agente seguiu com confiança média).
- 1 caso fora de escopo (mantido como tal).

Decisão: aprovado. Segue para homologação com cliente (módulo 05).

Relatório de teste impresso, 4 páginas. Maria assina como validadora dos casos.

## Erros comuns
- **Bateria pequena (< 20 casos).** Estatística não significativa. *Corrige:* mínimo 30, ideal 40-50.
- **Casos inventados pelo implementador.** Não refletem realidade. *Corrige:* histórico real anonimizado.
- **Critério de acerto subjetivo ("ficou bom").** Não dá métrica. *Corrige:* 4 critérios objetivos.
- **Validar acerto sozinho.** Operador precisa ser quem decide o esperado. *Corrige:* Maria/operador valida cada caso.
- **Não categorizar erros.** Ajuste fica chute. *Corrige:* categorização obrigatória.
- **Aceitar 80% "porque está perto".** Vira projeto frágil em produção. *Corrige:* piso é piso.

## Checklist de saída
- [ ] 30 a 50 casos reais selecionados do histórico.
- [ ] Casos anonimizados.
- [ ] Critério de acerto definido (4 critérios).
- [ ] Validação do esperado feita pelo operador.
- [ ] Bateria executada em ambiente de homologação.
- [ ] Taxa de acerto geral ≥ 85% após iteração.
- [ ] Erros categorizados em famílias.
- [ ] Relatório de teste documentado.

## Vai além
- Manual canônico, sub-etapa I.5 + exit-gate de I.
- Pesquisa B §8 e G §3.3.
- Exercício prático: para o cliente do exercício, montar bateria de 30 casos reais (ou simulados se não houver acesso a histórico real) e medir taxa de acerto na primeira rodada.
