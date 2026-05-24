---
nivel: 3
bloco: A
modulo: 5
titulo: Proposta vencedora — a estrutura que fecha
tempo_leitura: 17
exit_criteria: Aluno tem template de proposta de diagnóstico e de projeto prontos, com as sete seções obrigatórias
---

# Proposta vencedora — a estrutura que fecha

## O que você vai aprender

- Por que a proposta não começa pela solução, mas pela dor do cliente nas palavras dele.
- A estrutura de sete seções que converte (Dor → Solução → ROI → Prova → Plano → Investimento → Próximos passos).
- A técnica da âncora de três opções (Essencial / Recomendado / Completo).
- Como apresentar preço sem entrar em desconto e como criar urgência legítima.
- Diferença entre proposta de diagnóstico e proposta de projeto — duas peças distintas no funil.

## Por que isso importa

Pesquisa F §6.1 mostra que propostas de IA que convertem seguem padrão Pain → Value → Proof → Solution → ROI → Implementation → Pricing. Propostas que começam pela solução perdem o leitor na primeira página. O dono de PME não lê 30 páginas. Lê a primeira; se ele se reconhecer, lê a segunda. Se na primeira página estiver "Quem somos" e logo "Nossa metodologia", ele fecha o PDF.

A proposta é o documento que o cliente lê quando o aluno não está na sala. Ela precisa fazer o trabalho que a reunião fez, sozinha, em 5 minutos de leitura.

## Conteúdo principal

### Duas propostas, dois momentos

**Proposta do diagnóstico** (final da reunião de discovery). 1–2 páginas. Vende um produto pequeno (R$ 1.500–4.000) com entregável claro.

**Proposta do projeto** (final do diagnóstico, embutida no relatório DPIA). 6–10 páginas. Vende o projeto completo (R$ 8.000–60.000) com baseline numérico já assinado.

Ambas seguem a mesma estrutura de sete seções, com peso diferente em cada uma.

### As sete seções obrigatórias

**Seção 1 — O contexto deles (não de você)**

Primeiro parágrafo da proposta reescreve a dor do cliente nas palavras dele, com os números que ele mesmo deu na reunião. Se o cliente ler essa seção e pensar "é exatamente isso que eu disse", você ganhou atenção total.

Modelo para distribuidora:

> "Na reunião do dia [data], você descreveu o seguinte cenário: dois atendentes respondem WhatsApp das 8h às 18h. 30 perguntas/dia sobre prazo de entrega, 3 minutos por resposta. Isso significa 90 minutos/dia em cada atendente, ou aproximadamente 66 horas/mês por pessoa, R$ 3.696/mês de custo combinado (com hora-atendente all-in de R$ 28). Você mencionou ainda que em horário de almoço o tempo de resposta chega a 6h, e que estima perder cerca de 3 pedidos/dia por demora — o que dá um custo de oportunidade adicional não estimado, mas significativo."

**Seção 2 — A solução proposta**

Descrição objetiva do que será construído. Sem jargão técnico. Foco em "o que o cliente recebe", não em "o que vamos fazer". Use os nomes que o cliente entende: "WhatsApp", "Bling", "agente que responde", não "LLM com RAG via API".

Modelo:

> "Um agente de atendimento integrado ao seu WhatsApp Business e ao Bling, que responde automaticamente às 7 perguntas mais frequentes (prazo de entrega, preço de tabela, status de pedido, formas de pagamento, mínimo de pedido, área de cobertura, horário de funcionamento), valida estoque em tempo real, monta o pedido pronto para confirmação do operador, e escala para humano nos casos fora do padrão (reclamações, pedidos especiais, valor acima de R$ 5.000, ou palavras-chave de urgência)."

**Seção 3 — ROI estimado**

Cálculo de payback simples. Mesmo conservador, com número.

Modelo:

