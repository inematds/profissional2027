---
nivel: 2
trilha: P
modulo: 1
titulo: Mapeamento de Processos
tempo_leitura: 18
exit_criteria: aluno entrega mapa do processo as-is do cliente do exercício, com swimlanes, caixas numeradas, pontos de decisão e marcação dos passos candidatos a automação.
---

# Mapeamento de Processos

## O que você vai aprender
- Como desenhar o processo as-is com notação simples adequada à PME — caixas e setas, sem BPMN completo.
- Qual a granularidade certa: nem fluxo de alto nível inútil, nem detalhamento de manual.
- Como usar swimlanes para deixar claro quem faz o quê.
- Como identificar pontos de decisão, exceções e modos de falha no mapa.
- Como marcar no mapa quais passos são candidatos à automação e quais permanecem humanos.

## Por que isso importa
Mapa de processo é a base concreta sobre a qual P, I e A são construídos. Sem ele, o agente da fase I não tem onde se ancorar, e o deploy da fase A vira surpresa. Pesquisa B §4 mostra que processos não mapeados são a primeira causa de retrabalho no projeto. Em PME, o processo nunca está documentado — o aluno mapeia.

Mapear também valida com o dono que o implementador entendeu o negócio. Quando o dono vê o mapa, ele aponta os erros do próprio mapa — e esse apontamento é o que descobre regras tácitas adicionais. O mapa é instrumento de descoberta, não só de documentação.

## O passo a passo

**1. Escolher notação simples.** Para PME, BPMN 2.0 completo é overkill. Notação recomendada:
- Caixa retangular: tarefa ou ação.
- Caixa losango: decisão (sim/não, ramificação).
- Seta: fluxo.
- Caixa cilindro: sistema (Bling, WhatsApp, Supabase).
- Cor para humano vs. automatizável (azul vs. verde, por exemplo).
- Swimlane horizontal para cada ator (cliente / vendedor / dono / sistema).

Ferramentas: Excalidraw, Whimsical, Miro, Figma, ou até PowerPoint. Não usar BPMN profissional — assusta o dono.

**2. Listar os atores.** Antes de desenhar, listar:
- Atores externos (cliente final, fornecedor, banco).
- Atores internos (dono, operador, supervisor).
- Sistemas (ERP, WhatsApp, calendário, planilha).
- Eventos externos que disparam o processo.

Cada um vira uma swimlane no mapa.

**3. Desenhar o happy path.** Primeiro versão: o caminho ideal, quando tudo dá certo. 8 a 15 caixas. Numerar cada uma (P1, P2, ...). Cada caixa deve responder: quem faz, com qual ferramenta, em quanto tempo médio.

**4. Acrescentar pontos de decisão.** Onde o processo se ramifica? "Cliente é PJ?", "Pedido > R$ 5k?", "Cliente VIP?". Cada decisão vira um losango com saídas nomeadas.

**5. Acrescentar exceções e modos de falha.** O que pode dar errado em cada caixa? Exceção mais comum: cliente esquece de informar bairro, operador precisa perguntar. Cada exceção vira ramificação alternativa, em geral com loop de retorno.

**6. Marcar swimlane do tempo.** Anotar tempo médio em cada caixa (vem do baseline da fase D). Calcular tempo total do happy path e tempo do caminho-com-exceções. Diferença mostra onde o atrito está concentrado.

**7. Marcar passos candidatos à automação.** Critério: passo automatizável é aquele que (i) é determinístico ou tem padrão claro, (ii) acontece com frequência, (iii) consome tempo humano significativo, (iv) tem risco controlável. Cor verde no mapa. Passos que ficam humanos (decisão, exceção sensível, escalonamento) ficam em azul.

**8. Marcar gatilhos de HITL.** Mesmo nos passos verdes, indicar onde o humano precisa entrar — em qual condição o agente para e escala. Esses gatilhos virão para a matriz HITL na fase I.

**9. Validar com o operador.** Mostrar o mapa para o operador (em geral o champion identificado em D). Pedir: "isso bate com o que você faz?". Ele vai apontar 3 a 7 coisas que faltam. Cada apontamento vira correção do mapa ou regra tácita nova.

**10. Validar com o dono.** Mostrar versão revisada para o dono. Ele vai apontar 1 a 3 coisas adicionais. Validar com ambos é o que garante que o mapa reflete a realidade, não a fantasia de nenhum dos dois.

**11. Versionar o mapa.** Mapa fica no Pacote de Contexto, em formato editável. A cada manutenção (loop de Acompanhamento), o mapa é atualizado. Princípio 6 do DPIA: documentação viva ou nada.

## Exemplo aplicado — Polaris Bebidas (Joinville/SC)

Mapa do processo "Atendimento de pedido WhatsApp" — versão validada com Sr. Roberto e Maria:

