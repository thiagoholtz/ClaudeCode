# Sprint Status

Este comando mostra o status atual da sprint de marketing.

## Passos

1. Listar issues abertas do repositório `thiagoholtz/claudecode` com label `sprint: 1`
2. Separar por tipo:
   - Issues de infraestrutura (`infra`)
   - Issues de conteúdo/posts (`content`)
3. Para cada post, mostrar:
   - Número e título
   - Se tem label `approved` (pronto para disparar)
   - Se tem label `published` (já publicado)
   - Data planejada (se disponível no body)
4. Resumir:
   - Total de posts da sprint
   - Publicados
   - Aprovados aguardando disparo
   - Pendentes de aprovação
5. Sugerir próxima ação (ex: "3 posts aguardando aprovação — revise as issues #9, #10, #11")

## Formato de saída esperado

```
Sprint 01 — Status (DD/MM/AAAA)

Infraestrutura
  ✅ #1 LinkedIn OAuth2 — Fechado
  🔴 #2 Instagram API — Em aberto

Conteúdo
  ✅ #9  [título] — Publicado
  🟡 #10 [título] — Aprovado (aguardando disparo)
  ⬜ #11 [título] — Pendente aprovação
  ...

Resumo: 1/6 publicados | 1 aprovado | 4 pendentes
Próximo: /dispatch-linkedin-post 10
```
