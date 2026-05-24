---
nivel: 2
trilha: A
modulo: 3
titulo: Deploy em Produção
tempo_leitura: 20
exit_criteria: aluno executa checklist completo de go-live, conduz job shadowing presencial 3-5 dias, deixa champion operando autônomo a partir da semana 3.
---

# O que você vai aprender
- O checklist de go-live: 22 itens que separam projeto que entra em produção bem do que entra mal.
- Como conduzir job shadowing presencial nos primeiros 3-5 dias com pressão e dados reais.
- Como fazer ramp-up controlado: começar com volume baixo, expandir gradualmente.
- Como tratar incidente nas primeiras horas sem perder a confiança do cliente.
- Como ensinar o champion a operar sem virar bottleneck do implementador.

## Por que isso importa
Pesquisa D §4: 95% dos pilotos não chegam à produção; entre os que chegam, 78% das falhas são humanas (Pesquisa G §1). Deploy ruim é o erro mais caro do método — perde-se cliente, perde-se manutenção recorrente, perde-se case. Pesquisa G §3.3 é taxativa: job shadowing presencial nos primeiros dias com dados reais e pressão real, não simulação.

A fase A.1 do manual canônico — "Deploy em produção no nível HITL mais conservador" — é uma decisão de design, não de marketing. Mesmo que o agente tenha passado em 95% na homologação, deploy começa no nível mais conservador definido na matriz. Por confiar no Princípio 5 do DPIA: risco define autonomia.

## O passo a passo

**1. Checklist completo de go-live (22 itens).**

*Infraestrutura:*
- [ ] VPS Hetzner em produção, hardening aplicado.
- [ ] n8n self-host com HTTPS válido e backup diário.
- [ ] Postgres do n8n com snapshot.
- [ ] Supabase Pro ativo.
- [ ] Limite de gastos Anthropic configurado (10x o esperado mensal).
- [ ] Limite de gastos Meta WABA configurado.

*Aplicação:*
- [ ] Workflow n8n importado e ativo.
- [ ] System prompt v final (versão homologada) implantado.
- [ ] Tools conectadas com token de produção (não de homologação).
- [ ] Webhook Meta apontando para URL de produção.
- [ ] Templates Meta aprovados.
- [ ] HITL no nível inicial definido na matriz (conservador).

*Operação:*
- [ ] Página de pausa funcional, com acesso da Maria.
- [ ] Plano de rollback testado em ambiente de produção (simulação).
- [ ] Slack/Discord/WhatsApp de equipe ativo, com canal de alertas.
- [ ] Alertas configurados (erro > 5%, latência alta, Bling down).
- [ ] Dashboard de métricas básico (Supabase).

*Humano:*
- [ ] Champion treinado (entrevista de 15 min com pergunta direta).
- [ ] Operadores afetados informados do go-live.
- [ ] Dono informado do go-live (data, hora, plano).
- [ ] Reposicionamento concluído na prática (não no papel).

*Comunicação:*
- [ ] Mensagem de boas-vindas para clientes finais (se aplicável).
- [ ] Implementador presencial nos próximos 3 dias.

Sem todos os 22, **não há go-live**. Cada item ausente é potencial incidente.

**2. Definir o dia e a hora do go-live.** Critérios:
- Não na sexta à tarde (incidente no fim de semana).
- Não em dia de pico (segunda-feira após feriado).
- Idealmente: terça-feira ou quarta-feira de manhã, depois de Pico inicial passar.
- Implementador presencial na empresa no momento.
- Pelo menos 2 horas de janela tranquila para acompanhar primeiras interações.

**3. Ramp-up controlado.**

Estratégia recomendada para PME média:
- *Semana 1, dias 1-3:* nível HITL conservador, 100% do volume do canal redirecionado para o agente, implementador presencial.
- *Semana 1, dias 4-7:* implementador remoto intensivo (4-6h/dia disponível em chat e videocall), Maria conduzindo.
- *Semana 2:* implementador remoto pontual (2-3 check-ins/dia + plantão), Maria autônoma operacional.
- *Semana 3:* Maria opera sem implementador, exceto check-in 2x/semana.
- *Semana 4:* operação estável, prepara marco 30 dias.

