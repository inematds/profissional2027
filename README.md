# Profissional 2027

Programa gratuito de formação de **Implementadores de Soluções de IA** para pequenas e médias empresas brasileiras.

Construído pela comunidade [inema.vip](https://inema.vip) e publicado em texto direto e prático na plataforma [inema.club](https://inema.club).

> *"Não ensinamos a colecionar ferramentas. Formamos profissionais para sair da curiosidade sobre IA e entrar no mercado real: diagnosticar pequenas empresas, vender soluções simples, implementar assistentes personalizados e treinar clientes para vender mais, atender melhor e trabalhar com menos burocracia."*

> *"Ferramenta é commodity. Processo é o ativo."*

## Sobre o programa

- **Responsável e curador:** Nei Maldaner — 40 anos de TI, fundador do SISNEMA (100 mil profissionais de TI formados).
- **Modelo:** 100% gratuito, open source. O programa não vende; o aluno vende.
- **Distribuição:** comunidade inema.vip + plataforma inema.club + redes sociais (500k seguidores).
- **Método:** **DPIA** — Diagnosticar → Processar → Instruir → Automatizar, com Acompanhamento como loop externo de manutenção.
- **Duração:** 10–12 semanas, ~100 horas.
- **Certificação:** mediante entrega de 1 diagnóstico pago real + 1 piloto em PME real.
- **Foco geográfico:** Brasil, português brasileiro.
- **Modelo de relação:** mercado aberto, sem franquia, sem selo restritivo.

## Estrutura do repositório

```
.
├── PLANO.md                          Plano-mestre do projeto
├── DECISOES.md                       10 decisões estratégicas do Nei
│
├── pesquisa/                         Fundamentação de mercado
│   ├── 00-sintese-transversal.md     Documento-espinha-dorsal
│   ├── 01-mercado-pme-brasil.md      Mercado e dor real
│   ├── 02-metodos-frameworks.md      Frameworks e design AIOS
│   ├── 03-perfis-e-concorrencia.md   Cargos emergentes e concorrentes
│   ├── 04-casos-e-falhas.md          Casos reais e padrões de falha
│   ├── 05-stack-tecnico.md           Stack canônica
│   ├── 06-comercial-e-vendas.md      Pricing, venda, contratos, LGPD
│   └── 07-treinamento-cliente.md     Adoção e sustentação
│
├── metodo/
│   ├── dpia-canonico.md              Manual canônico do método
│   └── templates/                    9 templates operacionais
│
└── conteudo/
    ├── nivel-1-manifesto/            6 arquivos — manifesto, tese, perfil, oportunidade
    ├── nivel-2-metodo-dpia/          24 arquivos — 4 trilhas (D, P, I, A) × 6 módulos
    ├── nivel-3-aplicacao/            18 arquivos — venda, contrato, sustentação
    └── nivel-4-aios/                 29 arquivos — biblioteca de módulos prontos
```

## Estado atual

| Camada | Status | Arquivos | Palavras |
|--------|--------|---------:|---------:|
| Pesquisa de mercado (7 frentes) | ✅ Completa | 8 | ~55.000 |
| Síntese transversal | ✅ Completa | 1 | 5.332 |
| DPIA canônico + templates | ✅ Completo | 10 | ~22.000 |
| Nível 1 — Manifesto | ✅ Completo | 6 | 11.274 |
| Nível 2 — Trilhas DPIA | ✅ Completo | 24 | 34.108 |
| Nível 3 — Aplicação | ✅ Completo | 18 | 31.195 |
| Nível 4 — Biblioteca AIOS | ✅ Completo | 29 | ~36.900 |
| **Total** | | **~96** | **~182.000** |

## Stack canônica do programa

- **LLM:** Claude Sonnet 4.6 (com prompt caching) + Claude Haiku 4.5 para custo.
- **Orquestração:** n8n self-hosted em Hetzner (~US$ 5/mês).
- **Memória:** Supabase com pgvector quando necessário.
- **Mensageria:** WhatsApp Cloud API oficial (Meta).
- **ERPs locais:** integração via REST com Bling, Omie, Conta Azul.
- **Pagamentos:** PIX, integração via API.
- **Fiscal:** NF-e/NFS-e via provedores brasileiros.

## Princípios

1. **Ferramenta é commodity. Processo é o ativo.**
2. Diagnóstico antes de implementação. Sem baseline assinado, não há projeto.
3. Mercado tripartite: diagnóstico pago + projeto fixo + manutenção recorrente.
4. Human-in-the-loop em 4 níveis (Assistir, Co-piloto, Autopiloto-com-exceções, Autônomo).
5. Treinamento do cliente entra no contrato, não é venda extra.
6. Cláusula de transição sem lock-in em todo contrato (princípio anti-dependência).
7. Stack componível, sobrevive à troca de modelo.

## Licença

Conteúdo livre para uso, adaptação e redistribuição por qualquer pessoa ou comunidade.
