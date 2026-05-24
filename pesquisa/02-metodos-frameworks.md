# Pesquisa B — Métodos de implementação de IA + Design AIOS

**Data:** maio de 2026
**Responsável:** pesquisa conduzida como subsídio ao projeto Profissional 2027 (SISNEMA / Nei Maldaner)
**Escopo:** mapeamento de frameworks de implementação de IA em organizações + estado da arte em arquitetura de agentes e AI Operating Systems

---

## Resumo executivo

Existe um consenso robusto entre consultorias, big techs e institutos de pesquisa sobre o que constitui uma implementação bem-sucedida de IA em organizações: as fases são sempre alguma variação de "diagnosticar → experimentar → escalar → governar". Nenhum dos frameworks dominantes foi projetado para pequenas e médias empresas (PMEs) com baixo capital técnico. O DPIA (Diagnosticar, Processar, Instruir, Automatizar) ocupa uma lacuna real: é o único método orientado a implementadores não-técnicos em contexto de PME, com uma sequência que vai da leitura do negócio até a automação de processos. No plano de arquitetura de agentes, a Anthropic consolidou em 2024 um conjunto de padrões simples e compostos (augmented LLM, prompt chaining, routing, parallelization, orchestrator/subagents) que convergem com o que frameworks como LangGraph, CrewAI e AutoGen implementam em código. O conceito de AIOS (AI Operating System) — seja em sentido acadêmico (Rutgers, 2024) ou comercial (Letta, AutoGen, LangGraph) — ainda não tem uma forma canônica para PMEs, criando espaço para uma biblioteca de módulos orientada a pequenos negócios.

---

## 1. Comparativo de frameworks de implementação de IA

### 1.1 McKinsey QuantumBlack — 12 Melhores Práticas de Adoção Gen AI (2025)

**Fases/estrutura:** Não é um método sequencial, mas um conjunto de 12 práticas agrupadas em: liderança e comunicação, incorporação em processos, capacitação de papéis, governança e KPIs, escalonamento faseado.

**Premissas:** Organizações com função dedicada de IA, patrocínio de C-level, budget estabelecido, equipes de dados existentes.

**Público-alvo:** Grandes empresas (P&L >US$1B). O relatório State of AI 2025 mostra que 88% das organizações usam IA em ao menos uma função, mas apenas 6% são "high performers" (>5% de impacto no EBIT).

**Destaque:** Menos de um terço das organizações implementou a maioria das 12 práticas — sinal de que mesmo o framework dominante de mercado falha na execução.

