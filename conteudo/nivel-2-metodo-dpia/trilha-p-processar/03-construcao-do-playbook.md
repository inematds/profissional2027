---
nivel: 2
trilha: P
modulo: 3
titulo: Construção do Playbook
tempo_leitura: 20
exit_criteria: aluno entrega rascunho de playbook do agente (formato entrada/processo/critério/saída/exemplos/regras) com pelo menos 3 fluxos principais cobertos.
---

# Construção do Playbook

## O que você vai aprender
- O que é um playbook de agente e como ele difere de SOP tradicional.
- Como estruturar o playbook em blocos entrada / processo / critério / saída / exemplos / regras.
- Como o playbook conecta mapa de processo + regras tácitas + base de conhecimento em um documento operacional vivo.
- Como deixar o playbook editável pelo champion interno após o deploy.
- Como o playbook entra na fase I como insumo principal do system prompt.

## Por que isso importa
Princípio 6 do DPIA: documentação viva ou nada. SOP tradicional é PDF estático que vira anexo de e-mail nunca aberto. Playbook é documento operacional vivo: editado a cada manutenção, consultado pelo champion no dia a dia, expandido a cada situação nova. Pesquisa G §7 e §"princípio 5" mostram que playbook congelado é o primeiro sintoma de abandono iminente do contrato.

O playbook também é o instrumento que distingue o implementador DPIA do vendedor de plataforma. A plataforma vende agente; o implementador entrega agente + playbook + champion treinado para operar e atualizar. Defensibilidade comercial (manual canônico, apresentação, item 3).

## O passo a passo

**1. Decidir o escopo do playbook.** Um playbook por módulo do agente. Em PME, módulo inicial costuma ser um (atendimento + pedido, ou agendamento + lembrete, ou triagem + classificação). Em projeto maior, 2-3 módulos. Cada módulo tem seu playbook.

**2. Estrutura do playbook (template-playbook-agente.md):**

```
PLAYBOOK — <NOME DO MÓDULO>
Cliente: <nome>
Versão: <X.Y, data>
Champion: <nome, e-mail>

1. Visão geral (1 parágrafo)
2. Contexto: o que o agente sabe (link para Pacote de Contexto)
3. Fluxos
   3.1 Fluxo principal A — <nome>
       - Entrada: <o que dispara>
       - Processo: <passos>
       - Critério: <quando decide cada saída>
       - Saída: <o que entrega>
       - Exemplos: <2 a 5 casos reais>
       - Regras associadas: <RT-XX, RT-YY>
   3.2 Fluxo principal B...
4. Gatilhos de escalonamento (HITL)
5. Mensagens-padrão (cumprimento, encerramento, espera, erro)
6. O que o agente NÃO faz
7. Plano de rollback (como desligar em 15 min)
8. Histórico de atualizações
```

**3. Escrever fluxo principal A.** Cada fluxo é uma seção operacional. Exemplo de campo "Critério": "Se cliente menciona 'cancelar' ou 'reclamação' → escalar (HITL nível 2). Se valor estimado > R$ 5.000 → escalar. Se cliente é VIP (lista anexa) → escalar. Caso contrário, prossegue."

**4. Acoplar regras tácitas.** Para cada fluxo, listar as RT-XX que se aplicam. Não copiar o conteúdo da regra — referenciar e ter as regras em arquivo `regras-taticas.md` separado. Isso evita duplicação e mantém regras vivas em um único lugar.

**5. Acoplar exemplos few-shot reais.** Para cada fluxo, anexar 2 a 5 exemplos extraídos do histórico (pergunta real + resposta ideal validada pelo operador). Exemplos serão reutilizados no system prompt da fase I. Em P, eles ficam no playbook.

**6. Escrever mensagens-padrão.** O agente precisa de fraseado consistente:
- Cumprimento inicial.
- Pedido de espera ("um momento, vou consultar").
- Encerramento.
- Mensagem de erro técnico.
- Mensagem de escalonamento ("vou chamar a Maria para te atender").

Mensagens institucionais — não imitam estilo pessoal do operador (que era erro RT-18 da Polaris).

**7. Definir gatilhos de HITL.** Lista clara dos disparadores de escalonamento. Cada gatilho conecta-se à matriz HITL que será aprovada na fase I.

**8. Listar "o que o agente NÃO faz".** Limites explícitos. Pesquisa G §3.3: cliente que entende o limite do agente confia mais nele que cliente que descobre o limite na falha. Exemplos:
- Não autoriza fiado.
- Não fecha negócio acima de R$ X.
- Não dá descontos fora da tabela.
- Não responde reclamação formal.
- Não muda dados cadastrais sem confirmação humana.

**9. Plano de rollback.** Como desligar o agente em 15 minutos se algo der errado:
- Desabilitar webhook no n8n.
- Comunicar clientes: mensagem-padrão "estamos temporariamente sem o canal automático, em breve voltamos".
- Roteamento manual para o operador.
- Quem aciona (champion ou implementador).
- Como avisar o dono.

Esse plano é exit-gate da fase I e precisa ser testado em fase A (manual canônico, "Plano de rollback testado pelo menos uma vez").

**10. Versionar e dar acesso editável ao champion.** Playbook fica em pasta versionada (Notion, Google Drive, GitHub privado) com acesso de edição para o champion. A cada manutenção, atualiza-se. Bloco "Histórico de atualizações" registra cada mudança.

## Exemplo aplicado — Polaris Bebidas (Joinville/SC)

Playbook v0.3 (rascunho fase P), módulo "Atendimento de Pedido WhatsApp":

**3.1 Fluxo principal A — Pedido novo de cliente cadastrado**

