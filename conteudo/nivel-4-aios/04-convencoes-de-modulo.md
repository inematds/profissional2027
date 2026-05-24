# Convenções de Módulo AIOS

> Como cada módulo é organizado para garantir consistência editorial, navegabilidade e reuso por terceiros.

---

## Estrutura padrão de um módulo P1 (completo)

Cada módulo prioridade 1 mora em uma pasta com **3 arquivos obrigatórios**:

```
modulos-p1/modulo-<nome>/
├── README.md          ← Manual prático do módulo
├── system-prompt.md   ← System prompt pronto, com placeholders
└── playbook.md        ← Playbook operacional do champion
```

Nada mais. Quem precisar de mais arquivos (diagrama exportado, JSON do workflow n8n, vídeo) coloca em `anexos/` mas eles são opcionais.

---

## Estrutura padrão de um módulo P2 (especificação)

Cada módulo prioridade 2 mora em uma pasta com **1 arquivo obrigatório**:

```
modulos-p2/modulo-<nome>/
└── README.md          ← Especificação resumida (700–1.200 palavras)
```

Em V2 da biblioteca, esses módulos ganharão system-prompt e playbook.

---

## Esqueleto do README de módulo P1

Toda README de P1 segue esta ordem de seções, com o conteúdo adaptado:

1. **Nome e versão** no cabeçalho
2. **O que faz** (1 frase + 1 parágrafo)
3. **Quando usar**
4. **Quando NÃO usar**
5. **Resultado esperado** (métricas e ROI típico)
6. **Stack utilizada**
7. **Arquitetura** (diagrama textual)
8. **Nível HITL recomendado**
9. **Custo operacional estimado**
10. **Tempo de implementação**
11. **Métricas obrigatórias**
12. **Erros comuns e como evitar**
13. **Variações por vertical**
14. **Mapeamento para DPIA**

Esse esqueleto é o **contrato editorial** da biblioteca. Mudanças de esqueleto entram via decisão registrada em `DECISOES.md`.

---

## Esqueleto do README de módulo P2

P2 é especificação enxuta. Seções:

1. **Nome e versão** no cabeçalho
2. **O que faz**
3. **Quando usar**
4. **Stack utilizada**
5. **Arquitetura sumária**
6. **Nível HITL recomendado**
7. **Tempo estimado de construção** (para a V2 desta biblioteca)
8. **Pré-requisitos** (módulos P1 que precisam estar dominados antes)

Sem custos detalhados, sem variações por vertical, sem playbook. Esses entram em V2.

---

## Convenção de placeholders nos system-prompts

Placeholders sempre em **maiúsculas, entre chaves duplas, snake_case**:

```
{{NOME_EMPRESA}}
{{TOM}}
{{HORARIO_FUNCIONAMENTO}}
{{CIDADE}}
{{CATALOGO_REFERENCIA}}
{{NOME_CHAMPION}}
{{WHATSAPP_CHAMPION}}
{{NOME_DONO}}
```

Convenções específicas:

- `{{LISTA_*}}` para listas (ex: `{{LISTA_VIP}}`, `{{LISTA_PRODUTOS_SAZONAIS}}`) — estas geralmente vão para **base de conhecimento (RAG)**, não direto no prompt.
- `{{REGRA_*}}` para regras numeradas vindas do Mapa de Processos (ex: `{{REGRA_RN_01}}`).
- `{{TOOL_*}}` para nomes de ferramentas (ex: `{{TOOL_CONSULTAR_TABELA}}`) — caso queira renomear conforme nomenclatura do cliente.

Placeholder deixado no prompt em produção é bug bloqueante. Linter do n8n deve travar deploy se encontrar `{{`.

---

## Convenção de versionamento

Cada arquivo tem no cabeçalho:

```
Versão: X.Y — DD/mês/AAAA
```

- **X** muda em revisão estrutural (esqueleto mudou).
- **Y** muda em revisão de conteúdo (regras, exemplos, métricas).
- Data sempre presente.

Repositório oficial AIOS no inema.club mantém histórico Git. Mudanças no `main` passam por revisão editorial.

---

## Convenção de tom

- PT-BR direto, técnico.
- **Sem primeira pessoa** (nem singular nem plural — não escrevemos "vamos ver" ou "eu recomendo"). Voz institucional.
- Sem emoji.
- Sem exagero ("revolucionário", "incrível", "transformador").
- Frases curtas. Quando uma frase passa de 3 linhas, quebrar.
- Listas com bullets pontuais, não decorativas.
- Citar Pesquisa A, B, C, D, E, F, G ou DPIA Canônico quando aplicável — não inventar fontes.

---

## Convenção de exemplos

Os 3 casos de referência da biblioteca:

- **Polaris Bebidas** (distribuidora, Joinville/SC) — usada em módulos de Atendimento, Pedidos, Pós-venda.
- **Clínica MoveBem** (fisioterapia, Curitiba/PR) — usada em módulos de Agendamento e Cobrança no-show.
- **Contabilidade Verbo** (escritório contábil, São Paulo/SP) — usada em módulos de Cobrança recorrente, Triagem.

Esses três aparecem como exemplos preenchidos nos system-prompts e playbooks. **Não criar nomes novos** sem necessidade — consistência ajuda navegação.

---

## Convenção de métricas

Toda métrica tem:

- **Nome** (ex: "Tempo médio de resposta no WhatsApp")
- **Unidade** (minutos, %, R$, ocorrências/dia)
- **Fonte do dado** (de onde sai — n8n log, ERP, Supabase)
- **Frequência de medição** (real-time, diária, mensal)
- **Meta** (vinda do baseline assinado em D + KPI futuro)

Métrica sem unidade nem fonte é decorativa. Não entra.

---

## Convenção de "Quando NÃO usar"

Seção obrigatória em todo módulo P1. Diferente de "limitações", "quando não usar" é **lista positiva de critérios de descarte**:

- Cliente com perfil X → não use este módulo.
- Volume abaixo de Y → não compensa.
- Falta integração Z → bloqueia.
- Existe restrição regulatória W → outro caminho.

Forçar a equipe a escrever "quando não usar" é forçar honestidade sobre escopo. Módulos AIOS sem essa seção são incompletos.

---

## Convenção para variações por vertical

Em módulos P1, cada um traz **adaptações sumárias** para os 3 verticais-modelo (distribuidoras, clínicas, contábil) quando faz sentido. Não é manual completo da vertical — é nota editorial com:

- "Para distribuidora: o que muda no prompt"
- "Para clínica: o que muda no playbook"
- "Para contábil: integrações típicas"

Manuais completos por vertical entram em **Nível 5 da biblioteca** (não nesta versão).

---

## Convenção de licença e atribuição

Toda biblioteca AIOS é publicada com:

> Licença: aberta, gratuita, livre para adaptação com atribuição a **Profissional 2027 / INEMA.CLUB**.

Implementador pode adaptar, traduzir, vender consultoria usando os módulos. Não pode revender o material em si como produto fechado próprio.

---

## Como contribuir com a biblioteca

V1 é editorial — fechada, curada por Nei. A partir de V2:

- Contribuições via canal próprio do inema.vip.
- Toda contribuição é creditada.
- Revisão editorial valida: estrutura, conformidade com DPIA, evidência de campo (≥ 1 cliente real rodando).
- Aprovação publica no repositório oficial.

Módulos contribuídos que não passem na revisão ficam em pasta `contribs-pendentes/` com feedback.

---

*Convenções AIOS v1.0 — sujeitas a revisão a cada release maior.*
