
# Definition of Done (DoD) e Critérios de Aceite

Este documento explica dois conceitos que são frequentemente confundidos, mas que respondem perguntas diferentes:

- **Definition of Done (DoD)** responde: "o que precisa ser verdade para *qualquer* item ser considerado terminado?"
- **Critérios de Aceite** respondem: "o que precisa ser verdade para *este* item específico atender ao que foi pedido?"

## Definition of Done (DoD)

A DoD é uma lista de condições **genéricas e fixas**, aplicada a todo PBI (ou Task) do mesmo tipo, independente do que ele entrega. Ela garante um padrão mínimo de qualidade e consistência no trabalho do time.

> Regra prática: se a condição vale para *todo* item, é DoD. Se só vale para *esse* item específico, é Critério de Aceite.

### DoD no nível de Task

Uma Task pode ser movida para **Done** quando:

- [ ] O trabalho descrito na Task foi de fato realizado
- [ ] O resultado foi revisado por pelo menos uma outra pessoa (code review, revisão de design, revisão de documento)
- [ ] Não há pendências conhecidas que bloqueiam a Task seguinte
- [ ] O tempo trabalhado foi registrado (Remaining Work atualizado / horas logadas)
- [ ] Qualquer decisão tomada durante a execução foi documentada (comentário na Task, Wiki, ou onde fizer sentido)

### DoD no nível de PBI

Um PBI só pode ser considerado **Done** quando:

- [ ] Todas as Tasks filhas estão em Done
- [ ] Os Critérios de Aceite do PBI (ver seção abaixo) foram verificados e atendidos
- [ ] O código foi revisado e aprovado via Pull Request (quando aplicável)
- [ ] O Pull Request foi integrado à branch principal (quando aplicável)
- [ ] O pipeline de build/testes foi executado com sucesso (quando aplicável)
- [ ] Não existem bugs críticos ou impeditivos abertos relacionados ao item
- [ ] A funcionalidade foi validada por quem pediu (PO ou responsável pela área)
- [ ] A documentação relevante foi atualizada (Wiki de Requisitos/Casos de Uso, se aplicável)

> Esta lista é um ponto de partida. O time deve revisar e ajustar conforme a realidade do projeto for aparecendo (ex: adicionar "testado em homologação" quando houver ambiente de homologação).

## Critérios de Aceite

Critérios de Aceite são condições **específicas de um único PBI**, escritas no momento em que o item é criado ou refinado. Eles descrevem o comportamento esperado *daquela* entrega, do ponto de vista de quem vai usar ou se beneficiar dela.

### Como escrever um bom critério de aceite

Formato recomendado (Given / When / Then, ou em português Dado / Quando / Então):

```
Dado que [contexto/pré-condição]
Quando [ação realizada]
Então [resultado esperado]
```

**Exemplo aplicado a um PBI de login:**

```
Dado que o usuário possui cadastro ativo
Quando ele informa e-mail e senha corretos
Então o sistema o redireciona para a tela inicial

Dado que o usuário informa credenciais inválidas
Quando ele tenta fazer login
Então o sistema exibe uma mensagem de erro genérica, sem indicar se o e-mail existe
```

Se o formato Given/When/Then parecer pesado para itens simples, uma lista direta também funciona:

```
Critérios de aceitação:
- Login válido redireciona para a tela inicial
- Credenciais inválidas exibem mensagem de erro genérica
- Funciona na versão web e mobile
```

### Onde registrar

Os Critérios de Aceite ficam na **descrição do próprio PBI** no Azure Boards (campo Description), não na Wiki. Isso porque eles são específicos daquele item e precisam estar visíveis direto no board, no momento em que alguém for trabalhar ou revisar o item.

## Diferença resumida

| | Definition of Done | Critérios de Aceite |
|---|---------------------|----------------------|
| Escopo | Todo item do mesmo tipo | Um PBI específico |
| Quem define | O time, uma vez | A PO (ou quem descreve o PBI), a cada item |
| Onde fica registrado | Nesta página da Wiki | Na descrição do próprio PBI |
| Muda com frequência? | Raramente (só quando o processo evolui) | Sempre, é diferente em cada item |
| Pergunta que responde | "Isso está no padrão de qualidade do time?" | "Isso faz o que foi pedido?" |

---
Ver também: [Requisitos](../02-produto-e-requisitos/requisitos-funcionais-e-nao-funcionais-v2.md) · [Casos de Uso](../02-produto-e-requisitos/casos-de-uso-v2.md)
