# Matriz de tipos de sequência do Hospetse

Justificativa e rastreabilidade dos Diagramas de Sequência (#508 HSP-PBI-081-TSK-02).

## Por que um diagrama por tipo, e não um por caso de uso

O Regulamento ETI 2026, na lista de artefatos mínimos, pede:

> Diagramas de sequência apresentando o esquema geral dos tipos de caso de uso previstos (por exemplo: cadastros básicos, processamentos em batch, integração de serviços, emissão de relatórios, etc.).

O texto pede o **esquema geral** de cada **tipo** de caso de uso. Os exemplos citados são categorias, não casos específicos.

Além disso, vários casos de uso têm a mesma sequência de interação. Cadastrar pet, editar endereço e definir preços passam pelos mesmos participantes, na mesma ordem: aplicativo, validação do token, regra de negócio, banco e resposta. Um diagrama por caso repetiria o mesmo desenho e multiplicaria o trabalho de manter os artefatos atualizados junto com a implementação, como o Regulamento também exige.

A matriz abaixo garante que nenhum caso de uso fica sem cobertura: cada um dos 73 casos está associado a exatamente um tipo.

## Fonte

- Casos de uso: Revisão v2 da Wiki do projeto (página 78, `CASOS-DE-USO-V2`), com 73 casos. Cópia local em `CASOS_DE_USO__HOSPETSE.md`, feita em 01/11/2026.
- Implementação: repositórios `hospetse-auth-service` e `hospetse-backend`.

## Critério de agrupamento

Dois casos de uso pertencem ao mesmo tipo quando a sequência de interação é a mesma. Os pontos comparados foram:

- quais participantes entram (aplicativo, Auth Server, back-end, banco, serviço externo, Administrador);
- se a comunicação é síncrona (requisição e resposta) ou assíncrona (retorno posterior, evento, conexão persistente);
- se há concorrência a controlar (duas decisões ao mesmo tempo, vaga, idempotência).

Diferenças que não mudam a sequência, como os campos do formulário ou o papel exigido, não criam um tipo novo. Elas aparecem como fragmentos `opt` ou `alt` dentro do diagrama do tipo.

## Tipos

| # | Tipo | Casos | O que se repete | O que varia | Diagrama |
|---|---|---|---|---|---|
| 1 | Identidade | 6 | App, Auth Server e back-end; emissão de tokens | cadastro com PIN e provisionamento do usuário no back-end; login com verificação do estado da conta | Cadastro com PIN e provisionamento; Login e renovação do token |
| 2 | CRUD do próprio recurso | 17 | App → token → regra → banco → resposta; erro de validação | papel exigido; regra de bloqueio (pet com reserva ativa, uma avaliação por contratação); revalidação do perfil; auditoria nas ações do Administrador | CRUD genérico, com o cadastro de pet como exemplo |
| 3 | Consulta e relatório | 16 | consulta com filtro e paginação; lista vazia | cálculo de disponibilidade na busca; indicadores e relatórios do Administrador; três casos sem back-end (HPET31, HPET34, HPET35) | Consulta genérica, com a busca de Host como exemplo |
| 4 | Contratação transacional | 11 | solicitação, decisão do prestador e mudança de estado da contratação; notificação | trava de concorrência, vaga e pet livre; idempotência; check-in, atividades e check-out | Solicitar e aceitar/recusar reserva |
| 5 | Pagamento com gateway | 9 | cobrança no gateway; registro da transação | retorno assíncrono do gateway; estorno; repasse com taxa da plataforma; tratamento de falha pelo Administrador | Pagamento e retorno do gateway |
| 6 | Tempo real (WebSocket + IA) | 2 | conexão WebSocket; entrega em tempo real | resposta da IA; escalonamento para o Host; reconexão com fila de mensagens | Chat com IA e escalonamento |
| 7 | Decisão administrativa com auditoria | 12 | item entra em fila; o Administrador decide; auditoria; mudança de estado; notificação | envio de documentos; denúncia; penalidade com prazo; reenvio após rejeição | Validação de prestador |

Total: 73 casos de uso.

## Partes comuns

Estas interações aparecem em vários tipos e permanecem representadas dentro dos fluxos correspondentes:

- **Validar o token de acesso:** presente em todos os tipos. No tipo 1, só depois do login (parte 2 do diagrama de login).
- **Notificar o usuário:** presente em cerca de 30 casos de uso.
- **Registrar na trilha de auditoria:** ações do Administrador nos tipos 2 e 7. Não é um diagrama separado; aparece no fluxo da decisão administrativa do tipo 7.

## Diagramas a produzir

| # | Diagrama | Tipo | Base |
|---|---|---|---|
| 1 | Cadastro com PIN e provisionamento | 1 | implementado |
| 2 | Login e renovação do token | 1 | implementado |
| 3 | CRUD genérico (cadastro de pet) | 2 | implementado |
| 4 | Consulta genérica (busca de Host) | 3 | implementado |
| 5 | Solicitar e aceitar/recusar reserva | 4 | implementado |
| 6 | Pagamento e retorno do gateway | 5 | planejado |
| 7 | Chat com IA e escalonamento | 6 | planejado |
| 8 | Validação de prestador | 7 | planejado |

"Implementado" significa que o diagrama pode ser conferido com o código atual. "Planejado" significa que o diagrama descreve o fluxo previsto no caso de uso, ainda sem implementação.

## Casos de fronteira

- **HPET39 (Inativar Própria Conta):** classificado como Identidade porque encerra a sessão. A mudança de estado da conta também lembra o tipo 7.
- **HPET12c, HPET17b, HPET19b (registro de atividade e execução):** classificados como Contratação. O caminho de incidente para ocorrência segue o tipo 7.
- **HPET07 (Cadastrar Forma de Pagamento):** classificado como Pagamento porque depende de tokenização no gateway. Se for apenas um cadastro local, passa para o tipo 2.

## Matriz completa

| ID | Caso de uso | Canal | Tipo |
|---|---|---|---|
| HPET01 | Cadastrar-se | Mobile | 1. Identidade |
| HPET01a | Autenticar-se (Login) | Mobile | 1. Identidade |
| HPET01b | Recuperar Senha | Mobile | 1. Identidade |
| HPET01c | Editar Perfil do Tutor | Mobile | 2. CRUD do próprio recurso |
| HPET02 | Cadastrar Pet | Mobile | 2. CRUD do próprio recurso |
| HPET02a | Editar Dados do Pet | Mobile | 2. CRUD do próprio recurso |
| HPET02b | Inativar/Remover Pet | Mobile | 2. CRUD do próprio recurso |
| HPET03 | Buscar Serviços | Mobile | 3. Consulta e relatório |
| HPET04 | Solicitar Reserva | Mobile | 4. Contratação transacional |
| HPET04a | Cancelar Reserva/Solicitação | Mobile | 4. Contratação transacional |
| HPET05 | Solicitar Serviço (Pet Sitter/Walker) | Mobile | 4. Contratação transacional |
| HPET06 | Conversar com Host | Mobile | 6. Tempo real (WebSocket + IA) |
| HPET07 | Cadastrar Forma de Pagamento | Mobile | 5. Pagamento com gateway |
| HPET07a | Realizar Pagamento | Mobile | 5. Pagamento com gateway |
| HPET07b | Consultar Extrato de Pagamentos | Mobile | 3. Consulta e relatório |
| HPET08 | Avaliar Host/Serviço | Mobile | 2. CRUD do próprio recurso |
| HPET08a | Consultar Histórico de Reservas | Mobile | 3. Consulta e relatório |
| HPET09 | Acompanhar Rotina do Pet | Mobile | 3. Consulta e relatório |
| HPET29 | Consultar Central de Notificações | Mobile | 3. Consulta e relatório |
| HPET30 | Favoritar Host/Serviço | Mobile | 2. CRUD do próprio recurso |
| HPET31 | Compartilhar Perfil de Host | Mobile | 3. Consulta e relatório |
| HPET32 | Aplicar Cupom de Desconto | Mobile | 5. Pagamento com gateway |
| HPET33 | Consultar Avaliações | Mobile | 3. Consulta e relatório |
| HPET10 | Cadastrar Perfil de Host/Hotel | Mobile | 7. Decisão administrativa com auditoria |
| HPET10a | Editar Perfil de Host/Hotel | Mobile | 2. CRUD do próprio recurso |
| HPET10b | Inativar Perfil de Host | Mobile | 2. CRUD do próprio recurso |
| HPET11 | Gerenciar Agenda | Mobile | 2. CRUD do próprio recurso |
| HPET11a | Definir Preços e Tabela de Serviços | Mobile | 2. CRUD do próprio recurso |
| HPET12 | Receber Solicitação de Reserva | Mobile | 3. Consulta e relatório |
| HPET12a | Aceitar/Recusar Solicitação de Reserva | Mobile | 4. Contratação transacional |
| HPET12b | Confirmar Check-in | Mobile | 4. Contratação transacional |
| HPET12c | Registrar Atividades do Pet | Mobile | 4. Contratação transacional |
| HPET12d | Confirmar Check-out | Mobile | 4. Contratação transacional |
| HPET12e | Consultar Financeiro | Mobile | 3. Consulta e relatório |
| HPET12f | Solicitar Repasse | Mobile | 5. Pagamento com gateway |
| HPET06b | Configurar Assistente de IA do Chat | Mobile | 2. CRUD do próprio recurso |
| HPET06c | Gerenciar Conversas Escaladas pela IA | Mobile | 6. Tempo real (WebSocket + IA) |
| HPET16 | Cadastrar-se como Pet Sitter | Mobile | 7. Decisão administrativa com auditoria |
| HPET16a | Editar Perfil de Pet Sitter | Mobile | 2. CRUD do próprio recurso |
| HPET16b | Definir Área de Atuação/Disponibilidade (Pet Sitter) | Mobile | 2. CRUD do próprio recurso |
| HPET17 | Receber Solicitação de Visita | Mobile | 3. Consulta e relatório |
| HPET17a | Aceitar/Recusar Solicitação de Visita | Mobile | 4. Contratação transacional |
| HPET17b | Registrar Execução do Serviço | Mobile | 4. Contratação transacional |
| HPET18 | Cadastrar-se como Pet Walker | Mobile | 7. Decisão administrativa com auditoria |
| HPET18a | Editar Perfil de Pet Walker | Mobile | 2. CRUD do próprio recurso |
| HPET18b | Definir Área de Atuação/Disponibilidade (Pet Walker) | Mobile | 2. CRUD do próprio recurso |
| HPET19 | Aceitar/Recusar Passeio | Mobile | 4. Contratação transacional |
| HPET19b | Registrar Execução do Passeio | Mobile | 4. Contratação transacional |
| HPET36 | Denunciar Usuário ou Conteúdo | Mobile | 7. Decisão administrativa com auditoria |
| HPET37 | Corrigir e Reenviar Cadastro Rejeitado | Mobile | 7. Decisão administrativa com auditoria |
| HPET38 | Visualizar Aviso de Penalidade | Mobile | 7. Decisão administrativa com auditoria |
| HPET39 | Inativar Própria Conta | Mobile | 1. Identidade |
| HPET34 | Consultar Informações Institucionais | Web (área pública) | 3. Consulta e relatório |
| HPET35 | Conhecer e Obter o Aplicativo | Web (área pública) | 3. Consulta e relatório |
| HPET20 | Gerenciar Usuários | Web (admin) | 7. Decisão administrativa com auditoria |
| HPET20a | Autenticar-se (Administrador) | Web (admin, rota separada `/admin`) | 1. Identidade |
| HPET20b | Validar Cadastro de Host/Prestador | Web (admin) | 7. Decisão administrativa com auditoria |
| HPET25 | Gerenciar Penalidades | Web (admin) | 7. Decisão administrativa com auditoria |
| HPET25a | Acompanhar e Reverter Penalidades | Web (admin) | 7. Decisão administrativa com auditoria |
| HPET40 | Gerenciar Cupons de Desconto | Web (admin) | 2. CRUD do próprio recurso |
| HPET41 | Configurar Taxa da Plataforma | Web (admin) | 2. CRUD do próprio recurso |
| HPET42 | Moderar Conteúdo Denunciado | Web (admin) | 7. Decisão administrativa com auditoria |
| HPET42a | Solicitar Acesso à Conversa Completa | Web (admin) | 7. Decisão administrativa com auditoria |
| HPET43 | Tratar Transação Financeira com Falha | Web (admin) | 5. Pagamento com gateway |
| HPET44 | Consultar Dashboard Administrativo | Web (admin) | 3. Consulta e relatório |
| HPET45 | Consultar Contratações | Web (admin) | 3. Consulta e relatório |
| HPET46 | Consultar Financeiro da Plataforma | Web (admin) | 3. Consulta e relatório |
| HPET47 | Consultar Trilha de Auditoria | Web (admin) | 3. Consulta e relatório |
| HPET48 | Gerenciar Conta do Administrador | Web (admin) | 1. Identidade |
| HPET26 | Processar Pagamento | Backend | 5. Pagamento com gateway |
| HPET27 | Repassar Valores | Backend | 5. Pagamento com gateway |
| HPET28 | Processar Cancelamento / Estorno | Backend | 5. Pagamento com gateway |
| HPET28a | Registrar Transação Financeira | Backend | 5. Pagamento com gateway |
