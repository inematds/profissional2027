# Template — Relatório Mensal de Acompanhamento

> **Como usar.** Relatório entregue todo mês ao dono do cliente no escopo do contrato de manutenção. Função tripla: (1) provar que o sistema continua entregando contra o baseline assinado; (2) documentar evolução do playbook e do agente; (3) sustentar a renovação do contrato. **É o documento que mais defende o contrato de manutenção em momentos de aperto financeiro do cliente** — quando o dono pensa em cortar custos, este relatório é o que separa "isso traz resultado" de "isso é mais um custo". Tamanho: 4 a 8 páginas. Formato: PDF entregue por e-mail + apresentação presencial ou videochamada nos 5 primeiros dias úteis do mês subsequente. Mantenha as mesmas seções de mês a mês — a consistência é o que constrói leitura. Cláusula 4ª do contrato de manutenção exige este relatório.

---

# Relatório Mensal — Sistema [NOME] — [CLIENTE]

**Mês de referência:** [mês/ano]
**Tier:** [Básico / Intermediário / Avançado]
**Implementador:** [nome]
**Champion:** [nome]
**Data de entrega:** [data]
**Próxima reunião de apresentação:** [data, horário, local/link]

---

## 1. Resumo executivo (1 parágrafo)

[Em 4 a 6 frases: como foi o mês, principais números, principais aprendizados, próximo passo recomendado. Se o dono ler só isso, precisa ter ido bem comunicado.]

---

## 2. Métricas do mês contra Baseline

Apresentado em tabela única, com a mesma estrutura todo mês. Cor verde quando ≥ meta, amarelo quando entre 70–100% da meta, vermelho quando < 70%.

| Métrica | Baseline (fase D) | Meta 90d | Mês atual | Mês anterior | Variação | Status |
|---------|-------------------|-----------|------------|---------------|-----------|--------|
| Tempo médio de resposta | [valor] | [valor] | [valor] | [valor] | [+/-%] | verde/amarelo/vermelho |
| Volume processado pelo agente (%) | 0% | [valor]% | [valor]% | [valor]% | [+/-pp] | [...] |
| % override humano | — | ≤ [valor]% | [valor]% | [valor]% | [+/-pp] | [...] |
| Receita capturada / Custo evitado | [referência baseline] | [valor]/mês | [valor] | [valor] | [+/-%] | [...] |
| Taxa de erro de [processo] | [valor]% | [valor]% | [valor]% | [valor]% | [+/-pp] | [...] |

**Comentário sobre as métricas:**
[Explicar qualquer desvio relevante. Não esconder números ruins — declarar e propor ação.]

---

## 3. ROI acumulado

| Mês | Ganho mensal observado | Acumulado | Investimento inicial restante a recuperar |
|-----|-------------------------|-----------|--------------------------------------------|
| [Mês 1] | R$ [...] | R$ [...] | R$ [...] |
| [Mês 2] | R$ [...] | R$ [...] | R$ [...] |
| [Mês atual] | R$ [...] | R$ [...] | R$ [...] |

**Payback alcançado em:** [mês X] (ou "previsto para mês X" se ainda não alcançado).

**Comentário sobre ROI:** [...]

---

## 4. Operação do mês

### 4.1 Volume e cobertura
- Total de interações: [n]
- Tratadas pelo agente sem escalonamento: [n] ([%])
- Escaladas para humano por gatilho automático: [n] ([%])
- Escaladas por baixa confiança: [n] ([%])
- Override humano após resposta do agente: [n] ([%])

### 4.2 Incidentes
| Data | Severidade | Descrição | Tempo de resolução | Ação corretiva |
|------|------------|------------|---------------------|------------------|
| [...] | crítico/médio/baixo | [...] | [...] | [...] |

[Se nenhum incidente: declarar explicitamente. "Mês sem incidente crítico" é informação relevante.]

### 4.3 Plano de rollback
- [ ] Foi acionado este mês? Sim/Não. Se sim, [descrição].
- [ ] Foi testado este mês (simulação programada)? Sim/Não.

---

## 5. Ajustes realizados no agente

### 5.1 System prompt
- Versões publicadas: [v1.x → v1.y]
- Principais alterações: [...]

### 5.2 Base de conhecimento
- Documentos adicionados: [n] — [descrição]
- Documentos atualizados: [n] — [descrição]

### 5.3 Mapa de Processos
- Novas regras formais (RN-XX): [...]
- Novas regras tácitas (RT-XX) descobertas em produção: [...]
- Novos casos-armadilha (CA-XX): [...]

### 5.4 Matriz HITL
- Subidas de nível propostas: [...] — aprovadas/pendentes
- Reduções de nível por evidência adversa: [...]

### 5.5 Playbook do Agente
- Versão publicada: [v.x]
- Seções revisadas: [...]

