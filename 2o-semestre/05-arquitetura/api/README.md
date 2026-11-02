# Documentação da API REST

O contrato de cada serviço é um arquivo OpenAPI 3.1 versionado junto com o código. Cada serviço também mostra o contrato navegável no Swagger UI, em `/docs`.

| Serviço | Contrato OpenAPI | Explicação das rotas | Swagger local |
|---|---|---|---|
| Auth Server | [docs/openapi.yaml](https://github.com/hospetse/hospetse-auth-service/blob/develop/docs/openapi.yaml) | [README do Auth Server](https://github.com/hospetse/hospetse-auth-service/blob/develop/README.md#rotas) | `http://localhost:3001/docs` |
| Back-end | [docs/openapi.yaml](https://github.com/hospetse/hospetse-backend/blob/develop/docs/openapi.yaml) | [README do back-end](https://github.com/hospetse/hospetse-backend/blob/develop/README.md#documentação-da-api) | `http://localhost:3000/docs` |

**Swagger publicado:** os links do Swagger dos serviços em produção ficam na [Wiki #88 — APIs publicadas](https://dev.azure.com/escola-ti/Hospetse/_wiki/wikis/Hospetse.wiki/88), de acesso restrito ao projeto. A página também traz as regras dos tokens.

## Padrões

- Prefixo `/api/v1/` em todas as rotas de negócio.
- Erro no formato `{ "code", "message", "details"? }`.
- Toda rota sob `/api/v1` do back-end exige o access token JWT no header `Authorization: Bearer`. As únicas rotas públicas são `GET /health` e `GET /health/ready`.
- A rota interna `PUT /api/v1/internal/users/{authUserId}` só aceita o token de serviço do Auth Server.
