---
nivel: 3
bloco: B
modulo: 2
titulo: Modelo de projeto fechado — preço fixo, escopo amarrado, gestão de mudança
tempo_leitura: 18
exit_criteria: Aluno tem contrato de projeto pronto com 12 cláusulas obrigatórias e protocolo de mudança de escopo
---

# Modelo de projeto fechado — preço fixo, escopo amarrado, gestão de mudança

## O que você vai aprender

- Por que projeto é sempre preço fixo e nunca hora trabalhada.
- O baseline assinado como âncora do escopo — sem ele, o contrato é frágil.
- Os 12 itens obrigatórios do contrato de projeto.
- Protocolo de mudança de escopo durante o projeto (change request).
- Parcelamento padrão (30/40/30) e gatilhos de liberação.
- Cláusulas específicas para IA: propriedade de prompts, dados de treino, APIs de terceiros.

## Por que isso importa

Pesquisa F §1.1 e §6.1 documentam que projetos por hora pagam menos em mesmo escopo (US$ 25–35k fixo vs. US$ 50k por hora pelo mesmo trabalho). O problema mais grave do projeto sem contrato escrito, no entanto, não é o preço — é o escopo. O cliente vai pedindo "só mais essa coisinha" durante 8 semanas, e o aluno aceita por gentileza. No final, o projeto que deveria render R$ 22.000 por 8 semanas rendeu R$ 22.000 por 16 semanas. Margem destruída.

O contrato de projeto é o documento que impede isso. Ele amarra três coisas: escopo, prazo e preço, com baseline numérico como referência objetiva.

## Conteúdo principal

### Por que preço fixo

Três razões:

1. **Recompensa eficiência.** Conforme o aluno acumula repetição no nicho, entrega mais rápido. A margem cresce com a competência. No modelo por hora, a margem cai com a competência.
2. **Previsível para o cliente.** O dono de PME orça o ano. Saber "vou gastar R$ 22.000 em projeto + R$ 2.500/mês de manutenção" é planejável. "Vai depender das horas" não é.
3. **Foco no resultado.** Quando o aluno é pago pelo entregável, ele negocia com o cliente sobre **o resultado**, não sobre o tempo. Reunião de alinhamento de 4h vira reunião de 1h se o produto sai igual.

### O baseline como âncora do escopo

A fase D termina com baseline assinado. Esse baseline é o documento que sustenta o escopo do projeto. Cláusula central do contrato de projeto:

> "O escopo deste contrato refere-se exclusivamente ao processo identificado no Baseline Assinado em [data], conforme Anexo I. Qualquer outro processo ou expansão de escopo será tratado em contrato adicional."

Isso é a defesa contra "ah, mas também queria que respondesse no Instagram". O baseline é de WhatsApp. Instagram não está no contrato. Instagram é outro projeto, outro orçamento, outro prazo.

### Os 12 itens obrigatórios do contrato de projeto

**Item 1 — Partes e qualificação.** CNPJ completo, endereço, representante legal, contato. Para PME familiar, sócios ambos como signatários (evita "minha esposa não autorizou" depois).

**Item 2 — Objeto.** Referência ao produto produtizado vendido. "Implementação de Agente de Pedido para Distribuidoras, conforme Anexo I (escopo detalhado) e Anexo II (Baseline Assinado em [data])."

**Item 3 — Anexo I — Escopo detalhado.**

Liste em formato bullet:

- O que faz (módulos, integrações nominais).
- O que NÃO faz (cláusula de exclusão — fundamental).
- Tecnologia: n8n self-host na Hetzner (operada pelo Contratado), Supabase pgvector, Claude Sonnet + Haiku, WhatsApp Cloud API (conta WABA do Contratante, provisionada com apoio do Contratado), integração REST com [ERP nominal: Bling/Omie/Conta Azul/Tiny].
- Número de módulos do agente (atendimento, pedido, cobrança).
- Limite de N integrações com sistemas terceiros — adicional cobrado à parte.
- Quantidade de micro-vídeos incluídos (3, 6 ou 10 conforme opção Essencial/Recomendado/Completo).

**Item 4 — Anexo II — Baseline Assinado.** Cópia do documento da fase D, assinado por ambas as partes.