---

## 6. Treinamento e champion

- Champion: [nome] — segue ativo? Sim/Não.
- Horas de treinamento conduzidas com o champion no mês: [n]
- Reuniões realizadas (visitas presenciais + check-ins remotos): [n] de [n previstos no tier]
- Micro-vídeos novos gravados no mês: [n] — totalizando [N] na biblioteca.
- Outras pessoas do time que passaram a operar: [...]

---

## 7. Sinais precoces de queda (auto-monitoramento)

Status de cada indicador-bandeira:

- [ ] Uso do agente caiu mais de 20% mês a mês? **[Não]** / [Sim — descrever]
- [ ] Champion segue na empresa? **[Sim]** / [Não — gatilho de visita extraordinária]
- [ ] Override humano subiu acima de 30%? **[Não]** / [Sim — descrever]
- [ ] Playbook foi atualizado nos últimos 30 dias? **[Sim]** / [Não — atualizar imediatamente]
- [ ] Operadores voltaram a usar canal antigo (WhatsApp pessoal, planilha)? **[Não]** / [Sim — descrever]

Qualquer "Sim" nos três primeiros ou "Não" nos dois últimos gera **plano de recuperação** na seção 9.

---

## 8. O que melhorou neste mês

[Lista curta — 3 a 5 itens — de ganhos qualitativos do mês. Exemplo: "Cristiane operou semana inteira sem precisar acionar implementador"; "Cliente Casa do Carlos elogiou velocidade do atendimento espontaneamente"; "Volume de pedidos novos cresceu 14%".]

---

## 9. Recomendações e próximo mês

### 9.1 Ações para o próximo mês
- [ ] [Ação 1 — responsável, prazo]
- [ ] [Ação 2 — responsável, prazo]

### 9.2 Plano de recuperação (se aplicável — Seção 7)
[Descrever o quê + prazo + responsável.]

### 9.3 Recomendação estratégica ao dono
[1 parágrafo. Pode ser: continuar como está; subir nível HITL em alguma ação; iniciar próximo ciclo DPIA em outro processo; revisar pricing do cliente final aproveitando a velocidade nova; etc.]

---

# Exemplo preenchido — Polaris Bebidas — Junho/2026 (1º mês pós-deploy)

**Mês:** junho/2026. **Tier:** Intermediário. **Champion:** Cristiane Bertoldi. **Entrega:** 03/julho/2026.

## 1. Resumo executivo

Primeiro mês completo pós-deploy. Sistema entrou em produção em 03/jun e operou estável por 28 dias. **Todas as 4 metas de 90 dias foram atingidas já no primeiro mês**, com destaque para a recuperação de R$ 26.400 em receita que vinha sendo perdida por demora no atendimento. Adoção pelo agente alcançou 78%, acima da meta de 70%. Cristiane operou a semana 4 inteira sem acionar suporte do implementador. Nenhum incidente crítico no mês. Recomendação: manter operação atual no próximo mês, avaliar subida de HITL para nível 4 em "confirmação de status de pedido conhecido" e iniciar conversa sobre 2º ciclo DPIA (cobrança de inadimplentes).

## 2. Métricas

| Métrica | Baseline | Meta 90d | Jun/2026 | Mai/2026 | Status |
|---------|----------|-----------|-----------|-----------|--------|
| Tempo médio resposta | 47 / 11 min | ≤ 5 min | **3,1 min** | — | ✓ verde |
| Volume pelo agente | 0% | ≥ 70% | **78%** | — | ✓ verde |
| % override humano | — | ≤ 15% | **11%** | — | ✓ verde |
| Receita capturada/mês | R$ 0 (perda R$ 33.440) | R$ 22.000 | **R$ 26.400** | — | ✓ verde |
| Taxa erro pedido | 4,2% | ≤ 2,5% | **2,1%** | — | ✓ verde |

**Comentário:** todas as metas atingidas no 1º mês. Resultado acima da expectativa — atribuído a (i) pacote de contexto bem-feito na fase P (60+ regras tácitas capturadas no walk-the-floor), (ii) Cristiane assumir champion com energia desde a semana 1, (iii) job shadowing presencial 5 dias seguidos.

## 3. ROI

| Mês | Ganho observado | Acumulado | Restante a recuperar |
|-----|------------------|-----------|------------------------|
| Jun | R$ 26.400 (receita) + R$ 3.040 (operacional) - R$ 3.300 (custo agente) = **R$ 26.140** | R$ 26.140 | R$ 42.000 − R$ 26.140 = R$ 15.860 |

Payback projetado para **mês 2 (julho)**, considerando manutenção do ritmo.

## 4. Operação

