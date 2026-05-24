---
nivel: 2
trilha: A
modulo: 5
titulo: Handover para Acompanhamento
tempo_leitura: 18
exit_criteria: aluno entrega contrato de manutenção assinado, biblioteca inicial de 5-10 micro-vídeos, e conduz a primeira reunião mensal do tier contratado.
---

# O que você vai aprender
- A estrutura do contrato de manutenção: tier básico, intermediário e avançado.
- Como vender manutenção como continuação, não como upsell.
- Como produzir a biblioteca inicial de micro-vídeos com o champion (5-10 vídeos de 2-3 min).
- Como conduzir a primeira reunião mensal do loop de Acompanhamento.
- Como reconhecer sinais precoces de queda de adoção e ativar recuperação.

## Por que isso importa
Fato 8 da síntese transversal e Pesquisa F §1.2: receita recorrente é o motor, não o projeto. 5 clientes em manutenção × R$ 2.500/mês = R$ 12.500/mês de base estável. Sem essa recorrência, o implementador volta para o ciclo de venda nova mês a mês e o negócio não escala.

Acompanhamento (manual canônico) é o loop externo de operação contínua. Não é a quinta fase do DPIA — é o que sustenta o resultado depois que A fecha. Princípio 6: documentação viva ou nada. Playbook congelado é sinal precoce de abandono iminente (Pesquisa G §"princípio 5").

## O passo a passo

**1. Estrutura do contrato de manutenção (3 tiers).**

| Tier | Cadência | Inclui | Preço típico |
|------|----------|--------|---------------|
| Básico | 1 visita presencial/mês + check-in quinzenal remoto | Métrica mensal, ajuste de prompt, atualização de playbook | R$ 800-1.500 |
| Intermediário | 2 visitas/mês + relatório mensal formal | Tudo do básico + micro-vídeos novos + ajuste de RAG | R$ 1.500-3.500 |
| Avançado | Visita semanal + SLA + champion treinado mensalmente | Tudo do intermediário + SLA escrito + bônus por KPI | R$ 3.500-7.000 |

Modelo em `templates/template-contrato-manutencao.md`.

**2. Vender manutenção como continuação.** Apresentação no marco 30 dias deve seguir lógica:
- O projeto foi entregue.
- Para manter o ganho que vocês viram, o agente precisa: ajuste de prompt quando muda a política do negócio, atualização de RAG com produtos novos, monitoramento de drift, relatório mensal contra baseline, capacitação contínua da Maria.
- Tier sugerido (intermediário R$ 2.400/mês) inclui todos esses itens, com SLA de resposta de 2h úteis para incidente.
- Sem manutenção: agente continua rodando mas em 60-90 dias começa a degradar — drift do negócio + erros não corrigidos + Maria sem suporte.

A pergunta-chave: *"Sr. Roberto, o senhor quer manter o ganho que vimos no slide 3? Esse é o investimento."*

**3. Tratar a objeção "vou pensar".** Resposta: *"Sem problema. Posso deixar 2 semanas para decidir. Nesse meio tempo, o agente continua rodando mas eu fico fora — significa que se algo der errado, vocês ficam na fila do meu próximo cliente disponível. Combinamos um retorno em 2 semanas?"*

Em geral, decisão sai em 3-7 dias. Se não decidem em 2 semanas, é não.

**4. Biblioteca de micro-vídeos.** Sub-etapa A.4 do manual canônico. 5 a 10 vídeos de 2-3 minutos cada, gravados no contexto real do cliente, com casos reais (anonimizados).

Conteúdo recomendado da biblioteca inicial:
- *Vídeo 1:* Como o agente funciona no dia a dia (overview, 3 min).
- *Vídeo 2:* O que fazer quando o agente escala para você (2 min).
- *Vídeo 3:* Como pausar o agente em emergência (1 min).
- *Vídeo 4:* Como ler o painel de métricas (2 min).
- *Vídeo 5:* Como propor mudança no playbook (2 min).
- *Vídeo 6-10:* Casos de uso específicos do nicho (em distribuidora: cliente VIP, áudio, PJ novo, devolução, fim de mês).

