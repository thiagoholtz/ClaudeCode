# Dispatch LinkedIn Post

Este comando lê uma issue do GitHub pelo número fornecido como argumento, extrai o conteúdo da seção `## Conteúdo Final`, e dispara o post no LinkedIn via webhook do n8n.

## Passos

1. Ler a issue `thiagoholtz/claudecode#{ARGUMENTO}` usando a ferramenta GitHub MCP (`mcp__github__issue_read` ou `get_file_contents`)
2. Verificar se a issue tem a label `approved`. Se não tiver, avisar o usuário e não prosseguir.
3. Extrair o texto dentro da seção `## Conteúdo Final` (tudo entre esse cabeçalho e o próximo `---` ou `##`)
4. Pedir confirmação ao usuário mostrando o conteúdo extraído
5. Após confirmação, executar na VPS o comando:
```bash
python3 -c "
import urllib.request, json
content = '''CONTEUDO_AQUI'''
data = json.dumps({'content': content}).encode()
req = urllib.request.Request(
  'https://n8n.srv1191267.hstgr.cloud/webhook/linkedin-post',
  data=data,
  headers={'Content-Type': 'application/json'}
)
with urllib.request.urlopen(req) as r:
  print(json.loads(r.read()))
"
```
6. Se resposta tiver `success: true`, fechar a issue e adicionar label `published`
7. Confirmar ao usuário com link para o perfil do LinkedIn

## Notas
- Webhook: `https://n8n.srv1191267.hstgr.cloud/webhook/linkedin-post`
- Payload: `{"content": "texto"}`
- Token LinkedIn expira em ~60 dias (obtido em 11/04/2026)
