---
nivel: 2
trilha: I
modulo: 4
titulo: Iteração e Ajuste
tempo_leitura: 18
exit_criteria: aluno aplica ciclo de iteração em pelo menos 2 rodadas, documenta cada ajuste com motivo e impacto medido, e atinge ≥ 85% na bateria.
---

# O que você vai aprender
- A disciplina de iterar uma mudança por vez, não pacote de mudanças.
- Como decidir entre ajustar system prompt, regras, RAG ou few-shot.
- O anti-padrão "mais regras melhora" e por que ele falha.
- Como evitar regressão (correção que quebra outra coisa).
- Como saber quando parar de iterar e seguir para homologação.

## Por que isso importa
Iteração mal feita prolonga a fase I em semanas. Aluno junior altera 6 coisas ao mesmo tempo, taxa sobe, não sabe o que melhorou; aluno junior depois altera mais 4 coisas, taxa cai, não sabe o que regrediu. O método define disciplina: uma mudança por rodada, com medição. Pesquisa B §10.5 sobre AIOS: módulos que evoluem com disciplina de versionamento mantêm qualidade ao longo do tempo; módulos sem disciplina degradam.

O anti-padrão "mais regras melhora" é particularmente caro. Cada regra extra adiciona ruído no contexto do LLM. Em prompts muito longos com muitas regras, o modelo passa a confundir regras e a ignorar exemplos. A solução costuma ser **menos regras, mais exemplos**, não o contrário.

## O passo a passo

**1. Priorizar erros por impacto.** Da categorização do módulo 03:
- Categoria com mais erros → ataca primeiro.
- Dentro da categoria, casos com maior frequência esperada em produção → ataca primeiro.

Não atacar caso raro antes de caso comum. Ganho marginal de corrigir caso de 1 ocorrência/mês é desprezível.

**2. Decidir o tipo de ajuste por categoria de erro.**

| Categoria de erro | Ajuste indicado |
|--------------------|-----------------|
| Falha de contexto | Voltar para P, adicionar item ao Pacote de Contexto |
| Falha de regra | Reformular regra no system prompt + adicionar few-shot que ilustra a regra |
| Falha de interpretação | Mais few-shot reais com variação linguística; melhorar RAG |
| Falha de tool use | Revisar schema da tool, descrição da tool, exemplo de uso |
| Falha de tom | Adicionar restrição explícita; adicionar few-shot com tom correto |
| Caso fora de escopo | Decidir com cliente: entra no escopo (volta para P) ou fica fora |

**3. Aplicar uma mudança, executar bateria.** Disciplina:
- Versionar system prompt (v0.4 → v0.5).
- Documentar a mudança em commit ou nota: "v0.5 — corrige RT-09 desconto recorrente. Causa: tool não retornava histórico de 7 dias. Mudança: schema da tool expandido + few-shot exemplo 5 atualizado."
- Rodar a bateria completa de novo.
- Medir delta: quantos casos melhoraram, quantos regrediram, taxa final.

**4. Avaliar regressão.** Mudança boa: melhora alguns casos sem quebrar outros. Mudança ruim: melhora 3 casos e quebra 2. Quando há regressão, investigar a causa antes de prosseguir.

**5. Quando ajustar few-shot.** Few-shot é a alavanca mais subestimada. Em geral, melhor que adicionar regra. Cada few-shot real mostra ao modelo: "esta situação acontece assim, responde assim". Adicionar 2-3 exemplos cobrindo a falha quase sempre rende mais que adicionar uma regra.

Regra de bolso: se 3+ exemplos novos não corrigem o erro, aí sim acrescenta uma regra.

**6. Quando ajustar RAG.** RAG é para conteúdo de catálogo, FAQ, políticas. Não é para regras de decisão. Quando o erro é "agente não sabia que tal produto existe" ou "agente não conhecia tal política", melhorar o RAG: revisar chunks, ajustar embeddings, melhorar metadata.

**7. Quando recuar para P.** Se a iteração não converge depois de 3-4 rodadas, em geral o problema é falta de contexto. Volta-se para P:
- Pacote de Contexto incompleto?
- Regras tácitas faltando?
- Glossário insuficiente?
- Base de conhecimento mal estruturada?

Não é vergonha voltar para P. Vergonha é arrastar 6 semanas em I sem convergir.

**8. Limite de rodadas.** Em projetos saudáveis: 2 a 3 rodadas até passar do piso. Em projetos com 5+ rodadas sem chegar a 85%, há problema estrutural — convocar revisão completa com cliente, considerar redefinição de escopo.

**9. Anti-padrão "mais regras melhora".** Sinais de excesso de regras:
- System prompt > 12.000 tokens.
- LLM começa a confundir regras parecidas (R7 e R8 com sobreposição).
- Modelo "esquece" alguns casos.
- Tempo de resposta sobe.

Tratamento: simplificar. Junte regras parecidas. Converta regras descritivas em few-shot. Mova regras contextuais para RAG.

