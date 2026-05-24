---
nivel: 2
trilha: I
modulo: 2
titulo: Construção do Agente
tempo_leitura: 22
exit_criteria: aluno entrega workflow n8n funcional em ambiente de homologação, conectado ao LLM, ao Bling de teste e ao Supabase, respondendo a webhook real.
---

# O que você vai aprender
- Como montar o agente em n8n: webhook → classificação → LLM → tools → resposta.
- Como escolher entre Claude Sonnet e Haiku para cada módulo, com critério de custo × qualidade.
- Como conectar tools (Bling, Supabase pgvector, Pix) via HTTP node ou função custom.
- Como configurar RAG quando a base de conhecimento é grande.
- Como ativar logging estruturado para o monitoramento posterior.

## Por que isso importa
O agente em n8n self-host é a stack composable canônica do método (Princípio 7 do DPIA). Pesquisa E §10 e Pesquisa A §2.5 mostram que essa combinação (n8n + Claude + Supabase + WhatsApp Cloud API + REST para ERP brasileiro) cobre 80% dos projetos de PME, custa R$ 550-1.750/mês de infra (Pesquisa F §1.4), e não amarra cliente nem implementador a plataforma proprietária.

A construção do agente é a fase mais técnica do método. É também onde aluno técnico se diferencia do aluno comercial. Ambos precisam dominar — o comercial não precisa otimizar, mas precisa saber montar o workflow básico.

## O passo a passo

**1. Provisionar ambiente de homologação.** Antes de tocar em produção:
- n8n self-host em VPS Hetzner (CX22 ou CX32, ~R$ 50/mês).
- Domínio + HTTPS via Caddy ou Traefik.
- Acesso ao Bling de teste (pedir token de teste ao cliente — Bling permite ambiente sandbox).
- Supabase projeto novo, pgvector ativado, schema pronto.
- Conta Meta WhatsApp Cloud API — em homologação, usa-se número de teste fornecido pela Meta.
- Conta Anthropic com chave API e limite de gastos configurado.

**2. Topologia do workflow n8n.** Estrutura canônica:

```
[Webhook WhatsApp] 
   ↓
[Function: classificar intenção / extrair contexto]
   ↓
[Function: consultar memória (Supabase) — últimas N mensagens do cliente]
   ↓
[HTTP Request: chamar Claude (Anthropic API)]
   ↓ 
[Switch: ação retornada]
   ├─ responder_cliente → [WhatsApp Send Message]
   ├─ criar_pedido → [HTTP Bling create order] → [WhatsApp Send confirmation]
   ├─ escalar_humano → [WhatsApp Send escalation msg] + [notificar operador no canal de escala]
   └─ pedir_esclarecimento → [WhatsApp Send question]
   ↓
[Function: logar interação no Supabase]
```

**3. Escolher modelo Claude.** Critério:
- **Sonnet 4.x (raciocínio principal):** atendimento de pedido, agendamento com regras complexas, triagem documental jurídica. Custo: ~R$ 0,05-0,15 por interação completa.
- **Haiku 4.x (volume, periférico):** transcrição, sumarização, classificação simples, FAQ pura. Custo: ~R$ 0,01-0,03 por interação.
- **Combinação:** Haiku para classificar a intenção da mensagem; Sonnet para gerar a resposta com regras aplicadas.

Em PME, o custo do LLM é desprezível comparado ao valor do processo. Pedido de R$ 268 com interação que custa R$ 0,08 representa 0,03% do ticket. Otimizar custo de LLM antes da hora é gastar tempo onde não rende.

**4. Configurar memória / RAG no Supabase.** Para cada cliente, criar tabela `interactions`:
```sql
create table interactions (
  id uuid primary key default gen_random_uuid(),
  cliente_id text,
  telefone text,
  mensagem_input text,
  mensagem_output text,
  contexto_usado jsonb,
  acao text,
  confidence float,
  escalado boolean default false,
  created_at timestamp default now()
);

create index on interactions (telefone, created_at desc);
```

Para RAG, segunda tabela:
```sql
create table knowledge_base (
  id uuid primary key default gen_random_uuid(),
  conteudo text,
  embedding vector(1536),
  metadata jsonb
);

create index on knowledge_base using ivfflat (embedding vector_cosine_ops);
```

Carregar base de conhecimento gerada na fase P: FAQ + scripts + políticas + regras tácitas contextuais.

**5. Conectar tools via HTTP node ou função.** Cada tool do system prompt vira:
- HTTP node n8n configurado com endpoint, auth e schema.
- Ou função JavaScript no n8n com lógica adicional (validação, transformação).

Para Bling especificamente, REST endpoints principais:
- `GET /api/v3/contatos?telefone=...` — busca cliente.
- `GET /api/v3/produtos?nome=...` — busca produto.
- `POST /api/v3/pedidos` — cria pedido.
- `GET /api/v3/contas-receber?contato=...` — checa inadimplência.

Cada chamada com token Bearer, com retry policy (3 tentativas, backoff exponencial).

**6. Configurar tool use no Claude.** A API Anthropic permite registrar tools no payload. Cada tool tem:
- `name`
- `description` (vai entrar no contexto do modelo)
- `input_schema` (JSON Schema)

Quando o Claude decide chamar uma tool, retorna `tool_use` no response. n8n captura, executa a tool real (HTTP node), passa o resultado de volta em nova chamada do Claude com `tool_result`. Loop até a resposta final.

Em workflow n8n, esse loop é implementado com `Switch` + `Wait` ou com `Loop Over Items` apropriado.