**Fontes:** [McKinsey State of AI 2025](https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai); [AI Business Implementation Guide McKinsey 2025](https://vladimirsiedykh.com/blog/ai-business-implementation-guide-mckinsey-research-success-patterns-2025)

---

### 1.2 BCG GAMMA — Envision → Activate → Enable (2024)

**Fases:**
1. **Envision** — Definir ambição de IA, identificar onde analytics gera valor, estabelecer KPIs e alinhar liderança.
2. **Activate (AI Activator)** — Desenvolvimento e escalonamento de soluções IA via ciclos iterativos: testar casos de uso, validar premissas, construir protótipos, medir impacto.
3. **Enable** — Diagnóstico de infraestrutura de dados/TI, construção de capacidades organizacionais para sustentabilidade.

**Duração típica:** 9 a 12 meses.

**Premissas:** Acesso a dados corporativos estruturados, budget para projeto dedicado, equipe mista BCG + time interno.

**Público-alvo:** Médias e grandes empresas, frequentemente em setores regulados (financeiro, saúde, indústria).

**Fontes:** [BCG AI @ Scale](https://www.bcg.com/capabilities/artificial-intelligence); [BCG GAMMA Overview](https://www.casebasix.com/pages/bcg-gamma); [LinkedIn Navigating Digital to AI Transformation](https://www.linkedin.com/pulse/navigating-digital-ai-transformation-frameworks-from-leading-dutta-emc1f)

---

### 1.3 Accenture — The Art of AI Maturity (5 Estágios)

**Estágios de maturidade:**
1. Consciente (Aware)
2. Ativa (Active)
3. Operacional (Operational)
4. Sistemática (Systemic)
5. Transformacional (Transformational)

**5 capacidades-chave dos "AI Achievers":**
- Estratégia de talentos abrangente (78% dos líderes fornecem treinamento obrigatório em IA)
- Core de IA industrializado (plataformas integradas dados + tecnologia + pessoas)
- Colaboração cross-funcional (ponte negócio-tecnologia)
- IA Responsável (53% mais propensos a implementar frameworks éticos)
- Governança de dados centralizada

**Destaque:** Empresas com processos liderados por IA superam concorrentes em 2,5x no crescimento de receita. Projeção de 2024: AI Achievers passam de 12% para 27% das empresas globais.

**Público-alvo:** Grandes corporações multinacionais.

**Fontes:** [Accenture Art of AI Maturity](https://www.accenture.com/us-en/insights/artificial-intelligence/ai-maturity-and-transformation)

---

### 1.4 Deloitte — State of AI in the Enterprise (2026)

**Abordagem:** Framework de maturidade avaliado anualmente (survey com 3.235 líderes, agosto-setembro 2025). Foco em rastreamento de onde organizações se encontram no espectro da adoção, não em prescrição de método.

**Dimensões avaliadas:** Estratégia, dados, tecnologia, talentos, processos e governança.

**Destaques 2025-2026:** Ênfase crescente em IA agêntica e governança de agentes. Organizações avançadas já implementam múltiplos agentes em produção.

**Fontes:** [Deloitte State of AI in the Enterprise 2026](https://www.deloitte.com/us/en/what-we-do/capabilities/applied-artificial-intelligence/content/state-of-ai-in-the-enterprise.html)

---

### 1.5 Microsoft — Agentic AI Adoption Maturity Model (2025)

**5 níveis de maturidade:**
1. **Initial (L100):** Iniciativas não planejadas, experimentais, silos, dependência de indivíduos.
2. **Repeatable (L200):** Padrões emergentes, práticas informais, irregulares entre equipes.
3. **Defined (L300):** Capacidades formalizadas, documentadas, com governance e padrões; alinhamento com objetivos de negócio.
4. **Capable (L400):** Agentes incorporados em planejamento e operações; processos, governance e tecnologia suportam escalonamento.
5. **Efficient (L500):** Organização opera como "agent-first enterprise".

**Framework de adoção paralelo (Azure Cloud Adoption Framework for AI):** Plan → Ready → Adopt → Govern → Manage.

**Público-alvo:** Empresas de médio e grande porte usando stack Microsoft (Copilot, Azure AI).

**Fontes:** [Microsoft Agentic AI Maturity Model](https://learn.microsoft.com/en-us/microsoft-copilot-studio/guidance/maturity-model-overview); [Microsoft Inside Track AI Maturity](https://www.microsoft.com/insidetrack/blog/enterprise-ai-maturity-in-five-steps-our-guide-for-it-leaders/)

---

### 1.6 IBM — AI Lifecycle (Planejamento → Treinamento → Implantação → Monitoramento)

**Fases do ciclo de vida:**
1. **Planejamento:** Definir escopo, necessidades, problema a resolver, métricas de sucesso qualitativas e quantitativas.
2. **Coleta e preparação de dados:** Dados de treinamento, qualidade, governança.
3. **Seleção e treinamento de modelo:** Arquitetura, recursos computacionais.
4. **Avaliação e melhoria:** Benchmarks, testes, iteração.
5. **Implantação:** Integração em workflows reais.
6. **Monitoramento e manutenção contínua:** Drift, performance, re-treinamento.

**Governa estrutura com 7 dimensões:** Estratégia, governança, dados, tecnologia, organização, ética e medição de performance.

**Destaque 2025:** IBM lançou capacidades de governance para IA agêntica no watsonx.governance, cobrindo ciclo de vida de agentes.

**Fontes:** [IBM AI Lifecycle](https://www.ibm.com/think/topics/ai-lifecycle); [IBM Agentic AI Governance](https://www.ibm.com/new/announcements/agentic-ai-governance-evaluation-and-lifecycle)

---

### 1.7 Gartner — AI Maturity Model (5 Níveis)

**Níveis:**
1. **Awareness:** Conversas sobre IA sem projetos concretos.
2. **Active:** PoCs e projetos piloto; reuniões de knowledge sharing; início de padronização.
3. **Operational:** Ao menos um projeto em produção; melhores práticas, especialistas e tecnologia acessíveis; patrocínio executivo e budget.
4. **Systemic:** Todo novo projeto digital considera IA; novos produtos e serviços têm IA embutida.
5. **Transformational:** IA como driver central de modelo de negócio.

**Achado-chave 2024-2025:** 45% das organizações com alta maturidade em IA mantêm iniciativas em produção por 3+ anos. Barreiras primárias: qualidade/disponibilidade de dados e ameaças de segurança.

**Fontes:** [Gartner AI Maturity Model Toolkit](https://www.gartner.com/en/chief-information-officer/research/ai-maturity-model-toolkit); [BMC Gartner AI Maturity](https://www.bmc.com/blogs/ai-maturity-models/)

---

### 1.8 NIST AI RMF 1.0 — Govern → Map → Measure → Manage (2023)

**Quatro funções do núcleo:**
- **GOVERN:** Função transversal que informa todas as outras. Estabelece cultura de gestão de riscos, políticas e procedimentos. Fundação do framework.
- **MAP:** Estabelece o contexto para enquadrar riscos relacionados ao sistema de IA. Ao final, o usuário deve ter conhecimento contextual suficiente para uma decisão go/no-go.
- **MEASURE:** Emprega ferramentas quantitativas, qualitativas ou mistas para analisar, avaliar, fazer benchmarking e monitorar riscos de IA.
- **MANAGE:** Aloca recursos de risco para os riscos mapeados e medidos de forma regular.

**Caráter:** Voluntário, orientado a risco, aplicável a qualquer tipo e tamanho de organização.

**Relevância para PMEs:** O NIST AI RMF é o framework mais neutro a tamanho organizacional — é baseado em princípios, não em recursos. Porém, sua natureza acadêmica e focada em risco/compliance exige adaptação para o contexto operacional de PMEs.

**Fontes:** [NIST AI RMF Core Functions](https://airc.nist.gov/airmf-resources/airmf/5-sec-core/); [IS Partners NIST AI RMF Guide](https://www.ispartnersllc.com/hubs/nist-ai-rmf/core-functions/); [NIST AI 100-1 PDF](https://nvlpubs.nist.gov/nistpubs/ai/nist.ai.100-1.pdf)

---

### 1.9 ISO/IEC 23053:2022 — Framework for AI Systems Using Machine Learning

**Propósito:** Estabelecer vocabulário compartilhado e modelo conceitual para sistemas de IA que usam machine learning. Decompõe sistemas ML em blocos funcionais lógicos.

**Aplicação:** Todos os tipos e tamanhos de organização, mas de uso predominante técnico e regulatório.

**Contribuição principal:** Facilita comunicação entre desenvolvedores, gestores e reguladores; base para interoperabilidade e governança.

**Fontes:** [ISO/IEC 23053:2022](https://www.iso.org/standard/74438.html); [Nemko ISO 23053](https://digital.nemko.com/standards/iso-iec-23053)

---

### 1.10 EY / PwC / KPMG — Abordagens de Governança de Agentes (2026)

Três filosofias distintas emergindo no mercado de consultoria para IA agêntica:

- **EY:** Pipeline de desenvolvimento de agentes com meta declarada de 100.000 agentes; abordagem "scale-first" de capacidades internas.
- **KPMG:** Filosofia "trust-first" — governança e auditabilidade como pré-requisitos para adoção.
- **PwC:** Estratégia enterprise-wide com programa top-down: liderança sênior escolhe investimentos em IA focados em workflows-chave, aplicando "enterprise muscle" (talentos, recursos técnicos, change management). ~61% das organizações PwC já em estágio estratégico ou incorporado de IA responsável.

**Fontes:** [EY KPMG PwC Governance Philosophies](https://medium.com/agentic-ai-systems-and-the-future-ahead/eys-100-000-agent-ambition-kpmg-s-trust-first-pwc-s-scale-first-three-governance-philosophies-63c1593a8ea0); [PwC 2026 AI Predictions](https://www.pwc.com/us/en/tech-effect/ai-analytics/ai-predictions.html)

---

### 1.11 Contexto Brasileiro — Estado da Arte em PMEs (2024-2025)

- 61,41% das PMEs brasileiras já incorporaram alguma forma de IA (HostGator, jan 2025)
- 90% das PMEs brasileiras buscam adotar IA (Edelman, fev 2024)
- Apenas 15% das MPEs usam IA de forma estruturada (EY, 2025)
- Barreiras dominantes: custos elevados, escassez de habilidades técnicas, desconhecimento sobre benefícios reais
- Inexistência de frameworks brasileiros específicos para PMEs com método estruturado de implementação
- Estudos acadêmicos (2025) propõem modelos conceituais com três dimensões (inovação organizacional, transformação da proposta de valor, capacidade adaptativa), mas sem guia operacional

**Fontes:** [Implementação IA PMEs Brasil](https://blog.brspace.com.br/2025/04/22/relatorio-detalhado-implementacao-de-inteligencia-artificial-em-pequenas-e-medias-empresas-pmes-roadmap-metricas-de-sucesso-e-resultados-comprovados/); [Metodologias IA PMEs Brasileiras](https://revistatopicos.com.br/artigos/metodologias-para-avaliacao-do-impacto-da-inteligencia-artificial-na-inovacao-de-modelos-de-negocios-em-pmes-brasileiras)

---

## 2. Consenso entre frameworks

Analisando os 10+ frameworks/métodos acima, emergem 6 pontos de consenso estrutural:

**2.1 Fases sequenciais iterativas:** Todos os frameworks (BCG, McKinsey, Microsoft, Gartner, IBM) possuem alguma variação de: (1) diagnóstico/contexto → (2) experimentação/piloto → (3) operacionalização → (4) escalonamento → (5) governança contínua. A variação está na terminologia, não na lógica.

**2.2 Dados como pré-requisito:** Qualidade de dados aparece como barreira número 1 em Gartner (2024), NIST AI RMF (MAP), IBM AI Lifecycle e pesquisas de adoção em PMEs. Nenhum framework ignora dados — diferem apenas em quando e como abordá-los.

**2.3 Governança não é etapa final:** Frameworks maduros (NIST, IBM, Accenture) tratam governança como dimensão transversal, não como fase última. A função GOVERN do NIST é "cross-cutting" por design.

**2.4 Capacitação humana é tão crítica quanto tecnologia:** McKinsey (capability training), Accenture (talent-first), Microsoft (change management), BCG GAMMA (Enable) — todos dedicam uma fase ou dimensão específica a pessoas e cultura.

**2.5 Simplicidade antes de complexidade (Anthropic):** O guia "Building Effective Agents" da Anthropic afirma explicitamente: "find the simplest solution possible, and only increase complexity when needed." Isso reflete o consenso prático que emergiu em 2024 — a maioria das aplicações de produção usa padrões simples.

**2.6 ROI medido vs. expectativa:** McKinsey (State of AI 2025) identifica que apenas 6% das organizações são high performers. Gartner mostra que apenas 45% das organizações de alta maturidade mantêm projetos em produção por 3+ anos. O abismo entre piloto e escala é universal.

---

## 3. Lacunas para pequenas empresas

**3.1 Nenhum framework foi projetado para PMEs:** BCG GAMMA, McKinsey, Accenture, Deloitte — todos pressupõem equipes dedicadas de dados, budget para consultoria externa, infraestrutura de dados corporativa e capacidade técnica interna. O menor "público-alvo" explícito é "médias empresas" (KPMG, PwC em alguns materiais), nunca microempresas.

**3.2 Custo de entrada inacessível:** Os frameworks de consultoria custam centenas de milhares a milhões de reais para serem implementados com suporte especializado. Nenhum foi desenhado para ser executado sem consultores ou ferramentas específicas de alto custo.

**3.3 Vocabulário técnico exclui não-técnicos:** NIST AI RMF, ISO/IEC 23053 e documentação de frameworks de agentes (LangGraph, AutoGen) têm vocabulário inacessível para donos de PMEs sem background técnico. Mesmo materiais de consultoria assumem familiaridade com conceitos como MLOps, data pipelines, feature stores.

**3.4 Ausência de catálogo de módulos prontos para PMEs:** Não existe uma "biblioteca" validada de agentes e automações prontos para casos de uso comuns de PME (atendimento, vendas, financeiro, RH, estoque). Os catálogos existentes (Anthropic MCP, LangChain Hub, CrewAI Tools) são orientados a desenvolvedores.

**3.5 Falta de método para priorização de casos de uso com ROI rápido:** Frameworks existentes não orientam PMEs a escolher onde começar com base em impacto vs. facilidade de implementação sem expertise técnica interna.

**3.6 Ausência de trilha de aprendizado para implementadores não-desenvolvedores:** Não existe formação estruturada para um perfil de "implementador de IA em PME" — alguém que conhece o negócio, entende IA o suficiente para configurar agentes, mas não é desenvolvedor. Este perfil é o target do projeto Profissional 2027.

**3.7 Human-in-the-loop não calibrado para PME:** Frameworks de governance assumem equipes de compliance, comitês de ética, auditores internos. PMEs precisam de uma versão simplificada de HITL que qualquer gestor possa operar.

**Fonte confirmatória:** Pesquisa OECD (2025) mostra que empresas de 10-49 funcionários têm menos de 1/3 da probabilidade de usar IA comparado a grandes empresas. [OECD AI Adoption SME 2025](https://www.oecd.org/content/dam/oecd/en/publications/reports/2025/12/ai-adoption-by-small-and-medium-sized-enterprises_9c48eae6/426399c1-en.pdf)

---

## 4. Posicionamento do DPIA vs. existentes

### DPIA: Diagnosticar → Processar → Instruir → Automatizar

**4.1 Comparação por dimensão:**

| Dimensão | Frameworks enterprise (BCG/McKinsey/Accenture) | DPIA |
|---|---|---|
| Público-alvo | Grandes/médias empresas com equipe técnica | PME, implementador não-técnico |
| Ponto de entrada | Estratégia de dados e IA | Diagnóstico do processo de negócio |
| Sequência lógica | Varia: visão → piloto → escala | Diagnóstico → preparo → instrução → automação |
| Duração | 9-24 meses típico | Pode gerar valor em semanas |
| Custo de implementação | Alto (consultoria dedicada) | Baixo (self-service com guia e módulos) |
| Foco em dados | Infraestrutura de dados corporativa | Dados operacionais do processo |
| Capacitação | Treinamento técnico de equipes | Instrução de como o agente deve operar |
| Governança | Framework corporativo de compliance | Human-in-the-loop por nível de risco |
| Idioma | Inglês, vocabulário técnico/consultor | PT-BR, vocabulário operacional |

**4.2 Fortalezas do DPIA:**

- **Diagnóstico como ponto de partida:** Diferentemente de frameworks que começam pela estratégia de IA, o DPIA começa pelo processo de negócio — pergunta "o que você faz hoje?" antes de "como IA pode ajudar?". Isso reduz a barreira cognitiva.
- **Processar como fase intermediária crítica:** Nenhum framework identifica explicitamente a fase de preparação do contexto (limpar, organizar, contextualizar dados e processos para alimentar IA). Esta lacuna é crítica em PMEs com dados desestruturados.
- **Instruir como competência central:** A fase de instrução (prompt engineering, definição de papéis de agentes, configuração de comportamento) não aparece como fase explícita em nenhum framework de implementação — é tratada como habilidade técnica. O DPIA a eleva a competência de negócio.
- **Automatizar como consequência, não como objetivo inicial:** A automação no DPIA emerge de um processo bem instruído, não é o ponto de partida. Isso reduz o risco de automação de processos mal definidos.
- **Aplicabilidade imediata sem infraestrutura prévia:** O DPIA pode começar com ferramentas SaaS existentes (ChatGPT, Claude, Make, Zapier) sem necessidade de stack técnico próprio.

**4.3 Riscos/fraquezas a mitigar no desenvolvimento do DPIA:**

- **Falta de dimensão de dados:** O DPIA precisa endereçar explicitamente qualidade e preparação de dados como sub-etapa de "Processar" para não criar lacuna crítica.
- **Ausência de dimensão de governance:** Precisa integrar uma versão simplificada de governance (NIST-inspired) para não ignorar riscos reais.
- **Escalabilidade:** Como o DPIA escala de 1 processo implementado para múltiplos processos integrados? Precisa de uma camada de orquestração (o AIOS).
- **Validação empírica:** Frameworks de consultoria têm credibilidade por serem baseados em centenas de projetos. O DPIA precisará de casos documentados.

---

## 5. Arquitetura de agentes — estado da arte

### 5.1 Padrões fundamentais (Anthropic 2024)

O guia "Building Effective Agents" da Anthropic (dezembro de 2024) consolidou o vocabulário canônico de arquitetura de agentes em produção:

**Building block: Augmented LLM**
O LLM aumentado com retrieval, ferramentas e memória é o bloco fundamental. A recomendação da Anthropic é focar em duas dimensões: (a) calibrar capacidades para o caso de uso específico; (b) garantir interface bem documentada para o LLM. O Model Context Protocol (MCP) é a implementação de referência para integração com ferramentas externas.

**Workflows (padrões compostos):**
1. **Prompt Chaining:** Sequência de chamadas LLM onde saída de uma vira entrada da próxima. Checagens programáticas entre etapas ("gates"). Uso: tarefas com etapas claras, onde decomposição reduz erros.
2. **Routing:** Classificação de input e direcionamento para subtarefa especializada. Permite separação de concerns e prompts especializados. Uso: queries de suporte ao cliente (geral, reembolso, técnico), roteamento de perguntas fáceis para modelos menores (Claude Haiku) e difíceis para modelos maiores.
3. **Parallelization:** Sectioning (subtarefas independentes em paralelo) + Voting (mesma tarefa múltiplas vezes para consenso). Uso: guardrails (modelo A processa query, modelo B verifica conteúdo), revisão de código por vulnerabilidades com múltiplas perspectivas.
4. **Orchestrator-Subagents:** Orquestrador quebra tarefa e direciona a subagentes especializados. Subagentes executam e retornam resultados. Uso: agentes de código (busca, edição, execução, verificação), tarefas longas multi-etapa.
5. **Evaluator-Optimizer:** LLM gera output, LLM avaliador dá feedback, iteração até satisfatório. Uso: tradução literária com checagem de nuances, geração de código com suíte de testes.

**Agents (autônomos):**
Agentes plenos para tarefas abertas onde definir a priori as etapas é impossível. Planejam independentemente, podem pausar para feedback humano em checkpoints, têm condições de parada (máximo de iterações). Exemplo: agente SWE-bench (Anthropic reporta que otimizou mais definições de ferramentas do que prompts).

**Fonte:** [Anthropic Building Effective Agents](https://www.anthropic.com/research/building-effective-agents)

---

### 5.2 Padrões de raciocínio de agentes

**ReAct (Reasoning + Acting):** O modelo alterna entre "thought" (raciocínio) e "action" (chamada de ferramenta), usando cada observação para atualizar o próximo pensamento. Padrão canônico e padrão para agentes de propósito geral. Melhoria de 2024: Focused ReAct (Li et al.) reitera a tarefa original a cada etapa de raciocínio e usa critério de parada antecipada para combater drift de contexto.

**Reflexion:** Estende ReAct com 5 fases — raciocinar, agir, observar resultados, refletir sobre o que funcionou/falhou, repetir com aprendizado. Útil para tarefas onde erros fornecem informação valiosa para iteração.

**Planning (Planner-Executor):** Separação explícita entre agente planejador (decompõe tarefa em etapas) e agentes executores (implementam cada etapa). Permite auditabilidade do plano antes da execução.

**Multi-agent Orchestration:** Um agente orquestrador recebe tarefas e roteia para agentes especialistas. Complexidade multi-agente só se justifica quando especialização, fronteiras de segurança ou expertise multi-domínio melhoram materialmente os resultados.

**Fontes:** [AI Agent Architecture Patterns Redis](https://redis.io/blog/ai-agent-architecture-patterns/); [ReAct Architectures Emergent Mind](https://www.emergentmind.com/topics/reason-act-reflect-react-architectures); [Agentic AI Design Patterns 2026](https://www.innovatrixinfotech.com/blog/agentic-ai-design-patterns-react-reflection-tool-use)

---

### 5.3 Taxonomia 2026 de padrões de agentes

A vocabulário de arquitetura de agentes estabilizou em 2024-2026 em 8 padrões canônicos distribuídos em 4 quadrantes:
- **Single-agent:** LLM + tools (ReAct), LLM + memory + tools
- **Collaborative multi-agent:** Crew/team (CrewAI), Pipeline sequencial
- **Competitive multi-agent:** Debate, voting/consensus
- **Orchestration topology:** Hub-and-spoke (orquestrador central), Peer-to-peer (agentes negociam diretamente), Hierárquico (múltiplas camadas de orquestração)

A maioria dos sistemas em produção são composições de 2-3 padrões. **Fonte:** [Digital Applied Agent Architecture Taxonomy 2026](https://www.digitalapplied.com/blog/agent-architecture-patterns-taxonomy-2026)

---

## 6. Padrões de AI OS / plataformas modulares

### 6.1 LangGraph

Framework de orquestração de agentes de baixo nível para construção, gerenciamento e deploy de agentes stateful de longa duração. Focado exclusivamente em orquestração. Clientes em produção: Klarna, Uber, J.P. Morgan.

**Capacidades-chave:**
- **Durable execution:** Agentes persistem por falhas, retomam de onde pararam
- **Human-in-the-loop:** Inspecionar e modificar estado do agente em qualquer ponto
- **Comprehensive memory:** Memória de trabalho (curto prazo) + memória entre sessões (longo prazo)
- **Streaming:** Output em tempo real
- **LangSmith integration:** Debugging com visualização de traces de execução

**Posição no ecossistema:** Low-level, requer conhecimento técnico. Para iniciantes, LangChain Agents (alto nível) é recomendado antes. **Fonte:** [LangGraph Docs](https://docs.langchain.com/oss/python/langgraph/overview)

---

### 6.2 CrewAI

Framework open-source líder para orquestração de agentes autônomos e workflows multi-agente. +100.000 desenvolvedores certificados.

**Arquitetura dual:**
- **Flows:** Backbone da aplicação — workflows estruturados, orientados a eventos, com gerenciamento de estado e controle de execução. "Manager do processo."
- **Crews:** Times de agentes autônomos que colaboram em tarefas específicas delegadas pelo Flow. "Times que fazem o trabalho."

**Padrão recomendado:** Sempre começar com um Flow; usar Crews dentro de passos do Flow para tarefas complexas.

**Uso ideal:** Simple Automation (Flow + tarefas Python), Complex Research (Flow + Crew de pesquisa), Application Backend (Flow → Crew → Flow saving to DB). **Fonte:** [CrewAI Introduction](https://docs.crewai.com/introduction)

---

### 6.3 AutoGen (Microsoft)

Framework para construção de aplicações single e multi-agente conversacionais.

**Três camadas:**
- **AutoGen Studio:** UI web para prototipagem sem código (ideal para onboarding)
- **AgentChat:** Framework de programação para construção de agentes conversacionais (Python 3.10+)
- **Core:** Primitivas de baixo nível para construção de sistemas complexos

**Uso ideal:** Times que precisam de prototyping sem código antes de ir para implementação técnica. **Fonte:** [AutoGen](https://microsoft.github.io/autogen/stable/)

---

### 6.4 Letta (ex-MemGPT)

"Memory-first agents that continually learn." Born from MemGPT at UC Berkeley's Sky Computing Lab.

**Proposta central:** Agentes persistentes com capacidade de aprendizado contínuo — ensinados através de linguagem natural, melhoram com experiência.

**Arquitetura:** Separação entre memória de contexto ativo, memória de curto prazo (in-context), memória de longo prazo (external storage), com mecanismo de "sleep-time compute" (processamento offline para consolidação de memória).

**Relevância para AIOS:** Fornece o padrão de referência para agentes que acumulam conhecimento sobre o negócio ao longo do tempo — crítico para um AIOS de PME que precisa aprender os processos da empresa. **Fonte:** [Letta](https://www.letta.com/)

---

### 6.5 AIOS Acadêmico (Rutgers University, 2024)

Paper [AIOS: LLM Agent Operating System (arXiv:2403.16971)](https://arxiv.org/abs/2403.16971) define arquitetura onde LLMs atuam como "kernel" do OS, e agentes são tratados como aplicações.

**Módulos do AIOS kernel:**
- **Scheduler:** Atribui tarefas de agentes baseado em disponibilidade de recursos
- **Context Manager:** Salva e restaura estado de tarefas
- **Tool Manager:** Lida com chamadas de API e uso de ferramentas externas com segurança
- **Memory Layer:** Separa memória de longo e curto prazo
- **Access Controls:** Execução segura e baseada em papéis

**Modos de deployment:** Local Kernel Mode (dev/teste), Remote Kernel Mode (dispositivos com poucos recursos), Mode 2.5 (desenvolvimento distribuído).

**Cerebrum (Agent SDK para AIOS):** SDK com arquitetura de 4 camadas (LLM, memory, storage, tool management) + Agent Hub para compartilhamento de agentes.

**Fontes:** [AIOS LLM Agent Operating System](https://arxiv.org/abs/2403.16971); [Cerebrum AIOS SDK](https://arxiv.org/pdf/2503.11444); [Gate Learn What is AIOS](https://www.gate.com/learn/articles/what-is-aios-the-future-path-of-intelligent-agent-operating-systems/5815)

---

## 7. Catálogos de módulos/agentes existentes

### 7.1 O que existe hoje

**Anthropic MCP (Model Context Protocol):** Protocolo padrão para integração de ferramentas com LLMs. Ecossistema crescente de servidores MCP de terceiros (databases, APIs, SaaS tools). Orientado a desenvolvedores.

**LangChain Hub:** Repositório de prompts, chains e agentes compartilháveis. Técnico, requer Python.

**CrewAI Tools:** Biblioteca de ferramentas prontas para integração em Crews (busca web, scraping, análise de código, etc). Técnico.

**AutoGen Studio Templates:** Templates de agentes para prototipagem. Interface gráfica, mais acessível.

**Cerebrum Agent Hub:** Marketplace de agentes para o ecossistema AIOS. Acadêmico.

**OpenAI Apps SDK (2025):** Transformou ChatGPT em host para aplicações de terceiros (Spotify, Zillow, Figma). Modelo de "app store" para agentes.

### 7.2 Lacuna para PMEs

Não existe catálogo validado de módulos/agentes prontos para os casos de uso comuns de PME:
- Atendimento ao cliente (triagem, FAQ, escalonamento)
- Qualificação e follow-up de leads
- Emissão e controle de propostas comerciais
- Onboarding de clientes
- Controle financeiro básico (contas a pagar/receber, alertas)
- Gestão de agenda e recursos
- Treinamento interno e documentação de processos

Esta lacuna é uma oportunidade direta de posicionamento para a biblioteca AIOS do projeto Profissional 2027.

---

## 8. Human-in-the-loop por nível de risco

### 8.1 Três padrões fundamentais de HITL

Com base em pesquisa de padrões de produção em 2024-2026 [StackAI HITL](https://www.stackai.com/insights/human-in-the-loop-ai-agents-how-to-design-approval-workflows-for-safe-and-scalable-automation):

1. **Approval (Gatekeeper):** Humano aprova/rejeita antes de ações de alto risco. Crítico: a aprovação deve acontecer ANTES dos efeitos colaterais, não após.
2. **Intervention (Override):** Humano pode interromper execução no meio do processo.
3. **Clarification (Confidence Gate):** Agente pergunta quando confiança está abaixo do threshold.

### 8.2 Arquitetura estratificada por risco (recomendada para PMEs)

Baseado nos padrões de produção identificados:

| Nível | Regime | Descrição | Exemplos PME |
|---|---|---|---|
| **Nível 1 — Assistir** | Humano decide | IA propõe, humano aprova tudo | Rascunho de e-mail, sumário de reunião, análise de contrato |
| **Nível 2 — Co-piloto** | Humano aprova ações | IA prepara ação, humano autoriza | Exceção de preço, onboarding de fornecedor, resposta a reclamação |
| **Nível 3 — Autopiloto com exceções** | IA age, escala casos anômalos | IA executa automaticamente, aciona humano quando detecta risco | Resposta FAQ, triagem de leads, agendamentos rotineiros |
| **Nível 4 — Totalmente autônomo** | IA age sem intervenção | Apenas para processos totalmente validados e reversíveis | Organização de arquivos, categorização de dados, relatórios periódicos |

### 8.3 Sinais de risco para escalonamento automático

- Valores acima de threshold (ex: reembolsos acima de R$500)
- Padrões incomuns (anomalia em fatura, comportamento fora do padrão)
- Desvios de política (linguagem contratual não-padrão)
- Entidades de alto impacto (clientes estratégicos, parceiros-chave)

### 8.4 Fundamentação regulatória

O NIST AI RMF (2024 Generative AI Profile) exige revisão adicional, documentação e supervisão gerencial em contextos críticos. A EU AI Act (2024) classifica sistemas de IA por risco (inaceitável, alto, limitado, mínimo) e exige human oversight proporcional ao risco. Para PMEs brasileiras, a LGPD e o Marco Legal da IA (PL 2338/2023) são as referências normativas emergentes.

**Fontes:** [HITL Patterns Arun Baby](https://www.arunbaby.com/ai-agents/0025-human-in-the-loop-patterns/); [StackAI HITL Design](https://www.stackai.com/insights/human-in-the-loop-ai-agents-how-to-design-approval-workflows-for-safe-and-scalable-automation); [Strata HITL Guide 2026](https://www.strata.io/blog/agentic-identity/practicing-the-human-in-the-loop/)

---

## 9. Princípios da Anthropic para agentes efetivos

Síntese do guia "Building Effective Agents" (Anthropic, dezembro de 2024) — considerado o documento de referência mais relevante e acessível sobre design de agentes em produção:

### 9.1 Princípio da Simplicidade Mínima
"Encontre a solução mais simples possível e aumente a complexidade apenas quando necessário." Isso pode significar não construir sistemas agênticos de forma alguma. Otimizar uma única chamada LLM com retrieval e exemplos contextuais frequentemente é suficiente.

### 9.2 Workflows vs. Agentes
- **Workflows** oferecem previsibilidade e consistência para tarefas bem definidas.
- **Agentes** são melhor opção quando flexibilidade e decisão orientada pelo modelo são necessárias em escala.
- A maioria das aplicações começa com workflows; agentes autônomos são para tarefas abertas onde as etapas não podem ser predefinidas.

### 9.3 O LLM Aumentado como Bloco Fundamental
Qualquer sistema deve começar com o bloco básico: LLM + retrieval + ferramentas + memória. Aumento de complexidade deve vir apenas quando o bloco básico se mostrar insuficiente.

### 9.4 Ferramentas bem documentadas superam prompts elaborados
Quando a Anthropic otimizou seu agente para SWE-bench em 2024, gastou mais tempo em definições de ferramentas do que em prompts. Ferramentas com interfaces bem documentadas permitem que o LLM selecione e use corretamente sem instruções extensas.

### 9.5 Cinco workflows canônicos de produção
1. Prompt Chaining (sequência com gates)
2. Routing (classificação + direcionamento especializado)
3. Parallelization (sectioning + voting)
4. Orchestrator-Subagents (orquestrador + especialistas)
5. Evaluator-Optimizer (geração + avaliação + iteração)

### 9.6 Confiança em subagentes
Quando Claude atua como orquestrador e chama subagentes Claude, deve manter prompts de sistema que previnem prompt injection e validar inputs antes de usar em operações sensíveis. Minimize raio de explosão de ações errôneas.

### 9.7 Clareza na interrupção humana
Agentes devem ter checkpoints claros onde pausam para feedback humano. Deve-se definir a priori quais situações requerem confirmação — não descobrir durante execução.

### 9.8 Ground truth a cada passo
Durante execução, agentes devem obter "ground truth" do ambiente a cada passo (resultados de ferramentas, execução de código) para avaliar progresso. Não assumir que ações anteriores foram bem-sucedidas.

**Fonte:** [Anthropic Building Effective Agents](https://www.anthropic.com/research/building-effective-agents)

---

## 10. Recomendações para o AIOS do projeto Profissional 2027

### 10.1 Definição de AIOS para este projeto

O AIOS (AI Operating System) do Profissional 2027 não é um sistema operacional técnico. É uma **biblioteca de módulos de agentes prontos para PMEs**, organizada por função de negócio, com níveis de autonomia calibráveis e documentação em PT-BR orientada a implementadores não-técnicos.

### 10.2 Arquitetura recomendada do AIOS

**Camada 1 — Módulos Atômicos (building blocks)**
Agentes simples, single-purpose, com uma ferramenta ou capacidade:
- Módulo de Consulta (RAG sobre documentos da empresa)
- Módulo de Comunicação (e-mail, WhatsApp, notificações)
- Módulo de Registro (gravação de dados em planilhas/CRM/banco)
- Módulo de Leitura (extração de dados de PDFs, planilhas, e-mails)
- Módulo de Cálculo (operações financeiras, métricas, relatórios)

**Camada 2 — Módulos Compostos (workflows)**
Combinações de módulos atômicos para processos completos:
- Agente de Atendimento (Routing + Consulta + Comunicação)
- Agente de Vendas (Consulta + Registro + Comunicação)
- Agente Financeiro (Leitura + Cálculo + Comunicação)
- Agente de RH (Consulta + Registro + Comunicação)
- Agente de Projetos (Orquestrador + múltiplos atômicos)

**Camada 3 — Orquestrador de Negócio**
Um agente orquestrador que conhece os processos da empresa (memória de longo prazo, tipo Letta) e distribui tarefas para os módulos compostos conforme demanda.

### 10.3 Matriz de implementação DPIA × AIOS

| Fase DPIA | Papel do AIOS |
|---|---|
| **Diagnosticar** | AIOS Assessment Module: questionário guiado que mapeia processos e identifica candidatos a automação |
| **Processar** | AIOS Data Module: preparação e contextualização de dados do processo para alimentar os agentes |
| **Instruir** | AIOS Configuration: templates de instrução (system prompts) para cada módulo, personalizáveis sem código |
| **Automatizar** | AIOS Deploy: ativação do módulo composto com HITL configurado para o nível de risco do processo |

### 10.4 Princípios de design do AIOS para PMEs

1. **Zero-code first:** Toda configuração deve ser possível sem escrever código. Código é camada opcional para implementadores avançados.
2. **Simplicidade antes de potência:** Seguindo Anthropic — começar com o módulo mais simples. Upgrades de complexidade são progressivos.
3. **HITL por padrão:** Nível 1 (Assistir) é o padrão de todos os módulos. Usuário deve escolher ativamente aumentar autonomia.
4. **Memória de negócio:** Todo módulo deve acumular contexto sobre o negócio ao longo do tempo (padrão Letta), não começar do zero a cada execução.
5. **PT-BR nativo:** Documentação, templates de instrução e interface em português do Brasil. Sem tradução de jargão técnico.
6. **ROI visível:** Cada módulo deve ter métrica de impacto clara (tempo economizado, erros reduzidos, leads convertidos).
7. **Interoperabilidade:** Módulos devem ser combináveis entre si via padrão de orquestração (inspirado em LangGraph/CrewAI Flows), mas não exigem conhecimento desses frameworks.

### 10.5 Prioridade de módulos para V1 (maior impacto em PMEs)

Com base nas barreiras e motivações identificadas em pesquisas de adoção de IA em PMEs brasileiras:

1. **Módulo de Atendimento:** FAQ inteligente + triagem + escalamento humano — impacto direto em produtividade de equipes de atendimento.
2. **Módulo de Proposta Comercial:** Geração de propostas personalizadas a partir de briefing — ROI imediato para vendas.
3. **Módulo de Resumo e Documentação:** Transcrição e resumo de reuniões, chamadas, documentos — impacto universal em produtividade.
4. **Módulo Financeiro Básico:** Categorização, alertas de vencimento, relatórios simples — impacto direto em controle de PME.
5. **Módulo de Treinamento Interno:** Base de conhecimento da empresa consultável — multiplica capacidade de onboarding.

---

## Implicações para o DPIA e AIOS

**1. O DPIA é genuinamente original no espaço de PMEs.** Nenhum framework existente combina a sequência diagnóstico → preparação → instrução → automação em um método acessível a não-técnicos em PT-BR. O posicionamento é robusto.

**2. A fase "Processar" é a mais diferenciada.** Nenhum framework major nomeia explicitamente a preparação de contexto (organizar dados, documentar processos, preparar instrução) como fase. Esta é a contribuição conceitual mais clara do DPIA.

**3. A fase "Instruir" ressignifica prompt engineering como competência de negócio.** Em vez de "habilidade técnica de TI", o DPIA posiciona instrução de agentes como responsabilidade do gestor que conhece o processo — análoga a escrever um procedimento operacional padrão (POP).

**4. O AIOS deve ser construído de baixo para cima, não de cima para baixo.** Começar com módulos atômicos validados em PMEs reais antes de construir orquestração complexa. Seguir o princípio Anthropic de mínima complexidade necessária.

**5. Human-in-the-loop não é limitação — é vantagem competitiva para PMEs.** Para PMEs que desconfiam de automação, o HITL explícito e calibrado por risco é um argumento de adoção, não uma concessão. Posicionar como "automação com supervisão inteligente."

**6. A lacuna de catálogo de módulos para PMEs é real e defensável.** Criar e documentar 5-10 módulos validados em casos reais de PME gera defensibilidade e propriedade intelectual prática para o programa.

**7. O perfil de "implementador de IA em PME" não existe como formação estruturada.** O Profissional 2027 tem oportunidade de definir este perfil e as competências associadas (diagnóstico, preparação, instrução, orquestração básica) antes de qualquer concorrente.

---

## Fontes citadas

1. [McKinsey State of AI 2025](https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai)
2. [McKinsey AI Implementation Guide 2025](https://vladimirsiedykh.com/blog/ai-business-implementation-guide-mckinsey-research-success-patterns-2025)
3. [BCG AI @ Scale](https://www.bcg.com/capabilities/artificial-intelligence)
4. [BCG GAMMA Overview – Case Basix](https://www.casebasix.com/pages/bcg-gamma)
5. [BCG AI Transformation Workforce 2026](https://www.bcg.com/publications/2026/ai-transformation-is-a-workforce-transformation)
6. [Accenture Art of AI Maturity](https://www.accenture.com/us-en/insights/artificial-intelligence/ai-maturity-and-transformation)
7. [Deloitte State of AI in the Enterprise 2026](https://www.deloitte.com/us/en/what-we-do/capabilities/applied-artificial-intelligence/content/state-of-ai-in-the-enterprise.html)
8. [PwC 2026 AI Predictions](https://www.pwc.com/us/en/tech-effect/ai-analytics/ai-predictions.html)
9. [EY/KPMG/PwC Governance Philosophies](https://medium.com/agentic-ai-systems-and-the-future-ahead/eys-100-000-agent-ambition-kpmg-s-trust-first-pwc-s-scale-first-three-governance-philosophies-63c1593a8ea0)
10. [Microsoft Agentic AI Maturity Model](https://learn.microsoft.com/en-us/microsoft-copilot-studio/guidance/maturity-model-overview)
11. [Microsoft Inside Track AI Maturity 5 Steps](https://www.microsoft.com/insidetrack/blog/enterprise-ai-maturity-in-five-steps-our-guide-for-it-leaders/)
12. [IBM AI Lifecycle](https://www.ibm.com/think/topics/ai-lifecycle)
13. [IBM Agentic AI Governance 2025](https://www.ibm.com/new/announcements/agentic-ai-governance-evaluation-and-lifecycle)
14. [Gartner AI Maturity Model Toolkit](https://www.gartner.com/en/chief-information-officer/research/ai-maturity-model-toolkit)
15. [BMC Gartner AI Maturity Guide](https://www.bmc.com/blogs/ai-maturity-models/)
16. [Gartner Survey AI Maturity Operations 2025](https://www.gartner.com/en/newsroom/press-releases/2025-06-30-gartner-survey-finds-forty-five-percent-of-organizations-with-high-artificial-intelligence-maturity-keep-artificial-intelligence-projects-operational-for-at-least-three-years)
17. [NIST AI RMF Core Functions](https://airc.nist.gov/airmf-resources/airmf/5-sec-core/)
18. [IS Partners NIST AI RMF Core Functions Guide](https://www.ispartnersllc.com/hubs/nist-ai-rmf/core-functions/)
19. [NIST AI 100-1 PDF](https://nvlpubs.nist.gov/nistpubs/ai/nist.ai.100-1.pdf)
20. [ISO/IEC 23053:2022](https://www.iso.org/standard/74438.html)
21. [Nemko ISO/IEC 23053 Guide](https://digital.nemko.com/standards/iso-iec-23053)
22. [Anthropic Building Effective Agents](https://www.anthropic.com/research/building-effective-agents)
23. [LangGraph Docs](https://docs.langchain.com/oss/python/langgraph/overview)
24. [CrewAI Introduction](https://docs.crewai.com/introduction)
25. [AutoGen](https://microsoft.github.io/autogen/stable/)
26. [Letta Memory-First Agents](https://www.letta.com/)
27. [AIOS LLM Agent Operating System – arXiv](https://arxiv.org/abs/2403.16971)
28. [Cerebrum AIOS SDK – arXiv](https://arxiv.org/pdf/2503.11444)
29. [Gate Learn What is AIOS](https://www.gate.com/learn/articles/what-is-aios-the-future-path-of-intelligent-agent-operating-systems/5815)
30. [AI Agent Architecture Patterns Redis](https://redis.io/blog/ai-agent-architecture-patterns/)
31. [ReAct Architectures Emergent Mind](https://www.emergentmind.com/topics/reason-act-reflect-react-architectures)
32. [Agentic AI Design Patterns 2026](https://www.innovatrixinfotech.com/blog/agentic-ai-design-patterns-react-reflection-tool-use)
33. [Digital Applied Agent Architecture Taxonomy 2026](https://www.digitalapplied.com/blog/agent-architecture-patterns-taxonomy-2026)
34. [HITL Patterns Arun Baby](https://www.arunbaby.com/ai-agents/0025-human-in-the-loop-patterns/)
35. [StackAI HITL Approval Workflows](https://www.stackai.com/insights/human-in-the-loop-ai-agents-how-to-design-approval-workflows-for-safe-and-scalable-automation)
36. [Strata HITL Guide 2026](https://www.strata.io/blog/agentic-identity/practicing-the-human-in-the-loop/)
37. [OECD AI Adoption SMEs 2025](https://www.oecd.org/content/dam/oecd/en/publications/reports/2025/12/ai-adoption-by-small-and-medium-sized-enterprises_9c48eae6/426399c1-en.pdf)
38. [Implementação IA PMEs Brasil brspace 2025](https://blog.brspace.com.br/2025/04/22/relatorio-detalhado-implementacao-de-inteligencia-artificial-em-pequenas-e-medias-empresas-pmes-roadmap-metricas-de-sucesso-e-resultados-comprovados/)
39. [Metodologias IA PMEs Brasileiras Revista Tópicos](https://revistatopicos.com.br/artigos/metodologias-para-avaliacao-do-impacto-da-inteligencia-artificial-na-inovacao-de-modelos-de-negocios-em-pmes-brasileiras)
40. [SME AI Implementation Barriers ResearchGate 2024](https://www.researchgate.net/publication/384112060_Barriers_to_the_implementation_of_artificial_intelligence_in_small_and_medium_sized_enterprises_Pilot_study)
41. [Strategic AI Adoption SMEs arXiv 2408.11825](https://arxiv.org/pdf/2408.11825)