Para projetos com mais risco (volume alto, cliente final exigente, primeira automação do nicho), ramp-up pode ser ainda mais gradual: agente cobre 30% do volume na semana 1, 60% na semana 2, 100% na semana 3.

**4. Job shadowing presencial.**

Pesquisa G §3.3: "treinamento presencial inicial com pressão real, não simulação." 

Roteiro do dia 1 de job shadowing:
- *08h00:* aluno chega na empresa antes da Maria.
- *08h30:* Maria chega, aluno revisa com ela o painel de alertas, o botão de pausa, e a estrutura do dia.
- *09h00:* go-live. Agente começa a receber webhooks reais. Aluno ao lado da Maria.
- *09h00-12h00:* cada escalonamento, aluno e Maria revisam juntos. Aluno explica o porquê do escalonamento. Maria responde o cliente.
- *12h00-13h00:* almoço com Maria; conversa informal sobre o que sentiu.
- *13h00-17h00:* segunda janela. Maria começa a operar mais sozinha; aluno fica ao lado, intervém menos.
- *17h00-18h00:* revisão do dia. Métricas. Erros. Ajustes a fazer (hot fixes no system prompt na noite).

Dia 2 e 3: aluno presente mas senta-se um pouco mais afastado; intervém só quando solicitado. Termina o dia 3 com avaliação: Maria está pronta para operar com aluno remoto?

Em geral, sim. Se não, prolonga-se job shadowing por mais 1-2 dias. Não pula.

**5. Tratamento de incidente nas primeiras horas.**

Tipos de incidente comuns no dia 1:
- *Template Meta rejeita em produção que aprovou em homologação:* mensagem inicial não dispara. Solução: ajustar template e ressubmeter (24-48h). Enquanto isso, modo manual.
- *Bling rate limit atingido em pico:* algumas chamadas falham. Solução: queue + retry; verificar se cache está ativo.
- *Cliente final usa palavra-chave inesperada que dispara escalonamento errado:* solução de hot fix no system prompt na mesma noite.
- *Maria sente-se sobrecarregada com volume de escalonamento:* solução: investigar se gatilho está calibrado certo; eventualmente ajustar (com cuidado, sem subir nível HITL).
- *Cliente final percebe que é agente e reage mal:* raro com tom institucional bem feito. Solução: política de transparência ("estamos com sistema novo de atendimento; qualquer coisa fora do padrão, é só pedir que falo com a Maria").

Postura do implementador em incidente: calmo, técnico, transparente. Nunca defensivo. Cada incidente vira aprendizado documentado.

**6. Treinar champion (train-the-trainer).**

Sub-etapa A.3 do manual canônico. Maria sai capaz de:
- Ajustar respostas básicas no playbook (entendendo o que é regra dura vs. RAG vs. tom).
- Escalar incidente ao implementador via canal de alerta.
- Explicar para colegas o que o agente faz e o que não faz.
- Gravar micro-vídeo curto (2-3 min) quando aparecer situação nova.

Treinamento estruturado em 4 sessões de 1-2h durante a semana 1 + 2:
- *Sessão 1 — Painel de operação.* Onde ver interações, como pausar, como ver alertas.
- *Sessão 2 — Playbook como documento vivo.* Como ler, como propor mudança, como pedir atualização do agente.
- *Sessão 3 — Diagnóstico de erro.* Quando agente erra, como categorizar (falha de regra, falha de tom, etc.).
- *Sessão 4 — Gravação de micro-vídeo.* Maria grava o primeiro micro-vídeo guiada pelo aluno.

**7. Subir nível HITL com regra.**

A subida não acontece em A.3. Acontece quando os critérios são atingidos (regra de subida documentada em I):
- Métrica de qualidade ≥ 90% por 30 dias consecutivos.
- Zero incidente crítico no período.
- Champion concorda explicitamente.

Em geral, subida acontece entre o marco 30 e o marco 60.

## Exemplo aplicado — Polaris Bebidas (Joinville/SC)

Go-live: terça-feira 28/05, 09h.

