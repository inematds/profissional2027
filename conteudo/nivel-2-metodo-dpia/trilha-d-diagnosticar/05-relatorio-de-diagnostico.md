---
nivel: 2
trilha: D
modulo: 5
titulo: Relatório de Diagnóstico
tempo_leitura: 18
exit_criteria: aluno entrega relatório completo de diagnóstico (8-20 páginas + 10-15 slides + proposta de projeto) para o cliente do exercício, e simula a reunião de defesa em até 30 minutos.
---

# Relatório de Diagnóstico

## O que você vai aprender
- A estrutura padrão do relatório de diagnóstico DPIA — 8 a 20 páginas + apresentação.
- Como construir a narrativa que conecta dor real, baseline, KPI e proposta sem virar marketing.
- Como apresentar o relatório em 30-45 minutos e fechar a proposta na própria reunião.
- Como tratar objeções típicas do dono (preço, prazo, "eu mesmo faço").
- Como colher as assinaturas (baseline + proposta) sem deixar a reunião sem fechamento.

## Por que isso importa
O relatório é o produto da fase D. É o que o cliente recebeu em troca do que pagou. Mais que isso: é o documento que governa todo o restante do projeto. Em 90 dias, no marco de payback, o implementador vai voltar a esse relatório para mostrar o que foi entregue contra o que foi prometido. Relatório frouxo no início = projeto frouxo no final.

Pesquisa F §1.2 e §10.2 são consistentes: a proposta acoplada ao relatório de diagnóstico converte 2 a 3 vezes mais que proposta enviada depois por e-mail. O fechamento acontece na reunião, com o relatório na mesa, ou raramente acontece.

## O passo a passo

**1. Estrutura padrão do relatório (8-20 páginas).** Capítulos:

- *Capítulo 1 — Sumário executivo (1 página).* O que foi diagnosticado, qual a dor priorizada, qual o ganho esperado, qual o investimento, qual o prazo. Decisor lê só isso; o resto é técnico.
- *Capítulo 2 — A empresa e o contexto (1-2 páginas).* Resumo do que foi observado. Mostra ao dono que o implementador entendeu o negócio.
- *Capítulo 3 — Processo atual mapeado (2-3 páginas).* Fluxo do processo selecionado, com diagrama simples (caixas e setas, sem notação BPMN para PME). Quem faz, em que canal, com que ferramenta.
- *Capítulo 4 — Dor real e regras tácitas (2-3 páginas).* Tabela priorizada do módulo 03. Lista de regras tácitas RT-XX capturadas. Aqui aparece o trabalho do implementador.
- *Capítulo 5 — Baseline numérico (1-2 páginas).* Cópia em texto + referência ao documento Baseline Assinado anexo.
- *Capítulo 6 — Proposta de solução (2-3 páginas).* Em alto nível: qual módulo AIOS aplicável, quais integrações, qual matriz HITL prevista, qual reposicionamento das pessoas. Sem detalhe técnico — é fase P em diante.
- *Capítulo 7 — Reposicionamento e champion interno (1 página).* Decisão de reposicionamento de cada pessoa nomeada. Champion identificado.
- *Capítulo 8 — Riscos e LGPD (1-2 páginas).* Mapa de risco simplificado. RIPD enxuto (regime simplificado ANPD Resolução 2/2022). Plano de mitigação.
- *Capítulo 9 — Cronograma e investimento (1-2 páginas).* Linha do tempo das fases P/I/A. Preço fixo do projeto. Tier de manutenção sugerido pós-deploy.
- *Capítulo 10 — Anexos.* Baseline assinado (separado), atas de entrevista, prints de walk-the-floor, exportações do ERP.

Estrutura completa em `templates/template-diagnostico-relatorio.md`.

**2. Estrutura da apresentação (10-15 slides).** Slides extraem só o essencial:
- Slide 1: capa com nome do cliente, data, "Diagnóstico DPIA".
- Slide 2: sumário executivo em 4 bullets.
- Slide 3: contexto da empresa.
- Slides 4-5: processo atual (1 diagrama + 1 slide de dores observadas).
- Slide 6: tabela priorizada de dores (a tabela do módulo 03).
- Slide 7: baseline numérico (a tabela do módulo 04).
- Slide 8: KPIs de 90 dias.
- Slide 9: proposta de solução em alto nível.
- Slide 10: reposicionamento das pessoas + champion.
- Slide 11: cronograma.
- Slide 12: investimento e tier de manutenção.
- Slide 13: ROI projetado e payback.
- Slides 14-15: próximos passos (assinatura do baseline + assinatura da proposta) e "Q&A".

**3. Construir a narrativa.** O relatório segue arco: situação → dor real → custo → projeção → solução → ganho. Nunca o contrário. Apresentar a solução antes de a dor estar precificada gera reação "muito caro". Apresentar o custo antes da solução gera a pergunta "como resolver?", que é onde o implementador quer chegar.

Linguagem: direta, técnica sem jargão, frases curtas. Sem advérbios marqueteiros ("incrivelmente", "totalmente"). Sem comparação com concorrente.

**4. Preparar a reunião de defesa.** 90 minutos:
- 5 min — aquecimento e contrato da reunião.
- 30 min — apresentação dos slides (sem interrupções, exceto perguntas curtas).
- 30 min — discussão e objeções.
- 15 min — assinatura do baseline + assinatura da proposta + próximos passos da fase P.
- 10 min — folga.

Quem deve estar na reunião: dono + sócio (se houver) + champion identificado. Operador não precisa estar — informação operacional já foi colhida.

