# APIs publicadas: URLs e contratos

Onde o Mobile deve chamar e onde estão os contratos de cada serviço. Ambiente único por enquanto (`production` no Railway).

## URLs

| Serviço | URL pública | Para quê |
| --- | --- | --- |
| **Auth Server** | `https://auth-service-production-36e0.up.railway.app` | Cadastro (PIN por e-mail), login e renovação de tokens |
| **Back-end principal** | `https://backend-production-7298.up.railway.app` | Tudo do domínio (usuário, perfil do Tutor e as próximas funcionalidades) |

Para conferir se estão no ar: `GET /health` nos dois (o do back-end também tem `GET /health/ready`, que testa o banco).

Os dois serviços conversam entre si pela rede interna do Railway (o Auth chama o back-end para criar o USER no cadastro). O Mobile não chama a rota interna.

## Contratos (OpenAPI 3.1)

**Documentação navegável (Swagger UI)** — disponível depois do merge dos PRs da #501 (PBI #499):

| Serviço | `/docs` | Arquivo no repositório |
| --- | --- | --- |
| Auth Server | https://auth-service-production-36e0.up.railway.app/docs | [`docs/openapi.yaml`](https://dev.azure.com/escola-ti/Hospetse/_git/hospetse-auth-service?path=/docs/openapi.yaml&version=GBdevelop) (`develop`) |
| Back-end principal | https://backend-production-7298.up.railway.app/docs | [`docs/openapi.yaml`](https://dev.azure.com/escola-ti/Hospetse/_git/hospetse-backend?path=/docs/openapi.yaml&version=GBmain) (`main`) |

O contrato em YAML também fica em `/docs/openapi.yaml` de cada serviço (útil para gerar cliente ou importar no Postman/Insomnia). Rodando localmente: `http://localhost:3001/docs` (Auth) e `http://localhost:3000/docs` (back-end). O README de cada repositório explica o contexto e as decisões de cada rota.

## Rotas que o Mobile usa hoje

**Auth Server** (sem token: a autenticação é o próprio payload)

| Rota | O que faz |
| --- | --- |
| `POST /api/v1/auth/request-pin` | Envia o PIN para o e-mail e devolve o `challengeId` |
| `POST /api/v1/auth/register` | Valida o PIN, cria a conta e devolve os tokens |
| `POST /api/v1/auth/login` | E-mail e senha; devolve os tokens |
| `POST /api/v1/auth/refresh` | Troca o refresh token por tokens novos |

Resposta com tokens (register, login e refresh):

```json
{ "accessToken": "eyJ...", "tokenType": "Bearer", "expiresIn": 900, "refreshToken": "q3J9..." }
```

**Back-end principal** (sempre com `Authorization: Bearer <accessToken>`)

| Rota | O que faz |
| --- | --- |
| `GET /api/v1/me` | Usuário, status do perfil, campos pendentes (`missingFields`) e permissões (`canContract`) |
| `PUT /api/v1/me/tutor-profile` | Conclui ou edita o perfil do Tutor (CPF, nascimento, telefone, endereço, contato de emergência); `204` sem corpo, depois chamar o `/me` |

## Regras importantes para o Mobile

- **Access token vale 15 min; refresh token, 30 dias.** Quando o back-end responder `401`, renovar no `refresh` e refazer a chamada. Se o `refresh` responder `401`, a sessão acabou: apagar os tokens e mandar para o login.
- **Fila de refresh:** o refresh token vale uma vez só (rotação). Se duas chamadas renovarem ao mesmo tempo com o mesmo token, a segunda cai na detecção de reuso e **todas as sessões da conta são derrubadas**. Só uma renovação por vez; as outras chamadas esperam por ela (detalhes no README do Auth, seção do refresh).
- **Tokens em secure storage** (ADR-005). Nunca em log.
- **Formato de erro diferente em cada serviço** (o padrão único depende da #56):
  - Auth: `{ "error": "mensagem" }`
  - Back-end: `{ "code": "...", "message": "...", "details": [{ "field", "message" }] }`. No `400`, o `details` traz **todos** os campos inválidos (`address.zipCode`, `emergencyContact.name`...) para marcar na tela.
- **Login:** e-mail inexistente e senha errada respondem o mesmo `401`. `403` significa cadastro não concluído: pedir um novo PIN (`request-pin`) para terminar o cadastro.
- **Perfil do Tutor:** telefone só com dígitos e DDD (10 ou 11, sem `+55`) e CEP com ou sem hífen. **[assunção]** Se a tela enviar em outro formato, avisar o back-end para alinharmos.

## Limitações atuais

- **O e-mail com o PIN não é enviado de verdade:** o Auth publicado ainda usa o envio de desenvolvimento, e o PIN aparece só no log do serviço no Railway. Para testar o cadastro, alguém com acesso ao Railway precisa ler o PIN no log. Falta o provedor de e-mail.
- **Deploy:** o Auth publica sozinho a cada merge na `develop` (pipeline). O back-end passa a publicar sozinho a cada merge na `main` depois que o pipeline for criado no Azure (#500); até lá, deploy manual.
- **Um ambiente só:** não há homologação separada; os testes do Mobile usam os mesmos serviços e bancos.