Checklist de 22 itens validados na sexta anterior (24/05). 2 itens com nota:
- Template "encerramento" da Meta ainda em revisão (aprovação esperada segunda-feira). Decisão: go-live de qualquer forma; mensagem-padrão substituta usa template já aprovado.
- Maria pediu mais um dia de simulação. Aluno conduziu 4h de simulação na sexta à tarde com 12 casos.

Dia 1 (28/05):
- 09h: webhook ativado. Primeira mensagem chega às 09h11: cliente cadastrado pedindo 6 caixas Brahma + 4 fardos água. Agente processa em 3,8s, escalona para Maria aprovar mensagem final (nível 2). Maria aprova. Pedido criado no Bling. Cliente recebe confirmação.
- 09h00-12h00: 18 interações; 14 happy path, 4 escalonamentos (1 inadimplente, 1 VIP, 1 áudio longo, 1 cliente novo PJ). Maria opera com aluno comentando ao lado.
- 13h45: primeiro incidente. Cliente envia foto do alvará (RT-12), template não previa anexo de imagem; agente respondeu mensagem-padrão de "não entendi". Aluno faz hot fix na hora: ajusta system prompt para reconhecer "PJ novo + imagem" como confirmação válida. Atualização aplicada em 22 minutos.
- 17h00: revisão do dia. 42 interações totais, 5 escalonamentos, 1 incidente resolvido. Tempo médio de resposta: 4,1 min (vs. baseline 18 min inicial). Maria: "Foi menos estressante do que eu temia."

Dia 2 (29/05):
- Volume similar. 38 interações, 6 escalonamentos. Sem incidente.
- Aluno intervém menos. Maria opera direta. Sessão 2 de treinamento (playbook) à tarde.

Dia 3 (30/05):
- Aluno fica afastado, observa. Maria conduz tudo. 1 escalonamento por confidence baixo que era falso positivo (regra de gatilho ajustada).

Dia 4-7: aluno remoto intensivo. Maria sozinha presencial, em contato no Slack.

Semana 2: Maria autônoma. Aluno em call de 30 min a cada 2 dias.

Semana 3: Maria 100% autônoma. Métricas estáveis. Tempo médio caiu para 9,2 min. Sr. Roberto não atendeu WhatsApp depois das 18h em todo o período. Maria começou a fazer prospecção ativa de clientes B (15 ligações por dia em janela livre).

Plano de rollback testado: domingo 02/06, 22h, simulação programada. Maria clica botão de pausa. Sistema responde corretamente, mensagem de manutenção planejada é enviada para 12 conversas ativas, alerta no Slack disparado. Tempo de pausa: 8 segundos. Tempo de retomada: 11 segundos.

## Erros comuns
- **Go-live na sexta.** Incidente vira fim de semana. *Corrige:* terça-quarta de manhã.
- **Job shadowing por videochamada.** Cliente sente abandono. *Corrige:* presencial 3-5 dias.
- **Não pausar quando precisa.** Implementador relutante em desligar agente para "preservar imagem". Custa muito caro. *Corrige:* pausa é parte do operacional, não derrota.
- **Subir HITL "porque está bom" no dia 5.** Primeiro erro autônomo vira incidente. *Corrige:* regra de subida documentada, 30 dias mínimo.
- **Treinar dono em vez do operador.** Dono não opera. *Corrige:* Maria é o centro do treinamento.

## Checklist de saída
- [ ] Checklist de 22 itens completo antes do go-live.
- [ ] Go-live em dia/hora apropriada.
- [ ] Job shadowing presencial 3-5 dias.
- [ ] Ramp-up controlado conforme estratégia.
- [ ] Champion treinado em 4 sessões.
- [ ] Pelo menos 1 incidente tratado com hot fix sem perda de confiança.
- [ ] Plano de rollback testado em produção.
- [ ] Champion opera autônoma a partir da semana 3.

## Vai além
- Manual canônico, sub-etapas A.1, A.2, A.3.
- `metodo/templates/template-checklist-implementacao.md` — checklist oficial.
- Pesquisa G §3.3.
- Exercício prático: simular checklist de go-live para o cliente do exercício, escrevendo o que faltaria em cada item e a ordem de execução.
