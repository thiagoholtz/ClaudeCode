# Setup dos Workflows n8n

## 1. Importar os Workflows

1. Acesse seu n8n: https://n8n.srv1191267.hstgr.cloud/
2. Vá em **Workflows → Import from File**
3. Importe `linkedin-workflow.json`
4. Importe `instagram-workflow.json`

---

## 2. Configurar LinkedIn

### Credencial OAuth2
1. No n8n: **Settings → Credentials → New → LinkedIn OAuth2 API**
2. Você precisará de um App no LinkedIn Developer Portal:
   - Acesse: https://www.linkedin.com/developers/apps
   - Crie um novo app (use seu perfil como empresa)
   - Adicione o produto **Share on LinkedIn**
   - Em OAuth 2.0, adicione o redirect URL do n8n: `https://n8n.srv1191267.hstgr.cloud/rest/oauth2-credential/callback`
3. Copie o **Client ID** e **Client Secret** para a credencial no n8n
4. Autorize com sua conta LinkedIn

### Ativar o Workflow
1. Abra o workflow **Marketing — LinkedIn Post**
2. Configure a credencial criada
3. Ative o workflow (toggle no topo)
4. Copie a **Webhook URL** gerada (ex: `https://n8n.srv1191267.hstgr.cloud/webhook/linkedin-post`)

---

## 3. Configurar Instagram (Meta Graph API)

### Pré-requisitos
- Conta Instagram deve ser **Business ou Creator**
- Uma **Página no Facebook** vinculada ao Instagram

### Criar App no Meta
1. Acesse: https://developers.facebook.com/apps
2. **Create App → Other → Business**
3. Adicione o produto **Instagram Graph API**
4. Em **Settings → Basic**, copie o App ID e App Secret

### Obter o Access Token
1. Use o **Graph API Explorer**: https://developers.facebook.com/tools/explorer
2. Selecione seu App e sua Página do Facebook
3. Adicione as permissões:
   - `instagram_basic`
   - `instagram_content_publish`
   - `pages_read_engagement`
4. Gere o token e converta para **Long-Lived Token** (válido por 60 dias):
   ```
   GET https://graph.facebook.com/oauth/access_token?
     grant_type=fb_exchange_token&
     client_id={app_id}&
     client_secret={app_secret}&
     fb_exchange_token={short_lived_token}
   ```

### Obter o Instagram Account ID
```
GET https://graph.facebook.com/v19.0/me/accounts?access_token={seu_token}
```
Depois: `GET https://graph.facebook.com/v19.0/{page_id}?fields=instagram_business_account&access_token={token}`

### Configurar no n8n
1. Em **Settings → Variables**, adicione:
   - `INSTAGRAM_ACCOUNT_ID` = seu Instagram Business Account ID
   - `META_ACCESS_TOKEN` = seu Long-Lived Access Token
2. Ative o workflow **Marketing — Instagram Post**
3. Copie a **Webhook URL** gerada

---

## 4. Webhook URLs (após ativar)

Anote aqui após configurar:

```
LinkedIn Webhook:  https://n8n.srv1191267.hstgr.cloud/webhook/linkedin-post
Instagram Webhook: https://n8n.srv1191267.hstgr.cloud/webhook/instagram-post
```

## 5. Testar

### LinkedIn
```bash
curl -X POST https://n8n.srv1191267.hstgr.cloud/webhook/linkedin-post \
  -H "Content-Type: application/json" \
  -d '{"content": "Post de teste via API. #RevOps #B2BSaaS"}'
```

### Instagram
```bash
curl -X POST https://n8n.srv1191267.hstgr.cloud/webhook/instagram-post \
  -H "Content-Type: application/json" \
  -d '{"content": "Post de teste via API.", "image_url": "https://url-da-imagem.jpg"}'
```
