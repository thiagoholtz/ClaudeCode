# Retrospective

Este comando consolida os aprendizados de uma sprint e prepara o terreno para a próxima.

## Argumentos
- Número da sprint (ex: `1`)

## Passos

1. **Listar todas as issues de posts da sprint** com label `sprint: {N}` e `linkedin`
   - Separar: publicados (label `published`), pendentes, cancelados

2. **Ler as issues de métricas** (`metrics` + `sprint: {N}`) que já foram preenchidas
   - Consolidar: impressões totais, engajamento médio, melhor e pior post

3. **Identificar padrões de performance**:
   - Qual padrão de conteúdo performou melhor? (gancho-dado / framework / provocação / erro)
   - Qual horário teve mais engajamento? (07h vs 12h)
   - Qual tema gerou mais comentários?

4. **Atualizar `sprints/sprint-{N}.md`**:
   - Preencher seção "Revisão da Sprint" com aprendizados
   - Atualizar KPIs com resultados reais

5. **Criar entrada em `docs/learned/`** com o arquivo `sprint-{N}-learnings.md`:
   - Top 3 descobertas sobre audiência
   - Padrões de conteúdo validados vs. descartados
   - Ajustes para a próxima sprint

6. **Propor tema e estrutura para Sprint {N+1}**:
   - Tema baseado nos posts de maior engajamento
   - Manter padrões que funcionaram, testar 1 novo
   - Sugerir datas e volume

7. **Resumir para Thiago** com:
   - Performance geral (publicados / planejados)
   - Top 2 posts por engajamento
   - 3 ajustes para a próxima sprint
   - Pergunta: "Aprova o tema para Sprint {N+1}?"