```
SWIMLANE: CLIENTE
P1. Cliente envia mensagem WhatsApp (texto/áudio/imagem)
            │
SWIMLANE: VENDEDOR INTERNO (Maria/João)
P2. Lê mensagem ──► Decide: é pedido novo ou continuação?
            │
P3. (se áudio) Transcreve áudio mentalmente
            │
P4. Identifica cliente no Bling por nome ou número
            │
P5. Decisão: cliente cadastrado?
   ├─ sim ──► P7
   └─ não ──► P6. Coleta dados (CPF/CNPJ, endereço, bairro) ──► cadastra no Bling ──► P7
            │
P7. Decisão: cliente VIP (Bar do Zé)?
   ├─ sim ──► P8. Escala para Sr. Roberto (fila ~18min)
   └─ não ──► P9
            │
P9. Interpreta produtos (SKU + quantidade)
            │
P10. Decisão: SKU ambíguo no Bling?
   ├─ sim ──► P11. Pergunta cliente para esclarecer
   └─ não ──► P12
            │
P12. Decisão: bairro informado?
   ├─ não ──► P13. Pergunta cliente o bairro
   └─ sim ──► P14
            │
P14. Decisão: bairro = Glória E horário < 15h?
   ├─ sim ──► P15. Marca entrega same-day
   └─ não ──► P16. Marca entrega próximo dia útil
            │
P17. Decisão: forma de pagamento?
   ├─ Pix ──► gera cobrança
   ├─ Boleto ──► gera boleto Bling
   ├─ Fiado ──► escala para Sr. Roberto
            │
P18. Cria pedido no Bling com SKU+qtde+endereço+entrega+pagamento
            │
P19. Confirma para o cliente: "Pedido R$X confirmado, entrega Y"
            │
P20. Encerra atendimento

SWIMLANE: SISTEMA
- Bling (cadastro, pedidos, estoque)
- WhatsApp Cloud API (canal)
```

**Tempo médio do happy path:** 12 min (cliente conhecido, sem áudio, SKU claro, bairro informado, pagamento Pix).
**Tempo médio com exceções:** 47 min (o que o baseline mediu).

**Passos candidatos a automação (verde):** P2, P3 (transcrição via Whisper), P4, P9, P11, P12, P13, P14, P15, P16, P17 (Pix/boleto), P18, P19.

**Passos que permanecem humanos (azul):** P8 (cliente VIP escala), P17 (Fiado escala), qualquer exceção não prevista.

**Gatilhos de HITL identificados:** pedido > R$ 5.000, cliente VIP, pagamento fiado, palavra-chave "reclamação"/"cancelar", confidence score baixo na interpretação de SKU.

Maria, ao revisar o mapa, apontou:
- "Quando o cliente é restaurante novo, eu peço foto do alvará. Não está no mapa."  → RT-08 adicionada.
- "Se o pedido for entrega ao mesmo cliente duas vezes na semana, dou desconto 5% na segunda."  → RT-09 adicionada.

Sr. Roberto apontou:
- "No fim do mês, todo cliente recebe oferta de produto promocional. Maria não faz isso porque ela não acompanha. Eu queria que o agente fizesse."  → escopo registrado mas postergado para fase futura.

## Erros comuns
- **Usar BPMN profissional.** O dono não lê, perde-se uma hora explicando notação. *Corrige:* caixas + setas + losangos.
- **Mapear em alto nível demais.** Mapa de 5 caixas com "atendimento → confirmação → entrega" não serve para nada. *Corrige:* 8 a 15 caixas no happy path, mais ramificações.
- **Mapear em detalhe demais.** 80 caixas para um processo de WhatsApp vira manual de operação inútil. *Corrige:* granularidade no nível do passo cronometrável.
- **Pular validação com o operador.** Mapa fica fantasia do dono. *Corrige:* validação obrigatória, sempre.
- **Não marcar gatilhos de HITL.** Mapa fica completo mas a fase I começa do zero na matriz. *Corrige:* gatilhos identificados desde P.

## Checklist de saída
- [ ] Mapa do processo as-is em ferramenta editável, com swimlanes.
- [ ] 8 a 15 caixas no happy path, numeradas.
- [ ] Pontos de decisão com losangos e saídas nomeadas.
- [ ] Exceções e modos de falha marcados.
- [ ] Tempo médio por caixa anotado (vindo do baseline).
- [ ] Passos verdes (automatizáveis) e azuis (humanos) coloridos.
- [ ] Gatilhos de HITL identificados.
- [ ] Mapa validado com operador e dono.

## Vai além
- `metodo/templates/template-mapa-processos.md` — estrutura oficial.
- Manual canônico, sub-etapa P.1.
- Pesquisa E §10 — stack composable e mapeamento como insumo de integração.
- Exercício prático: mapear o processo escolhido na trilha D para o cliente do exercício, em ferramenta editável, e validar com pelo menos um operador real (ou simulado).
