# Template — Mapa de Processos (Saída da Fase P)

> **Como usar.** O Mapa de Processos é parte do Pacote de Contexto entregue ao final da fase P. Documenta o processo do cliente em três camadas: (1) **fluxo explícito** — o que todo mundo descreveria se perguntado; (2) **regras de negócio formais** — políticas, tabelas, gatilhos objetivos; (3) **regras tácitas** — o que o operador faz na prática e ninguém escreveu. A terceira camada é o diferencial do DPIA; é onde projetos enterprise quebram. Este documento serve de fonte para escrita do system prompt em I.1 e para a base de conhecimento (RAG) em I.2. Formato sugerido: markdown versionado no Git ou Notion, com 4 a 12 páginas. Atualizar a cada manutenção quando processo mudar.

---

# Mapa de Processos — [NOME DO CLIENTE] — [PROCESSO]

**Cliente:** [...]
**Processo:** [...]
**Versão:** [n] — [data]
**Autor:** [implementador + operador co-autor]
**Próxima revisão prevista:** [data — em geral em cada manutenção mensal]

---

## 1. Visão geral

### 1.1 Para que serve este processo
[1 parágrafo curto: qual o objetivo de negócio do processo, qual valor entrega para o cliente final, qual receita ou economia depende dele.]

### 1.2 Atores envolvidos
| Papel | Pessoa atual | Decisões que toma | Sistemas que opera |
|-------|--------------|----------------------|---------------------|
| [...] | [...] | [...] | [...] |

### 1.3 Sistemas envolvidos
| Sistema | Função | Acesso por |
|---------|--------|------------|
| WhatsApp Business / WABA | canal de entrada | [pessoa(s)] |
| [ERP — Bling/Omie/etc] | registro do pedido | [pessoa(s)] |
| [outros] | [...] | [...] |

### 1.4 Frequência e volumes
[Diário / semanal / mensal — números do baseline.]

---

## 2. Fluxo explícito (o que todo mundo descreveria)

### 2.1 Diagrama textual

```
[Gatilho de entrada]
        │
        ▼
[Passo 1 — quem faz, o que faz]
        │
        ▼
[Decisão A] ──── não ────▶ [caminho alternativo]
        │
       sim
        │
        ▼
[Passo 2 — ...]
        │
        ▼
[Saída — entregável final]
```

### 2.2 Passo a passo detalhado

**Passo 1 — [Nome do passo].**
- Quem: [...]
- O que faz: [...]
- Onde registra: [...]
- Tempo típico: [...]
- Critério de qualidade: [...]

**Passo 2 — [Nome do passo].**
- Quem: [...]
- O que faz: [...]
- (etc)

---

## 3. Regras de negócio formais

Regras objetivas, que o dono articularia se perguntado. Numeradas para serem referenciáveis no system prompt do agente.

**RN-01:** [Enunciado da regra]. *Origem:* política da empresa / tabela / contrato. *Aplicável quando:* [...].

**RN-02:** [...]

**RN-03:** [...]

[Use quantas forem necessárias. Em PME típica, de 8 a 30 regras formais.]

---

## 4. Regras tácitas (capturadas no walk-the-floor)

**Esta é a seção crítica.** Cada regra tácita é uma exceção ou critério que o operador aplica e que ninguém escreveu antes. Se faltar uma, o agente vai falhar publicamente.

**RT-01:** [Enunciado literal, do jeito que o operador descreveu].
- *Quando aplicar:* [...]
- *Por que existe:* [...]
- *Quem decide:* [...]
- *Risco se ignorar:* [...]

**RT-02:** [...]

[Em PME típica, de 5 a 20 regras tácitas — quanto mais o operador articular, melhor o agente.]

---

## 5. Vocabulário do cliente (glossário operacional)

| Termo | O que o cliente quer dizer | Mapeamento técnico / no ERP |
|-------|------------------------------|------------------------------|
| [...] | [...] | [...] |

