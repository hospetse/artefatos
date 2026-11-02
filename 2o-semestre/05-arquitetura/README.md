# 5. Arquitetura (System Design)

Como o Hospetse é construído: decisões, estrutura, ambientes e integrações.

| # | Artefato | Arquivo | O que responde |
|---|---|---|---|
| 1 | ADR-001 a ADR-015 | [adrs.md](adrs.md) ([Wiki #55](https://dev.azure.com/escola-ti/Hospetse/_wiki/wikis/Hospetse.wiki/55)) | Por que cada tecnologia e padrão foi escolhido |
| 2 | Diagramas C4 | [c4](c4) | Contexto, containers e componentes do sistema |
| 3 | Ambientes, Deploy e CI/CD | [ambientes-e-deploy.md](ambientes-e-deploy.md) | Como rodar localmente, variáveis de ambiente, Docker, pipelines e Railway |
| 4 | API REST e Swagger | [Wiki #88](https://dev.azure.com/escola-ti/Hospetse/_wiki/wikis/Hospetse.wiki/88) (só na Wiki) | Rotas, parâmetros e respostas do Auth Server e do back-end |
| 5 | Integração de Pagamento | [integracao-de-pagamento](integracao-de-pagamento) | Como cobrar, estornar e registrar pagamentos com o Stripe em sandbox |

## Visão rápida

```text
App Mobile (Expo) ──► Auth Server ──► PostgreSQL do Auth
        │                  │
        │                  └── rota interna (provisiona o usuário)
        ▼                  ▼
     Back-end API ──► PostgreSQL do back-end
                  ├─► Redis (cache)
                  └─► Stripe, e-mail, mapas e IA
```

- O Auth Server cuida só da identidade: PIN por e-mail, senha com Argon2id e JWT RS256. O back-end nunca recebe senha (ADR-011 e ADR-012).
- O back-end valida o JWT com a chave pública e aplica as regras de negócio.
- Os dois serviços rodam em containers Docker no Railway, com deploy pelo Azure Pipelines a partir da `develop` (ADR-007).
