---
nivel: 2
trilha: I
modulo: 0
titulo: Visão Geral da Trilha I — Instruir
tempo_leitura: 12
exit_criteria: aluno explica em até 3 minutos o que é "instruir" no DPIA, por que o exit-gate é ≥ 85% nos casos de homologação, e a diferença entre playbook e system prompt.
---

# Visão Geral da Trilha I — Instruir

## O que você vai aprender
- O que a fase I entrega — agente configurado, system prompts versionados, base de conhecimento conectada, matriz HITL aprovada, playbook escrito.
- Por que o exit-gate da fase I é taxa de acerto ≥ 85% em bateria de homologação medida (não estimada).
- Como o system prompt traduz o playbook para o LLM, mantendo os dois artefatos sincronizados.
- Como escolher entre Claude Sonnet e Haiku para cada módulo, com critério de custo × qualidade.
- Como conduzir a bateria de testes com casos reais do cliente.

## Por que isso importa
A fase I é onde a tecnologia entra. Antes dela, o trabalho foi com pessoas, processo e contexto. Agora se constrói o agente. A tentação do aluno é antecipar I para o início do projeto — começar pelo prompt antes do baseline. O método inverte: I só começa quando P fechou.

A fase I também é onde se testa, antes da produção, se tudo o que foi construído em P bate com a realidade. Bateria de 30 a 50 casos reais é o teste. Abaixo de 85% de acerto, volta-se para P (faltou contexto) ou refaz-se I (faltou prompt). Acima de 85%, segue-se para A.

Pesquisa B §10 e Pesquisa G §7 são consistentes: agente sem teste estruturado em homologação tem 4 vezes mais incidentes na primeira semana de produção. A fase I é o filtro técnico antes do deploy real.

## O passo a passo

A trilha I tem cinco sub-etapas (I.1 a I.5 do manual canônico) e seis módulos pedagógicos:

1. **System prompt canônico (módulo 01).** Estrutura padrão: contexto, papel, objetivo, regras, formato de saída, exemplos few-shot. Todo system prompt nasce do template AIOS + Pacote de Contexto, nunca do zero.
2. **Construção do agente (módulo 02).** Montar o agente: escolha de modelo, tool use, RAG, integrações. Workflow n8n com webhook → LLM → tools → resposta.
3. **Bateria de testes (módulo 03).** 30 a 50 casos reais. Métrica de acerto. Exit-gate ≥ 85%.
4. **Iteração e ajuste (módulo 04).** Analisar erros, ajustar prompt, regras ou base. Anti-padrão: mais regras nem sempre melhora.
5. **Homologação com cliente (módulo 05).** Entrega do agente homologado, treinamento básico do dono, exit-gate de I.

## Diferença entre playbook e system prompt

Playbook (output de P) | System prompt (output de I)
---|---
Audiência: humano | Audiência: LLM
Linguagem: portuguesa, narrativa | Linguagem: instrução direta, em blocos
Atualização: champion edita | Atualização: implementador edita (com cuidado)
Formato: markdown longo | Formato: estruturado, com seções e exemplos
Fonte de verdade | Tradução fiel do playbook

Quando o playbook muda, o system prompt muda. Quando o system prompt muda sem o playbook ser atualizado, gera-se inconsistência: agente faz uma coisa, champion entende outra. Princípio 6 do DPIA aplica-se aos dois artefatos.

## Exemplo aplicado — Polaris Bebidas (Joinville/SC)
Saída esperada de I na Polaris:
- 1 system prompt principal (atendimento de pedido) + 1 system prompt auxiliar (transcrição de áudio).
- Workflow n8n: webhook WhatsApp → função classificadora → Sonnet (raciocínio principal) → tools (Bling, RAG Supabase) → mensagem de volta.
- 8 few-shot examples extraídos do histórico real Polaris.
- Bateria de 42 casos reais → taxa de acerto medida 87% (passou no exit-gate).
- Playbook v0.6 atualizado com refinamentos pós-teste.
- Matriz HITL assinada pelo Sr. Roberto.
- Plano de rollback testado (Maria desabilita webhook em 90 segundos no ambiente staging).

## Erros comuns
- **Começar I antes de P fechar.** Prompt sai chute. *Corrige:* exit-gate de P obrigatório.
- **Prompt copiado de tutorial sem adaptar.** Genérico, falha em campo. *Corrige:* template AIOS + Pacote de Contexto.
- **Pular bateria de homologação.** Vai para produção sem teste. *Corrige:* ≥ 85% em 30-50 casos.
- **Subir nível HITL na fase I "para acelerar A".** *Corrige:* HITL inicial é a mais conservadora.
- **Não documentar plano de rollback.** *Corrige:* exit-gate obrigatório.

## Checklist de saída
- [ ] Sabe descrever a estrutura do system prompt canônico.
- [ ] Sabe explicar a diferença entre playbook e system prompt.
- [ ] Sabe enumerar os 5 itens do exit-gate de I.
- [ ] Sabe explicar por que ≥ 85% é o piso, não meta opcional.

## Vai além
- Manual canônico, seção "Fase I — Instruir".
- `metodo/templates/template-system-prompt.md` — estrutura oficial.
- Pesquisa E — stack técnico e escolha Sonnet × Haiku.
- Exercício: para o Pacote de Contexto da Polaris (módulo P-05), começar a esboçar a estrutura do system prompt principal.
