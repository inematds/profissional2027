---
nivel: 2
trilha: I
modulo: 5
titulo: Homologação com Cliente
tempo_leitura: 16
exit_criteria: aluno conduz reunião de homologação com dono + champion, coleta as 5 assinaturas do exit-gate de I, e fecha agenda do go-live da fase A.
---

# O que você vai aprender
- Como conduzir a reunião de homologação com cliente — apresentação do agente em ambiente de teste com casos reais.
- Como apresentar o relatório de teste sem soar defensivo sobre erros.
- Como treinar o dono no básico (não é o operador — o dono recebe overview, o operador será treinado em A).
- Como colher as 5 assinaturas do exit-gate de I.
- Como combinar a agenda do go-live e do job shadowing.

## Por que isso importa
A homologação é a transição entre construir o agente e operá-lo com cliente real. Bem conduzida, ela cria confiança e prepara o cliente para a fase A com expectativa calibrada. Mal conduzida, ela gera ou euforia ("perfeito, pode ir tudo no autônomo") ou desconfiança ("não funciona direito, vou desistir"). O implementador conduz para o meio termo: "funciona dentro do escopo, com limites conhecidos, com humano no loop, com expectativa calibrada para evolução em 4-12 semanas".

Pesquisa G §3.3: o "champion interno declarado confortável para operar" é critério de exit-gate. Homologação é onde se confirma isso.

## O passo a passo

**1. Preparar a reunião (60-90 min).** Materiais:
- Agente em ambiente de homologação (acesso testado, sem surpresa técnica).
- Bateria de 5-8 casos para demo ao vivo (mix happy path + 2 exceções + 1 escalonamento + 1 erro do agente que será mostrado intencionalmente).
- Relatório de teste impresso (saída do módulo 03 + 04).
- Playbook v0.6 impresso.
- Matriz HITL para assinatura.
- Plano de rollback impresso, com instruções da página de pausa para Maria.
- Acordo de homologação impresso (declaração de que cliente aprovou o agente para entrar em A).

**2. Estrutura da reunião (75 min).**

- *0-5 min — Contrato.* "Hoje vamos rodar o agente em frente a vocês, mostrar onde ele acerta, onde ele erra propositalmente, e o que vai acontecer na próxima fase A. Ao final, se vocês aprovarem, vocês assinam 5 itens."
- *5-30 min — Demo ao vivo.* Aluno digita no WhatsApp staging cada caso, mostra resposta do agente, comenta a decisão tomada. Inclui 1 caso de escalonamento bem-sucedido (mostra que Maria recebeu alerta) e 1 caso onde o agente sabidamente erra (demonstra o gatilho de confidence, demonstra o plano de rollback).
- *30-45 min — Relatório de teste.* Apresenta taxa de acerto, erros remanescentes documentados, expectativa de taxa em produção. Não esconde erros — apresenta com naturalidade técnica.
- *45-55 min — Matriz HITL.* Cada linha da matriz com nível inicial. Reforça a regra de subida. Dono assina cada linha (ou um único bloco de aprovação).
- *55-65 min — Treinamento básico do dono.* O dono não vai operar, mas precisa saber:
  - Como Maria pausa o agente em emergência.
  - Como Maria recebe escalonamento.
  - O que esperar nas primeiras semanas (taxa de override 15-25% nas semanas 1-2, decrescente).
  - Quando o implementador retorna (job shadowing semana 1, depois remoto, depois visita mensal).
- *65-75 min — Exit-gate de I + agenda da fase A.* 5 itens assinados. Agenda do go-live combinada com Maria presente.

**3. Os 5 itens do exit-gate de I.**

- [ ] Agente passa em ≥ 85% dos casos de homologação (taxa medida).
- [ ] Matriz HITL assinada pelo dono.
- [ ] Playbook escrito e revisado com o operador.
- [ ] Champion interno declarou-se confortável para operar (entrevista curta de 15 min com pergunta direta: "você está pronto para receber o agente na próxima semana?").
- [ ] Plano de rollback documentado (como desligar o agente em 15 minutos se algo der errado).

A entrevista de 15 min com o champion é separada da reunião principal — em geral logo após a reunião, com aluno e champion sozinhos. Pergunta direta. Resposta sim ou não, sem "talvez".

**4. Quando o champion não está pronto.** Se Maria responde "tenho dúvida" ou "preciso de mais treino", **não** avança para A. Aluno volta com 1-2 sessões adicionais de simulação, depois refaz a entrevista. Pulando esse gate, sabotagem em produção em < 5 dias.

**5. Quando a matriz HITL é alvo de pressão para subir.** Dono pode pedir "começa direto no nível 3, vai mais rápido". Resposta: "o método é começar no nível 2 e subir em 4-6 semanas com evidência. A diferença é que no nível 2, se o agente errar, a Maria pega antes do cliente sofrer. No nível 3, o cliente sofre primeiro. Por R$ 500 a R$ 2.000 de hora-operador a mais nas primeiras semanas, evitamos um incidente com cliente." Em geral o dono recua.