```
Tempo economizado: 60h/mês combinadas (60% das 100h atuais em perguntas repetidas)
Custo de hora-atendente: R$ 28
Economia mensal direta: R$ 1.680
Pedidos adicionais capturados (estimativa conservadora): 1/dia × 22 dias × R$ 180 ticket médio = R$ 3.960
Ganho mensal total estimado: R$ 5.640
Custo do projeto: R$ 22.000
Custo mensal de operação (LLM + n8n + Supabase + WABA): R$ 580
Custo da manutenção: R$ 2.500/mês
Payback do investimento de projeto: 7,2 meses
ROI 12 meses: 207%
```

Não invente número. Use o baseline real assinado em D (para proposta de projeto) ou estimativas conservadoras (para proposta de diagnóstico).

**Seção 4 — Prova social**

Caso real, mesmo que único. Anonimizado se necessário. Com número.

Modelo:

> "Caso de referência: distribuidora de bebidas em Joinville/SC, 14 funcionários, mesmo perfil. Após 90 dias de operação, tempo médio de resposta caiu de 47 min para 4 min, atendimento operando 18h/dia (era 10h/dia), 0 pedidos perdidos por demora documentados. Disponível para visita de validação mediante agendamento prévio."

**Seção 5 — Plano de execução**

Timeline em 5–6 linhas, não Gantt detalhado. O cliente quer saber quando vê resultado.

Modelo:

```
Semanas 1–2: Diagnóstico em campo (entrevistas, walk-the-floor, exportação de histórico, baseline assinado).
Semanas 3–4: Pacote de contexto (base de conhecimento, integrações Bling e WABA configuradas, glossário).
Semanas 5–6: Configuração do agente, playbook, matriz HITL aprovada, homologação ≥ 85%.
Semanas 7–8: Deploy em produção em nível conservador, job shadowing presencial, treinamento da equipe.
Mês 2 pós-deploy: Primeiro relatório de payback contra baseline assinado.
Manutenção mensal: a partir do mês 2 pós-deploy.
```

**Seção 6 — Investimento (não "custo")**

A palavra é **investimento**. Três opções (técnica da âncora). A coluna do meio é a oferta principal.

| Item | Essencial | Recomendado | Completo |
|------|-----------|-------------|----------|
| Diagnóstico | Incluído | Incluído | Incluído |
| Agente WhatsApp + Bling | Sim | Sim | Sim |
| Integração com Pix/Asaas | Não | Sim | Sim |
| Confirmação automática pós-pedido | Não | Sim | Sim |
| Painel de métricas para o dono | Não | Não | Sim |
| Micro-vídeos de treinamento | 3 | 6 | 10 |
| Período de suporte inicial incluso | 30 dias | 60 dias | 90 dias |
| **Investimento de projeto** | **R$ 16.000** | **R$ 22.000** | **R$ 32.000** |
| **Manutenção mensal** | **R$ 1.500** | **R$ 2.500** | **R$ 4.000** |

Pagamento: 30% na assinatura (sinal e início do diagnóstico) — 40% na entrega do piloto homologado — 30% no go-live de produção. Manutenção começa no mês 2 pós-deploy.

Validade: 10 dias corridos a partir do envio.

**Seção 7 — Próximos passos**

Instrução clara, sem ambiguidade.

> "Para confirmar, responda este e-mail com 'aceito a opção [Essencial / Recomendado / Completo]'. Ao receber, envio o contrato e o boleto/Pix da primeira parcela. O kick-off do diagnóstico fica agendado para [data específica, 5 dias úteis após a confirmação]."

### A técnica da âncora de três opções

Pesquisa F §6.2 confirma que a maioria dos clientes de PME escolhe a opção do meio. A coluna alta cria âncora ("o que existe mais caro"). A coluna baixa mostra que você não está empurrando ("tem opção menor, eu não estou forçando"). A coluna do meio aparece como "razoável" por contraste — e é onde você projeta margem confortável.

Diferenças entre as três colunas:

- Escopo (mais ou menos integrações).
- Suporte inicial (30/60/90 dias).
- Manutenção (tier básico/intermediário/avançado).

Nunca diferencie por qualidade do agente. As três opções entregam o mesmo agente principal. A diferença está em complementos, não em compromisso de resultado.