**7. Logar tudo no Supabase.** Cada interação completa gera um insert em `interactions`. Campos críticos:
- `mensagem_input` (do cliente).
- `mensagem_output` (para o cliente).
- `contexto_usado` (cliente_id, fluxo aplicado, regras disparadas).
- `acao` (responder, criar pedido, escalar).
- `confidence` (do LLM).
- `escalado` (sim/não).

Esse log é a base do monitoramento e dos relatórios mensais da fase de Acompanhamento. Sem log estruturado, não há métrica de adoção nem de qualidade.

**8. Webhook do WhatsApp.** O webhook da Meta recebe payload JSON com mensagem do cliente. Configuração no n8n: webhook node com path único, autenticação por verify token (definido na Meta). Validar assinatura no first hit.

**9. Configurar plano de rollback (testado).** Em n8n, o "botão de pausar" é uma flag em variável do workflow:
- Quando flag = "pausa", o webhook responde mensagem-padrão e roteia para canal humano.
- Quando flag = "ativo", processa normalmente.

Champion (Maria) tem acesso a uma interface simples (botão em página privada, ou comando no Slack/WhatsApp interno) que muda essa flag em < 15 segundos. Teste obrigatório antes do exit-gate de I.

**10. Configurar alerting básico.** n8n + webhook para canal interno (Slack, Discord, WhatsApp da equipe):
- Erro HTTP no Bling.
- Latência LLM > 8s.
- Confidence < 0.5 em mais de N% das interações na última hora.
- Taxa de escalonamento > 40% em uma janela curta.

Sem alerting, problema em produção só é descoberto pelo cliente reclamando.

## Exemplo aplicado — Polaris Bebidas (Joinville/SC)

Workflow n8n da Polaris, ambiente de homologação:

```
[Webhook Meta WABA staging]
   ↓
[Function: parse payload, extrair {telefone, texto, audio_url?}]
   ↓
[IF audio? → Haiku: transcrever via Whisper-like → texto]
   ↓
[Function: load últimas 5 mensagens do telefone no Supabase]
   ↓
[HTTP: Claude Sonnet com system_prompt_v0_4 + contexto + tools]
   ↓
[Loop tool_use:
   - consulta_cliente_bling → HTTP Bling
   - consulta_produto_bling → HTTP Bling
   - consulta_rag_polaris → Supabase pgvector
   - cria_pedido_bling → HTTP Bling
   - escala_humano → função n8n + notificação canal Slack interno
   - retorno tool_result para Claude]
   ↓
[Switch acao:
   responder_cliente → [HTTP WhatsApp Send]
   criar_pedido → [HTTP Bling + WhatsApp Send]
   escalar_humano → [WhatsApp Send + alerta Slack para Maria]
   pedir_esclarecimento → [WhatsApp Send]]
   ↓
[Function: insert interaction no Supabase]
```

Decisões técnicas Polaris:
- Sonnet 4.6 (atualização disponível em maio 2026) como modelo principal.
- Haiku 4.6 para transcrição de áudio + classificação preliminar.
- Custo médio estimado por interação: R$ 0,09 (300 interações/dia × 30 dias = R$ 810/mês de LLM, dentro da projeção).
- Latência alvo: < 4s entre mensagem do cliente e início de resposta.
- Supabase pgvector com 47 chunks da FAQ Polaris + 18 regras tácitas em formato de embedding.
- Bling token de teste isolado, ambiente sandbox.
- WABA: número de teste Meta usado em homologação.
- Plano de rollback: Maria recebe link de página interna com botão "PAUSAR AGENTE". Clique muda flag no n8n. Teste: pausa em 8 segundos, alerta no WhatsApp da equipe em 12 segundos. OK.

## Erros comuns
- **Construir direto em produção.** Sem ambiente de homologação, erro afeta cliente real. *Corrige:* sandbox obrigatório.
- **Pular memória de sessão.** Agente esquece o contexto da conversa entre mensagens. *Corrige:* últimas N mensagens carregadas a cada chamada.
- **Tool use sem schema rigoroso.** Claude inventa parâmetros. *Corrige:* JSON Schema completo em cada tool.
- **Não logar.** Sem log, sem métrica. *Corrige:* insert estruturado a cada interação.
- **Esquecer rate limit da Meta.** WhatsApp Cloud API tem limites por número. *Corrige:* queue interno do n8n com retry.
- **Otimizar custo de LLM antes da hora.** Trocar Sonnet por Haiku antes de validar qualidade. *Corrige:* primeiro qualidade, depois custo.

## Checklist de saída
- [ ] Ambiente de homologação provisionado (n8n, Supabase, Bling sandbox, WABA staging).
- [ ] Workflow n8n estruturado (webhook → classificador → LLM → tools → resposta → log).
- [ ] Tools conectadas com schema rigoroso.
- [ ] Memória de sessão funcionando (últimas N mensagens carregadas).
- [ ] RAG configurado se aplicável (Supabase pgvector + base).
- [ ] Logging estruturado em `interactions`.
- [ ] Plano de rollback funcional (pausa em < 15s).
- [ ] Alerting básico configurado.

## Vai além
- Manual canônico, sub-etapa I.2 + Princípio 7.
- Pesquisa E §10 — stack composable.
- Documentação Anthropic Tool Use (versão atual).
- Documentação n8n Webhook + HTTP nodes.
- Exercício prático: construir o workflow do cliente do exercício em ambiente de homologação real (n8n local ou VPS pessoal); fazer 5 chamadas de teste end-to-end.
