# ADR 002 — n8n como middleware de automação

**Data:** 10/04/2026  
**Status:** Aceito  
**Contexto:** Escolha da ferramenta de automação para o pipeline de marketing

## Problema

Precisávamos de um middleware que:
- Receba requisições de disparo (webhook)
- Execute chamadas autenticadas a APIs externas (LinkedIn, Instagram)
- Seja self-hosted (dados e credenciais sob controle)
- Permita gerenciamento programático via API/SDK

## Opções consideradas

### Opção A: n8n self-hosted
- Open source, self-hosted na VPS
- API REST completa + MCP server para gerenciamento via Claude
- SDK TypeScript para criar workflows programaticamente

### Opção B: Make (Integromat) / Zapier
- SaaS, fácil de usar
- Sem controle sobre credenciais
- Sem API para gerenciamento programático

### Opção C: Código direto (scripts Python/Node na VPS)
- Máximo controle
- Sem interface visual
- Maior esforço de manutenção

## Decisão

**Opção A — n8n self-hosted**

## Motivo

- Credenciais ficam na VPS (sem dependência de terceiros)
- MCP server permite que Claude gerencie workflows diretamente
- Webhook URL pública estável para GitHub Actions acionar
- SDK permite criar/atualizar workflows via código, versionável no GitHub

## Limitações conhecidas

- n8n MCP não suporta criação/atualização de credenciais → usar REST API
- Workflows com `availableInMCP: false` não podem ser gerenciados via MCP
- Atualizações de workflow via MCP SDK às vezes não vinculam credenciais automaticamente em nós HTTP Request com `genericCredentialType` → usar PUT via REST API
