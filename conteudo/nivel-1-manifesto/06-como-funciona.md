---
nivel: 1
modulo: 6
titulo: Como o programa funciona
tempo_leitura: 12
---

# Como o programa funciona

## Duração e ritmo

O Profissional 2027 é um programa de **10 a 12 semanas**, com carga total aproximada de **100 horas**, distribuídas em 8 a 10 horas semanais.

A faixa é deliberada. Quem tenta concluir em menos de 8 semanas perde profundidade — não consegue aplicar em cliente real durante o programa, e a certificação fica para depois. Quem estica para mais de 16 semanas perde ritmo — esquece o conteúdo anterior, perde a tração da comunidade, atrasa o primeiro diagnóstico real.

A faixa ideal mantém duas características simultâneas: tempo suficiente para aplicar em PME real durante a formação (não apenas em exercício fictício), e ritmo suficiente para manter atenção e progresso semanal.

A dedicação parcial é a norma. A maioria dos alunos mantém emprego ou atividade atual durante o programa. O conteúdo é em texto direto, leitura própria, com exercícios práticos e checkpoints semanais — sem aulas ao vivo obrigatórias. Isso permite estudar em horário próprio (manhã cedo, fim de tarde, final de semana).

## Estrutura: núcleo generalista + módulos verticais opcionais

A estrutura tem duas camadas.

### Núcleo (obrigatório, ~100 horas)

8 módulos sequenciais. Aplicáveis a qualquer PME brasileira. Saída do núcleo: aluno tem competência para fazer 1 diagnóstico de empresa e 1 piloto técnico funcional.

| # | Módulo | Carga | Resultado prático |
| --- | --- | --- | --- |
| 1 | Método DPIA e Diagnóstico de Processo | 12h | Diagnóstico de empresa fictícia entregue |
| 2 | n8n na Prática e Integrações Básicas | 16h | Workflow funcionando ponta a ponta |
| 3 | LLM Aplicado (Claude/GPT) e Engenharia de Prompt | 12h | Primeiro chatbot funcional |
| 4 | WhatsApp Business API e Agente com Memória | 16h | WABA + agente funcional integrado |
| 5 | Integrações Brasileiras (Bling, Omie, Conta Azul, PIX, NF-e) | 16h | IA conectada ao ERP do cliente |
| 6 | RAG e Base de Conhecimento (Supabase pgvector) | 12h | Chatbot com knowledge base estruturado |
| 7 | Human-in-the-loop, LGPD-PME e Governança | 8h | Matriz HITL desenhada e aprovada |
| 8 | Comercial: Diagnóstico Pago, Proposta e Manutenção | 12h | Playbook comercial + 1 proposta real |

O fio condutor entre os módulos não é a ferramenta. É o método DPIA aplicado em três nichos-modelo (distribuidoras, clínicas, contábeis) que atravessam os exemplos. Cada módulo termina com um entregável próprio. Quem termina o módulo 8 termina o núcleo.

### Módulos verticais opcionais (após o núcleo, 20 a 30h cada)

Especialização setorial. O aluno escolhe a vertical após o núcleo, em função do nicho de cliente que quer focar. Verticais previstas para liberação progressiva:

- Vertical Clínicas e Saúde
- Vertical Distribuidoras e Atacado
- Vertical Contábil
- Vertical Jurídico
- Vertical Imobiliário
- Vertical E-commerce e Varejo

Não são obrigatórias. O aluno pode certificar-se sem vertical e operar como generalista. A pesquisa F (§10.5) indica que implementadores com nicho definido chegam ao primeiro projeto em metade do tempo de generalistas — por isso a recomendação é escolher uma vertical no fim do núcleo. Mas é recomendação, não imposição.

### Eletivas avançadas

Para quem quer subir ticket ou atender nichos mais técnicos:

- Claude Agent SDK, tool use, MCP
- LangGraph e agentes multi-step
- UI própria com Lovable e V0
- Voz: TTS e STT no atendimento
- Gestão de equipe de implementadores

São opcionais e não bloqueiam certificação. O núcleo no-code (n8n + LLM via HTTP node) cobre 80% dos projetos de PME. As eletivas servem aos 20% restantes.

## O método DPIA

O programa é organizado em torno do método DPIA — abreviação para os quatro verbos canônicos: **Diagnosticar, Processar, Instruir, Automatizar**.

