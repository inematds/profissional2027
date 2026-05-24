---
nivel: 3
bloco: B
modulo: 3
titulo: Contrato de manutenção recorrente — o motor econômico
tempo_leitura: 16
exit_criteria: Aluno tem contrato de manutenção pronto com tiers, cláusula 14ª de transição sem lock-in, e modelo de relatório mensal
---

# Contrato de manutenção recorrente — o motor econômico

## O que você vai aprender

- Por que a manutenção é o produto principal, não a implementação.
- A estrutura mínima do contrato de manutenção e os três tiers de referência.
- A cláusula 14ª (transição sem lock-in) e por que ela diferencia o aluno do método DPIA.
- Diferença entre escopo coberto (bug, ajuste, atualização) e escopo cobrado à parte (novo agente, nova integração).
- Gatilho de revisão anual de preço e política de banco de horas.

## Por que isso importa

Pesquisa F §1.2 e §3.3 e Pesquisa G §8 estabelecem que a manutenção é o motor econômico da operação. O aluno com 5 clientes em manutenção a R$ 2.500/mês tem R$ 12.500 de base mensal antes de qualquer venda nova. Sem manutenção, o aluno vive de projeto novo todo mês — modelo que não escala e quebra na primeira gripe.

O cliente não compra "manutenção técnica". Compra **tranquilidade e evolução continuada** (Pesquisa F §3.3). A venda da manutenção é diferente da venda do projeto: argumenta-se sobre risco de drift, mudança de stack do mercado, e crescimento do agente conforme o negócio muda.

Pesquisa G §6 (Princípio 1 das implicações): "o contrato de manutenção é o produto principal, não a implementação." A implementação é a porta de entrada; o contrato recorrente é a receita sustentável.

## Conteúdo principal

### Por que vender manutenção junto, não depois

Pesquisa F §1.2 e DPIA canônico (anti-armadilha 9): manutenção é parte do contrato vendido em D, não venda nova em A. Quem trata manutenção como upsell pós-projeto fecha 30% dos casos. Quem vende junto fecha 75%.

O argumento de venda: "instalar e ir embora é trabalho de 8 semanas. Instalar e ficar do lado é parceria de anos. O agente vai aprender, o seu negócio vai mudar, as APIs vão mudar. A manutenção mantém o investimento vivo."

A proposta de projeto (módulo 05 do Bloco A) **sempre** inclui a manutenção como linha separada, com preço definido. O contrato de manutenção é assinado simultaneamente ao de projeto, mas só **entra em vigor** no marco 30 dias pós-deploy (início do mês 2 após go-live).

### Os três tiers de manutenção

Pesquisa F §1.1 e Pesquisa G §8.3 estabelecem padrão de mercado:

**Tier Básico — R$ 1.200–1.500/mês**

Cliente típico: PME com 1 agente único, 1–3 usuários, processo de baixa complexidade.

Entregas:

- 1 visita presencial mensal de 2h (ou videochamada se geograficamente inviável).
- 1 check-in remoto quinzenal de 30 min.
- Suporte assíncrono via WhatsApp Business (resposta em 4h úteis).
- Ajustes de prompt e atualização de playbook mensal.
- Banco de horas: 5h/mês inclusas para pequenos ajustes.

**Tier Intermediário — R$ 1.800–3.500/mês**

Cliente típico: PME com 1–2 agentes, 4–10 usuários, integrações múltiplas.

Entregas (inclui tudo do Básico, mais):

- 2 visitas presenciais/mês.
- Relatório mensal formal de 1 página + apresentação em vídeo de 5 min.
- Gravação de 1–2 micro-vídeos novos por mês.
- Ajuste de base de conhecimento (RAG) mensal.
- SLA de 2h úteis para suporte assíncrono.
- Banco de horas: 10h/mês.

**Tier Avançado — R$ 3.500–7.000/mês**

Cliente típico: PME média com múltiplos agentes ou múltiplos pontos de operação.

Entregas (inclui tudo do Intermediário, mais):

- Visita semanal no primeiro trimestre, depois quinzenal.
- SLA escrito formal com penalidade definida.
- Programa de formação contínua do champion interno.
- Bônus por KPI atingido (cláusula opcional — ver módulo 05 do Bloco C).
- Banco de horas: 20h/mês.
- Plano de rollback testado trimestralmente.