[Exemplo: "encaixe" em clínica = "agendamento de última hora aproveitando vaga liberada"; "bonificação" em distribuidora = "produto grátis vinculado a volume comprado"; "diferimento" em contábil = "imposto adiado conforme regime tributário do cliente final".]

---

## 6. Casos de uso reais (extraídos do histórico)

**Caso 1 — [tipo de interação, ex: "cliente pede preço de produto novo"]**

- Entrada (mensagem real anonimizada): "Bom dia, vocês têm aquela cerveja Stout da Schornstein?"
- Resposta ideal (validada com operador): "[texto exato que o operador daria]"
- Regras aplicadas: RN-04, RT-02

**Caso 2 — [outro tipo]**
[...]

[5 a 10 casos por módulo. Estes casos viram few-shot examples na fase I.2.]

---

## 7. Casos-armadilha

Situações onde o agente provavelmente vai errar se não for instruído. Documente o quê e o por quê.

**CA-01:** [Descrição]. *Por que armadilha:* [...]. *Conduta esperada:* escalar para humano / responder de jeito X / pausar e perguntar.

**CA-02:** [...]

---

## 8. Plano de integrações

| # | Sistema | Direção | Endpoint / método | Autenticação | Escopo | Owner do acesso |
|---|---------|---------|-------------------|--------------|--------|------------------|
| 1 | Bling | bidirecional | REST /pedidos | OAuth2 + token | leitura+escrita pedidos | [responsável no cliente] |
| 2 | WABA | entrada+saída | Webhook + Send Message | Meta token | mensagens, mídia | [responsável] |
| 3 | Supabase pgvector | escrita | client SDK | service key | base de conhecimento | implementador |
| 4 | [...] | [...] | [...] | [...] | [...] | [...] |

**Status das integrações:**
- [ ] Credencial provisionada
- [ ] Teste de chamada real bem-sucedido
- [ ] Rate limit conhecido e documentado
- [ ] Plano de fallback se a integração cair

---

## 9. Restrições e dependências

- **LGPD:** [dados pessoais tocados, base legal, regime aplicável — referência ao RIPD enxuto].
- **Horário comercial:** [quando o agente opera, quando dorme, quem cobre madrugada].
- **Idioma:** PT-BR. Não responder em outros idiomas mesmo se cliente final escrever em outro idioma — escalar.
- **Limites de valor:** acima de R$ [X], escalar para humano sempre.
- **Cliente VIP:** lista de [N] clientes tratados sempre por humano (anexar lista).

---

# Exemplo preenchido — Polaris Bebidas Ltda — Processo "Atendimento e tomada de pedido via WhatsApp"

## 1.1 Para que serve este processo
Atender pedidos e dúvidas que entram pelo WhatsApp Business da distribuidora, capturar o pedido no Bling e dar retorno de prazo de entrega ao cliente final. É a porta de entrada de aproximadamente 78% da receita mensal — quando trava, perde-se pedido para o concorrente em até 30 min.

## 1.2 Atores envolvidos
| Papel | Pessoa | Decisões | Sistemas |
|-------|--------|----------|----------|
| Atendente principal | Cristiane Bertoldi | preço, prazo, desconto até 5% | WhatsApp Business, Bling |
| Atendente apoio | Felipe Schmitz | preço, prazo (sem desconto) | WhatsApp Business, Bling |
| Dono | Reinaldo K. Salvi | desconto acima de 5%, exceções de prazo | Bling |

## 2 Fluxo (resumido)
Cliente envia mensagem → atendente identifica tipo (preço / status / pedido novo / reclamação) → consulta tabela ou Bling → responde → se for pedido novo, registra no Bling e devolve número de pedido + prazo de entrega.

## 3 Regras formais (extrato)
- **RN-01:** Pedido mínimo: R$ 250 para entrega na Grande Joinville; R$ 500 para fora.
- **RN-02:** Desconto-padrão: 3% à vista (Pix), 0% em boleto a 14 dias.
- **RN-03:** Frete grátis acima de R$ 800 dentro da Grande Joinville.
- **RN-04:** Cerveja sem álcool sempre confirmar marca + tipo (Heineken 0,0 vs Brahma Zero etc).