**5. Tratar objeções típicas.**
- *"É caro."* Resposta: mostra payback em meses. "Em N meses o projeto se paga com o ganho mensal de R$ X. O senhor já gasta R$ Y/mês no processo atual." Nunca baixar preço sem reduzir escopo correspondente.
- *"Posso esperar para ano que vem?"* Resposta: "Esperar 6 meses significa perder ≈ R$ Y de custo agregado. O senhor pode esperar; o projeto também pode esperar. Só que o custo continua correndo."
- *"Eu mesmo faço."* Resposta: "Pode. A maioria não termina porque acumula com a operação. O senhor consegue dedicar 80-120 horas nas próximas 8 semanas para isso?"
- *"Quero outro orçamento."* Resposta: "Pegue. Volto em 7 dias para conversar. Só peço que compare o que entreguei — diagnóstico + baseline + reposicionamento + cronograma — com o que o concorrente entrega na pré-venda dele." Em geral, o concorrente entrega só proposta sem diagnóstico, e o cliente volta.
- *"E se não der certo?"* Resposta: "Garantia: se o KPI de 90 dias não for atingido, X" (modelo de bônus por resultado, Pesquisa G §8.3). Apenas oferecer se o aluno tiver confiança no baseline.

**6. Colher assinaturas na própria reunião.** Documentos impressos prontos para assinar:
- Baseline Assinado (formulário do template-baseline-assinado.md).
- Proposta de projeto (preço fixo, escopo, prazo, condições de pagamento 40/30/30, contrato de manutenção esboçado).

Se o dono pede tempo, combinar retorno em até 3 dias úteis. Sem retorno em 3 dias úteis, é não — o implementador segue para o próximo cliente.

**7. Encerrar a fase D.** Fase D só termina quando os 6 itens do exit-gate estão marcados:
- Baseline assinado.
- KPIs futuros com número, prazo e responsável.
- Champion nomeado (nome, cargo, e-mail).
- Reposicionamento documentado.
- Proposta aceita por escrito.
- Pagamento da fase D liquidado (exceto piloto 1).

Sem os seis, não se passa para P. Não importa o quanto o cliente está empolgado (manual canônico, exit-gate de D).

## Exemplo aplicado — Polaris Bebidas (Joinville/SC)

Sexta-feira, segunda semana, 14h. Sala da diretoria. Sr. Roberto + Sr. Cláudio (sócio comercial externo) + Maria (champion identificada). Aluno chega 15 minutos antes, monta o notebook na TV da sala, deixa o relatório impresso (12 páginas) e o baseline (3 páginas) sobre a mesa.

Apresentação dura 28 minutos. Sumário executivo no slide 2:
- *Dor priorizada:* atendimento de pedido no WhatsApp.
- *Baseline:* 47min/pedido × 480/mês = 376h/mês, custo agregado R$ 23.136/mês.
- *Meta 90 dias:* < 8min/pedido em 80% dos casos; dono fora do WhatsApp 21h.
- *Investimento:* R$ 52.500 de projeto + tier intermediário R$ 2.400/mês de manutenção.
- *Payback projetado:* 3,7 meses.

Sr. Roberto interrompe no slide 6 (dores observadas) para confirmar a regra tácita do "Bar do Zé" — não sabia que a Maria já fazia esse desvio, mas concorda que faz sentido. Mais 12 minutos de discussão. Objeção: "R$ 52.500 é caro." Resposta: "O processo custa R$ 23k/mês hoje. Em 4 meses, paga o projeto, e a partir do mês 5 é ganho líquido."

Sr. Cláudio pede prazo de 1 semana para discutir entre os sócios. O aluno responde: "Combinado. Para não perder tempo, posso deixar o Baseline assinado hoje? O baseline registra o estado atual, não compromete o senhor com o projeto. Se vocês não fecharem, o documento fica com vocês para usarem como referência interna." Sr. Roberto assina o baseline na hora.

Próxima quarta, Sr. Roberto liga: "Fechamos. Manda o contrato." Aluno envia proposta digital, assinada eletronicamente até sexta. Pagamento de 40% (R$ 21.000) na conta na segunda. Champion Maria nomeada formalmente em e-mail. Fase D fechada.

## Erros comuns
- **Mandar relatório por e-mail sem reunião.** Cliente lê pela metade, não assina. *Corrige:* reunião presencial ou videoconferência, com slides e impressão.
- **Apresentar solução antes de precificar dor.** "Muito caro" inevitável. *Corrige:* arco situação → dor → custo → projeção → solução → ganho.
- **Baixar preço sem reduzir escopo.** Sinaliza junior e destrói margem. *Corrige:* "se quiser R$ X, escopo Y; se quiser escopo completo, R$ Z".
- **Não levar documentos impressos para assinatura.** "Te mando depois" significa nunca. *Corrige:* baseline + proposta impressos na mesa.
- **Aceitar "te aviso na próxima semana" indefinidamente.** Sem prazo, é não. *Corrige:* 3 dias úteis ou next.

## Checklist de saída
- [ ] Relatório de 8-20 páginas seguindo a estrutura de 10 capítulos.
- [ ] Apresentação de 10-15 slides com arco narrativo correto.
- [ ] Reunião de defesa simulada (com colega ou cliente real).
- [ ] Pelo menos 3 objeções respondidas com argumento numérico.
- [ ] Baseline assinado e proposta assinada (ou rejeitada com data definida).
- [ ] Os 6 itens do exit-gate de D marcados.

## Vai além
- `metodo/templates/template-diagnostico-relatorio.md` — estrutura oficial do relatório.
- `metodo/templates/template-baseline-assinado.md` — formulário do exit-gate.
- Manual canônico, "Fase D — Artefato entregável" e "Exit-gate".
- Pesquisa F §4.3 — modelo Challenger no fechamento.
- Exercício prático: produzir relatório completo + apresentação + proposta para o cliente do exercício, e gravar role-play da reunião de 30-45 min com colega assumindo o papel do dono.
