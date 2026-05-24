---
nivel: 2
trilha: A
modulo: 1
titulo: Stack de Automação
tempo_leitura: 20
exit_criteria: aluno provisiona ambiente de produção em VPS Hetzner com n8n self-host, HTTPS, backup, e workflow rodando webhook WhatsApp Cloud API com SLA documentado.
---

# O que você vai aprender
- A arquitetura canônica do método: n8n self-host na Hetzner + Supabase + WhatsApp Cloud API + Claude.
- Como provisionar VPS Hetzner, configurar n8n com HTTPS e backup automatizado.
- Padrões de workflow n8n para webhook WhatsApp em PME (com retry, queue, e fallback).
- Como integrar WhatsApp Cloud API direto da Meta (sem BSP no caminho crítico).
- Como configurar observabilidade básica (logs, métricas, alertas).

## Por que isso importa
Princípio 7 do DPIA: stack composable, sem lock-in. Pesquisa E §10 e Pesquisa A §2.5 mostram que n8n self-host + Claude + Supabase + WABA direto é a stack que cabe no custo da PME (R$ 550 a R$ 1.750/mês de infra total, Pesquisa F §1.4) e ainda dá ao implementador independência total. Plataforma proprietária no caminho crítico amarra o implementador à plataforma e o cliente ao fornecedor. Quando a plataforma sobe preço ou descontinua, perde-se cliente.

A stack canônica é também o que a comunidade pode contribuir. Pesquisa C §6 e §7: a comunidade longitudinal (inema.vip) é defesa estrutural — alunos contribuem com módulos AIOS testados em campo, network effect de conteúdo. Stack proprietária não acumula contribuição.

## O passo a passo

**1. Provisionar VPS Hetzner.**
- Tipo CX22 (2 vCPU, 4 GB RAM): atende projetos com até ~300 interações/dia. R$ ~45/mês.
- Tipo CX32 (4 vCPU, 8 GB RAM): atende ~1.000 interações/dia. R$ ~90/mês.
- Localização: Nuremberg ou Helsinki (latência ~250ms para Brasil — aceitável para webhook async).
- OS: Ubuntu 22.04 LTS.
- Hostname: `n8n-<cliente>.<seu-dominio>.com`.

**2. Hardening básico.**
- Usuário non-root, chave SSH (sem senha).
- UFW: portas 22 (SSH), 80 (HTTPS redirect), 443 (HTTPS).
- Fail2ban configurado.
- Sem login root, sem senha SSH.
- Backup snapshot diário Hetzner (R$ ~10/mês adicional).

**3. Instalar n8n via Docker.** Configuração mínima:
```yaml
# docker-compose.yml
services:
  postgres:
    image: postgres:16
    environment:
      POSTGRES_USER: n8n
      POSTGRES_PASSWORD: <senha>
      POSTGRES_DB: n8n
    volumes:
      - postgres-data:/var/lib/postgresql/data
    restart: always

  n8n:
    image: n8nio/n8n:latest
    environment:
      DB_TYPE: postgresdb
      DB_POSTGRESDB_HOST: postgres
      DB_POSTGRESDB_USER: n8n
      DB_POSTGRESDB_PASSWORD: <senha>
      DB_POSTGRESDB_DATABASE: n8n
      N8N_HOST: n8n-<cliente>.<seu-dominio>.com
      N8N_PROTOCOL: https
      WEBHOOK_URL: https://n8n-<cliente>.<seu-dominio>.com/
      GENERIC_TIMEZONE: America/Sao_Paulo
      N8N_BASIC_AUTH_ACTIVE: "true"
      N8N_BASIC_AUTH_USER: <user>
      N8N_BASIC_AUTH_PASSWORD: <senha-forte>
    volumes:
      - n8n-data:/home/node/.n8n
    depends_on:
      - postgres
    restart: always

  caddy:
    image: caddy:2
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - caddy-data:/data
    restart: always
```

Caddyfile:
```
n8n-<cliente>.<seu-dominio>.com {
  reverse_proxy n8n:5678
}
```

Caddy resolve HTTPS via Let's Encrypt automaticamente.

**4. Backup do n8n.**
- Backup do banco Postgres do n8n diário (workflows + credenciais).
- Snapshot Hetzner diário do VPS inteiro.
- Backup do Supabase (incluso no plano Pro Supabase).
- Retenção: 30 dias rotativos.

Sem backup, perda de workflow do n8n é desastre — uma semana de configuração some.

**5. Configurar WhatsApp Cloud API direto da Meta.**
- Conta Meta Business Manager do cliente (não do implementador).
- App Meta criado na conta do cliente, com produto "WhatsApp" adicionado.
- Número de telefone do cliente verificado no Meta WABA.
- Template de mensagens submetidos para aprovação (cumprimento, espera, encerramento, escalonamento, erro técnico). Aprovação Meta: 24-72h.
- Webhook configurado apontando para `https://n8n-<cliente>...com/webhook/<id>`.
- Verify token configurado.

