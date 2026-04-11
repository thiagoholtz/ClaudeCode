# SOP — Aprovação e Publicação de Posts

## Fluxo Completo

```
1. Claude cria issue com rascunho
2. Thiago revisa no GitHub
3. Thiago edita o conteúdo (opcional)
4. Thiago adiciona label "approved"
5. Claude dispara o post via /dispatch-linkedin-post {número}
6. Post aparece no LinkedIn
7. Issue fechada com label "published"
```

## Passo a Passo Detalhado

### 1. Acessar a Issue
Acesse https://github.com/thiagoholtz/claudecode/issues e filtre por `label:sprint:1`.

### 2. Revisar o Conteúdo
Cada issue de post tem a seção **Conteúdo Final** com o texto que será publicado.

### 3. Editar (se necessário)
- Clique em **Edit** no corpo da issue (ícone de lápis)
- Modifique o texto na seção `## Conteúdo Final`
- Clique em **Update comment** / **Save changes**

> ⚠️ Edite **apenas** dentro da seção `## Conteúdo Final`. O restante é metadata.

### 4. Aprovar
Adicione a label `approved` à issue:
- Via GitHub UI: sidebar direita → Labels → approved
- Via CLI: `gh issue edit {número} --add-label approved`

### 5. Solicitar Publicação
Diga para o Claude:
```
/dispatch-linkedin-post {número da issue}
```

Exemplo: `/dispatch-linkedin-post 9`

O Claude irá:
1. Ler o conteúdo da issue
2. Verificar se tem label `approved`
3. Enviar para o webhook do n8n
4. Confirmar publicação
5. Fechar a issue com label `published`

## Formato Padrão de Issue de Post

```markdown
## Conteúdo Final
<!-- Edite apenas este bloco antes de aprovar -->

[texto do post aqui]

---

## Metadados
- **Data planejada**: DD/MM/AAAA
- **Plataforma**: LinkedIn
- **Sprint**: 1

## Checklist
- [ ] Conteúdo revisado
- [ ] Tom de voz adequado
- [ ] Hashtags verificadas
- [ ] Aprovado para publicação → adicionar label `approved`
```

## Resolução de Problemas

| Problema | Solução |
|----------|--------|
| Post não apareceu | Verificar execução no n8n dashboard |
| Token expirado (401) | Executar `/linkedin-token-refresh` |
| Conteúdo errado publicado | Não é possível editar post no LinkedIn via API — deletar manualmente |
| Webhook não responde | Verificar se workflow está ativo: `GET /api/v1/workflows/k3UZ4R82KMmD7U3Z` |