A escolha do tier sai do diagnóstico — não é decisão de meio do projeto. O baseline define complexidade, e a complexidade define tier.

### Estrutura do contrato (referência ao template)

O contrato de manutenção tem 16 cláusulas. Estrutura mínima:

**Cláusulas 1–3.** Partes, objeto (manutenção do sistema entregue no contrato de projeto X de [data]), vigência (12 meses, renovação automática).

**Cláusulas 4–5.** Escopo coberto (lista nominal), escopo NÃO coberto (lista nominal).

**Cláusula 6.** Tier escolhido, com referência a anexo descritivo.

**Cláusula 7.** Investimento mensal e reajuste (INPC ou IPCA, gatilho anual).

**Cláusulas 8–9.** SLA por severidade (crítico/moderado/informativo) e canais de comunicação.

**Cláusulas 10–12.** Banco de horas, rollover (horas não usadas expiram no mês ou acumulam até 50% do banco), gatilhos de cobrança adicional.

**Cláusula 13.** Confidencialidade e LGPD (operador continua nos termos do contrato de projeto).

**Cláusula 14 — Transição sem lock-in.** Detalhada abaixo.

**Cláusulas 15–16.** Rescisão (aviso prévio de 30 dias por qualquer parte) e foro.

### A cláusula 14ª — transição sem lock-in

Este é o diferencial do método DPIA (Princípio 7: stack composable, sem lock-in). A cláusula 14ª garante que o cliente nunca fica refém do aluno.

Texto-base:

> "Em caso de rescisão ou não-renovação, o Contratado se compromete a entregar à Contratante, em até 30 dias do encerramento: (i) todos os system prompts versionados; (ii) base de conhecimento completa em formato consultável; (iii) workflows n8n exportados; (iv) credenciais de todos os serviços de terceiros (Supabase, WABA, ERP, etc.) que estão em conta da Contratante; (v) documentação operacional atualizada; (vi) micro-vídeos gravados durante o contrato; (vii) plano de rollback documentado. Adicionalmente, o Contratado fornecerá até 4 horas de transição técnica para profissional indicado pela Contratante, sem custo adicional."

Por que isso é virtude e não ingenuidade:

1. **Sinaliza confiança.** O aluno que escreve cláusula de transição está dizendo "eu não preciso te prender. Você fica porque o trabalho vale".
2. **Atrai cliente sério.** Cliente que valoriza autonomia (a maioria) prefere fornecedor que não tenta amarrar.
3. **Diferencia de plataforma fechada.** Quando o concorrente é HubSpot/Salesforce/plataforma proprietária, a cláusula 14ª é argumento de venda. "Lá você nunca sai. Aqui, você pode sair quando quiser."
4. **Quase nunca é acionada.** Cliente que tem boa entrega não rescinde. Cliente que rescinde gerou pouca receita futura — não vale a pena reter à força.

### Escopo coberto pela manutenção

Lista padrão (Cláusula 4):

- Ajuste de system prompts quando o comportamento deriva.
- Atualização de base de conhecimento (RAG) com novos documentos do cliente.
- Correção de bugs do sistema entregue.
- Atualização quando APIs de fornecedores mudam (dentro do banco de horas).
- Relatório mensal de métricas vs. baseline.
- Visitas e check-ins conforme tier.
- Atualização do playbook do agente (documento vivo, Princípio 6 do DPIA).
- Treinamento de até 1 novo funcionário do cliente por trimestre (acima disso, cobrado à parte).
- Suporte ao champion interno via canal assíncrono.

### Escopo NÃO coberto (cláusula 5)

Lista padrão:

- Desenvolvimento de novo agente em processo diferente do contratado.
- Integração com novo sistema (ex: contratou com Bling, quer agora com Omie).
- Treinamento de novos funcionários acima do limite previsto.
- Visitas emergenciais acima do número contratado (mesmo no Avançado).
- Recuperação de dados perdidos por erro da equipe do cliente.
- Atualização de stack para novas versões mais caras (ex: cliente quer usar GPT-5 em vez de Claude — análise de impacto cobrada).
- Aumento substancial de volume além do baseline (ex: agente foi projetado para 100 atendimentos/dia, cliente cresce para 500 — exige redimensionamento).
- Suporte fora do horário comercial, exceto SLA de tier Avançado.

