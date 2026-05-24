---
nivel: 2
trilha: A
modulo: 2
titulo: Integração com ERPs Brasileiros
tempo_leitura: 22
exit_criteria: aluno integra ao menos um ERP (Bling, Omie ou Conta Azul) ao workflow, com chamada real funcional para criar pedido, consultar cliente e consultar inadimplência, e configura cobrança Pix via Asaas ou nativo do ERP.
---

# O que você vai aprender
- A diferença prática entre Bling, Omie, Conta Azul e Tiny — quando cada um faz sentido.
- Autenticação, escopos e padrões de leitura/escrita das APIs REST de cada ERP.
- Como criar pedido, consultar produto, consultar cliente, consultar inadimplência.
- Como integrar Pix (direto pelo ERP, Asaas, ou Mercado Pago) e NF-e (emissão automática pós-venda).
- Anti-padrões e armadilhas: rate limit, paginação, autenticação que expira, mudança de versão da API.

## Por que isso importa
Fato 9 da síntese transversal: o ecossistema brasileiro é barreira de entrada defensiva. WABA + PIX + NF-e + Bling/Omie/Conta Azul + LGPD-PME forma um conjunto que o profissional americano não cobre. Esse conhecimento específico é o moat do implementador brasileiro. Pesquisa A §6.2-6.4 e Pesquisa E §9.1: 71% dos projetos em produção tocam o ecossistema local; saber operar essas integrações é competência mínima do egresso.

Pesquisa E (Implicações para o currículo, Módulo 5) lista as integrações brasileiras como módulo de 16 horas — peso significativo. Sem esse domínio, o aluno fica preso a casos simples (FAQ-only) e não acessa ticket bom (R$ 20k+).

## O passo a passo

**1. Escolher o ERP do cliente.** Decisão do cliente, não do implementador. Em PME brasileira, distribuição típica (Pesquisa A §6.4):
- Bling: ~40% do mercado PME — varejo, atacado, e-commerce.
- Omie: ~25% — comércio + serviço com gestão financeira mais robusta.
- Conta Azul: ~15% — serviços, contabilidade integrada.
- Tiny (ex-Olist): ~10% — e-commerce.
- Outros (TOTVS RM/Protheus, Sankhya, próprio): ~10%.

O implementador domina pelo menos um (Bling é o mais coberto pelo programa); aprende os outros conforme aparece cliente.

**2. Bling (REST v3).**

Autenticação: OAuth 2.0 com refresh token. Token de acesso dura 1h; refresh token dura 30 dias. Implementação:
- Tela de autorização única do cliente para a aplicação criada no Bling Developer.
- Tokens armazenados criptografados no Supabase.
- Refresh automático no n8n via cron a cada 50 minutos.

Endpoints principais:
- `GET /api/v3/contatos?filters[telefone]=...` — busca cliente por telefone.
- `GET /api/v3/contatos/{id}` — detalhe do contato.
- `GET /api/v3/produtos?criterio=...` — busca produto por nome/SKU.
- `GET /api/v3/produtos/{id}` — detalhe.
- `POST /api/v3/pedidos/vendas` — cria pedido.
- `GET /api/v3/contas/receber?contato={id}&situacao=aberta` — inadimplência.
- `POST /api/v3/notas/{id}/emitir` — emite NF-e pós-pedido.

Rate limit: 3 requisições/segundo por aplicação. Implementar throttle.

**3. Omie (REST).**

Autenticação: app_key + app_secret no body de cada requisição (não em header). Stateless, mais simples que OAuth.

Endpoints principais:
- `POST /api/v1/geral/clientes/` com `param: [{call: "ListarClientes", ...}]` — buscar cliente.
- `POST /api/v1/geral/produtos/` — buscar produto.
- `POST /api/v1/produtos/pedido/` — criar pedido.
- `POST /api/v1/financas/contareceber/` — inadimplência.

Rate limit: 60 requisições/minuto. Documentação tem padrão de "call" que é específico — uma curva de aprendizado.

**4. Conta Azul.**

Autenticação: OAuth 2.0, similar ao Bling.

Endpoints: similar lógica, com diferença no schema. Conta Azul foca mais em serviços; campos como "centro de custo" e "categoria DRE" são proeminentes — relevante para clínicas e contábeis.

**5. Padrões comuns de integração.**

Para qualquer ERP brasileiro:

- *Cache curto de produto:* lista de produtos não muda 50 vezes por dia. Cachear no Supabase com TTL de 4-12h reduz drasticamente chamadas à API. Invalidar quando confirmação de mudança.
- *Cache curto de cliente:* dados básicos do cliente (id, nome, status_inadimplencia) cacheáveis por 30-60 minutos. Inadimplência crítica: cache de 5 min, ou consulta direta.
- *Idempotência:* ao criar pedido, gerar `external_id` único (UUID) e checar duplicidade. Cliente reenvia mensagem, agente não pode criar pedido em dobro.
- *Backoff em rate limit:* 429 do ERP retorna `Retry-After`. Honrar.
- *Versionamento de API:* sempre fixar versão (v3, v1, ...) em todas as URLs. Mudança de versão é evento programado, nunca surpresa.

**6. Pix.**