- Total de interações jun: 6.247.
- Tratadas pelo agente sem escalonamento: 4.873 (78%).
- Escaladas por gatilho automático: 821 (13%).
- Escaladas por baixa confiança: 187 (3%).
- Override humano: 366 (6%).

**Incidentes:**
| Data | Severidade | Descrição | Resolução | Ação |
|------|------------|------------|-----------|------|
| 09/jun | médio | Agente respondeu R$ 178 para Heineken Long Neck — preço da caixa 600ml chumbinho | 8 min — Cristiane corrigiu na thread | Ajuste no glossário Bloco 3 do system prompt |
| 22/jun | médio | Agente cobrando preço cheio Heineken 0,0 com promoção de inverno ativa | dia mesmo | Atualização RN-02 c/ tabela sazonal |

Plano de rollback testado em 22/jun (simulação programada — 11 min). Não acionado em produção.

## 5. Ajustes

- System prompt: v1.0 (03/jun) → v1.1 (10/jun) → v1.2 (22/jun). Principais: glossário Heineken; tabela sazonal; reforço RT-04 (validade sempre escala).
- Base de conhecimento: +6 documentos (catálogo atualizado, política sazonal de inverno, lista VIP atualizada com 2 novos).
- Mapa de Processos v1.3: +1 RT-05 ("cliente interior aceita boleto 21 dias").
- Matriz HITL: sem alteração. Avaliação para julho: subir "confirmar status de pedido conhecido" de nível 3 para 4.
- Playbook: v1.3, seções 6 e 7 revisadas com Cristiane.

## 6. Treinamento

- Cristiane: ativa, com energia. 6h de treinamento total no mês (5 dias job shadowing + 1 sessão sobre uso do diário de aprendizado).
- 2 visitas presenciais realizadas (07/jun e 21/jun). 2 check-ins remotos (14/jun, 28/jun). 100% das reuniões previstas no tier intermediário.
- 6 micro-vídeos novos gravados (biblioteca total: 6).
- Felipe acompanhou as visitas — em 6 meses migra para financeiro interno conforme combinado em D.

## 7. Sinais precoces — todos OK

- [x] Uso do agente caiu > 20%? **Não**.
- [x] Champion na empresa? **Sim**.
- [x] Override > 30%? **Não** (11%).
- [x] Playbook atualizado < 30 dias? **Sim** (v1.3 em 22/jun).
- [x] Operadores voltaram ao canal antigo? **Não**.

## 8. O que melhorou

- Cristiane operou semana 4 sem acionar implementador — sinal claro de propriedade.
- Carlos da Casa do Carlos comentou espontaneamente no telefone com Reinaldo: "o atendimento de vocês ficou muito mais rápido."
- Volume de pedidos novos por dia cresceu 14% — sinal de que clientes que perdíamos por demora voltaram.
- Reinaldo já mencionou estender o método para cobrança.

## 9. Próximo mês

### Ações para julho
- [ ] Subir HITL: "confirmar status de pedido conhecido" para nível 4 — Cristiane + implementador na visita de 07/jul.
- [ ] Atualizar lista VIP (+ 2 nomes novos) — Cristiane, até 05/jul.
- [ ] 1 micro-vídeo novo (caso "cliente novo do interior") — implementador, até 21/jul.
- [ ] Conversa inicial com Reinaldo sobre 2º ciclo DPIA (cobrança) — visita 21/jul.

### Recomendação estratégica
Mês 1 indica que o sistema está estável, o time absorveu bem, e há capacidade adicional do champion. Recomendamos: iniciar avaliação de um 2º ciclo DPIA sobre cobrança de inadimplentes (R$ 47k em atraso > 30 dias no Bling — potencial recuperação significativa). Diagnóstico paga (R$ 4.500), começa após 90 dias do projeto atual estabilizado.

---

## Dicas de uso

- **Mesma estrutura todo mês.** A consistência é o que constrói leitura. Não reinvente o relatório a cada mês.
- **Apresente presencialmente quando possível.** Relatório por e-mail vira anexo não-aberto; reunião de 30 min em torno do PDF vira contrato renovado.
- **Sem maquiagem.** Mês ruim é mês ruim. Esconder vira descoberta dolorosa 3 meses depois. Declarar + plano de recuperação no mesmo relatório constrói confiança.
- **Status colorido funciona.** Verde/amarelo/vermelho é universal e o dono lê em 10 segundos.
- **Recomendação estratégica é a parte mais negligenciada — e mais valiosa.** É onde o implementador deixa de ser "operador" e vira "consultor". É o que diferencia tier Intermediário/Avançado de Básico aos olhos do dono.
- **Use o relatório como insumo para pedir indicações.** Mês com resultado bom é mês para perguntar "quem mais você conhece com problema parecido?". Ver Pesquisa F §10.2 — estratégia cliente #3.
