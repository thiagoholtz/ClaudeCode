# Marketing Automation — Governança

## Arquitetura

```
GitHub Issues → Aprovação → Dispatch (Claude) → n8n Webhook → LinkedIn/Instagram
```

## Stack

| Camada | Ferramenta | URL |
|--------|-----------|-----|
| Planejamento | GitHub Issues + Projects V2 | https://github.com/users/thiagoholtz/projects/1 |
| Automação | n8n self-hosted | https://n8n.srv1191267.hstgr.cloud |
| Controle de versão | GitHub | thiagoholtz/claudecode |
| Agente | Claude Code | - |

## Estrutura do Repositório

```
claudecode/
├── .claude/
│   └── commands/        # Skills/comandos customizados do Claude Code
├── agents/              # Definições de agentes Claude
├── docs/
│   ├── learned/         # Aprendizados técnicos documentados
│   └── processes/       # SOPs e processos operacionais
├── n8n/                 # Workflows n8n (JSON backup)
├── sprints/             # Planejamentos de sprint
└── GOVERNANCE.md        # Este arquivo
```

## Credenciais

| Serviço | Tipo | Local | Validade | Renovação |
|---------|------|-------|----------|----------|
| LinkedIn Bearer Token | Header Auth | n8n `v28WfyzfZLACpQac` | ~60 dias | Ver `/linkedin-token-refresh` |
| n8n API Key | JWT | n8n Settings | Sem expiração | n8n UI → Settings → API |
| GitHub Classic PAT | Token | Uso pessoal | Verificar | GitHub → Settings → Developer Settings |

## Workflows n8n

| ID | Nome | Status | Endpoint |
|----|------|--------|----------|
| `k3UZ4R82KMmD7U3Z` | Marketing — LinkedIn Post | ✅ Ativo | `POST /webhook/linkedin-post` |
| `S9Pk6v6WJotMquNI` | Marketing — Instagram Post | ⏳ Pendente (issue #2) | - |

## Labels GitHub

| Label | Uso |
|-------|-----|
| `sprint: 1` | Pertence à Sprint 1 |
| `priority: high` / `medium` / `low` | Prioridade |
| `size: S` / `M` / `L` | Esforço estimado |
| `infra` | Tarefa de infraestrutura |
| `content` | Post de conteúdo |
| `approved` | Post aprovado para publicação |
| `published` | Post já publicado |

## Skills Disponíveis

| Comando | O que faz |
|---------|----------|
| `/dispatch-linkedin-post` | Lê issue aprovada e posta no LinkedIn |
| `/linkedin-token-refresh` | Guia renovação do token LinkedIn |
| `/sprint-status` | Mostra status da sprint atual |

## Processo de Sprint

1. Claude cria issues de conteúdo com rascunho do post
2. Thiago revisa, edita se necessário, adiciona label `approved`
3. Claude executa `/dispatch-linkedin-post {número}` → post publicado
4. Issue fechada com label `published`

## Alerta de Expiração

O token LinkedIn expira em ~60 dias (obtido em 11/04/2026 → expira ~10/06/2026).
Quando expirar, executar skill `/linkedin-token-refresh`.
