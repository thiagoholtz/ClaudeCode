# ADR 001 — LinkedIn: Header Auth vs OAuth2 Nativo do n8n

**Data:** 11/04/2026  
**Status:** Aceito  
**Contexto:** Setup da integração LinkedIn para publicação automatizada de posts

## Problema

Precisávamos autenticar chamadas à LinkedIn Community Management API (v202504) a partir do n8n.

## Opções consideradas

### Opção A: OAuth2 nativo do n8n (LinkedIn Community Management)
- n8n tem suporte nativo a OAuth2 do LinkedIn
- Aparentemente mais simples de configurar

### Opção B: Header Auth manual com Bearer Token
- Obter token manualmente via Authorization Code Flow
- Armazenar como credencial Header Auth no n8n

## Decisão

**Opção B — Header Auth com Bearer Token manual**

## Motivo

A Opção A falhou por dois problemas encadeados:
1. A URL de autorização gerada pelo n8n era longa demais → erro HTTP 414
2. A credential "LinkedIn Community Management" do n8n inclui scope `w_organization_social` que requer permissões de organização (não de pessoa física), causando erros de autorização

A Opção B funciona porque:
- Controle total dos scopes (`w_member_social openid profile`)
- Não depende da implementação OAuth2 do n8n
- Permite usar a API `/v2/userinfo` (OIDC) para obter o Person ID dinamicamente

## Consequências

- Token expira em ~60 dias → precisa de renovação manual (~10/06/2026)
- Skill `/linkedin-token-refresh` documenta o processo de renovação
- Não é possível usar o nó nativo "LinkedIn" do n8n — usar HTTP Request com credencial Header Auth

## Campos proibidos na API v202504

`isReshareDisableForOperator` → retorna erro 422. Não incluir no body do POST.
