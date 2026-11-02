# Documentação da API REST

O contrato de cada serviço é um arquivo OpenAPI 3.1 versionado junto com o código. Cada serviço publicado também mostra o contrato navegável no Swagger UI, em `/docs`.

| Serviço | Swagger publicado | Contrato OpenAPI | Explicação das rotas |
|---|---|---|---|
| Auth Server | [auth-service-production-36e0.up.railway.app/docs](https://auth-service-production-36e0.up.railway.app/docs) | [docs/openapi.yaml](https://github.com/hospetse/hospetse-auth-service/blob/develop/docs/openapi.yaml) | [README do Auth Server](https://github.com/hospetse/hospetse-auth-service/blob/develop/README.md#rotas) |
| Back-end | [backend-production-7298.up.railway.app/docs](https://backend-production-7298.up.railway.app/docs) | [docs/openapi.yaml](https://github.com/hospetse/hospetse-backend/blob/develop/docs/openapi.yaml) | [README do back-end](https://github.com/hospetse/hospetse-backend/blob/develop/README.md#documentação-da-api) |

URLs publicadas, contratos e regras dos tokens: [apis-publicadas.md](apis-publicadas.md) ([Wiki #88](https://dev.azure.com/escola-ti/Hospetse/_wiki/wikis/Hospetse.wiki/88)).

## Padrões

- Prefixo `/api/v1/` em todas as rotas de negócio.
- Erro no formato `{ "code", "message", "details"? }`.
- Toda rota sob `/api/v1` do back-end exige o access token JWT no header `Authorization: Bearer`. As únicas rotas públicas são `GET /health` e `GET /health/ready`.
- A rota interna `PUT /api/v1/internal/users/{authUserId}` só aceita o token de serviço do Auth Server.