Esses itens viram aditivo ou novo projeto. O aluno mantém o cliente, mas com receita extra.

### Banco de horas e rollover

Banco de horas serve para pequenos ajustes ad hoc. Política recomendada:

- Horas não usadas no mês acumulam até 50% do banco mensal para o mês seguinte.
- Horas que ultrapassam 50% expiram.
- Horas extras (além do banco) são cobradas a R$ 200–350/hora (3x o custo-hora interno do aluno).

Exemplo: tier Intermediário com 10h/mês. Usou 3h em julho — 7h acumulam? Não. Acumulam 5h (metade do banco). Em agosto, banco fica em 15h. Usou 18h em agosto — 3h cobradas a R$ 300/h = R$ 900 adicionais.

### Revisão anual de preço

Cláusula obrigatória:

> "O valor mensal será reajustado anualmente pelo INPC acumulado dos últimos 12 meses, na data de aniversário do contrato. Reajustes adicionais (acima do INPC) podem ser propostos pelo Contratado com 60 dias de antecedência, mediante justificativa de aumento de escopo ou custo operacional."

Sem cláusula de reajuste, o aluno fica preso a preço de 2026 atendendo cliente em 2030. Inflação corrói margem silenciosamente.

## Exemplo aplicado — Polaris Bebidas

Polaris fechou tier Intermediário (R$ 2.500/mês). Contrato de manutenção assinado junto com o de projeto. Entrada em vigor no marco 30 dias pós-deploy.

Mês 4 de operação: Cláudio pediu integração com Pix Asaas. Isso virou aditivo (R$ 4.800, 3 semanas), porque não era escopo da manutenção.

Mês 7: Cláudio pediu agente para responder também no Instagram. Aluno apontou cláusula 5 — novo canal é novo projeto. Cláudio pediu proposta, recebeu, fechou um projeto adicional de R$ 8.000.

Mês 11: gripe forte do aluno, atrasou check-in quinzenal por 5 dias. Cláusula SLA aplicada: aluno propôs compensação de 2h extras no banco do mês seguinte. Cláudio aceitou.

Mês 12: reajuste anual. INPC acumulado de 4,3% → R$ 2.500 vira R$ 2.607. Cláudio recebeu carta de comunicação 60 dias antes, sem objeção.

## Materiais prontos

- Template completo de contrato de manutenção — 16 cláusulas + anexo de tier (referência cruzada com `metodo/templates/template-contrato-manutencao.md`).
- Anexo de tier descritivo por nível.
- Modelo de carta de comunicação de reajuste anual.
- Tabela comparativa de tiers para apresentação ao cliente.

## Erros comuns

- **Vender manutenção como "se precisar de ajuste me chama".** *Correção:* sem contrato, vira trabalho gratuito infinito. Contrato sempre.
- **Não escrever escopo NÃO coberto.** *Correção:* a lista de exclusões da cláusula 5 é o que protege a margem do aluno.
- **Banco de horas com rollover ilimitado.** *Correção:* 50% máximo. Sem isso, o cliente acumula 30h e descarrega num mês — o aluno trabalha de graça.
- **Esquecer reajuste anual.** *Correção:* INPC sempre. Reajustes adicionais com 60 dias de antecedência se necessário.
- **Não incluir cláusula 14ª.** *Correção:* a transição sem lock-in é diferencial do método. Sem ela, o aluno é "mais um".
- **Vender manutenção depois do go-live.** *Correção:* manutenção vendida na proposta de projeto, assinada simultaneamente, ativa no mês 2 pós-deploy.

## Checklist de saída

- [ ] Aluno tem template de contrato de manutenção em arquivo editável.
- [ ] Aluno definiu três tiers com escopo e preço próprios (calibrados ao nicho).
- [ ] Aluno inclui cláusula 14ª (transição sem lock-in) em todos os contratos.
- [ ] Aluno tem política de banco de horas escrita (rollover 50%, expiração, valor hora extra).
- [ ] Aluno consegue listar o que está coberto e o que NÃO está, sem consultar.
- [ ] Aluno entendeu que manutenção vende junto com projeto, não depois.
