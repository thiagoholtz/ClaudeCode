# New Sprint

Este comando cria toda a infraestrutura de uma nova sprint de marketing.

## Argumentos esperados
- Número da sprint (ex: `2`)
- Tema principal (ex: "Aquisição e conversão B2B")
- Período (ex: "25/04 a 08/05/2026")

Se não fornecidos, perguntar ao usuário.

## Passos

1. **Criar arquivo de sprint** em `sprints/sprint-{N}.md` usando o template `sprints/_template.md`
   - Preencher número, tema, período
   - Deixar calendário de posts em branco (serão criados nas issues)

2. **Gerar 6 rascunhos de post** baseados no tema, usando os padrões em `content/patterns/`:
   - 2 posts com padrão `gancho-dado-numerico`
   - 2 posts com padrão `framework-numerado`
   - 1 post com padrão `gancho-provocacao-mercado`
   - 1 post com padrão `erro-que-cometi`
   - Distribuir nas datas da sprint (segunda 07h, terça 12h, quinta 07h, sexta 12h, segunda 07h, quarta 12h)

3. **Criar issues GitHub** para cada post usando o template `.github/ISSUE_TEMPLATE/post-linkedin.md`
   - Labels: `linkedin`, `post`, `sprint: {N}`
   - Título: `Post LinkedIn 0{N} — {gancho da primeira linha}`

4. **Atualizar `sprints/sprint-{N}.md`** com os números das issues criadas

5. **Informar Thiago** com resumo:
   - Arquivo de sprint criado
   - Issues criadas (#XX a #XX)
   - Próximo passo: revisar rascunhos e adicionar label `approved`

## Notas
- Consultar `content/patterns/` para os formatos
- Consultar `content/hashtags/` para os sets de hashtags
- Manter coerência de tom: direto, baseado em dados, sem buzzwords vazios