Três caminhos:
- *Pix direto pelo Bling/Omie/Conta Azul:* gera cobrança Pix com QR Code retornado pelo ERP. Quando o cliente paga, ERP recebe webhook do banco e marca como pago. Vantagem: integração única no ERP. Desvantagem: depende do contrato bancário do cliente.
- *Asaas (gateway):* API Asaas cria cobrança Pix, envia link de pagamento, recebe webhook. R$ ~1,99 por boleto + Pix Sandbox grátis. Vantagem: independente do ERP. Desvantagem: mais um sistema.
- *Mercado Pago:* similar Asaas. Taxa um pouco maior.

Decisão por projeto: começar pelo Pix nativo do ERP se o cliente já usa; Asaas se cliente precisa de cobrança fora do escopo do ERP (recorrência, parcelamento, etc.).

**7. NF-e (emissão automática pós-venda).**

Em distribuidora/varejo, NF-e é parte do fluxo. Padrão:
- Após pedido confirmado e pagamento liberado, agente dispara `POST /notas/.../emitir` no ERP.
- Ou ERP emite NF-e automaticamente quando pedido entra com status "faturado" (depende da configuração do cliente).
- NF-e tem regras fiscais complexas (CFOP, CST, NCM) — o ERP cuida disso desde que cadastros estejam corretos. Implementador valida configuração com contador externo do cliente em P, não em A.

Em PME pequena, NF-e fica em modo "operador confirma e emite", não autônomo, na fase inicial. Subir nível depois de 60-90 dias estáveis.

**8. LGPD e dados pessoais.**

Cada chamada ao ERP move dado pessoal do cliente final. Princípios:
- Acessar só o necessário.
- Não armazenar dado pessoal no log do n8n além do mínimo (telefone + cliente_id é suficiente).
- Retenção: alinhar com retenção do ERP (em geral 5 anos por questão fiscal).
- RIPD enxuto da fase P cobre isso.

## Exemplo aplicado — Polaris Bebidas (Joinville/SC)

Integração Bling para a Polaris:

- **Autenticação OAuth 2.0** configurada. Refresh automático cron 50 min.
- **Cache de produtos** no Supabase com TTL 6h. 1.847 produtos sincronizados.
- **Cache de clientes** TTL 30 min. ~640 clientes ativos sincronizados.
- **Cache de inadimplência** TTL 5 min — crítico para regra RT-15.
- **Idempotência de pedido:** UUID gerado no n8n no momento de "decidir criar pedido"; antes do POST, checa Supabase se já existe pedido com esse UUID nas últimas 24h.
- **Rate limit** respeitado (3 req/s). Em janela de pico, queue interno.

Pix:
- Decisão: Pix direto pelo Bling, pois Polaris já tem integração bancária Sicoob via Bling. Não introduz Asaas.
- Fluxo: agente cria pedido com pagamento "Pix" → Bling gera QR Code → URL do QR retorna no payload → agente envia ao cliente via WhatsApp.

NF-e:
- Decisão: nas primeiras 8 semanas, NF-e fica em modo "Maria confirma e emite". Depois de 60 dias estáveis, sobe para emissão automática condicional a (pedido confirmado + pagamento liberado).

Funções n8n criadas:
- `bling_consulta_cliente(telefone)` → retorna {id, nome, status_inadimplencia, ultimo_pedido_data}.
- `bling_consulta_produto(termo)` → retorna [{sku, nome, preco, estoque}] (top 5 matches).
- `bling_cria_pedido(cliente_id, items[], entrega, pagamento)` → retorna {pedido_id, valor_total, qr_code_url_se_pix}.
- `bling_consulta_inadimplencia(cliente_id)` → retorna boolean + valor_aberto.

Testes funcionais em homologação antes do go-live: 5 chamadas reais de cada função, todas passaram. Documentação técnica das funções entregue à Maria (1 página por função).

## Erros comuns
- **Não fazer cache.** API do ERP fica saturada, latência sobe, cliente reclama. *Corrige:* cache com TTL agressivo nos dados estáveis.
- **Sem idempotência.** Cliente reenvia "ok", agente cria pedido duplicado. *Corrige:* UUID antes do POST.
- **Token expirado em produção.** Agente trava de uma hora pra outra. *Corrige:* refresh automático cron.
- **Versão da API hardcoded em URL solta.** Mudança de versão quebra tudo. *Corrige:* variável de ambiente única para versão.
- **NF-e autônoma cedo demais.** Erro de NF-e gera multa fiscal. *Corrige:* humano confirma primeiros 60 dias.
- **WABA da Polaris com restrição da Meta não notada.** Templates rejeitados, agente não consegue iniciar conversa. *Corrige:* validar templates aprovados antes de marcar go-live.

## Checklist de saída
- [ ] ERP do cliente identificado e API documentada lida.
- [ ] Autenticação implementada (OAuth 2.0 ou app_key).
- [ ] Refresh automático configurado (se OAuth).
- [ ] Funções de busca cliente, produto, inadimplência e criar pedido implementadas e testadas.
- [ ] Cache configurado nos dados estáveis.
- [ ] Idempotência na criação de pedido.
- [ ] Pix funcionando (nativo ou Asaas).
- [ ] NF-e em modo "humano confirma" para os primeiros 60 dias.
- [ ] RIPD revisado para os dados que serão acessados.

## Vai além
- Manual canônico, sub-etapa A.1 e Princípio 7.
- Documentação oficial Bling v3, Omie REST, Conta Azul.
- Pesquisa E §9 — ecossistema brasileiro.
- Pesquisa A §6.2-6.4.
- Exercício prático: integrar o ERP do cliente do exercício no workflow, com 4 funções (cliente, produto, inadimplência, criar pedido) testadas com chamada real.
