# Diagramas de Sequência

O Regulamento pede o esquema geral de cada **tipo** de caso de uso. Por isso há um diagrama por tipo de fluxo, e não um por caso de uso. A [matriz de tipos de sequência](matriz-tipos-de-sequencia.md) explica essa escolha e liga cada um dos 73 casos de uso a um dos tipos.

Cada diagrama tem uma página `.md` com a imagem, os participantes, a narração passo a passo e as limitações. Comece por ela.

| # | Diagrama | Casos de uso | Página |
|---|---|---|---|
| 01 | Cadastro com PIN e provisionamento | Identidade | [01-cadastro-com-pin-e-provisionamento.md](01-cadastro-com-pin-e-provisionamento.md) |
| 02 | Login e renovação do token | Identidade | [02-login-e-renovacao-do-token.md](02-login-e-renovacao-do-token.md) |
| 03 | CRUD genérico (cadastro de pet) | CRUD do próprio recurso | [03-crud-generico-cadastro-de-pet.md](03-crud-generico-cadastro-de-pet.md) |
| 04 | Consulta genérica (busca de Host) | Consulta e relatório | [04-consulta-generica-busca-de-host.md](04-consulta-generica-busca-de-host.md) |
| 05 | Solicitar e decidir reserva | Contratação transacional | [05-solicitar-e-decidir-reserva.md](05-solicitar-e-decidir-reserva.md) |
| 06 | Pagamento e retorno do gateway | Pagamento com gateway | [06-pagamento-e-retorno-do-gateway.md](06-pagamento-e-retorno-do-gateway.md) |
| 07 | Chat com IA e escalonamento | Tempo real com WebSocket e IA | [07-chat-com-ia-e-escalonamento.md](07-chat-com-ia-e-escalonamento.md) |
| 08 | Validação de prestador | Decisão administrativa com auditoria | [08-validacao-de-prestador.md](08-validacao-de-prestador.md) |

O diagrama 07 atende ao requisito do Regulamento de ter pelo menos um caso de uso com WebSocket.

## Convenções

- `actor` para pessoas, `boundary` para o App, `control` para o Auth Server e o back-end, `database` para os bancos.
- Serviço externo aparece como `participant` com o estereótipo `<<serviço externo>>`.
- Mensagens em cinza ainda não existem no App. Cada página informa o que já existe no código.