**Item 5 — Prazo e cronograma.** Marcos por fase do DPIA:

- Início da fase P: dia 1.
- Fim da fase P (Pacote de Contexto entregue): dia 14.
- Fim da fase I (Homologação aprovada): dia 28.
- Go-live em produção (início da fase A): dia 42.
- Primeiro relatório de payback (30 dias pós go-live): dia 72.

Total: ~72 dias corridos do início ao primeiro relatório. Cronograma é referência; atraso por parte da Contratante (não disponibilizar credenciais, não comparecer a reunião agendada) suspende o prazo.

**Item 6 — Investimento e pagamento.** Valor total. Parcelamento padrão:

- **30% na assinatura** — libera o início da fase P.
- **40% na entrega do Pacote de Contexto + homologação ≥ 85% (fim da fase I)** — libera o início da fase A.
- **30% no go-live de produção + entrega do primeiro relatório de payback** — fecha o projeto.

Pagamento via Pix ou boleto, prazo de 5 dias úteis. Atraso superior a 10 dias suspende o trabalho.

**Item 7 — Propriedade intelectual.** Cláusula crítica adaptada de Pesquisa F §8.1, item 5:

> "Os entregáveis criados especificamente para este projeto — system prompts customizados, base de conhecimento, workflows n8n, micro-vídeos gravados com a tela do Contratante, documentação do agente — são de propriedade da Contratante. A metodologia DPIA, frameworks de prompt, templates de configuração e quaisquer ativos pré-existentes utilizados no trabalho são de propriedade do Contratado. O Contratado se reserva o direito de reutilizar a metodologia e templates em outros clientes, sem reutilização dos prompts ou dados específicos da Contratante."

**Item 8 — Dados e LGPD.** Resumo aqui, detalhe no módulo 04:

> "O Contratado, durante a execução do projeto, atua como operador nos termos da LGPD (Lei 13.709/2018). A Contratante permanece controladora. O Contratado se compromete a: (i) acessar dados pessoais apenas para fins deste projeto; (ii) não usar os dados para treinamento de modelos próprios ou de terceiros; (iii) devolver ou deletar todos os dados em até 30 dias após o encerramento; (iv) notificar a Contratante em até 72h em caso de incidente de segurança; (v) implementar medidas técnicas de segurança adequadas (criptografia em trânsito e repouso, controle de acesso por chave individual, logs auditáveis)."

**Item 9 — APIs e fornecedores de terceiros.**

> "As APIs de modelos de linguagem (Anthropic, OpenAI), de orquestração (n8n), de armazenamento (Supabase), de mensageria (Meta WhatsApp Cloud API) e de pagamento (Asaas, Pix) são contratadas diretamente pela Contratante, em conta de sua titularidade, com apoio operacional do Contratado para provisionamento. Os custos mensais de operação dessas APIs (faixa estimada: R$ 550–1.750/mês, podendo variar com volume) são de responsabilidade da Contratante e não estão incluídos no valor do projeto nem da manutenção."

Isso protege o aluno: ele não é responsável quando a Anthropic muda preço da API, ou quando a Meta suspende a conta WABA por uso indevido pelo cliente.

**Item 10 — Evolução de escopo (change request).**

> "Qualquer funcionalidade, integração ou processo não descrito no Anexo I é tratado como escopo adicional. A Contratante pode solicitar inclusão via documento escrito (e-mail formal ou WhatsApp Business com confirmação). O Contratado responderá em até 5 dias úteis com proposta de aditivo contendo prazo, investimento adicional e impacto no cronograma original. A execução do aditivo depende de assinatura formal."

Este item é o que protege a margem. Sem ele, todo "só mais essa coisinha" vira trabalho não pago.

**Item 11 — SLA do projeto (não da manutenção).**

- Bugs críticos durante o projeto (sistema parado em homologação ou produção): correção em até 8h úteis.
- Bugs não-críticos: correção em até 3 dias úteis.
- Disponibilidade de produção após go-live: 99%/mês (permite ~7h de indisponibilidade).

**Item 12 — Rescisão e foro.**

Rescisão pela Contratante:

- Antes do início da fase P: reembolso de 70% do valor pago até o momento.
- Durante a fase P: reembolso de 30% do valor pago até o momento.
- Durante ou após a fase I: sem reembolso. O Contratado entrega o que foi produzido até o momento.