Gravação: smartphone na mão da Maria, ou tela compartilhada. Edição mínima (cortes simples). Hospedagem: pasta do Drive ou Notion do cliente. Atualização: a cada manutenção (princípio 6 — documentação viva).

**5. Primeira reunião mensal do tier.** Estrutura padrão (60-75 min):

- *0-5 min — Contrato da reunião.* Reafirmar o que vão cobrir.
- *5-25 min — Relatório do mês.* As 4 métricas obrigatórias:
  - Métrica de processo: número atual vs. baseline.
  - Métrica de adoção: % das interações pelo agente vs. canal antigo.
  - Métrica de qualidade: % de respostas com override humano.
  - Métrica financeira: payback acumulado e ROI mês a mês.
- *25-40 min — Discussão com champion.* O que está fluindo, o que está travando, o que precisa ajustar.
- *40-55 min — Ajustes técnicos.* Atualizações no system prompt, no RAG, no playbook. Em geral 2-5 mudanças/mês.
- *55-65 min — Próximo ciclo.* Plano para o mês seguinte (subida de nível HITL? novo micro-vídeo? expansão de escopo?).
- *65-75 min — Combinar próxima reunião.*

**6. Métricas mensais obrigatórias.** Já listadas no manual canônico, "Acompanhamento, Métricas obrigatórias por mês":
1. Métrica de processo: número atual vs. baseline.
2. Métrica de adoção: % das interações pelo agente vs. canal antigo.
3. Métrica de qualidade: % de respostas que precisaram de override humano.
4. Métrica financeira: payback acumulado, ROI mês a mês.

Sem essas 4 a cada mês, não é manutenção — é "passa lá tomar café".

**7. Sinais precoces de queda de adoção.**

- Uso do agente cai 20% mês a mês em volume.
- Champion deixa a empresa (gatilho automático de visita extraordinária em 7 dias).
- Operadores voltam ao canal antigo (WhatsApp pessoal, planilha do operador).
- Override humano sobe acima de 30%.
- Playbook não foi atualizado nos últimos 60 dias (sinal de desengajamento do próprio implementador).

Cada sinal aciona resposta padrão.

**8. Resposta padrão à queda de adoção.**

- Visita extraordinária em até 7 dias (não esperar a próxima do tier).
- Re-entrevista com 3 operadores principais.
- Identificação do atrito específico — não "o agente está ruim" — qual situação específica está falhando.
- Re-treinamento focado.
- Atualização do playbook + 1 ou 2 micro-vídeos novos.
- Relatório de recuperação enviado ao dono em até 14 dias do gatilho.

Recuperação que não funciona em 30 dias é diagnóstico errado lá atrás — abre-se um D parcial sobre o processo, sem cobrar de novo (vai para o custo de manutenção).

**9. Quando expandir escopo.** Cliente em manutenção há 3+ meses, com métricas estáveis, é candidato a expansão. Padrão:
- Cliente menciona dor secundária ("agora a cobrança de fatura vencida está ruim").
- Implementador propõe nova fase D (simplificada, aproveitando Pacote de Contexto existente).
- Novo projeto, novo preço, novo cronograma — não embute no manutenção atual.

**10. Encerramento natural de contrato.** Acontece: cliente vende a empresa, troca de setor, processo automatizado deixa de ser crítico. Encerramento profissional:
- Aviso prévio de 60 dias.
- Última visita: transferência completa de acesso (workflows n8n, credenciais, base Supabase) para o cliente.
- Documento "estado atual" com tudo que está em produção, fornecedores, custos recorrentes.
- Oferta de retorno em 30/60/90 dias com tarifa avulsa.

Cliente que sai bem volta. Cliente que sai mal vira ruído público.

## Exemplo aplicado — Polaris Bebidas (Joinville/SC)

Marco 30 dias (28/06) — proposta de manutenção apresentada.

Tier intermediário R$ 2.400/mês:
- 2 visitas presenciais/mês (1ª e 3ª terça do mês).
- Relatório mensal formal (4 slides).
- Ajustes contínuos de system prompt + RAG.
- Atualização de playbook a cada visita.
- Micro-vídeos novos a cada 2-3 meses.
- SLA de incidente: 2h úteis.

