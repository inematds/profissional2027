---
nivel: 3
bloco: C
modulo: 5
titulo: Acompanhamento mensal — ciclo de manutenção, métricas, recuperação
tempo_leitura: 18
exit_criteria: Aluno consegue executar visita mensal completa com relatório de 1 página, reconhecer sinais de queda e ativar plano de recuperação
---

# Acompanhamento mensal — ciclo de manutenção, métricas, recuperação

## O que você vai aprender

- A estrutura da visita mensal: o que entregar, o que medir, o que decidir.
- As 4 métricas obrigatórias (processo, adoção, qualidade, financeira).
- Como produzir o relatório mensal de 1 página que sustenta o contrato.
- 5 sinais precoces de queda de adoção e como agir em cada um.
- Diagnóstico por ADKAR quando a adoção cai.
- Os 4 níveis de recuperação, do menos ao mais intensivo.

## Por que isso importa

Pesquisa G §9, §10 e DPIA canônico (seção Acompanhamento) estabelecem que o loop mensal é o motor financeiro do aluno. Sem ele, projeto entregue vira projeto abandonado em 6 meses. Com ele, projeto entregue vira receita recorrente plurianual.

Pesquisa G §10.4 (Prosci): 43% das falhas de adoção vêm de patrocínio executivo insuficiente. Acompanhamento mensal mantém o dono como sponsor ativo. Sem isso, o agente vai morrendo silenciosamente — e quando o aluno percebe, é tarde.

DPIA canônico, seção "sinais precoces de queda de adoção": uso cai 20% mês a mês, champion sai, override sobe acima de 30%, playbook não atualizado por 60 dias. Cada um desses sinais é gatilho — não notícia para arquivo.

## Conteúdo principal

### A visita mensal — estrutura padrão (2–3 horas)

**Bloco 0 — Preparação (1 dia antes, 30 min do aluno)**

- Puxar dados de adoção e qualidade do mês.
- Calcular as 4 métricas obrigatórias.
- Comparar contra baseline assinado em D e contra mês anterior.
- Identificar 1 conquista do mês + 1 problema do mês.
- Escrever relatório de 1 página em formato padrão.

**Bloco 1 — Reunião com o dono (60 min)**

- Min 0–10: relatório de 1 página apresentado em 10 min. Foco em números, não em narrativa.
- Min 10–30: discussão do principal problema identificado e plano de ação.
- Min 30–45: aprovação de mudanças estruturais se houver (subida de nível HITL, ajuste de tier, novo aditivo).
- Min 45–60: planejamento do mês seguinte (1 meta-número, 1 ajuste no playbook, possíveis novos vídeos).

**Bloco 2 — Reunião com o champion (45 min)**

- Min 0–15: revisão do log de erros do mês com champion.
- Min 15–30: ajustes no playbook que serão feitos hoje.
- Min 30–45: decisão do tema do micro-vídeo do mês (gravar agora se possível).

**Bloco 3 — Tempo no posto operacional (30–60 min)**

- Aluno senta com o operador principal, vê o sistema rodando 30–60 min em horário real.
- Coleta observações que não aparecem no log.
- Conversa breve sobre eventuais atritos que o operador não verbalizou.

**Bloco 4 — Atualizações executadas no dia (variável, dentro do banco de horas)**

- Ajustes de prompt definidos com champion.
- Cheatsheets atualizados, impressos, plastificados.
- Micro-vídeo gravado (se tier Intermediário+).
- Backup da biblioteca.

### As 4 métricas obrigatórias

Pesquisa G §9.2 e DPIA canônico, seção "Métricas obrigatórias por mês":

**Métrica 1 — Processo.** Número atual vs. baseline assinado em D. Sempre na mesma unidade. Exemplo Polaris: tempo médio de resposta no WhatsApp. Baseline (D): 47 min. Mês 1: 12 min. Mês 6: 6 min. Mês 12: 5 min. Tendência: estável ou melhorando? Se piora, gatilho de investigação.