- *Entrada:* mensagem WhatsApp de cliente já no Bling, contendo intenção de compra (palavras como "quero", "manda", "preciso", "passa um pedido", produto+quantidade).
- *Processo:*
  1. Identifica cliente pelo número.
  2. Cumprimenta usando primeiro nome.
  3. Interpreta produtos: SKU + quantidade.
  4. Se algum item ambíguo, pergunta. Se claro, segue.
  5. Confirma bairro (usa Bling se cadastrado; pergunta se não).
  6. Aplica RT-07 (Glória same-day) se aplicável.
  7. Aplica RT-09 (desconto recorrente) se aplicável.
  8. Pergunta forma de pagamento.
  9. Confirma o pedido: lista + valor + entrega + pagamento.
  10. Cria pedido no Bling (status: confirmado).
  11. Encerra.
- *Critério:*
  - Se cliente é VIP (lista anexa, RT-06): escala antes do passo 3.
  - Se cliente é inadimplente (RT-15): escala antes do passo 9.
  - Se forma de pagamento = "fiado": escala antes do passo 10.
  - Se valor > R$ 5.000: escala antes do passo 10.
- *Saída:* pedido criado no Bling + mensagem de confirmação ao cliente.
- *Exemplos:* (3 trechos reais do WhatsApp, anonimizados)
- *Regras associadas:* RT-06, RT-07, RT-09, RT-12, RT-15.

**3.2 Fluxo principal B — Pedido de cliente novo**

- *Entrada:* número não está no Bling.
- *Processo:* cadastra (CNPJ/CPF, endereço, bairro) → se PJ novo, aplica RT-12 (pede foto alvará) → escala para Maria validar cadastro → após validação, retorna para fluxo A passo 3.
- *Saída:* cadastro criado + pedido criado + cliente atendido.

**3.3 Fluxo principal C — Reclamação / cancelamento**

- *Entrada:* palavra-chave em mensagem ("reclamar", "cancelar", "errado", "devolver", "Procon", "péssimo").
- *Processo:* responde mensagem-padrão de espera + escala imediatamente para Maria (HITL nível 1).
- *Saída:* atendimento humano em até 5 minutos.

**4. Gatilhos de escalonamento:**
- VIP (lista anexa, hoje só Bar do Zé).
- Inadimplente (consulta Bling em tempo real).
- Valor estimado > R$ 5.000.
- Pagamento fiado.
- Palavras-chave de reclamação.
- Áudio > 3 min sem possibilidade de transcrição.
- Confidence score do agente < 70% em interpretação de SKU.

**5. Mensagens-padrão:**
- Cumprimento: *"Olá, [nome]! Polaris Bebidas, em que posso ajudar?"*
- Espera: *"Um momento, vou conferir aqui no sistema."*
- Encerramento: *"Pedido confirmado, [nome]. Qualquer dúvida, é só chamar."*
- Erro técnico: *"Estamos com uma instabilidade momentânea. A Maria já está sendo avisada."*
- Escalonamento: *"Para esse caso, vou chamar a Maria. Ela responde em até 5 minutos."*

**6. O que o agente NÃO faz:**
- Não autoriza fiado.
- Não dá desconto além de RT-09 e tabela.
- Não confirma pedido > R$ 5.000 sem aprovação.
- Não atende Bar do Zé.
- Não muda endereço cadastrado sem confirmação humana.
- Não responde a tema fora de pedido (jurídico, RH, dúvida tributária).

**7. Plano de rollback (15 min):**
- Maria desabilita o webhook n8n via botão "pausar fluxo" (treinamento em fase A).
- Mensagem automática para clientes ativos: *"Voltamos ao atendimento humano por algumas horas. A Maria atende de 09h às 18h, Sr. Roberto fora desse horário."*
- Implementador é avisado por WhatsApp (número de plantão).
- Comunicação para o Sr. Roberto.

**8. Histórico:**
- v0.3 — 19/05 — rascunho P (aluno).
- v0.4 — 22/05 — review com Maria.
- v0.5 — 26/05 — aprovado pelo Sr. Roberto.

## Erros comuns
- **Misturar playbook com system prompt.** O playbook é para humano operar e atualizar. O system prompt é a tradução técnica do playbook para o LLM. *Corrige:* dois artefatos separados.
- **Playbook em PDF.** Vira documento morto. *Corrige:* markdown em ferramenta editável com acesso ao champion.
- **Listar regras dentro do fluxo (duplicação).** *Corrige:* regras ficam em arquivo separado, fluxo referencia.
- **Esquecer "o que o agente NÃO faz".** Cliente final tenta usar para tudo, descobre limite na falha. *Corrige:* seção 6 obrigatória.
- **Plano de rollback genérico.** "Em caso de problema, contate o implementador." Não serve. *Corrige:* passos concretos com nome do responsável e ferramenta exata.

## Checklist de saída
- [ ] Playbook rascunhado com a estrutura completa.
- [ ] Pelo menos 3 fluxos principais cobertos.
- [ ] Mensagens-padrão escritas.
- [ ] Gatilhos de HITL listados.
- [ ] "O que o agente NÃO faz" listado.
- [ ] Plano de rollback concreto, com responsável.
- [ ] Versionado, com acesso editável ao champion.

## Vai além
- `metodo/templates/template-playbook-agente.md` — estrutura oficial.
- Manual canônico, glossário "Playbook do Agente" e Princípio 6.
- Pesquisa G §7 — playbook vs. SOP.
- Exercício prático: para o cliente do exercício, produzir playbook v0.x com pelo menos 3 fluxos e revisar com colega de turma assumindo papel de champion.
