---
nivel: 2
trilha: P
modulo: 4
titulo: Design do Fluxo com IA
tempo_leitura: 18
exit_criteria: aluno entrega diagrama de fluxo com IA marcando, para cada caixa, qual nível HITL inicial, quais gatilhos de escalonamento e qual fallback em caso de falha do agente.
---

# Design do Fluxo com IA

## O que você vai aprender
- Como decidir onde a IA entra no fluxo e onde o humano permanece.
- Os 4 níveis da matriz HITL e como aplicar cada um nas caixas do mapa.
- Como desenhar gatilhos de escalonamento que não sufoquem o operador.
- Como projetar fallback (o que acontece quando o agente trava ou erra).
- Por que se entra sempre no nível HITL mais conservador, mesmo que o agente "esteja bom".

## Por que isso importa
Princípio 5 do DPIA: risco define autonomia. NIST AI RMF, EU AI Act e a prática Nautis convergem em uma regra simples: começa-se com humano no loop e relaxa-se à medida que a evidência se acumula (Pesquisa B §8). Sem isso, o primeiro erro do agente vira incidente público — e em PME, vira fim de contrato.

Esta é a fase em que o implementador toma a decisão técnica que governa a operação: onde o humano permanece no caminho do agente. Decisão tomada em P, aprovada em I, executada em A. Errar aqui é caro: agente conservador demais frustra o cliente do cliente (todas as conversas escalam, perde-se a vantagem da automação); agente autônomo demais erra rápido e perde confiança em uma semana.

## O passo a passo

**1. Revisar os 4 níveis HITL.** (Detalhe completo em manual canônico, seção "Aprovação humana por nível de risco".)

- **Nível 1 — Assistir.** Agente lê e sugere internamente. Humano executa toda ação externa. Usado para ação sensível em primeira fase.
- **Nível 2 — Co-piloto.** Agente prepara ação externa, humano aprova clique-a-clique. Padrão de entrada para atendimento WhatsApp em primeiras semanas.
- **Nível 3 — Autopiloto com exceções.** Agente executa, pausa em gatilhos de risco. Padrão de regime maduro (após 4-8 semanas em nível 2).
- **Nível 4 — Autônomo com auditoria.** Agente executa tudo dentro do escopo. Humano audita por amostragem. Só após 90+ dias estáveis em nível 3, com métrica comprovada.

**2. Classificar cada caixa do mapa em nível HITL inicial.** Para o mapa do módulo 01, decidir caixa-a-caixa:
- Caixas verdes (automatizáveis): qual nível inicial (1, 2, 3, 4)?
- Caixas azuis (humanas): ficam em nível 0 (não-IA).

Regra geral para PME no primeiro deploy: caixas de comunicação com cliente final entram em nível 2; caixas de leitura/processamento interno podem entrar em nível 3; caixas de ação irreversível (criar pedido, emitir cobrança, alterar cadastro) começam em nível 2; ações puramente informativas (enviar lembrete, confirmar agendamento) podem começar em nível 3.

**3. Listar todos os gatilhos de escalonamento.** Já levantados no módulo 03; agora consolidar e classificar:
- *Gatilhos de valor:* "valor > R$ X".
- *Gatilhos de palavra-chave:* "reclamar", "cancelar", "Procon".
- *Gatilhos de cliente:* lista de VIPs, inadimplentes, blacklist.
- *Gatilhos de confiança:* confidence score < N%.
- *Gatilhos de horário:* fora do horário comercial em nível 2, deve não escalar em nível 3 com horário estendido.
- *Gatilhos técnicos:* integração falhou, ERP indisponível, mensagem em formato não suportado.

Cada gatilho carrega o nível de escalonamento (qual humano recebe; em que canal; com que SLA).