## 4 Regras tácitas
- **RT-01:** Cliente "Casa do Carlos" (CNPJ na lista anexa) recebe 7% de desconto fixo sem perguntar. *Origem:* acordo verbal de 6 anos entre o Reinaldo e o Carlos. *Risco se ignorar:* perde-se o cliente em 1 pedido.
- **RT-02:** Para os 28 clientes do "roteiro de quinta" (lista anexa), liberar entrega na sexta mesmo quando o pedido vem fora do prazo padrão. *Origem:* logística da empresa só faz Itajaí na quinta.
- **RT-03:** Quando o cliente pergunta "tem [marca premium]?" e não tem, oferecer marca equivalente mas sempre informar primeiro a falta — nunca empurrar substituto sem aviso.
- **RT-04:** Reclamação sobre validade de produto sempre escalar para Reinaldo, mesmo se for R$ 30. Reposição imediata + verificação na CD.

## 5 Glossário
| Termo | Significado | No Bling |
|-------|-------------|----------|
| "bonificação" | produto grátis vinculado a volume | item com preço R$ 0,01 + observação |
| "balcão" | venda pessoal na sede (não delivery) | natureza de operação "Venda" |
| "roteiro" | região fechada com dia fixo de entrega | grupo de clientes no Bling |

## 6 Casos (extrato — 8 casos totais no documento real)

**Caso 1 — Pergunta de preço de item de tabela**
- Entrada: "Quanto a Heineken 600 com 12 garrafas?"
- Resposta ideal: "Bom dia! A caixa com 12 unidades de Heineken Long Neck 330ml está R$ 142,80. Para 600ml chumbinho temos a caixa com 24 a R$ 178. Posso confirmar qual prefere?"
- Regras: RN-04 (confirmar tipo), tabela de preços.

**Caso 2 — Cliente da Casa do Carlos pede pedido**
- Entrada: "Bom dia. 30 caixas Brahma 269, 20 caixas Skol 269, 10 caixas Stella 330."
- Resposta ideal: "Bom dia, Carlos! Anotando: 30cx Brahma 269 + 20cx Skol 269 + 10cx Stella 330 long neck. Com seu desconto fica R$ X. Entrega quinta? Confirma?"
- Regras: RT-01 (7% automático), RT-02 (quinta), RN-04 não aplicável aqui.

## 8 Integrações
| # | Sistema | Direção | Auth | Status |
|---|---------|---------|------|--------|
| 1 | Bling | RW | OAuth2 token | OK, testado 19/maio |
| 2 | WABA | RW | Meta token + webhook n8n | OK, testado 19/maio |
| 3 | Supabase pgvector | W | service key | OK |
| 4 | Asaas Pix | W (links de cobrança) | API key | OK |

## 9 Restrições
- LGPD: trata CPF (em notas fiscais) e nome+telefone (cadastro). Regime simplificado ANPD aplicável (microempresa).
- Horário: agente responde 6h–22h. Fora desse intervalo: mensagem automática + fila para 1ª resposta humana às 6h.
- Limites: pedido acima de R$ 5.000 ou desconto acima de 5% → escala para Reinaldo.

---

## Dicas de uso

- **Co-escreva com o operador.** O operador é coautor desta página, não revisor. Sente do lado dele para escrever as regras tácitas, em PT-BR direto, do jeito que ele falaria.
- **Numere tudo (RN-XX, RT-XX, CA-XX).** Numeração permite referenciar no system prompt e auditar depois.
- **Anexe listas concretas.** "Clientes VIP", "roteiro de quinta", "produtos em falta" — sempre como anexo versionado, não embarcado no corpo.
- **Atualize a cada manutenção mensal.** Versão e data no topo. Documento congelado = projeto morrendo (Princípio 6).
- **Não invente regra para preencher.** Se o operador não usa regra ali, deixe em branco; volte depois com mais observação.
