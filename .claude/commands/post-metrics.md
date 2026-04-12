# Post Metrics

Este comando cria uma issue de rastreamento de métricas para um post publicado.

## Argumentos
- Número da issue do post publicado (ex: `9`)

## Passos

1. Ler a issue #{N} para obter título, data de publicação e conteúdo
2. Criar nova issue com título: `Métricas — Post #{N} | {título do post}`
3. Labels: `metrics`, `sprint: {N_sprint}`, `linkedin`
4. Body da issue:

```markdown
## Métricas — Post #{N} (coletar 7 dias após publicação)

**Post:** #{N} — {título}
**Publicado em:** {data}
**Coletar em:** {data + 7 dias}

---

## Resultados (preencher após 7 dias)

| Métrica | Resultado |
|---------|-----------|
| Impressões | |
| Reações (curtidas) | |
| Comentários | |
| Compartilhamentos | |
| Cliques no perfil | |
| Novos seguidores | |

## Análise

**Padrão usado:** [gancho-dado-numerico / framework-numerado / provocacao / erro]  
**Horário:** [07h / 12h]  
**Engajamento rate:** [calcular: (reações + comentários) / impressões × 100]%

## Aprendizado

- O que funcionou:
- O que mudar no próximo post similar:
- Vale repetir o padrão? [ ] Sim  [ ] Não  [ ] Com ajuste

---

> Fechar esta issue após preencher os dados.
```

5. Comentar na issue original #{N}: "📊 Issue de métricas criada: #{nova_issue}"
6. Informar Thiago com link para a nova issue