**4. Definir o fallback técnico.** O que acontece quando o agente literalmente falha (timeout, erro de API, LLM indisponível, ERP fora do ar)? Três caminhos:
- *Degrade graceful:* agente responde mensagem de espera padrão + log + alerta para implementador.
- *Roteamento humano:* a interação cai direto no canal do operador.
- *Retry com backoff:* para integrações que podem voltar (Bling caiu 30s, tenta de novo).

Cada caixa do mapa precisa ter um plano de fallback.

**5. Desenhar o fluxo final com IA.** Atualizar o mapa do módulo 01 com:
- Nível HITL inicial em cada caixa verde.
- Gatilhos marcados com símbolo (ex.: triângulo vermelho).
- Caminho de escalonamento.
- Fallback técnico.

**6. Calcular volume esperado de escalonamento.** Estimativa: que percentual das interações vai escalar para humano no primeiro mês? Em PME média:
- Nível 2 puro: 100% das interações têm gate humano (operador aprova tudo).
- Nível 3 com gatilhos típicos: 15-30% das interações escalam.
- Nível 3 maduro: 8-15%.
- Nível 4: 3-8% (auditoria, não escalonamento em tempo real).

Esse volume estimado precisa ser dimensionado contra a capacidade do champion/operador. Se escalonamento esperado é 30 interações/dia e o operador só tem 4 horas livres, o sistema vai colapsar — ajustar gatilhos ou ajustar reposicionamento.

**7. Planejar regra de subida de nível.** Princípio: não sobe por confiança subjetiva. Sobe quando:
- Métrica de qualidade ≥ 90% por 30 dias consecutivos.
- Zero incidente crítico no período.
- Champion concorda explicitamente.

Documentar a regra como compromisso por escrito do dono.

**8. Documentar a matriz HITL.** Tabela final, uma linha por caixa verde / por ação do agente:

| # | Ação | Nível inicial | Gatilhos | Escala para | SLA | Plano subida |
|---|------|---------------|----------|-------------|-----|--------------|
| A1 | Confirmar pedido < R$ 5k, cliente conhecido | 2 (co-piloto) | nenhum específico | Maria | 5 min | nível 3 após 30d com ≥90% acerto |
| A2 | Confirmar pedido > R$ 5k | 1 (assistir) | sempre | Sr. Roberto | 30 min | nível 2 após 60d |
| A3 | Responder FAQ (prazo, preço tabela) | 3 (autopiloto c/ exceção) | reclamação, valor incomum | Maria | 5 min | nível 4 após 90d |
| A4 | Lembrete de entrega no dia | 3 | nenhum específico | Maria (auditoria) | n/a | nível 4 após 30d |
| A5 | Cobrança Pix vencida | 1 | sempre escala | Sr. Roberto | 1h | nível 2 após 60d |

**9. Apresentar matriz para aprovação do dono em I.** Matriz aprovada por escrito é exit-gate de I. Em P, o aluno entrega rascunho da matriz; em I, o dono assina.

## Exemplo aplicado — Polaris Bebidas (Joinville/SC)

Fluxo final com IA, módulo "Atendimento de Pedido WhatsApp":

Caixas do mapa (do módulo 01) recebem classificação HITL:

| Caixa | Ação | Nível inicial |
|-------|------|----|
| P2 | Lê mensagem, classifica intenção | 3 |
| P3 | Transcreve áudio (Whisper) | 3 |
| P4 | Identifica cliente no Bling | 3 |
| P5 | Cadastra cliente novo | 2 (operador valida) |
| P6 | RT-12 PJ novo: pede alvará | 2 |
| P7→P8 | Detecta VIP, escala | 1 (Sr. Roberto) |
| P9 | Interpreta SKU + qtde | 2 (operador aprova lista) |
| P10→P11 | SKU ambíguo: pergunta cliente | 3 |
| P12→P13 | Bairro ausente: pergunta cliente | 3 |
| P14 | Aplica RT-07 (Glória same-day) | 3 |
| P17 | Forma de pagamento Pix/boleto | 2 |
| P17 fiado | Escala | 1 |
| P18 | Cria pedido Bling < R$ 5k | 2 |
| P18 | Cria pedido Bling > R$ 5k | 1 |
| P19 | Confirma para cliente | 2 (operador aprova mensagem final) |