Apresentação breve aqui. Conteúdo completo no Nível 2 do programa, que detalha cada fase com checklists de entrada, artefatos de saída e exemplos por vertical.

**D — Diagnosticar.** Mapear o processo atual da PME antes de qualquer ferramenta. Identificar gargalo real com volume (não com gosto). Estabelecer baseline numérico assinado pelo cliente (tempo, volume, taxa de erro, custo). Definir KPI de sucesso pós-implantação. Decidir reposicionamento das pessoas envolvidas. Entregar Diagnóstico DPIA pago como artefato final.

**P — Processar.** Limpar e organizar os dados do processo (planilhas, ERP, histórico WhatsApp). Contextualizar com vocabulário, regras de negócio e exceções. Estruturar base de conhecimento (FAQ, scripts, políticas). Definir integrações necessárias com WABA, ERP, CRM, financeiro. Entregar Pacote de Contexto pronto para alimentar o agente.

**I — Instruir.** Escrever system prompts por módulo a partir dos templates AIOS. Configurar few-shot examples com casos reais do cliente. Definir matriz human-in-the-loop por nível de risco (4 níveis). Escrever playbook operacional (não SOP — playbook de agente, com revisão viva). Entregar agente configurado + playbook + matriz HITL aprovada.

**A — Automatizar.** Deploy em produção com HITL ligado no nível mais conservador. Job shadowing com operador nos primeiros dias. Treinar champion interno (train-the-trainer). Gravar micro-vídeos de operação (5 a 10 vídeos de 2 a 3 minutos cada). Medir contra baseline em 30, 60 e 90 dias. Entregar sistema em operação + relatório de payback + contrato de manutenção ativo.

**Loop de manutenção (paralelo e recorrente).** Não é fase. É operação contínua que segue após o ciclo D-P-I-A: visita presencial mensal ou quinzenal conforme tier, check-in remoto, atualização contínua do playbook, métrica mensal contra baseline assinado, resposta de recuperação quando adoção cai.

O conteúdo canônico do método está no Nível 2 — Manual DPIA. Todo o programa se organiza em torno dele.

## Critério de certificação

A certificação não vem por completar módulos. Vem por entregar resultado real no mercado.

**Critério único e duplo:**

- 1 diagnóstico pago real entregue a uma PME real (não exercício fictício, não diagnóstico gratuito além do primeiro piloto).
- 1 piloto em PME real implementado e em produção (sistema funcionando, equipe minimamente treinada, baseline medido pelo menos em 30 dias).

Quem entrega os dois vira "Profissional 2027 Certificado". Quem só completa o conteúdo, mas não entrega, permanece como "Aluno do Programa". Não é certificado. Não pode usar o selo público nem o badge.

A escolha é deliberada. Certificação por horas-aula ou por prova teórica não filtra qualidade real no mercado de PME. Certificação por entrega de projeto real filtra. Quem certifica passou por cliente real, por diagnóstico pago real, por implementação real, por baseline real. É o filtro que o mercado de PME enxerga como diferencial.

Não há prazo limite para entregar. O aluno pode concluir o núcleo em 12 semanas e entregar a certificação em 6 meses. Ou em 12 meses. Ou em 24 meses. O importante é o entregável, não a velocidade.

## A comunidade ao redor

O programa não cria comunidade própria fechada. Acopla-se à comunidade já existente: **inema.vip**.

inema.vip é a comunidade premium independente que reúne ex-alunos do SISNEMA (aproximadamente 100 mil profissionais formados ao longo das últimas décadas) e novos profissionais em formação. É longitudinal — pessoas continuam ali por anos após o curso. É ativa — discussões diárias, troca de cases, indicações de clientes entre membros, mentoria informal entre seniores e novatos.

O aluno do Profissional 2027 entra automaticamente na comunidade. Não há mensalidade adicional pelo acesso. inema.vip continua sendo o que sempre foi — comunidade-base — e o programa Profissional 2027 é mais um conteúdo curado que circula nela. Outras comunidades brasileiras também podem usar o conteúdo do programa, sem restrição. O programa não exige exclusividade comunitária.

A comunidade cumpre quatro papéis para o aluno:

1. **Distribuição de oportunidade.** Egressos compartilham vagas, indicações de cliente, projetos abertos. Aluno em formação encontra oportunidade real durante o curso.
2. **Suporte entre pares.** Dúvida técnica, dúvida comercial, dúvida ética sobre como conduzir um cliente — alguém na comunidade já passou pelo mesmo. Resposta vem em horas, não em semanas.
3. **Atualização contínua.** IA muda rápido. Comunidade detecta mudança antes do material formal ser atualizado. Mensagem na inema.vip "atenção, o Claude mudou X" antecede o módulo atualizado em semanas.
4. **Mentoria escalonada.** Egressos mais experientes ajudam novatos. Não é exigência formal. É cultura. O programa só funciona em escala porque a comunidade sustenta o suporte.

A comunidade não é "bônus do curso". É o mecanismo que viabiliza o modelo gratuito. Sem ela, o programa colapsaria sob volume de dúvidas individuais.

## Como entrar

A entrada é simples e direta. Não há vestibular, não há entrevista, não há prova de admissão, não há ranking de matrícula.

Quem quer começar acessa o inema.club, abre o conteúdo do programa, começa pelo Nível 1 (manifesto e tese, este conjunto de seis textos), avança para o Nível 2 (manual DPIA), e depois para o Nível 3 (trilha principal de 8 módulos). Em paralelo, entra na comunidade inema.vip e participa das discussões.

Não há turma. Não há data de início. Não há data limite. Cada aluno controla o próprio ritmo. Quem prefere ritmo coletivo pode acompanhar grupos informais na comunidade que estudam em paralelo.

A certificação, quando o aluno entregar o critério (1 diagnóstico pago + 1 piloto real), é solicitada via processo descrito no programa, com submissão dos artefatos do diagnóstico e do piloto para revisão da curadoria. A revisão tem prazo. O retorno é uma das três respostas: certificado emitido, ajuste solicitado (com orientação clara do que falta), ou indicação de reentrega com mais profundidade.

## Pré-requisitos honestos

Não há pré-requisito formal. Mas há pré-requisito realista. Vale ser explícito.

**Útil ter:**

- Conhecimento básico de uso de computador, planilha eletrônica, navegação web. O programa não ensina informática básica.
- Capacidade de leitura em português técnico. O conteúdo é direto, não acadêmico, mas exige atenção.
- Mínimo de 8 horas semanais reservadas durante 10 a 12 semanas. Sem isso, o programa não cabe na vida do aluno.
- Disponibilidade para ir presencialmente a pelo menos uma PME real durante a formação — para o piloto. Implementação 100% remota não funciona em PME brasileira no estágio atual.

**Não exigido (mas ajuda):**

- Experiência prévia com programação. O programa é no-code por default. Quem nunca codou consegue concluir.
- Background técnico em TI. O programa é desenhado para receber profissionais sem TI (gestores, profissionais liberais, operadores de PME) e migrá-los.
- Capital inicial. Os custos operacionais do aluno em formação são baixos: assinatura de Claude ou ChatGPT (~R$ 100 por mês), Supabase free tier, n8n self-host gratuito ou cloud baixo custo, WABA com tarifa por mensagem. Total estimado durante a formação: R$ 200 a R$ 500 mensais.

**Não recomendado:**

- Esperar enriquecimento rápido. Quem entra esperando "R$ 10 mil em 30 dias sem cliente" vai abandonar. A média realista é 8 a 15 meses para R$ 10 mil mensais estáveis em dedicação parcial.
- Querer apenas aprender ChatGPT para uso pessoal. Existem cursos gratuitos do Sebrae, Microsoft e Bradesco para esse perfil. O Profissional 2027 forma quem implementa em terceiros.
- Recusar interação com cliente. PME brasileira exige presença. Quem só quer trabalhar de casa em frente ao terminal não vai entregar o piloto real exigido para certificação.

## Em resumo

O programa tem 10 a 12 semanas de núcleo (~100 horas), com 8 módulos sequenciais que cobrem o método DPIA, n8n, LLM aplicado, WABA, integrações brasileiras, RAG, governança e comercial. Tem módulos verticais opcionais para especialização setorial e eletivas avançadas para subir ticket. O método canônico é o DPIA — quatro verbos no infinitivo (Diagnosticar, Processar, Instruir, Automatizar) com loop de manutenção paralelo. A certificação exige entrega real (1 diagnóstico pago + 1 piloto em PME). A comunidade é o inema.vip, longitudinal e ativa. A entrada é livre, sem turma, sem data limite. Os pré-requisitos honestos são 8 horas semanais, leitura em português técnico, e disposição para ir até a PME.

Tudo gratuito. Tudo aberto. O trabalho é do aluno.
