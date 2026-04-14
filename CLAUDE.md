# Marketing Automation — Contexto para Claude

> Leia este arquivo no início de cada sessão para ter contexto completo sem precisar explorar o repositório.

## O Projeto

Área de marketing automatizada do Thiago Holtz (CRO/Head of Revenue).
Publicação de conteúdo no LinkedIn (e futuramente Instagram) via aprovação em GitHub Issues + n8n.

## Stack

| Camada | Ferramenta | Detalhes |
|--------|-----------|----------|
| Planejamento | GitHub Issues + Projects V2 | https://github.com/users/thiagoholtz/projects/1 |
| Automação | n8n self-hosted | https://n8n.srv1191267.hstgr.cloud |
| Agente | Claude Code | Branch de trabalho: `main` |

## Fluxo Operacional

```
1. Claude cria issue com rascunho do post (seção "## Conteúdo Final")
2. Thiago revisa/edita o Conteúdo Final na issue
3. Thiago adiciona label "approved"
4. n8n polling (a cada 1 min) detecta → remove "approved" (lock) → POST para webhook LinkedIn
5. Webhook LinkedIn publica via API → retorna {success: true/false}
6. Se sucesso: issue fechada + label "published" + issue de métricas criada + comentário
7. Se erro: label "approved" restaurada + comentário de falha (issue NÃO avança)
```

## Credenciais (referências — não expor valores)

| Serviço | Onde está | Validade | Skill de renovação |
|---------|-----------|----------|-------------------|
| LinkedIn OAuth2 | n8n Credentials → "OAuth2 API" | ~10/06/2026 | `/linkedin-token-refresh` |
| n8n API Key | n8n Settings → API | Sem expiração | — |
| GitHub Classic PAT | hardcoded no workflow `UR8qcuZzxs2O46Sz` | Verificar | — |
| LinkedIn App | Client ID: `77okgr3xp3x069` | — | — |
| LinkedIn Person ID | `a16QFCMFTY` (fixo no workflow webhook) | — | — |

## Workflows n8n

| ID | Nome | Status | Função |
|----|------|--------|--------|
| `UR8qcuZzxs2O46Sz` | GitHub LinkedIn Auto-Publish | ✅ Ativo | Poll GitHub a cada 1 min, orquestra todo o fluxo |
| `k3UZ4R82KMmD7U3Z` | Marketing — LinkedIn Post | ✅ Ativo | Webhook que publica no LinkedIn via OAuth2 |
| `S9Pk6v6WJotMquNI` | Marketing — Instagram Post | ⏳ Pendente | — |

**Arquitettura do webhook `k3UZ4R82KMmD7U3Z`:**
- Recebe `POST /webhook/linkedin-post` com `{ content: "..." }`
- Publica via LinkedIn Community Management API v202504
- Verifica HTTP 201 explicitamente → retorna `{success: true}` ou `{success: false, error: ...}`
- Nunca retorna `{success: true}` em caso de erro

## Estado Atual das Issues

### Sprint 01 (10/04 – 24/04/2026)

| # | Tipo | Título | Status |
|---|------|--------|--------|
| #1 | Infra | LinkedIn OAuth2 | ✅ Fechado |
| #2 | Infra | Meta Graph API (Instagram) | 🔴 Aberto — desconsiderado por ora |
| #9 | Post | ARR +200% sem contratar | 🔄 Reaberto — aguardando re-aprovação (post anterior foi truncado) |
| #10 | Post | Erro de GTM no primeiro milhão | 📅 15/04 — aguardando aprovação |
| #11 | Post | IA não substitui vendedor | 📅 17/04 — aguardando aprovação |
| #12 | Post | Framework pipeline previsível | 📅 18/04 — aguardando aprovação |
| #13 | Post | R$0 para R$1M em 8 meses | 📅 21/04 — aguardando aprovação |
| #14 | Post | O que é CRO de verdade em 2026 | 📅 23/04 — aguardando aprovação |

## Skills Disponíveis

| Comando | O que faz |
|---------|----------|
| `/dispatch-linkedin-post {n}` | Disparo manual como fallback |
| `/linkedin-token-refresh` | Guia renovação do token (~10/06/2026) |
| `/sprint-status` | Mostra status completo da sprint |

## Estrutura do Repositório

```
claudecode/
├── .claude/commands/      # Skills Claude Code
├── .github/
│   ├── ISSUE_TEMPLATE/    # Templates de issues
│   └── workflows/         # README apenas (GitHub Actions removidas)
├── CLAUDE.md              # Este arquivo
├── GOVERNANCE.md          # Governança completa
├── agents/                # Definições de agentes
├── content/               # Padrões e hashtags de conteúdo
├── docs/
│   ├── decisions/         # ADRs — decisões arquiteturais
│   ├── learned/           # Aprendizados técnicos
│   └── processes/         # SOPs
├── n8n/                   # Backup de workflows JSON
└── sprints/               # Planejamentos de sprint
```

## VPS

- **Host**: srv1191267.hstgr.cloud / 72.62.12.127
- **Usuário**: root
- **Nota**: Claude não consegue fazer SSH diretamente (sandbox sem acesso de rede externo)

## Próximos Passos (P1)

- [ ] Thiago: deletar post truncado do LinkedIn (publicado em 13/04)
- [ ] Thiago: re-aprovar issue #9 adicionando label `approved`
- [ ] Configurar Instagram / Meta API (issue #2) — quando retomar
- [ ] Criar `sprints/sprint-02.md` ao final da Sprint 01
