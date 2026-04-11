# LinkedIn Token Refresh

Este comando guia o processo completo de renovação do token Bearer do LinkedIn quando o atual expirar (validade ~60 dias).

## Credenciais necessárias
- Client ID: `77okgr3xp3x069`
- Client Secret: disponível no LinkedIn Developer Portal (Primary Client Secret)
- n8n API Key: disponível em n8n → Settings → n8n API

## Passos

### 1. Gerar URL de autorização
Mostrar ao usuário esta URL para abrir no browser:
```
https://www.linkedin.com/oauth/v2/authorization?response_type=code&client_id=77okgr3xp3x069&redirect_uri=https://localhost&scope=w_member_social%20openid%20profile
```

### 2. Obter o code
Aguardar o usuário colar a URL de redirect completa (formato: `https://localhost/?code=AQ...`)
Extrair o valor do parâmetro `code`.

### 3. Trocar code por token (executar na VPS)
Fornecer o comando para o usuário rodar na VPS:
```bash
curl -X POST "https://www.linkedin.com/oauth/v2/accessToken" \
  --data-urlencode "grant_type=authorization_code" \
  --data-urlencode "code={CODE_EXTRAIDO}" \
  --data-urlencode "redirect_uri=https://localhost" \
  --data-urlencode "client_id=77okgr3xp3x069" \
  --data-urlencode "client_secret={CLIENT_SECRET}"
```

### 4. Atualizar credencial no n8n
Com o novo `access_token`, criar nova credencial via API:
```bash
curl -X POST "https://n8n.srv1191267.hstgr.cloud/api/v1/credentials" \
  -H "X-N8N-API-KEY: {n8n_api_key}" \
  -H "Content-Type: application/json" \
  -d '{"name": "LinkedIn Bearer Token", "type": "httpHeaderAuth", "data": {"name": "Authorization", "value": "Bearer {NEW_TOKEN}"}}'
```

### 5. Atualizar workflow com nova credencial
Pegar o novo ID da credencial e fazer PUT no workflow `k3UZ4R82KMmD7U3Z` com o novo ID nos dois nós HTTP Request (Buscar Person ID e Publicar no LinkedIn).

### 6. Reativar workflow
```bash
curl -X POST "https://n8n.srv1191267.hstgr.cloud/api/v1/workflows/k3UZ4R82KMmD7U3Z/activate" \
  -H "X-N8N-API-KEY: {n8n_api_key}"
```

### 7. Testar
```bash
curl -s -X POST "https://n8n.srv1191267.hstgr.cloud/webhook/linkedin-post" \
  -H "Content-Type: application/json" \
  -d '{"content": "Teste de renovação de token."}'
```

## Notas
- O `code` expira em segundos. Executar o curl de troca imediatamente.
- Anotar a data de obtenção do novo token para calcular a próxima expiração.
