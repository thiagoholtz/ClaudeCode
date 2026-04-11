# GitHub Actions — Marketing Automation

## dispatch-linkedin-post.yml

**Trigger:** quando uma issue recebe a label `approved` (e já tem a label `linkedin`)

**Fluxo:**
1. Extrai o conteúdo da seção `## Conteúdo Final` da issue
2. Faz POST para o webhook do n8n → publica no LinkedIn
3. Adiciona label `published`, remove `approved`, fecha a issue
4. Comenta na issue confirmando a publicação

**Secret necessário:**
- `N8N_WEBHOOK_URL` → URL do webhook do n8n

**Para aprovar um post:**
1. Abra a issue do post (ex: #9)
2. Edite o `## Conteúdo Final` se necessário
3. Adicione a label `approved`
4. A Action executa automaticamente em segundos