Decisão estratégica: WABA é da conta Meta do cliente, não do implementador. Quando o cliente troca de implementador, o WABA fica com ele. Isso é defesa do cliente — mas também é defesa do implementador (nunca ser preso a uma estrutura que não é sua).

**6. Padrões de workflow para webhook WhatsApp.**

Componentes essenciais de todo workflow WABA em PME:
- **Webhook receiver** com verify token + assinatura.
- **Função de classificação** rápida (Haiku ou regex) para descartar mensagens não-relevantes (ack, sticker isolado, mensagem de status).
- **Queue interna** quando volume sobe (n8n Queue Mode com Redis).
- **Loop de tool use** com Claude.
- **Retry policy** em chamadas externas (Bling, Pix): 3 tentativas com backoff 2s/5s/10s.
- **Logging estruturado** em Supabase a cada interação completa.
- **Alerting** via webhook para Slack/Discord/WhatsApp interno em condições anormais.

**7. Observabilidade básica.**

Métricas a expor (em dashboard simples, Grafana ou Supabase Dashboard):
- Interações/hora.
- Taxa de escalonamento (rolling 24h).
- Taxa de erro (rolling 24h).
- Latência p50 / p95 do LLM.
- Latência p50 / p95 do Bling.
- Tokens consumidos / dia.

Alertas:
- Taxa de erro > 5% em janela de 1 hora.
- Latência LLM p95 > 12s.
- Bling indisponível > 5 min seguidos.
- Tokens consumidos > 2x o esperado em janela de 24h (sinal de loop ou abuso).

**8. SLA documentado.** Documento de 1 página que o cliente recebe:
- Disponibilidade alvo: 99% (≈ 7h36 de downtime aceitável/mês).
- Latência alvo: < 5s na primeira resposta em 95% dos casos.
- Janela de manutenção planejada: domingo 04h-06h.
- SLA de resposta a incidente: tier básico até 4h úteis; intermediário até 2h; avançado < 1h.

## Exemplo aplicado — Polaris Bebidas (Joinville/SC)

Stack provisionada para a Polaris:
- VPS Hetzner CX22, hostname `n8n-polaris.inema.club.br`, localização Nuremberg.
- Custos infra: VPS R$ 45 + snapshot R$ 10 + Supabase Pro R$ 130 + WABA Meta (custo por mensagem da Meta, ~R$ 35/mês para volume Polaris) + Claude API R$ 810/mês ≈ R$ 1.030/mês. Dentro do range Pesquisa F §1.4.
- Caddy para HTTPS automático, certificado Let's Encrypt válido.
- n8n com PostgreSQL como backend, backup diário do banco + snapshot Hetzner.
- WABA configurado na conta Meta da Polaris (Sr. Roberto admin), 5 templates aprovados Meta.
- Workflow principal com Queue Mode (Redis), suporta picos de 80 interações/min.
- Dashboard Supabase com 6 métricas-chave atualizadas em tempo real.
- Slack interno da equipe Polaris (Sr. Roberto, Sr. Cláudio, Maria, aluno) com canal `#agente-alertas` para incidentes.
- SLA: 99% disponibilidade, latência < 5s. Janela de manutenção domingo 04h-05h.

Documento de SLA + arquitetura entregue ao Sr. Roberto em formato 2 páginas. Anexo do contrato de manutenção.

## Erros comuns
- **n8n em modo single-process sem queue.** Trava quando entra rajada. *Corrige:* Queue Mode em produção.
- **WABA na conta do implementador.** Cliente vira refém. *Corrige:* sempre na conta Meta do cliente.
- **Esquecer backup de workflow.** Perda de configuração é desastre. *Corrige:* backup do Postgres do n8n + snapshot diário.
- **Sem alerting.** Problema descoberto só pelo cliente reclamando. *Corrige:* alertas configurados antes do go-live.
- **Otimizar custo de VPS antes da hora.** CX11 economiza R$ 20/mês e cliente sofre lentidão. *Corrige:* CX22 mínimo; otimizar só com volume real medido.

## Checklist de saída
- [ ] VPS Hetzner provisionado, hardening básico aplicado.
- [ ] n8n via Docker + Postgres + Caddy + HTTPS funcionando.
- [ ] Backup diário configurado.
- [ ] WABA na conta Meta do cliente, templates aprovados.
- [ ] Webhook conectado, workflow ativo.
- [ ] Logging estruturado em Supabase.
- [ ] Alerting básico funcional.
- [ ] SLA de 1 página entregue ao cliente.

## Vai além
- Manual canônico, "Stack composable" + Princípio 7.
- Pesquisa E §10 — convergência sobre n8n self-host.
- Documentação Hetzner Cloud + n8n Docker.
- Exercício prático: provisionar ambiente de produção do cliente do exercício em VPS pessoal (pode ser ambiente de homologação adaptado), com HTTPS válido e backup automatizado.
