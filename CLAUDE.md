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
| Disparo automático | GitHub Actions | `.github/workflows/dispatch-linkedin-post.yml` |
| Agente | Claude Code | Branch de trabalho: `main` |

## Fluxo Operacional

```
1. Claude cria issue com rascunho do post (seção "## Conteúdo Final")
2. Thiago revisa/edita o Conteúdo Final na issue
3. Thiago adiciona label "approved"
4. GitHub Action dispara automaticamente → POST para n8n → LinkedIn
5. Issue fechada com label "published" + comentário de confirmação
```

## Credenciais (referências — não expor valores)

| Serviço | Onde está | Validade | Skill de renovação |
|---------|-----------|----------|-------------------|
| LinkedIn Bearer Token | n8n, id: `v28WfyzfZLACpQac` | ~10/06/2026 | `/linkedin-token-refresh` |
| n8n API Key | n8n Settings → API | Sem expiração | — |
| GitHub Classic PAT | Uso pessoal | Verificar | — |
| LinkedIn App | Client ID: `77okgr3xp3x069` | — | — |

## Workflows n8n

| ID | Nome | Status | Endpoint |
|----|------|--------|----------|
| `k3UZ4R82KMmD7U3Z` | Marketing — LinkedIn Post | ✅ Ativo | `POST /webhook/linkedin-post` |
| `S9Pk6v6WJotMquNI` | Marketing — Instagram Post | ⏳ Pendente | — |

> ⚠️ O workflow LinkedIn tem `availableInMCP: false` — alterações precisam ser feitas via REST API (script Python na VPS ou direto no n8n UI).

## Estado Atual das Issues

### Sprint 01 (10/04 – 24/04/2026)

| # | Tipo | Título | Status |
|---|------|--------|--------|
| #1 | Infra | LinkedIn OAuth2 | ✅ Fechado |
| #2 | Infra | Meta Graph API (Instagram) | 🔴 Aberto — desconsiderado por ora |
| #9 | Post | ARR +200% sem contratar | 📅 14/04 — aguardando aprovação |
| #10 | Post | Erro de GTM no primeiro milhão | 📅 15/04 — aguardando aprovação |
| #11 | Post | IA não substitui vendedor | 📅 17/04 — aguardando aprovação |
| #12 | Post | Framework pipeline previsível | 📅 18/04 — aguardando aprovação |
| #13 | Post | R$0 para R$1M em 8 meses | 📅 21/04 — aguardando aprovação |
| #14 | Post | O que é CRO de verdade em 2026 | 📅 23/04 — aguardando aprovação |

## GitHub Actions

**`dispatch-linkedin-post.yml`** — dispara quando issue com label `linkedin` recebe label `approved`:
- Extrai `## Conteúdo Final` da issue
- POST para `${{ secrets.N8N_WEBHOOK_URL }}`
- Fecha issue + adiciona label `published`

**Secret necessário:** `N8N_WEBHOOK_URL` = `https://n8n.srv1191267.hstgr.cloud/webhook/linkedin-post`

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
│   └── workflows/         # GitHub Actions
├── CLAUDE.md              # Este arquivo
├── GOVERNANCE.md          # Governança completa
├── agents/                # Definições de agentes
├── docs/
│   ├── learned/           # Aprendizados técnicos
│   └── processes/         # SOPs
├── n8n/                   # Backup de workflows JSON
└── sprints/               # Planejamentos de sprint
```

## VPS

- **Host**: srv1191267.hstgr.cloud / 72.62.12.127
- **Usuário**: root
- **Nota**: Claude não consegue fazer SSH diretamente (sandbox sem acesso de rede externo)
- **Alternativa**: GitHub Actions ou scripts Python executados pelo Thiago na VPS

## Próximos Passos (P1)

- [ ] Ativar `availableInMCP: true` no workflow LinkedIn (n8n UI → Settings)
- [ ] Definir `main` como branch padrão (GitHub Settings → Branches)
- [ ] Configurar Instagram / Meta API (issue #2) — quando retomar
- [ ] Criar `sprints/sprint-02.md` ao final da Sprint 01