**Métrica 2 — Adoção.** % das interações no canal/processo que passou pelo agente vs. canal antigo. Indicador antecedente. Exemplo Polaris: % de mensagens recebidas pelo WABA do agente vs. WhatsApp pessoal das atendentes. Mês 1: 78%. Mês 6: 92%. Queda significa que a equipe voltou ao canal antigo — alerta vermelho.

**Métrica 3 — Qualidade.** % de respostas que precisaram de override humano. Quando sobe, sinal de degradação do prompt ou mudança no negócio. Pesquisa G §9.2 estabelece alerta em > 30%. Exemplo Polaris: mês 1 com 35% (esperado nas primeiras semanas), mês 3 com 18%, mês 8 com 12%. Subida indica problema.

**Métrica 4 — Financeira.** Payback acumulado e ROI mês a mês. Exemplo Polaris: investimento de R$ 22.000 (projeto) + R$ 2.500/mês (manutenção). Ganho mensal: R$ 5.640. Payback: 7,2 meses. ROI 12 meses: 207%. Esse número é o que sustenta a renovação do contrato.

### O relatório de 1 página

Template padrão, sempre o mesmo formato:

```
RELATÓRIO MENSAL — POLARIS BEBIDAS — MÊS 06 — JUN/2026

EM 3 NÚMEROS:
- Tempo médio de resposta: 5 min  (baseline D: 47 min · meta: ≤ 5)
- Adoção do canal agente: 92%  (mês anterior: 89% · meta: ≥ 85%)
- Override humano: 12%  (mês anterior: 14% · meta: ≤ 20%)

PAYBACK ACUMULADO: R$ 33.840 ganho / R$ 22.000 investimento = atingido no mês 4

CONQUISTA DO MÊS:
- 0 pedidos perdidos por demora documentados.
- Júlia (nova) treinada pelo champion, opera autonomamente desde dia 15.

PROBLEMA DO MÊS:
- 4 casos de cliente pedindo orçamento de evento (>R$ 8.000), agente escalou corretamente mas tempo de resposta foi alto (35 min em média).
- Plano: criar fluxo específico para "orçamento de evento" com pré-resposta automática + escala. Implementação no mês 7.

PRÓXIMO MÊS:
- Meta de processo: manter ≤ 5 min.
- Ajuste estrutural: novo fluxo de orçamento de evento.
- Tema do micro-vídeo: "Como atender pedido de evento corporativo".
- Decisão pendente para o dono: subir HITL nível 3 em FAQ pura? Sim/Não.

Assinatura: [Aluno] · Data: [data]
```

1 página. Sempre. PDF entregue por e-mail antes da reunião e impresso para a reunião.

### Os 5 sinais precoces de queda de adoção

DPIA canônico, seção Acompanhamento. Cada sinal é gatilho de ação imediata:

**Sinal 1 — Uso do agente cai 20% mês a mês em volume.** Ação: visita extraordinária em até 14 dias.

**Sinal 2 — Champion deixa a empresa.** Ação: gatilho automático de visita extraordinária em 7 dias (módulo 02).

**Sinal 3 — Operadores voltam ao canal antigo (WhatsApp pessoal, planilha do operador).** Ação: conversa imediata com champion + dono para identificar o atrito específico.

**Sinal 4 — Override humano sobe acima de 30%.** Ação: investigação técnica em até 7 dias. Geralmente é mudança no negócio do cliente (preço novo, novo produto, novo processo) que o playbook não acompanhou.

**Sinal 5 — Playbook não foi atualizado nos últimos 60 dias.** Ação: este é sinal do **aluno**, não do cliente. Significa que o aluno desengajou. Corrigir imediatamente ou aceitar que vai perder o contrato.

### Diagnóstico por ADKAR quando a adoção cai

Pesquisa G §10.2. Quando algum sinal dispara, o aluno identifica em qual etapa do ADKAR a equipe travou:

| Sintoma | Etapa ADKAR | Intervenção |
|---------|-------------|-------------|
| "Esqueci que tínhamos isso" | Awareness | Dashboard no mural, notificações visíveis, dono mencionando em reunião de equipe |
| "Não estou vendo resultado" | Desire | Mostrar dados que a equipe não percebeu, depoimento de um cliente final satisfeito |
| "Não sei mais como fazer" | Knowledge | Nova sessão de treinamento, micro-vídeo novo, cheatsheet atualizado |
| "Tentei, travou, desisti" | Ability | Sessão hands-on imediata, presencial, mesmo fora do calendário |
| "Ninguém usa mais, então também parei" | Reinforcement | Envolver dono como sponsor ativo, pequena campanha interna |

A intervenção é completamente diferente para cada etapa. Aluno que aplica o mesmo "vamos fazer mais treinamento" para qualquer queda erra em 4 dos 5 casos.

### Os 4 níveis de recuperação

Pesquisa G §10.3, ordenados do menos ao mais intensivo:

**Nível 1 — Intervenção remota (resposta em até 7 dias)**

- Check-in não agendado: "Percebi que o uso caiu — o que está acontecendo?"
- Envio de caso de sucesso relevante (outro cliente do mesmo setor melhorando) por e-mail ou WhatsApp.
- Micro-vídeo curto resolvendo a dificuldade identificada.

Quando usar: sinal isolado, queda pequena, sem ruído de outros indicadores.

**Nível 2 — Visita extraordinária de recuperação (em até 14 dias)**

- Sessão hands-on presencial focada exclusivamente no ponto de atrito.
- "Win rápido": entregar resultado visível na própria visita (ex.: resolver em 30 min algo que o dono pediu há semanas).
- Gamificação simples: desafio de 7 dias com pequena recompensa interna.

Quando usar: 2+ sinais simultâneos, ou queda mantida por 2 meses.

**Nível 3 — Revisão de escopo (em até 30 dias)**

- Quando a adoção cai porque o fluxo instalado **não serve mais** para a operação real do cliente.
- Revisão do mapeamento de processos.
- Redesenho de 1 ou 2 fluxos principais com nova perspectiva.

Custo: dentro da manutenção se a mudança couber no banco de horas. Acima disso, aditivo. Importante: a revisão de escopo nesse contexto **não é cobrada como novo diagnóstico** — vai para o custo de manutenção até 30 dias da identificação (DPIA canônico, seção Acompanhamento: "Recuperação que não funciona em 30 dias é diagnóstico errado lá atrás — abre-se um D parcial sobre o processo, sem cobrar de novo").

**Nível 4 — Envolvimento do dono como sponsor ativo**

- Conversa direta com o dono: "Para esse sistema funcionar, você precisa ser o primeiro a usá-lo e pedir para a equipe usá-lo. Sua ausência está sendo sentida."
- Definir compromisso público do dono: mencionar IA na reunião de equipe semanal, usar em frente à equipe, cobrar pessoalmente.

Quando usar: sinal claro de que o dono saiu do papel de sponsor (Pesquisa G §10.4 — modo de falha 3). Esta intervenção é desconfortável mas necessária — sem ela, todos os outros níveis fracassam.

### O honeymoon effect — prevenção

Pesquisa G §10.4. O padrão mais comum: entusiasmo nas primeiras semanas, queda abrupta na semana 3–4 quando aparecem os primeiros atritos.

Prevenção estrutural:

- **Metas progressivas no relatório.** Mês 1 não exige 90% de adoção. Mês 3 exige.
- **Normalizar a curva.** Na sessão de treinamento do dono (módulo 01) e do champion (módulo 02), o aluno explica que dificuldade inicial é esperada.
- **Vitórias documentadas semanalmente nas primeiras 4 semanas.** Comunicação ativa do que está dando certo.
- **Check-in proativo na semana 3 ou 4.** Mesmo que não esteja no calendário formal, aluno aparece (presencial ou remoto) — momento crítico em que o entusiasmo esfria.

### Quando o contrato deve ser encerrado

