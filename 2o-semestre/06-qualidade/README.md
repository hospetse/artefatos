# 6. Qualidade — Estratégia de Testes

A estratégia tem três etapas. A primeira está em uso. As duas seguintes completam a validação antes da entrega.

| Etapa | O que valida | Situação |
| --- | --- | --- |
| 1. Testes automatizados nos serviços | Regras de negócio, rotas e integração com o banco no back-end e no Auth Server | Em uso, executados no CI |
| 2. Testes end-to-end | Fluxos completos, do App ao banco, passando pelo Auth Server e pelo back-end | A montar |
| 3. Validação manual dos fluxos | Execução dos fluxos principais pelo time, como o usuário faria | A montar |

## 1. Testes automatizados

O back-end e o Auth Server usam o test runner nativo do Node (`node:test` e `node:assert`), conforme a ADR-010. O `app.js` é separado do `index.js`, então os testes sobem o servidor em porta aleatória (`app.listen(0)`) e fazem requisições HTTP reais com Axios.

### Back-end ([hospetse-backend/test](https://github.com/hospetse/hospetse-backend/tree/develop/test))

| Tipo | Comando | O que cobre |
| --- | --- | --- |
| Sem banco | `npm test` | Regras isoladas: validação de CPF, perfil do Tutor, disponibilidade, regras de reserva e de busca, CORS, health check e proteção das rotas |
| Integração com PostgreSQL | `npm run test:integration` | Rotas com o banco: provisionamento do usuário, token do usuário, perfil do Tutor, pets, busca e detalhe de Host, reservas, decisões do Host e seed |

O `test/auth-guard.test.js` percorre todas as rotas registradas e falha se alguma rota fora da lista de públicas responder sem autenticação.

### Auth Server ([hospetse-auth-service/test](https://github.com/hospetse/hospetse-auth-service/tree/develop/test))

| Comando | O que cobre |
| --- | --- |
| `npm test` | Pedido de PIN, cadastro, login, renovação do token, emissão e validação do JWT, CORS, envio de e-mail, provisionamento no back-end, fluxo completo e seed |

Os testes usam implementações falsas do envio de e-mail e do provisionamento, e um back-end falso por HTTP no fluxo completo. Também conferem que PIN, senha, tokens e chaves não aparecem nos logs.

### Mobile ([hospetse-mobile/\_\_tests\_\_](https://github.com/hospetse/hospetse-mobile/tree/develop/__tests__))

O App usa Jest com `jest-expo` e `@testing-library/react-native`, o padrão do Expo. Os testes cobrem telas e funções, como cadastro, código PIN, login, cliente do Auth, busca, resultados e detalhe do Host. O comando é `npm test`, com checagem de tipos por `npx tsc --noEmit`. O mobile ainda não tem pipeline de CI.

### Execução no CI

Os pipelines do Azure DevOps do back-end e do Auth Server rodam a cada push na `develop` e na `main`, e também na validação de Pull Request:

1. `npm ci` e lint.
2. Geração do Prisma Client.
3. PostgreSQL 16 descartável, criado só para o job.
4. Migrations com `npx prisma migrate deploy`.
5. Testes.

Se um teste falhar, o estágio de deploy no Railway não roda. Detalhes em [Ambientes, Deploy e CI/CD](../05-arquitetura/ambientes-e-deploy.md).

## 2. Testes end-to-end

Próxima etapa. Vão validar os fluxos completos entre App, Auth Server, back-end e banco, começando pelos fluxos dos [Diagramas de Sequência](../04-modelagem/sequencia): cadastro com PIN, login e renovação do token, busca de Host e solicitação de reserva.

## 3. Validação manual

Execução dos fluxos principais pelo time no Expo Go (Android e iOS) e nos ambientes publicados, seguindo os critérios de aceite das [User Stories](../02-produto-e-requisitos). O resultado de cada execução fica registrado na task correspondente no Azure Boards.
