# Padrões de Design de Agentes — os 5 workflows canônicos

> Adaptação do material "Building Effective Agents" (Anthropic, 2024) ao contexto PME brasileira. Cada padrão tem caso de uso, exemplo de módulo AIOS aplicado, e armadilha comum.

---

## Princípio geral antes dos padrões

**Comece sempre pelo workflow mais simples que resolve o problema.** Agente multi-step é caro, opaco e quebra em produção sem o implementador presente. Em PME, a hierarquia de escolha é:

1. **Resposta direta com LLM** (sem ferramentas) — funciona em 40% dos casos.
2. **LLM + 1 ferramenta** (consultar tabela, consultar pedido) — 30% dos casos.
3. **Prompt chaining** — 15%.
4. **Routing** — 10%.
5. **Orchestrator-subagents ou evaluator-optimizer** — 5%, com justificativa.

Padrão usado sem necessidade é **dívida técnica disfarçada de sofisticação**.

---

## Padrão 1 — Prompt Chaining (encadeamento)

### O que é
Dividir uma tarefa complexa em **N chamadas LLM sequenciais**, onde a saída de cada uma alimenta a próxima. Entre as chamadas pode haver validação programática.

### Quando aplicar
- Tarefa tem etapas claramente separáveis (extrair → validar → formatar → enviar).
- Cada etapa é mais bem feita por um prompt específico do que por um prompt monolítico.
- A validação intermediária evita erros caros no fim.

### Quando NÃO aplicar
- Tarefa cabe em 1 prompt bem escrito — chaining adiciona latência sem ganho.
- Etapas têm forte dependência circular (precisa voltar).

### Exemplo de módulo AIOS
**Módulo Proposta Comercial** — quatro elos:

```
Briefing do cliente (texto livre do vendedor)
        │
        ▼
[Elo 1] LLM: extrair { dor, contexto, objetivo, restrições } em JSON
        │
        ▼  validação programática: 4 campos obrigatórios preenchidos?
        │
        ▼
[Elo 2] LLM: gerar escopo de projeto (3 fases DPIA aplicadas)
        │
        ▼
[Elo 3] LLM: estimar preço a partir de tabela do implementador + escopo
        │
        ▼
[Elo 4] LLM: redigir proposta em PT-BR no template editorial do implementador
        │
        ▼
PDF gerado, enviado por e-mail
```

### Caso PME — Polaris Bebidas
Vendedor da Polaris recebe pedido de orçamento por WhatsApp. Workflow Polaris:
1. **Extrai** itens, quantidades e prazo de entrega do texto livre.
2. **Valida** itens contra a tabela de preços vigente (Bling).
3. **Calcula** subtotal, desconto aplicável (RT-01 se cliente é Casa do Carlos) e frete.
4. **Redige** mensagem de confirmação no tom Polaris.

Cada elo é prompt curto (200–400 tokens), barato em Haiku.

### Armadilha comum
**Prompt chaining usado quando 1 prompt resolveria.** Resultado: 4x mais latência, 4x mais custo, 4x mais ponto de falha. Antes de chained, escreva o prompt monolítico e teste.

---

## Padrão 2 — Routing (roteamento)

### O que é
Um **classificador inicial** identifica o tipo da entrada e roteia para o **handler especializado** (que pode ser outro LLM, outro workflow, ou um humano).

### Quando aplicar
- Entradas têm tipos discretos bem definidos.
- Cada tipo precisa de tratamento diferente (prompt, modelo, ferramenta).
- Tratar tudo com um só prompt grande sai mais caro ou pior.

### Quando NÃO aplicar
- Tipos não são distinguíveis com confiança.
- A diferença entre handlers é cosmética.

### Exemplo de módulo AIOS
**Módulo Atendimento WhatsApp** — roteador clássico:

```
Mensagem entra no WhatsApp
        │
        ▼
[Classificador Haiku] → tipo ∈ {
    saudação,
    pergunta_preco,
    pergunta_prazo,
    pedido_novo,
    reclamacao,
    duvida_geral,
    fora_escopo
}
        │
        ├── saudação → resposta-padrão (sem LLM, custo zero)
        ├── pergunta_preco → handler Haiku + tool consultar_tabela
        ├── pergunta_prazo → handler Haiku + tool consultar_pedido
        ├── pedido_novo → handler Sonnet + módulo Pedidos
        ├── reclamacao → escalar_humano imediato
        ├── duvida_geral → handler Sonnet + RAG
        └── fora_escopo → resposta-padrão + escalar
```

### Caso PME — Polaris Bebidas
Roteamento na Polaris reduz custo médio por interação em **65%** comparado a chamar Sonnet em todas: 40% das mensagens viram saudação ou resposta-padrão (custo zero), 35% viram Haiku com tool simples, 25% sobem para Sonnet.

### Armadilha comum
**Classificador sem few-shot real.** Classificador genérico erra em vocabulário do cliente. Solução: 10 exemplos de cada tipo, extraídos do histórico, no prompt do classificador.

---

## Padrão 3 — Parallelization (paralelização)

### O que é
Dividir a tarefa em **N chamadas LLM independentes em paralelo**, depois agregar.

Duas variantes:

- **Sectioning** — cada chamada cobre uma seção/aspecto diferente.
- **Voting** — N chamadas resolvem o mesmo problema; vota-se na resposta majoritária.

### Quando aplicar
- Subtarefas são genuinamente independentes (não há sequência lógica).
- Latência total importa mais do que custo bruto.
- Para voting: tarefa é crítica e benefício de múltiplos passes justifica o custo Nx.

### Quando NÃO aplicar
- Há dependência (saída de A alimenta B).
- Volume alto e voting fica caro sem ganho mensurável.

