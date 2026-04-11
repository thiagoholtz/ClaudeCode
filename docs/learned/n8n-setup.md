# Aprendizados — n8n Setup

## Instância

- **URL**: https://n8n.srv1191267.hstgr.cloud
- **API Base**: `https://n8n.srv1191267.hstgr.cloud/api/v1`
- **Autenticação API**: Header `X-N8N-API-KEY: {jwt_token}`

## Limitações do n8n MCP

| Operação | Via MCP | Via REST API |
|----------|---------|-------------|
| Criar workflow | ✅ | ✅ |
| Atualizar workflow | ✅* | ✅ |
| Publicar/ativar workflow | ✅* | ✅ |
| Criar credencial | ❌ | ✅ |
| Ler execuções | ✅* | ✅ |

*Requer `availableInMCP: true` nas settings do workflow.

## Vinculação de Credenciais em Workflows HTTP

O SDK do n8n usa `newCredential('Nome')` mas **não vincula automaticamente** nós `httpRequest` com `genericCredentialType`. É necessário usar a REST API diretamente:

```python
# Ao fazer PUT /api/v1/workflows/{id}, incluir nos nós:
"credentials": {
  "httpHeaderAuth": {
    "id": "v28WfyzfZLACpQac",
    "name": "LinkedIn Bearer Token"
  }
}
```

## Ativação de Workflow

```bash
# POST para ativar (não PATCH)
curl -X POST "https://n8n.srv1191267.hstgr.cloud/api/v1/workflows/{id}/activate" \
  -H "X-N8N-API-KEY: {key}"
```

## Estrutura do Workflow LinkedIn Post

```
Webhook (POST /linkedin-post)
  → Buscar Person ID (GET /v2/userinfo)
  → Publicar no LinkedIn (POST /rest/posts)
  → Resposta (respondToWebhook)
```

### Parâmetros do Webhook
```json
{ "content": "Texto do post aqui" }
```

### Expressão do body LinkedIn no nó HTTP
```
={{ { 
  "author": "urn:li:person:" + $("Buscar Person ID").item.json.sub, 
  "commentary": $("Webhook").item.json.body.content,
  "visibility": "PUBLIC",
  "distribution": { "feedDistribution": "MAIN_FEED", "targetEntities": [], "thirdPartyDistributionChannels": [] },
  "lifecycleState": "PUBLISHED"
} }}
```

## Diagnóstico de Erros

```bash
# Última execução
curl -s "https://n8n.srv1191267.hstgr.cloud/api/v1/executions?workflowId={id}&limit=1" \
  -H "X-N8N-API-KEY: {key}"

# Detalhes de execução específica
curl -s "https://n8n.srv1191267.hstgr.cloud/api/v1/executions/{exec_id}?includeData=true" \
  -H "X-N8N-API-KEY: {key}"
```