**Gatilhos consolidados (15 gatilhos):**
- valor > R$ 5.000 (escala Sr. Roberto)
- cliente VIP "Bar do Zé" (escala Sr. Roberto sempre)
- cliente inadimplente, fatura > 30d (escala Sr. Roberto)
- pagamento "fiado" (escala Sr. Roberto)
- palavra "cancelar", "reclamar", "Procon", "devolver" (escala Maria)
- áudio > 3 min sem transcrição (escala Maria)
- confidence < 70% interpretação SKU (escala Maria)
- mensagem em formato imagem/sticker/localização (escala Maria, exceto imagem de alvará no fluxo RT-12)
- ERP Bling indisponível (fallback: agente avisa espera, retry 3x; se persiste, escala)
- LLM indisponível (fallback: mensagem-padrão, alerta implementador)
- fora do horário 06h-23h (fallback: agente responde "atenderemos amanhã às 06h")
- cliente novo PF (escala Maria para validação CPF)
- cliente novo PJ (RT-12: pede alvará, escala Maria)
- pedido inclui produto fora do catálogo Bling (escala Maria)
- cliente solicita "mudar endereço" (escala Maria)

**Volume estimado de escalonamento no primeiro mês:** ~22% das interações (≈ 105 escalonamentos/mês = ~5/dia). Compatível com a capacidade da Maria (que terá ~4h livres/dia no novo papel).

**Regra de subida documentada:** após 30 dias com qualidade ≥ 90% e zero incidente crítico, sobe ações A1, A3 e A4 para nível 3 / nível 4 (com aprovação Sr. Roberto e Maria).

**Fallback técnico:**
- Timeout LLM > 8s: mensagem "estou consultando, um momento" + retry 1x + se ainda falhar, escala.
- Bling 500: retry 3x com backoff 2s/5s/10s; se falhar, escala.
- WhatsApp Cloud API rate limit: queue interno do n8n com retry.

## Erros comuns
- **Começar em nível 3 ou 4 "para o cliente ver IA de verdade".** Primeiro erro vira incidente. *Corrige:* sempre o nível mais conservador inicial.
- **Não dimensionar volume de escalonamento.** Operador inundado, sistema colapsa em 1 semana. *Corrige:* calcular escalonamento esperado vs. capacidade do humano.
- **Esquecer fallback técnico.** Quando Bling cai, agente trava com cliente esperando. *Corrige:* fallback em cada caixa.
- **Subir nível por pressão do dono.** "Funciona, deixa autônomo." Primeira semana, erro de R$ 14k. *Corrige:* regra de subida por evidência, documentada.
- **Não consolidar gatilhos.** Gatilhos espalhados pelo playbook viram fonte de inconsistência no system prompt. *Corrige:* lista única em P.

## Checklist de saída
- [ ] Cada caixa verde do mapa classificada em nível HITL inicial.
- [ ] Gatilhos consolidados em lista única.
- [ ] Volume de escalonamento estimado e compatível com capacidade humana.
- [ ] Fallback técnico definido para cada modo de falha.
- [ ] Regra de subida de nível documentada.
- [ ] Rascunho da matriz HITL pronto para a fase I.

## Vai além
- Manual canônico, seção "Aprovação humana por nível de risco (4 níveis)" — referência canônica dos 4 níveis.
- Pesquisa B §8 — síntese NIST + EU AI Act + StackAI HITL.
- Princípio 5 do DPIA.
- Exercício prático: para o cliente do exercício, classificar todas as caixas do mapa em níveis HITL, calcular volume esperado de escalonamento e simular o impacto na agenda do champion.
