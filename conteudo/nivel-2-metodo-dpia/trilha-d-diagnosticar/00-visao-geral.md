---
nivel: 2
trilha: D
modulo: 0
titulo: Visão Geral da Trilha D — Diagnosticar
tempo_leitura: 12
exit_criteria: aluno explica em até 3 minutos, sem consultar material, o que a fase D entrega, por que o baseline assinado é exit-gate, e como a fase D filtra cliente.
---

# Visão Geral da Trilha D — Diagnosticar

## O que você vai aprender
- O que a fase D entrega ao cliente e ao próprio implementador.
- Por que o baseline assinado é o exit-gate sem o qual nenhum projeto DPIA avança.
- Como a fase D opera, na prática, como filtro de cliente — quem não passa por D não é cliente, é dor de cabeça futura.
- A diferença entre dor declarada e dor real, e por que esse contraste define todo o restante do projeto.
- Como D se conecta com as fases P, I e A, e o que acontece quando se pula etapas.

## Por que isso importa
Sessenta e um por cento das PMEs brasileiras contrataram plataforma de IA sem processo definido e 83% dessas plataformas viraram prateleira (Nautis 2026, Pesquisa A §2.5). A Gartner registrou em 2025 que 85% das falhas em IA estão ligadas a dados e processo, não a modelo. A RAND mediu em 2026 que 80,3% dos projetos sem clareza no caso de uso falham (Pesquisa D §4). Em todos esses números há uma origem comum: ausência ou superficialidade da fase de diagnóstico. A fase D existe para inverter essa estatística. Quem aplica D corretamente não vende um sistema — vende um projeto sustentável com baseline mensurável, KPI assinado, reposicionamento decidido e proposta aceita.

D também sustenta o Princípio 1 do método (processo antes de ferramenta) e o Princípio 2 (sem baseline, sem projeto). Os dois primeiros princípios do DPIA têm exit-gate na fase D. Quem ignora D viola os fundamentos do método, sem exceção.

## O passo a passo
A trilha D tem seis sub-etapas operacionais e seis módulos pedagógicos. Os módulos seguem o fluxo real de campo:

1. **Preparação pré-diagnóstico (módulo 01).** Antes de pisar no cliente, o implementador estuda a empresa, escolhe o nicho de entrada, prepara materiais e decide a postura da reunião inicial.
2. **Roteiro de entrevista (módulo 02).** Como conduzir a primeira conversa estruturada com o dono e com os operadores. A entrevista é conduzida — não improvisada.
3. **Mapeamento de dor real (módulo 03).** Como separar o que o dono diz da empresa (dor declarada) do que de fato acontece no chão (dor real). Aqui o SPIN aplicado e o walk-the-floor entram em cena.
4. **Baseline numérico (módulo 04).** Tempo, volume, taxa de erro, custo. Sem chute. Sem "depois a gente mede". Cálculo de ROI esperado a partir de números reais.
5. **Relatório de diagnóstico (módulo 05).** Como entregar o relatório que vira a base contratual. Estrutura, narrativa, defesa em reunião, assinatura do baseline.
6. **Encerramento da fase com exit-gate.** Seis itens marcados; sem todos, não se passa para P.

Cada módulo da trilha tem um exemplo aplicado ao case Polaris Bebidas (Joinville/SC), o case-fio do manual canônico. Estudar com um único case do início ao fim cria continuidade pedagógica e simula o que o aluno vai viver no campo: um cliente real, do começo ao fim, com nome e contexto.

## Exemplo aplicado — Polaris Bebidas (Joinville/SC)
A Polaris Bebidas é uma distribuidora de bebidas em Joinville (SC), 14 funcionários, faturamento aproximado R$ 8 milhões/ano, ERP Bling, atendimento todo no WhatsApp pessoal de dois vendedores internos. O dono atende celular até 21h. O processo candidato à automação inicial é o **atendimento de pedido recebido no WhatsApp** — desde a saudação inicial até a criação do pedido no Bling.

Antes da fase D, o dono diz: "Quero automatizar o WhatsApp." Depois da fase D (que o aluno conduz nas próximas 1-2 semanas), o dono assina um documento que registra: tempo médio para concluir um pedido (47 minutos), volume mensal (≈ 480 pedidos), taxa de retrabalho por erro de SKU (11%), custo de hora-operador-vendedor (R$ 38/hora), e KPIs-meta de 90 dias (tempo de resposta cai para < 5 min, taxa de erro cai para < 3%, conversão sobe 8 pontos). Esse documento, **assinado**, é o exit-gate da fase D.

Sem ele, ninguém sabe se o projeto melhorou nada. Com ele, em 90 dias o implementador tem o slide 1 da apresentação trimestral — o slide que separa consultor sério de consultor amador, conforme o manual canônico em "Indicadores e medição".

## Erros comuns
- **Tratar a fase D como reunião de discovery.** D não é uma conversa exploratória; é um trabalho de campo com produto entregue. Quando o aluno trata como discovery, sai sem baseline e sem proposta.
- **Pular o pagamento da fase D para "fechar mais rápido".** Diagnóstico gratuito recorrente sinaliza junior e atrai cliente que não quer pagar. Gratuito apenas no piloto 1 (Pesquisa F §10.2).
- **Diagnosticar dois ou três processos ao mesmo tempo.** DPIA é por processo. Cada processo é outro DPIA, outra fase D, outra proposta.
- **Aceitar "depois a gente mede" para o baseline.** O cliente nunca volta com os números. O implementador mede ele mesmo, na fase D, com cronômetro e exportação.
- **Sair com baseline sem assinatura.** Baseline não assinado vira "ah, mas era diferente" no relatório 90 dias depois. Assinatura é defesa do implementador.

## Checklist de saída
- [ ] Sabe explicar a diferença entre dor declarada e dor real.
- [ ] Sabe enumerar os seis itens do exit-gate de D sem consultar.
- [ ] Sabe justificar por que D é fase paga, não pré-venda gratuita.
- [ ] Sabe explicar por que reposicionamento é decisão de design no dia zero, não consequência da entrega.
- [ ] Sabe identificar três sinais de cliente que não passa pela fase D.

## Vai além
- Manual canônico, seção "Fase D — Diagnosticar" — leitura obrigatória paralela à trilha.
- `metodo/templates/template-diagnostico-roteiro-entrevista.md` — usar como base do módulo 02.
- `metodo/templates/template-diagnostico-relatorio.md` — estrutura do documento final.
- `metodo/templates/template-baseline-assinado.md` — formulário oficial do exit-gate.
- Exercício prático sugerido: aplicar a estrutura da trilha D a uma empresa real do círculo do aluno (parente, ex-empregador, vizinho de bairro), sem cobrar, antes do primeiro cliente piloto.