Rescisão pelo Contratado: por inadimplência da Contratante superior a 30 dias, com aviso prévio de 10 dias.

Foro: comarca a definir, padrão Brasil.

### Protocolo de mudança de escopo (change request)

Sequência operacional sempre que o cliente pede algo fora do Anexo I:

1. Aluno reconhece o pedido na hora: "Boa ideia, vou avaliar se cabe no escopo atual ou se vira aditivo."
2. Em até 48h, manda e-mail formal: "O pedido X não está no escopo do contrato (Anexo I, item Y). Posso fazer como aditivo. Avalio em até 5 dias e te mando proposta com prazo e investimento adicional."
3. Em até 5 dias úteis, envia proposta de aditivo.
4. Cliente assina aditivo separado (1 página). Só então o trabalho começa.

Nunca aceite "depois a gente acerta". Aditivo verbal vira aditivo grátis.

### Parcelamento 30/40/30 — por que esses números

- **30% na assinatura.** Cobre o custo de oportunidade e os primeiros 14 dias de trabalho (fase P).
- **40% na entrega da homologação.** Concentrado aqui porque é o momento de maior trabalho técnico (fase I) e o pagamento garante que o aluno seguirá para a fase A.
- **30% no go-live + relatório de payback.** Alinha o último pagamento ao entregável que mais importa para o cliente (sistema rodando + número provando).

**Variações aceitáveis.**

- 40/30/30 em cliente recorrente já testado.
- 50/50 em projeto menor (< R$ 12.000).
- Nunca aceitar 10/90 ou similar — concentra risco para o aluno.

## Exemplo aplicado — Polaris Bebidas

Contrato de projeto Polaris: R$ 22.000 (opção Recomendada). Crédito de R$ 2.500 do diagnóstico = R$ 19.500 efetivos.

Parcelamento:
- 30% (R$ 5.850) na assinatura — pago em 3 dias.
- 40% (R$ 7.800) na entrega da homologação — pago em 5 dias (homologação fechou em 92% de acerto).
- 30% (R$ 5.850) no go-live + primeiro relatório de payback.

Durante a fase I, Cláudio pediu "também resposta automática no Instagram". Aluno: "boa ideia, mas não está no escopo (Anexo I). Posso fazer como aditivo. Te mando proposta em 5 dias."

Proposta de aditivo enviada: R$ 6.500 adicional, prazo 14 dias após go-live do projeto principal. Cláudio assinou. Sem aditivo, esse trabalho viraria gratuito.

## Materiais prontos

- Template completo de contrato de projeto (12 cláusulas + 2 anexos).
- Modelo de Anexo I (escopo) por produto produtizado.
- Modelo de aditivo de change request (1 página).
- Cronograma-padrão em formato planilha (72 dias).

## Erros comuns

- **Não anexar o baseline assinado ao contrato.** *Correção:* baseline é o Anexo II, sem ele o escopo flutua.
- **Aceitar mudança de escopo "no calor" sem aditivo formal.** *Correção:* protocolo de change request sempre, mesmo em pedido pequeno.
- **Parcelamento 10/90 ou "tudo no final".** *Correção:* 30/40/30 protege o aluno.
- **Não definir o que NÃO faz.** *Correção:* a lista de exclusões no Anexo I é tão importante quanto a lista de inclusões.
- **Misturar custo das APIs no valor do projeto.** *Correção:* APIs são da Contratante, em conta dela. O aluno ajuda a provisionar, mas não é responsável pelo custo.
- **Esquecer SLA.** *Correção:* SLA do projeto (item 11) é diferente do SLA da manutenção (módulo 03). Ambos precisam estar definidos.

## Checklist de saída

- [ ] Aluno tem template de contrato de projeto com 12 cláusulas + Anexo I + Anexo II.
- [ ] Aluno entendeu a função de cada parcela do 30/40/30.
- [ ] Aluno tem template de aditivo de change request (1 página).
- [ ] Aluno consegue listar 5 itens que tipicamente são pedidos como "só mais um detalhe" e merecem aditivo.
- [ ] Aluno sabe que APIs de terceiros são da Contratante, em conta dela.