### Como apresentar preço sem entrar em desconto

Quando o cliente pergunta "dá pra fazer mais barato?", a resposta padrão é:

> "Posso te oferecer a opção Essencial em vez da Recomendada. Você fica com o agente principal funcionando, sem as integrações de Pix e sem o painel. O investimento cai para R$ 16.000. Aceita?"

Você nunca baixa o preço da mesma opção. Você oferece a opção menor. Isso preserva o preço de tabela e ainda dá ao cliente a sensação de ter "ganhado" algo.

### Urgência legítima

Validade de 10 dias é padrão. Não use "promoção válida até sexta" — soa amador. Use "validade da proposta: 10 dias". Se o cliente ultrapassa o prazo, o aluno reenvia com preço atualizado (mesmo que igual) e nova validade. Isso cria padrão profissional sem manipulação.

### Formato de entrega

- PDF de 6–10 páginas (não 30).
- Enviado por e-mail com texto curto: "Anexo a proposta combinada na nossa conversa. Validade de 10 dias. Qualquer dúvida me chama no WhatsApp."
- Apresentação presencial ou em vídeochamada de 30 min agendada **no momento do envio** ("posso te ligar quinta às 15h para passar a proposta com você?").
- Sem ligar do nada três dias depois cobrando resposta. O follow-up é uma mensagem por WhatsApp ou e-mail na metade do prazo ("vi que você ainda não respondeu, está com alguma dúvida específica?").

## Exemplo aplicado — Polaris Bebidas

A proposta de diagnóstico da Polaris tinha 2 páginas. Primeira seção começava com "Na reunião de 12/03, você mencionou que duas atendentes gastam 90 minutos/dia respondendo prazo de entrega...". Seção investimento: R$ 2.500, abatível do projeto. Validade 10 dias. Cláudio respondeu em 4 dias.

A proposta de projeto, enviada após o diagnóstico, tinha 8 páginas. Trazia baseline assinado, três opções (R$ 16.000 / R$ 22.000 / R$ 32.000). Cláudio escolheu a do meio (Recomendada), pediu desconto, recebeu como resposta a oferta da opção Essencial. Pensou meia hora, voltou para a Recomendada (porque o painel do Completo não era essencial para ele, mas a integração Pix sim).

## Materiais prontos

- Template de proposta de diagnóstico (2 páginas) com placeholders.
- Template de proposta de projeto (8 páginas) com placeholders.
- Calculadora de ROI em planilha (referenciada em `metodo/templates/template-baseline-assinado.md`).
- Modelo de e-mail de envio + modelo de follow-up.

## Erros comuns

- **Começar a proposta com "Quem somos" / "Sobre nossa metodologia".** *Correção:* começa com a dor do cliente nas palavras dele. Apresentação fica em rodapé ou anexo.
- **Não usar três opções.** *Correção:* sempre três. Sem âncora, o preço único parece arbitrário.
- **Diferenciar opções por qualidade do agente.** *Correção:* a diferenciação é por escopo (integrações, suporte, manutenção). O agente principal é o mesmo nas três.
- **Baixar preço da mesma opção quando o cliente pede desconto.** *Correção:* ofereça a opção menor. Nunca quebre o preço da opção escolhida.
- **PDF de 30 páginas com Gantt detalhado.** *Correção:* 6–10 páginas, timeline em 6 linhas.
- **Não definir validade.** *Correção:* 10 dias, sempre. Sem validade, o cliente posterga indefinidamente.

## Checklist de saída

- [ ] Aluno tem template de proposta de diagnóstico em PDF.
- [ ] Aluno tem template de proposta de projeto em PDF, com âncora de 3 opções.
- [ ] Aluno consegue escrever a primeira seção (contexto do cliente) em 15 minutos usando notas da reunião de discovery.
- [ ] Aluno tem planilha de ROI preenchível com nomes de campo claros.
- [ ] Aluno escreveu modelo de e-mail de envio e de follow-up que vai usar.
- [ ] Aluno entendeu a regra: não baixar preço da mesma opção, oferecer opção menor.