### Exemplo de módulo AIOS
**Módulo Resumo de Reunião** — sectioning paralelo:

```
Transcrição completa
        │
        ├── [P1 Sonnet] extrair decisões tomadas
        ├── [P2 Sonnet] extrair ações com responsável + prazo
        ├── [P3 Sonnet] extrair riscos e bloqueios
        └── [P4 Sonnet] extrair próximos passos
                │
                ▼
        Agregação → ata estruturada em PT-BR
```

Em transcrição de 60 minutos, fazer 4 chamadas em paralelo entrega ata em ~30s contra ~90s sequencial. Custo igual (mesmo total de tokens).

### Voting em caso PME
**Módulo Cobrança** — classificação de perfil de devedor (cooperativo, esquivo, hostil) é crítica para escolher tom. Usar voting de 3 chamadas Haiku reduz erro de classificação em ~40% comparado a 1 chamada. Em cobrança ativa, errar o tom pode custar a relação.

### Armadilha comum
**Paralelizar tarefas que não são independentes.** Resultado: respostas desconectadas, agregação ruim, retrabalho. Antes de paralelizar, desenhe a dependência num grafo.

---

## Padrão 4 — Orchestrator-Subagents (orquestrador + subagentes)

### O que é
Um **agente orquestrador** decompõe dinamicamente uma tarefa em subtarefas, delega para **subagentes especializados**, agrega o resultado.

### Quando aplicar
- A decomposição da tarefa não é conhecida a priori (depende da entrada).
- Subtarefas exigem ferramentas ou prompts muito diferentes.
- Justifica o overhead de coordenação (latência + token + complexidade).

### Quando NÃO aplicar
- Decomposição é conhecida → use prompt chaining ou routing.
- Volume alto + margem apertada → custo explode.
- Primeiro projeto do aluno → simplesmente não.

### Exemplo de módulo AIOS — caso avançado
**Módulo Cobrança** versão pro (após 3 meses de operação, dados suficientes):

```
Orquestrador (Sonnet): recebe lista de inadimplentes do mês
        │
        ├── Subagente "perfil" → consulta histórico + classifica devedor
        ├── Subagente "abordagem" → gera mensagem por perfil
        ├── Subagente "agenda" → escolhe melhor horário (segmentos do histórico)
        └── Subagente "follow-up" → planeja sequência (D+3, D+7, D+15)
                │
                ▼
        Lote de mensagens agendadas no n8n + plano de escalonamento
```

### Armadilha comum
**Orchestrator-subagents como default.** Em PME, esse padrão é exceção. A regra: **escolher esse padrão exige justificativa documentada de que routing + chaining não resolvem.**

---

## Padrão 5 — Evaluator-Optimizer (avaliador + otimizador)

### O que é
Um LLM **gera** a saída; outro LLM **avalia** e pede revisão; o gerador **otimiza**. Loop até critério atendido ou limite de iterações.

### Quando aplicar
- Existe critério de qualidade claro e mensurável (gramática, tom, completude, conformidade a template).
- Saída final precisa de qualidade alta e o cliente paga pelo polimento.
- Tempo de resposta permite iterar.

### Quando NÃO aplicar
- Resposta precisa ser síncrona em < 5 segundos (WhatsApp).
- Critério de qualidade não é mensurável objetivamente.
- Volume alto — custo Nx por iteração.

### Exemplo de módulo AIOS
**Módulo Proposta Comercial** — modo polido:

```
[Gerador Sonnet] redige proposta inicial
        │
        ▼
[Avaliador Sonnet] checa: tem dor? tem escopo? tem ROI numérico?
        │  tom institucional do implementador? tem call-to-action?
        │
        ▼  se algum critério falha →
        │
[Gerador Sonnet] revisa com feedback específico
        │
        ▼  loop até OK ou 3 iterações
        │
        ▼
Proposta final
```

### Armadilha comum
**Evaluator que aceita tudo na primeira iteração.** Avaliador precisa de critério estrito; senão é só latência adicional.

---

## Mapa dos módulos AIOS por padrão

| Módulo | Padrão dominante | Padrões secundários |
|---|---|---|
| Atendimento WhatsApp | Routing | Resposta direta, chaining |
| Pedidos via WhatsApp | Chaining (extrai → valida → cria) | Routing na entrada |
| Agendamento | Routing | Resposta direta |
| Cobrança | Chaining (perfil → mensagem → agendar) | Voting na classificação de perfil |
| Proposta Comercial | Chaining | Evaluator-optimizer opcional |
| Resumo de Reunião | Parallelization (sectioning) | — |

---

## Quando subir de complexidade

Regra editorial:

- **Primeiros 3 clientes do aluno:** apenas Resposta Direta + Routing + Chaining.
- **Cliente 4 em diante:** Parallelization pode entrar.
- **Cliente 6 em diante:** Evaluator-optimizer em casos específicos.
- **Orchestrator-subagents:** só com 1 ano de prática operacional e mentor sênior revisando.

Quem pula etapas acaba carregando complexidade que não consegue manter quando o cliente liga reclamando às 22h.

---

## Anti-padrões

- **"Vamos colocar um agente que faz tudo."** Sintoma: ausência de diagnóstico DPIA. Trate como bloqueio.
- **Multi-agent quando 1 prompt resolveria.** Sintoma: fascinação com a moda do momento. Desencoraje.
- **Workflow com 40+ nós no n8n.** Sintoma: faltou refatorar. Quebrar em sub-workflows.
- **Agente sem matriz HITL declarada.** Sintoma: pressa. Recuse o deploy.

---

*Padrões de Design AIOS v1.0 — adaptação livre de "Building Effective Agents" (Anthropic) ao contexto PME brasileira.*