**6. Quando a homologação resulta em "não aprovado".** Acontece. Possíveis causas:
- Dono mudou de ideia sobre escopo.
- Maria pede mais tempo.
- Regra crítica não estava no Pacote (descoberta agora).
- Performance do agente decepcionou o dono apesar dos 88%.

Tratamento:
- Não forçar aprovação.
- Documentar o motivo.
- Combinar próxima reunião em 7-14 dias.
- Decidir entre: ajustar I, voltar para P, ou redefinir escopo.

**7. Agenda de fase A.** Antes de sair da reunião:
- Data do go-live (em geral 5 a 7 dias após homologação aprovada).
- Janela de job shadowing (3 a 5 dias presenciais, com aluno na empresa).
- Marcos de medição: 30, 60, 90 dias.
- Reunião de assinatura do contrato de manutenção: marco 30 dias.

## Exemplo aplicado — Polaris Bebidas (Joinville/SC)

Reunião de homologação, quarta-feira 14h, sala da diretoria. Sr. Roberto + Maria + Sr. Cláudio.

Aluno demonstra 7 casos ao vivo no WhatsApp de homologação:
1. Cliente cadastrado, 5 caixas Skol, bairro Glória, 11h da manhã. Agente confirma same-day. ✓
2. Cliente novo PJ, 3 fardos água. Agente pede foto do alvará (RT-12). ✓
3. Cliente do Bar do Zé. Agente escala imediatamente. ✓
4. Áudio de 25s do cliente. Agente transcreve + responde. ✓
5. Cliente fala "quero cancelar pedido de ontem". Agente escala (palavra-chave). ✓
6. Cliente pede 38 caixas (acima do limite same-day em Glória). Agente pergunta se entrega pode ser próximo dia útil. ✓
7. Caso intencional de erro: cliente pede "duas dúzias da gelada de sempre" (ambíguo). Agente, com confidence 0.62, escala para Maria pedir esclarecimento. Maria recebe alerta no Slack. ✓ — demonstra gatilho funcionando.

Relatório de teste apresentado: 88% de acerto na bateria de 42 casos; 5 erros remanescentes documentados.

Matriz HITL revisada linha a linha. Sr. Roberto pediu uma alteração: ação A4 (lembrete de entrega) começa em nível 2 (não 3) nos primeiros 30 dias. Aluno aprova; já estava previsto na conversa de P.

Treinamento básico do Sr. Roberto: 10 minutos. Mostra a página de pausa do agente, mostra o painel simples de Supabase onde ele pode ver últimas 20 interações.

Entrevista com Maria (15 min, sala separada):
- "Você está pronta para receber o agente na próxima semana?" — *"Acho que sim, com você do lado nos primeiros dias."*
- "O que ainda te preocupa?" — *"Áudio longo. Tenho medo do agente errar e o cliente reclamar."*
- Aluno reforça: "Nos primeiros 3 dias eu estou aqui presencial. Áudio longo, agente escala para você direto."
- "OK, tô pronta."

Exit-gate de I:
- [x] Taxa ≥ 85% (88%).
- [x] Matriz HITL assinada (Sr. Roberto).
- [x] Playbook v0.6 assinado pela Maria.
- [x] Maria declarou-se confortável.
- [x] Plano de rollback documentado e testado.

Agenda fase A combinada:
- Go-live: segunda-feira 09h, 8 dias após a reunião.
- Job shadowing: segunda-quarta da semana de go-live (3 dias presenciais).
- Marco 30 dias: apresentação dia 30/06.
- Reunião de contrato de manutenção: 30/06, com primeiro relatório.

## Erros comuns
- **Esconder erros do agente na demo.** Cliente descobre depois e perde confiança. *Corrige:* mostrar erro intencionalmente, demonstrar como o sistema lida.
- **Pular entrevista com champion.** Maria entra em A sem estar pronta, sabota. *Corrige:* entrevista obrigatória, 15 min, pergunta direta.
- **Aceitar pressão para subir nível HITL.** *Corrige:* explicar custo de incidente, manter conservador.
- **Não testar plano de rollback na frente do cliente.** Cliente não sabe que existe. *Corrige:* incluir teste de pausa na demo.
- **Não combinar agenda da fase A.** Atrasa go-live em 1-2 semanas. *Corrige:* agenda definida na reunião.

## Checklist de saída
- [ ] Reunião de homologação conduzida com dono + champion.
- [ ] Demo ao vivo de 5-8 casos, incluindo erro intencional + escalonamento + rollback.
- [ ] Relatório de teste apresentado sem esconder erros.
- [ ] Matriz HITL assinada.
- [ ] Entrevista de 15 min com champion realizada.
- [ ] 5 itens do exit-gate marcados.
- [ ] Agenda da fase A combinada.

## Vai além
- Manual canônico, exit-gate de I e "Erros comuns".
- Pesquisa G §3.3.
- Princípios 4 e 5 do DPIA.
- Exercício prático: simular reunião de homologação completa com colega de turma assumindo papel de dono + outro de champion. Conduzir os 75 min cronometrados.
