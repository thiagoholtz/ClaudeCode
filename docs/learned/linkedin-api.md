# Aprendizados — LinkedIn API

## Configuração OAuth2

### Escopos necessários
- `openid` — Para OIDC (userinfo endpoint)
- `profile` — Para dados de perfil
- `w_member_social` — Para publicar posts

> ⚠️ **NÃO usar** `w_organization_social` — causa erro de escopo inválido

### Fluxo de Autorização (Authorization Code)

1. Gerar URL de autorização:
```
https://www.linkedin.com/oauth/v2/authorization?response_type=code&client_id=77okgr3xp3x069&redirect_uri=https://localhost&scope=w_member_social%20openid%20profile
```
2. Abrir no browser, aceitar permissões
3. Copiar o `code` da URL de redirect: `https://localhost/?code=AQ...`
4. Trocar por token:
```bash
curl -X POST "https://www.linkedin.com/oauth/v2/accessToken" \
  --data-urlencode "grant_type=authorization_code" \
  --data-urlencode "code=SEU_CODE" \
  --data-urlencode "redirect_uri=https://localhost" \
  --data-urlencode "client_id=77okgr3xp3x069" \
  --data-urlencode "client_secret=SEU_CLIENT_SECRET"
```

> ⚠️ O `code` expira em segundos. Executar o curl imediatamente após copiar.

### Erros conhecidos

| Erro | Causa | Solução |
|------|-------|--------|
| `redirect_uri does not match` | URI não cadastrada | Adicionar `https://localhost` no Developer Portal → Auth → Redirect URLs |
| `invalid_client` | Client secret errado | Verificar Primary vs Secondary secret no portal |
| `414 Request-URI Too Long` | n8n OAuth2 nativo gera URL longa demais | Usar Header Auth com token manual |
| `w_organization_social not found` | Organization Support ativado no n8n | Usar Header Auth em vez do OAuth2 nativo |

## Publicação de Posts

### Endpoint
`POST https://api.linkedin.com/rest/posts`

### Headers obrigatórios
```
Authorization: Bearer {token}
LinkedIn-Version: 202504
X-Restli-Protocol-Version: 2.0.0
Content-Type: application/json
```

### Body correto (v202504)
```json
{
  "author": "urn:li:person:{person_id}",
  "commentary": "Texto do post",
  "visibility": "PUBLIC",
  "distribution": {
    "feedDistribution": "MAIN_FEED",
    "targetEntities": [],
    "thirdPartyDistributionChannels": []
  },
  "lifecycleState": "PUBLISHED"
}
```

> ⚠️ Campo `isReshareDisableForOperator` **não é aceito** pela API — retorna erro 422.

### Obter Person ID
```bash
curl -s -H "Authorization: Bearer {token}" https://api.linkedin.com/v2/userinfo
# Campo "sub" é o Person ID. Ex: "a16QFCMFTY"
```

## Credencial no n8n

- **Tipo**: `httpHeaderAuth` (Header Auth)
- **Header name**: `Authorization`
- **Header value**: `Bearer {token}`
- **Nome**: `LinkedIn Bearer Token`
- **ID atual**: `v28WfyzfZLACpQac`

Para criar/atualizar via API:
```bash
curl -X POST "https://n8n.srv1191267.hstgr.cloud/api/v1/credentials" \
  -H "X-N8N-API-KEY: {n8n_api_key}" \
  -H "Content-Type: application/json" \
  -d '{"name": "LinkedIn Bearer Token", "type": "httpHeaderAuth", "data": {"name": "Authorization", "value": "Bearer {token}"}}'
```
