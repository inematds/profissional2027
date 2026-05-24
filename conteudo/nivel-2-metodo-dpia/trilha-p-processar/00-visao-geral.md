---
nivel: 2
trilha: P
modulo: 0
titulo: Visão Geral da Trilha P — Processar
tempo_leitura: 12
exit_criteria: aluno explica em até 3 minutos a diferença entre fase P e fase I, e por que P é a fase que nenhum framework enterprise endereça.
---

# Visão Geral da Trilha P — Processar

## O que você vai aprender
- O que é o Pacote de Contexto, saída da fase P, e por que ele é diferente de "base de conhecimento" tradicional.
- Por que P é a fase de preparação de contexto que falta nos frameworks enterprise — e por que isso é a vantagem competitiva do implementador DPIA.
- Como capturar regras tácitas que só o operador conhece.
- O que diferencia P (estruturar contexto) de I (escrever instruções).
- Como deixar o cliente pronto para receber o agente sem virar bottleneck na fase I.

## Por que isso importa
Pesquisa B §4.2: *"nenhum framework identifica explicitamente a fase de preparação do contexto. Esta lacuna é crítica em PMEs com dados desestruturados."* É a validação mais rara e mais forte que o método recebeu nas 7 pesquisas. McKinsey, BCG e Gartner começam por estratégia de dados — assumem que os dados existem estruturados. Em PME, dado é planilha caótica + WhatsApp + cabeça do operador. Sem uma fase específica para limpar, contextualizar e estruturar isso, o agente da fase I sai fantasioso.

P também é onde se evita o erro mais caro do método: pular regra tácita. Operador atende clientes há anos com regras que ninguém documentou. Se essas regras não entram no Pacote de Contexto, o agente colide com elas no deploy e o operador volta ao canal antigo (Pesquisa G §3.3, Princípio 4 do DPIA).

## O passo a passo

A trilha P tem cinco sub-etapas (P.1 a P.5 do manual canônico) e seis módulos pedagógicos:

1. **Mapeamento de processos (módulo 01).** Desenhar o processo as-is com notação simples (caixas e setas), granularidade de PME, identificação clara de quem faz o quê.
2. **Extração de conhecimento tácito (módulo 02).** Sessões dirigidas com o operador para capturar regras que não estão em manual nenhum. Cada regra vira uma linha RT-XX.
3. **Construção do playbook (módulo 03).** Transformar processo + regras tácitas em playbook estruturado (entrada / processo / critério / saída / exemplos / regras).
4. **Design do fluxo com IA (módulo 04).** Onde a IA entra no fluxo, onde o humano permanece, qual o gatilho de escalonamento, qual o fallback quando o agente trava.
5. **Validação com cliente (módulo 05).** Review do Pacote de Contexto com dono e operador, ajustes, assinatura do exit-gate.

A trilha pressupõe que a fase D foi feita corretamente. Sem baseline assinado, champion nomeado e reposicionamento decidido, o aluno **não entra em P**. Tentar fazer P sem D fechado é o erro mais comum de implementador junior.

## Diferença entre P e I

Tema | Fase P (Processar) | Fase I (Instruir)
---|---|---
Foco | Contexto e dados | Comportamento do agente
Saída | Pacote de Contexto | Agente configurado + Playbook
Pergunta central | "O que o agente precisa saber?" | "Como o agente decide?"
Material | Markdown, planilhas, exports | System prompts, few-shot, matriz HITL

Misturar as duas fases é o segundo erro mais comum. Aluno começa a escrever system prompt antes de ter regras tácitas mapeadas, e o prompt vira chute. Disciplina: em P estrutura-se, em I escreve-se.

## Exemplo aplicado — Polaris Bebidas (Joinville/SC)
Saída esperada de P na Polaris:
- Mapa do processo "Atendimento de pedido WhatsApp", 11 caixas, 3 swimlanes (cliente / vendedor / Bling).
- 18 regras tácitas RT-01 a RT-18, incluindo as 5 capturadas em D (Bar do Zé, Glória same-day, etc.) e mais 13 descobertas em P (regras de desconto progressivo, exceções de horário, validação de PJ novo, blacklist de inadimplente, ...).
- Glossário de 32 termos do vocabulário Polaris (bonificação, devolução parcial, "pedido coringa", etc.).
- Base de conhecimento de 47 perguntas frequentes extraídas dos últimos 90 dias de WhatsApp.
- Plano de integração: Bling REST API (escopo produtos + pedidos + clientes), WhatsApp Cloud API (provisionamento Meta direto), Supabase pgvector (memória RAG).
- Conversas de reposicionamento concluídas: Maria + João + Sr. Roberto.

Esse Pacote de Contexto chega na fase I pronto para alimentar system prompt e few-shot. Sem fase P, a fase I vira tentativa-e-erro de 8 semanas.

## Erros comuns
- **Tratar P como "preparação técnica".** P é trabalho com o cliente, não com o terminal. *Corrige:* 60% do tempo de P é com o operador, não programando.
- **Pular extração de regra tácita.** Operador sabota o agente em 3 dias. *Corrige:* sessões específicas de extração.
- **Misturar P com I.** Escrever system prompt antes da estrutura. *Corrige:* disciplina; primeiro contexto, depois instrução.
- **Não validar integrações com chamada real.** "API do Bling funciona" sem ter feito GET. *Corrige:* credenciais provisionadas + chamada real testada em P, não em A.
- **Esquecer LGPD.** *Corrige:* RIPD enxuto entra no Pacote de Contexto, exit-gate obrigatório.

## Checklist de saída
- [ ] Sabe descrever o conteúdo do Pacote de Contexto em 5 bullets.
- [ ] Sabe explicar a diferença entre P e I.
- [ ] Sabe enumerar os 5 itens do exit-gate de P sem consultar.
- [ ] Sabe explicar por que regra tácita ignorada vira sabotagem no deploy.

## Vai além
- Manual canônico, seção "Fase P — Processar" — leitura obrigatória.
- Pesquisa B §4.2 — validação da lacuna nos frameworks enterprise.
- `metodo/templates/template-mapa-processos.md` — estrutura do mapa.
- `metodo/templates/template-playbook-agente.md` — estrutura do playbook.
- Exercício: revisitar o cliente do exercício da trilha D e listar 10 prováveis regras tácitas antes de entrar em P.