Sr. Roberto: "Topa, mas com SLA de 2h tem multa se atrasar?" Aluno: "Tem. Cláusula 7 do contrato: cada hora atrasada além de 2h custa 5% do valor do mês, máximo 50%."

Contrato assinado na própria reunião. Pagamento mensal via boleto Bling.

Biblioteca inicial — gravada nas semanas 3 e 4 da fase A:
- V1: Overview do agente Polaris (2:48). Aluno + Maria narrando.
- V2: Quando o agente escala (2:12). Maria mostrando casos reais no painel.
- V3: Botão de pausa (1:34). Maria demonstrando.
- V4: Painel de métricas (2:08). Maria abrindo o dashboard.
- V5: Como pedir mudança no playbook (1:55).
- V6: Cliente VIP (Bar do Zé) — fluxo completo (2:42).
- V7: PJ novo com alvará (2:18).
- V8: Cliente inadimplente (2:01).

8 vídeos, ~18 min totais. Hospedados no Google Drive da Polaris. Acessíveis a Maria + Sr. Roberto + Sr. Cláudio + aluno.

**Primeira reunião mensal — 30/julho.**

- Métrica de processo: tempo médio 7,1 min (meta marco 60: 7,5 min — atingida).
- Métrica de adoção: 92% das interações de pedido passaram pelo agente (meta: 90% — atingida).
- Métrica de qualidade: override em 11% (em queda — meta 30 dias: < 15%).
- Métrica financeira: payback acumulado 45%.

Discussão: Sr. Roberto satisfeito. Maria pede ajustes (3 itens): formato de mensagem de espera curto demais, agente respondendo "Boa tarde" em mensagens da madrugada, dúvida sobre subida nível HITL.

Ajustes feitos na própria reunião (2 mudanças no system prompt + 1 ajuste de RAG); a subida de nível foi confirmada para 5/agosto (dia 70 da operação).

Próxima reunião agendada para 13/agosto. Próximo micro-vídeo programado: caso de devolução parcial.

**Mês 4 — sinal de alerta:** override sobe de 9% para 14% em uma semana. Aluno investiga, descobre que Bling teve mudança de API silenciosa que afetou tool de consulta de estoque. Ajuste em 6 horas. Override volta para 10%. Caso reportado no relatório do mês.

**Mês 6:** Sr. Roberto pede expansão de escopo — automatizar cobrança de fatura vencida. Aluno propõe fase D simplificada por R$ 4.000 (aproveitando 60% do Pacote de Contexto existente). Contrato à parte do manutenção em vigor. Aceito.

## Erros comuns
- **Vender manutenção como upsell pós-projeto.** Vira venda do zero. *Corrige:* parte do contrato vendido em D.
- **Aceitar manutenção "sem contrato escrito".** Vira trabalho gratuito infinito. *Corrige:* sempre contrato.
- **Não produzir métricas mensais.** Vira "café com cliente". *Corrige:* 4 métricas obrigatórias.
- **Ignorar sinais de queda de adoção.** Cliente cancela em 2 meses. *Corrige:* visita extraordinária em 7 dias.
- **Não atualizar playbook a cada visita.** Princípio 6 violado. *Corrige:* atualização é parte do escopo do tier.
- **Embutir expansão no manutenção sem cobrar a parte.** Destrói margem. *Corrige:* novo escopo, novo preço.

## Checklist de saída
- [ ] Contrato de manutenção assinado, tier escolhido com justificativa.
- [ ] Biblioteca inicial de 5-10 micro-vídeos produzida.
- [ ] Primeira reunião mensal conduzida com 4 métricas obrigatórias.
- [ ] Plano para o mês seguinte combinado.
- [ ] Próxima reunião agendada.
- [ ] Champion confirmou-se confortável para manter operação.

## Vai além
- Manual canônico, seção "Acompanhamento (loop contínuo de manutenção)".
- `metodo/templates/template-contrato-manutencao.md` — modelo do contrato.
- `metodo/templates/template-relatorio-mensal-acompanhamento.md` — modelo do relatório.
- Pesquisa G §8.3, §"princípio 5".
- Pesquisa F §1.2.
- Exercício prático: para o cliente do exercício, produzir contrato de manutenção customizado para o tier intermediário e simular a primeira reunião mensal cronometrada em 75 min.