Nem todo contrato deve ser mantido a qualquer custo. O aluno deve propor encerramento (ou aceitar) quando:

- Nível 3 + Nível 4 de recuperação aplicados e a adoção continua abaixo de 50% por 60 dias.
- Cliente não atualiza credenciais ou bloqueia acesso necessário para manutenção.
- Mudança estrutural no negócio do cliente faz o processo automatizado deixar de existir (ex.: distribuidora migrou de WhatsApp para call-center terceirizado).
- Cliente se torna inadimplente por mais de 60 dias.

Encerramento bem-conduzido segue a cláusula 14ª (transição sem lock-in, módulo 03 do Bloco B). Cliente sai com tudo na mão. Aluno sai sem disputa e com boa reputação para futuras indicações.

## Exemplo aplicado — Polaris Bebidas

12 visitas mensais no ano 1. Estrutura padrão em todas. Variações:

- **Mês 4:** sinal 4 (override subiu para 28%). Investigação: Cláudio tinha mudado tabela de preços e o playbook estava com a antiga. Recuperação Nível 1 (atualização de RAG + comunicação ao Cláudio). Override voltou para 14% em 2 semanas.

- **Mês 7:** sinal nenhum, mas Cláudio pediu novo fluxo de orçamento de evento. Recuperação não aplicada — virou aditivo (R$ 4.800).

- **Mês 9:** Maria promovida, Júlia assumiu como nova champion candidata. Aluno fez 1 sessão acelerada (4h em 1 dia) com Júlia. Sem gatilho de queda — operação manteve métricas.

- **Mês 12:** renovação automática do contrato. Reajuste INPC. Cláudio sinalizou interesse em segundo agente (cobrança). Aluno vendeu novo projeto (R$ 18.000) que começou no início do ano 2.

Adoção 12 meses: 94% sustentada. Override estável em 11–14%. ROI ano 1: 187%.

## Materiais prontos

- Template de relatório mensal de 1 página (referência cruzada com `metodo/templates/template-relatorio-mensal-acompanhamento.md`).
- Calendário-padrão da visita mensal (2–3h, blocos).
- Fluxograma "sinal precoce → ação" para os 5 sinais.
- Tabela de diagnóstico ADKAR com intervenção por etapa.
- Protocolo dos 4 níveis de recuperação.
- Modelo de comunicação de encerramento (cláusula 14ª aplicada).

## Erros comuns

- **Visita mensal sem relatório de 1 página.** *Correção:* relatório é a evidência do valor entregue. Sem ele, o cliente esquece.
- **Esperar a próxima visita para agir em sinal de queda.** *Correção:* gatilhos são gatilhos. Ação em até 7–14 dias, fora do calendário regular.
- **Aplicar a mesma intervenção para qualquer queda.** *Correção:* ADKAR define a intervenção. "Mais treinamento" só serve para Knowledge.
- **Não envolver o dono no nível 4 quando ele saiu do papel de sponsor.** *Correção:* conversa direta, mesmo que desconfortável. Sem o dono, nada se sustenta.
- **Manter contrato a qualquer custo após 60 dias de adoção abaixo de 50% com intervenções esgotadas.** *Correção:* encerrar bem é melhor que arrastar mal.
- **Esquecer de comemorar conquista do mês no relatório.** *Correção:* uma linha de conquista preserva moral da equipe e justifica o gasto para o dono.

## Checklist de saída

- [ ] Aluno tem template do relatório mensal em 1 página.
- [ ] Aluno consegue calcular as 4 métricas obrigatórias em até 30 min com dados de qualquer cliente.
- [ ] Aluno tem fluxograma dos 5 sinais precoces de queda com ação por sinal.
- [ ] Aluno tem tabela ADKAR com intervenção por etapa.
- [ ] Aluno conhece os 4 níveis de recuperação e quando aplicar cada um.
- [ ] Aluno tem calendário-padrão da visita mensal (blocos 0–4).
- [ ] Aluno entendeu que sinal precoce é gatilho de ação fora do calendário regular.
