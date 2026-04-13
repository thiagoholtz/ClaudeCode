# ADR 003 — GitHub Issues como fila de aprovação de conteúdo

**Data:** 10/04/2026  
**Status:** Aceito  
**Contexto:** Escolha do sistema de aprovação de posts antes da publicação

## Problema

Thiago precisa revisar e aprovar cada post antes de publicar. O sistema precisa:
- Permitir edição do conteúdo antes da aprovação
- Ter histórico auditável (quem aprovou, quando)
- Disparar publicação automaticamente após aprovação
- Servir como documentação pós-publicação (métricas, aprendizados)

## Opções consideradas

### Opção A: GitHub Issues + Labels + GitHub Actions
- Issue = unidade de conteúdo
- Label `approved` = gatilho de publicação
- GitHub Actions = executor automático

### Opção B: Notion/planilha + disparo manual
- Familiar, visual
- Sem automação nativa
- Sem histórico de versionamento do conteúdo

### Opção C: n8n com formulário de aprovação
- Tudo na mesma ferramenta
- Interface de aprovação menos familiar
- Sem versionamento de conteúdo

## Decisão

**Opção A — GitHub Issues + Labels + GitHub Actions**

## Motivo

- Thiago já usa GitHub (repositório já existia)
- Issues têm histórico de edições do conteúdo
- Labels são o mecanismo mais simples de "estado"
- GitHub Actions dispara em segundos após label ser adicionada
- Toda a operação (código + conteúdo + automação) fica no mesmo ecossistema
- Claude consegue criar, ler e atualizar issues via MCP

## Estrutura de estado de uma issue de post

```
[criada] → [approved] → [published]
```

- `approved`: Thiago revisou e aprovou → Action dispara
- `published`: post publicado, issue fechada, métricas a coletar

## Formato padrão da issue

Seção `## Conteúdo Final` no topo → editável antes da aprovação  
Seção `## Metadados` → data planejada, plataforma, sprint  
Seção `## Checklist` → itens de qualidade antes de aprovar