**10. Decidir parar.** Critério para parar de iterar:
- Taxa ≥ 85% em duas rodadas consecutivas.
- Nenhuma categoria de erro com > 30% dos erros restantes.
- Erros remanescentes documentados como "fora de escopo aprovado" ou "caso raro de baixa frequência".

Não persegue 100%. Após 95%, o ganho marginal não compensa o tempo.

## Exemplo aplicado — Polaris Bebidas (Joinville/SC)

Saída da bateria do módulo 03: 79% (v0.4). Erros categorizados:
- Falha de regra (3): RT-09 desconto recorrente não aplicado.
- Falha de interpretação (3): variações "Skol lata" sem volume claro.
- Falha de tom (2): "Vamos lá!" disparado em 2 casos.
- Caso fora de escopo (1): cerveja sem álcool nova.

**Iteração 1 (v0.4 → v0.5).** Ataca falha de tom primeiro (mais simples).
- Adiciona à seção RESTRIÇÕES: "NUNCA use as expressões: 'Vamos lá', 'Bora', 'Beleza', 'Ok então'."
- Adiciona 1 few-shot mostrando substituição correta: "Vamos lá!" → "Posso prosseguir?"
- Reroda bateria.

Resultado v0.5: 84,5% (35/42). Tom corrigido (0 erros). Mas surgiu 1 regressão nova: cliente cadastrado mas com nome diferente do número, agente confundiu.

**Iteração 1.1.** Investiga regressão: a mudança de tom não devia ter quebrado isso. Verifica: nenhuma relação direta. Provável instabilidade de inferência (caso marginal). Decide aceitar e rodar Iteração 2.

**Iteração 2 (v0.5 → v0.6).** Ataca falha de regra (RT-09).
- Diagnóstico: tool `consulta_historico_cliente_bling` só retornava últimos 3 pedidos. Cliente PEDRO005 tinha 2 pedidos em janeiro e 1 em março (apareceu). Mas o que faltava era o pedido de 6 dias atrás (estava no lugar 4 da lista, fora da janela).
- Mudança: schema da tool expandido para retornar últimos 30 dias com paginação.
- Atualiza system prompt na seção FERRAMENTAS.
- Reroda bateria.

Resultado v0.6: 88,1% (37/42). Acima do piso. RT-09 corrigida.

Erros remanescentes:
- 2 áudios com ruído (transcrição falhou — limitação técnica conhecida).
- 2 SKU ambíguos onde agente decidiu sozinho (confidence 0.72, acima do gatilho 0.70 — agente seguiu).
- 1 cerveja sem álcool (caso fora de escopo).

**Iteração 3 (v0.6 → v0.7).** Ataca o gatilho de confidence.
- Mudança: gatilho 0.70 → 0.75 para SKU ambíguo.
- Atualiza system prompt na seção RESTRIÇÕES.
- Reroda bateria.

Resultado v0.7: 88,1% (mesma taxa, mas com 2 casos novos escalando em vez de seguir com erro). Maria valida que escalonamento extra é aceitável.

**Decisão: parar.** Taxa 88,1%, nenhuma categoria com > 30% dos erros, 5 erros remanescentes documentados:
- 2 áudios ruidosos: limitação técnica, marcado para revisão na fase A.
- 1 caso fora de escopo: discutir com Sr. Roberto se entra no escopo (cerveja sem álcool).
- 2 escalações novas que não eram esperadas: ok, são corretas.

Segue para homologação com cliente (módulo 05).

## Erros comuns
- **Mudar muitas coisas por rodada.** Não sabe o que ajudou. *Corrige:* uma mudança, uma rodada.
- **Não versionar.** Perde rastro. *Corrige:* Git/Notion com motivo da mudança.
- **Adicionar regra antes de tentar few-shot.** System prompt incha. *Corrige:* few-shot antes, regra depois.
- **Perseguir 100%.** Custo cresce exponencial. *Corrige:* parar ≥ 85% com erros documentados.
- **Não voltar para P quando deveria.** Arrasta 6 semanas em I sem solução. *Corrige:* depois de 3-4 rodadas sem convergir, voltar para P.

## Checklist de saída
- [ ] Pelo menos 2 rodadas de iteração documentadas.
- [ ] Cada rodada com uma mudança principal isolada.
- [ ] Cada mudança documentada com motivo + impacto medido.
- [ ] Taxa final ≥ 85%.
- [ ] Erros remanescentes categorizados.
- [ ] Decisão de parar tomada com critério claro.

## Vai além
- Manual canônico, "Fase I — Erros comuns e como evitar".
- Pesquisa B §10 — disciplina de versionamento em AIOS.
- Anthropic Prompt Engineering Guide — seção "Iterate".
- Exercício prático: para o cliente do exercício, conduzir 2 rodadas de iteração documentadas e atingir ≥ 85% na bateria.
