<!-- Fonte: Wiki Azure DevOps escola-ti/Hospetse, página 78 CASOS-DE-USO-V2, copiada em 01/11/2026. A wiki é a fonte oficial. -->

      
Casos de Uso HOSPETSE — Revisão v2
==================================

Sobre esta revisão
------------------

O conjunto passa de **56 para 73 casos de uso**: 17 novos e 36 existentes alterados; 20 seguem sem alteração de regra. Nenhum identificador existente foi renumerado.

### Base da revisão

*   **Divisão de canais (DF-01/DF-02):** Web = módulo Administrativo + área comercial pública; Mobile = Tutor, Host, Pet Sitter e Pet Walker.
*   **Papel único de Administrador (DF-03)** e decisões DA-01 a DA-15 (out/2026).
*   **Decisões de PO anteriores:** pet como parâmetro da busca (tela 08I), inativação lógica do pet, pagamento somente após o aceite do prestador.
*   Planilha de Fluxo Web v2, Planilha de Fluxo do Tutor (inventário de jornadas) e Visão Macro (PBIs HSP-PBI-001 a 076).

### Legenda

| Revisão | Significado |
| --- | --- |
| Novo | Caso de uso criado nesta revisão |
| Alterado | Regra, ator, pré-condição ou fluxo ajustado |
| Sem alteração | Mantido sem alteração de regra |

* * *

Módulo Tutor (Mobile)
---------------------

### HPET01 – Cadastrar-se

| Campo | Conteúdo |
| --- | --- |
| Revisão | Sem alteração |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o tutor crie uma conta na plataforma para acessar os serviços disponíveis. |
| Atores | Tutor |
| Pré-Condições | O usuário não deve possuir cadastro prévio na plataforma. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O tutor acessa a tela de cadastro |  |
|  | Solicitar o preenchimento dos dados pessoais |
| O tutor informa dados pessoais (nome, e-mail, telefone e senha) |  |
|  | O sistema valida os dados informados |
|  | O sistema envia confirmação por e-mail ou telefone |
| O tutor valida sua conta |  |
|  | A conta é criada com sucesso |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o tutor não preencha algum campo de forma correta, emitir mensagem de erro |

### HPET01a – Autenticar-se (Login)

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o usuário do aplicativo acesse a plataforma mediante validação de suas credenciais e do estado da conta, conforme pré-condição de autenticação prevista no padrão de Manter Caso de Uso (Geral). |
| Atores | Usuário do app (Tutor, Host, Pet Sitter, Pet Walker) |
| Pré-Condições | O usuário deve possuir cadastro validado na plataforma (HPET01). |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O usuário acessa a tela de login |  |
|  | Solicitar e-mail e senha |
| O usuário informa e-mail e senha |  |
|  | O sistema valida as credenciais |
|  | O sistema verifica o estado da conta (ativa, suspensa, banida ou inativa) |
|  | O sistema autentica o usuário e libera o acesso conforme os papéis do usuário |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso as credenciais sejam inválidas, emitir mensagem de erro genérica, sem revelar se a conta existe |
|  | Caso a conta esteja suspensa, negar o acesso e informar o motivo resumido e a data de fim da suspensão |
|  | Caso a conta esteja banida, negar o acesso e informar o banimento |
|  | Caso a conta esteja inativa, negar o acesso e orientar sobre a reativação (HPET20) |
|  | Caso exista penalidade não visualizada (ex.: advertência), exibir o aviso após o login (HPET38) |

### HPET01b – Recuperar Senha

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o usuário do aplicativo redefina sua senha de acesso, restabelecendo os dados informados no cadastro (HPET01). A recuperação de senha do Administrador não é coberta por este caso (ver HPET20a). |
| Atores | Usuário do app (Tutor, Host, Pet Sitter, Pet Walker) |
| Pré-Condições | O usuário deve possuir cadastro na plataforma com e-mail ou telefone válido. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O usuário seleciona a opção "esqueci minha senha" |  |
|  | Solicitar e-mail ou telefone cadastrado |
| O usuário informa e-mail ou telefone |  |
|  | O sistema envia confirmação por e-mail ou telefone |
| O usuário valida o código recebido e define nova senha |  |
|  | O sistema atualiza a senha de acesso |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o e-mail/telefone informado não esteja cadastrado, emitir mensagem de erro |
|  | A redefinição de senha não altera o estado da conta: conta suspensa, banida ou inativa continua sem acesso (HPET01a) |

### HPET01c – Editar Perfil do Tutor

| Campo | Conteúdo |
| --- | --- |
| Revisão | Sem alteração |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o tutor altere os dados pessoais informados no cadastro (nome, e-mail, telefone e senha). |
| Atores | Tutor |
| Pré-Condições | O tutor deve estar autenticado na plataforma. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O tutor seleciona a opção "alterar" |  |
|  | O sistema habilita os campos para edição |
| O tutor altera os campos pretendidos |  |
|  | O sistema valida os dados alterados |
|  | O sistema apresenta os dados atualizados |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o tutor não preencha algum campo de forma correta, emitir mensagem de erro |

### HPET02 – Cadastrar Pet

| Campo | Conteúdo |
| --- | --- |
| Revisão | Sem alteração |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o tutor registre um ou mais pets no sistema para utilização dos serviços da plataforma. |
| Atores | Tutor |
| Pré-Condições | O tutor deve estar autenticado na plataforma. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O tutor acessa a área de cadastro de pets |  |
|  | Solicitar dados do pet |
| O tutor informa dados do pet (nome, espécie, raça, porte e idade) |  |
| O tutor informa características comportamentais |  |
| O tutor adiciona informações médicas (vacinas, alergias ou medicações) |  |
| O tutor opcionalmente envia documentos ou comprovantes |  |
|  | O sistema registra o pet no perfil do tutor |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o tutor não preencha algum campo de forma correta, emitir mensagem de erro |

### HPET02a – Editar Dados do Pet

| Campo | Conteúdo |
| --- | --- |
| Revisão | Sem alteração |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o tutor altere os dados cadastrados do pet (nome, espécie, raça, porte, idade, características comportamentais e informações médicas). |
| Atores | Tutor |
| Pré-Condições | O pet deve estar previamente cadastrado no perfil do tutor (HPET02). |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O tutor seleciona o pet e a opção "alterar" |  |
|  | O sistema habilita os campos para edição |
| O tutor altera os campos pretendidos |  |
|  | O sistema valida os campos alterados |
|  | O sistema apresenta os dados atualizados |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o tutor não preencha algum campo de forma correta, emitir mensagem de erro |

### HPET02b – Inativar/Remover Pet

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o tutor inative o cadastro de um pet por inativação lógica: o registro e o histórico são preservados e não há exclusão física. |
| Atores | Tutor |
| Pré-Condições | O pet deve estar previamente cadastrado no perfil do tutor (HPET02). |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O tutor seleciona a opção "inativar" |  |
|  | O sistema apresenta os registros de pets a inativar |
| O tutor seleciona e inativa o pet pretendido |  |
|  | O sistema inativa o registro do pet e o retira da seleção de pets da busca e da reserva, mantendo o histórico consultável |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso ocorra falha ao processar a inativação, emitir mensagem de erro e manter o pet ativo |
|  | Caso o pet possua reserva ou solicitação ativa ou futura, impedir a inativação e informar o motivo |

### HPET03 – Buscar Serviços

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o tutor solicite/busque os serviços disponíveis para hospedar seu pet. |
| Atores | Tutor |
| Pré-Condições | O tutor deve estar autenticado na plataforma. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O tutor acessa a busca de hospedagens |  |
| O tutor define filtros (localização, preço e disponibilidade), tem a opção de selecionar o pet para direcionar a busca |  |
|  | O sistema aplica espécie e porte do pet selecionado como critério da busca (caso tenha selecionado o pet) |
|  | O sistema exibe os hosts disponíveis (somente perfis validados e com conta ativa) |
| O tutor visualiza perfis de hosts |  |
| O tutor seleciona um host para visualizar detalhes |  |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso nenhum host seja encontrado com os filtros informados, informar ausência de resultados |
|  | Caso o tutor não possua pet ativo cadastrado, exibir aviso (tela 08I) e direcioná-lo ao cadastro de pet (HPET02) |

### HPET04 – Solicitar Reserva

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o tutor solicite a hospedagem de seu pet em um host disponível. |
| Atores | Tutor |
| Pré-Condições | O tutor deve possuir ao menos um pet ativo cadastrado e ter selecionado um host. |

**Fluxo principal**

| Ações do Ator | Ações do Sistema |
| --- | --- |
| O tutor seleciona um host |  |
| O tutor define as datas da hospedagem |  |
| O tutor seleciona o pet que será hospedado |  |
| O tutor informa os medicamentos do pet (tipo, horários e duração, forma de aplicação), se houver e se o host oferecer administração de medicamentos |  |
| O tutor escolhe serviços adicionais (se disponíveis) |  |
| O tutor confirma a solicitação |  |
|  | O sistema envia a solicitação ao host com status "aguardando aceite" |
|  | O sistema informa que o pagamento será solicitado somente após o aceite do host (HPET07a) |
|  | O sistema informa que o host tem 1 dia útil (segunda a sexta, sem feriados) para responder |

**Fluxo alternativo**

| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o tutor não preencha algum campo de forma correta, emitir mensagem de erro |
|  | Caso o pet selecionado não seja aceito pelo host (espécie/porte), exibir mensagem "Pet não permitido" – Infelizmente este anfitrião não aceita gatos/cães de grande porte. |
|  | Caso o host não esteja disponível nas datas selecionadas, exibir mensagem "Data indisponível" – Infelizmente o hotel não está disponível na data selecionada. |
|  | Caso ocorra falha ao processar a solicitação, exibir mensagem "Algo deu errado" – Não conseguimos processar sua solicitação no momento. Tente novamente mais tarde. |
|  | Caso o host não responda em 1 dia útil, a solicitação é cancelada automaticamente e o tutor é notificado
### HPET04a – Cancelar Reserva/Solicitação

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o tutor cancele uma reserva ou solicitação de serviço realizada (HPET04/HPET05). |
| Atores | Tutor |
| Pré-Condições | O tutor deve possuir uma reserva ou solicitação ativa. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O tutor acessa o histórico de reservas |  |
| O tutor seleciona a reserva/solicitação e a opção "cancelar" |  |
|  | O sistema apresenta a reserva a cancelar |
| O tutor confirma o cancelamento |  |
|  | O sistema atualiza o status da reserva e notifica o host ou prestador |
|  | Caso a reserva já tenha pagamento aprovado, o sistema aciona o estorno (HPET28) |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso a reserva já esteja concluída, emitir mensagem de erro |

### HPET05 – Solicitar Serviço (Pet Sitter/Walker)

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o tutor solicite o serviço de pet sitter ou pet walker para seu pet. |
| Atores | Tutor |
| Pré-Condições | O tutor deve possuir ao menos um pet ativo cadastrado. |

**Fluxo principal**

| Ações do Ator | Ações do Sistema |
| --- | --- |
| O tutor acessa a solicitação de serviço |  |
| O tutor seleciona o tipo de serviço (pet sitter ou pet walker) |  |
| O tutor define filtros (localização, preço e disponibilidade), podendo selecionar o pet como filtro |  |
|  | O sistema aplica espécie e porte do pet selecionado como critério e exibe somente prestadores validados e com conta ativa |
| O tutor seleciona o prestador de serviço |  |
| O tutor define data/horário |  |
| O tutor informa os medicamentos do pet (tipo, horários e duração, forma de aplicação), se houver e se o prestador oferecer administração de medicamentos |  |
| O tutor confirma a solicitação |  |
|  | O sistema envia a solicitação ao prestador com status "aguardando aceite"; o pagamento ocorre somente após o aceite (HPET07a) |
|  | O sistema informa que o prestador tem 1 dia útil (segunda a sexta, sem feriados) para responder |

**Fluxo alternativo**

| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o tutor não preencha algum campo de forma correta, emitir mensagem de erro |
|  | Caso o tutor não possua pet ativo cadastrado, exibir aviso (tela 08I) e direcioná-lo ao cadastro de pet (HPET02) |
|  | Caso o prestador não responda em 1 dia útil, a solicitação é cancelada automaticamente e o tutor é notificado |
### HPET06 – Conversar com Host

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite comunicação entre tutor e host, com suporte de um Assistente de IA para dúvidas frequentes (baseadas em informações pré-definidas pelo host) antes e durante a reserva, com escalonamento para atendimento humano quando necessário, via WebSocket. |
| Atores | Tutor / Host / Assistente de IA (ator de sistema) |
| Pré-Condições | Tutor autenticado. Deve existir um host válido para iniciar conversa com a IA (reserva ativa não é obrigatória). Para falar diretamente com o host sem reserva, o host deve ter habilitado atendimento direto pré-reserva. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O tutor acessa a página do host ou a reserva |  |
| O tutor inicia a conversa, escolhendo o Assistente de IA ou (se houver reserva ativa) o host diretamente |  |
|  | Sistema abre o chat via WebSocket e direciona a conversa conforme a escolha do tutor |
| O tutor envia mensagens ou mídia ao Assistente de IA ou ao host |  |
|  | Sistema entrega a mensagem em tempo real; se destinada à IA, esta responde com base na base de informações pré-definidas do host; se destinada ao host, este a recebe e responde |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso a reserva não esteja mais ativa, restringir o envio de novas mensagens diretas ao host; o Assistente de IA permanece disponível para dúvidas gerais, se o host permitir |
|  | Caso a IA não consiga responder com base na sua base de informações, o sistema escalona automaticamente a conversa para o host e o notifica |
|  | Caso o tutor solicite falar com um atendente humano a qualquer momento, o sistema encerra o atendimento da IA e transfere a conversa para o host, mantendo o histórico |
|  | Caso a conexão via WebSocket seja perdida, o sistema tenta reconexão automática e mantém as mensagens não entregues em fila até restabelecer a conexão |
| Tutor ou host denuncia uma mensagem recebida | O sistema registra a denúncia (HPET36); somente a mensagem denunciada fica visível à moderação (HPET42) |

### HPET07 – Cadastrar Forma de Pagamento

| Campo | Conteúdo |
| --- | --- |
| Revisão | Sem alteração |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o tutor cadastre a forma de pagamento a ser utilizada na plataforma, conforme etapa "escolhe a forma de pagamento" de Realizar Pagamento (HPET07a). |
| Atores | Tutor |
| Pré-Condições | O tutor deve estar autenticado na plataforma. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O tutor acessa a tela de formas de pagamento |  |
|  | Solicitar os dados da forma de pagamento |
| O tutor informa os dados solicitados |  |
|  | O sistema valida os dados informados |
|  | O sistema registra a forma de pagamento |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o tutor não preencha algum campo de forma correta, emitir mensagem de erro |

### HPET07a – Realizar Pagamento

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o tutor realize o pagamento da reserva ou do serviço (Pet Sitter/Walker) pela plataforma, após o aceite do prestador. |
| Atores | Tutor |
| Pré-Condições | A solicitação deve ter sido aceita pelo host/prestador (status "aguardando pagamento") há no máximo 1 dia útil (segunda a sexta, sem feriados) e o tutor deve possuir forma de pagamento cadastrada. |

**Fluxo principal**

| Ações do Ator | Ações do Sistema |
| --- | --- |
| O tutor acessa a tela de pagamento |  |
| O tutor escolhe a forma de pagamento |  |
| O tutor opcionalmente aplica um cupom (HPET32) |  |
| O tutor confirma a transação |  |
|  | O sistema processa o pagamento (HPET26) |
|  | O sistema altera o status da reserva/serviço para "confirmada" e notifica o prestador |
| O tutor recebe confirmação e comprovante |  |

**Fluxo alternativo**

| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o pagamento seja recusado, emitir mensagem de erro e permitir nova tentativa com outra forma de pagamento, sem refazer a solicitação |
|  | Caso o prazo de 1 dia útil após o aceite vença sem pagamento, a solicitação é cancelada automaticamente, a agenda do prestador é liberada e as partes são notificadas |
|  | Caso o pagamento esteja em processamento no gateway quando o prazo vencer, aguardar o retorno (HPET26) antes de cancelar |
|  | Ao fim do serviço (check-out ou encerramento da visita/passeio), caso existam custos extras registrados, como atendimento veterinário causado pelo tutor, o sistema solicita ao tutor o pagamento complementar |
### HPET07b – Consultar Extrato de Pagamentos

| Campo | Conteúdo |
| --- | --- |
| Revisão | Sem alteração |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o tutor consulte o histórico de pagamentos realizados, conforme os "resumos financeiros" referenciados no acompanhamento de rotina (HPET09). |
| Atores | Tutor |
| Pré-Condições | O tutor deve estar autenticado na plataforma. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O tutor acessa a área financeira |  |
|  | O sistema exibe o extrato de pagamentos realizados |
| O tutor visualiza detalhes de uma transação |  |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso não existam transações registradas, informar que o extrato está vazio |

### HPET08 – Avaliar Host/Serviço

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o tutor avalie hosts e prestadores. |
| Atores | Tutor |
| Pré-Condições | O tutor deve possuir ao menos uma reserva ou serviço concluído e ainda não avaliado com o host ou prestador. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O tutor acessa a tela de detalhes do host/prestador |  |
|  | O sistema exibe o campo de avaliações |
| O tutor seleciona a opção "Avalie como foi a sua experiência" |  |
|  | O sistema exibe o campo de avaliação |
| O tutor posta a sua avaliação |  |
|  | O sistema registra a avaliação (uma por contratação), sujeita à moderação de conteúdo (HPET42) |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso falte preencher algum campo, o sistema apresenta uma mensagem de erro |
|  | Caso a contratação já tenha sido avaliada, impedir nova avaliação. Somente permitida edição da avaliação |

### HPET08a – Consultar Histórico de Reservas

| Campo | Conteúdo |
| --- | --- |
| Revisão | Sem alteração |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o tutor consulte as reservas realizadas anteriormente, conforme "histórico de reservas" referenciado em Avaliar Serviço (HPET08). |
| Atores | Tutor |
| Pré-Condições | O tutor deve estar autenticado na plataforma. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O tutor acessa o histórico de reservas |  |
|  | O sistema exibe as reservas realizadas |
| O tutor seleciona uma reserva para visualizar detalhes |  |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso não existam reservas registradas, informar que o histórico está vazio |

### HPET09 – Acompanhar Rotina do Pet

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o tutor acompanhe as atualizações enviadas pelo host durante a hospedagem ou pelo pet sitter/pet walker durante o serviço, inclusive incidentes, e autorize ou recuse atendimento veterinário quando proposto. |
| Atores | Tutor |
| Pré-Condições | A hospedagem ou o serviço deve estar ativo. |

**Fluxo principal**

| Ações do Ator | Ações do Sistema |
| --- | --- |
| O tutor acessa a contratação ativa |  |
|  | O sistema exibe os registros enviados pelo prestador |
| O tutor visualiza informações como alimentação, recreação, medicação, fotos, vídeos e incidentes |  |

**Fluxo alternativo**

| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso não haja registros enviados pelo prestador, informar que não há atualizações disponíveis |
| Caso haja proposta de atendimento veterinário, o tutor autoriza | O sistema registra a autorização como contratação adicional vinculada à original e notifica o prestador; se o atendimento foi causado pelo tutor, sem responsabilidade do prestador, o valor entra como custo extra a ser acertado ao fim do serviço (HPET07a) |
| O tutor recusa o atendimento veterinário | O sistema registra a recusa e notifica o prestador, que segue a orientação do tutor |
|  | Caso o incidente seja grave e com possível responsabilidade do prestador, os custos são decididos pelo Administrador com base no ocorrido (HPET25) |
### HPET29 – Consultar Central de Notificações

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o usuário visualize as notificações recebidas sobre mudanças de status de reservas, solicitações e pagamentos, resultado da validação de cadastro de prestador (aprovação ou rejeição com feedback) e penalidades aplicadas ou encerradas. |
| Atores | Tutor / Host / Pet Sitter / Pet Walker |
| Pré-Condições | O usuário deve estar autenticado na plataforma. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O usuário seleciona o ícone de notificações |  |
|  | O sistema exibe a lista de notificações recebidas |
| O usuário seleciona uma notificação para visualizar detalhes |  |
|  | O sistema direciona à tela relacionada (ex.: pagamento pendente após aceite, feedback de cadastro rejeitado – HPET37, aviso de penalidade – HPET38) |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso não existam notificações, o sistema informa que a central está vazia |

### HPET30 – Favoritar Host/Serviço

| Campo | Conteúdo |
| --- | --- |
| Revisão | Sem alteração |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o tutor marque um host ou prestador de serviço como favorito, conforme o ícone de coração presente nos cards. |
| Atores | Tutor |
| Pré-Condições | O tutor deve estar autenticado na plataforma. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O tutor seleciona o ícone de coração no card ou na tela de detalhes |  |
|  | O sistema registra o host/serviço como favorito do tutor |
|  | O sistema atualiza o ícone indicando o item como favoritado |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O tutor seleciona novamente o ícone de coração para desfavoritar | O sistema remove o item da lista de favoritos |

### HPET31 – Compartilhar Perfil de Host

| Campo | Conteúdo |
| --- | --- |
| Revisão | Sem alteração |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o tutor compartilhe o perfil de um host, conforme o ícone de compartilhamento presente na tela de detalhes. |
| Atores | Tutor |
| Pré-Condições | O tutor deve estar visualizando a tela de detalhes de um host. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O tutor seleciona o ícone de compartilhar |  |
|  | O sistema apresenta as opções de compartilhamento disponíveis |
| O tutor seleciona o canal de compartilhamento desejado |  |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso ocorra falha ao compartilhar, o sistema emite mensagem de erro |

### HPET32 – Aplicar Cupom de Desconto

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o tutor aplique um cupom de desconto no momento do pagamento, conforme o campo "Código do cupom" presente na tela de Pagamento. Os cupons são criados e mantidos pelo Administrador (HPET40). |
| Atores | Tutor |
| Pré-Condições | O tutor deve estar na tela de pagamento de uma reserva (HPET07a). |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O tutor informa o código do cupom |  |
| O tutor seleciona a opção "Aplicar" |  |
|  | O sistema valida o cupom informado conforme as regras cadastradas pelo Administrador (situação ativa, vigência e limite de uso) |
|  | O sistema atualiza o total estimado com o desconto aplicado |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o cupom seja inválido, inativo, expirado ou tenha atingido o limite de uso, o sistema emite mensagem de erro e mantém o total sem desconto |

### HPET33 – Consultar Avaliações

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o tutor consulte as notas e avaliações de hosts e prestadores, conforme as notas e o link "Ver todas" exibidos na Home, nos Resultados de busca e na tela de detalhes do protótipo. Complementa o HPET08 (Avaliar Serviço), que trata do ato de avaliar. |
| Atores | Tutor |
| Pré-Condições | O host ou prestador deve possuir ao menos uma avaliação registrada. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O tutor acessa a tela de detalhes do host/prestador |  |
|  | O sistema exibe a nota média e o número de avaliações (desconsiderando avaliações removidas pela moderação – HPET42) |
| O tutor seleciona a opção "Ver todas" |  |
|  | O sistema exibe a lista completa de avaliações publicadas |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso não existam avaliações registradas, o sistema informa que ainda não há avaliações |
| O tutor denuncia uma avaliação | O sistema registra a denúncia (HPET36) |

* * *

Módulo Host/Hotel (Mobile)
--------------------------

### HPET10 – Cadastrar Perfil de Host/Hotel

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que uma pessoa ou empresa se cadastre como host para oferecer hospedagem de pets. O perfil só é publicado após validação do Administrador (HPET20b). |
| Atores | Host |
| Pré-Condições | O usuário deve estar autenticado com sua conta da plataforma (papel Tutor) e não deve possuir cadastro prévio como host. |

**Fluxo principal**

| Ações do Ator | Ações do Sistema |
| --- | --- |
| O usuário acessa o cadastro de host |  |
| O usuário informa dados do estabelecimento ou residência |  |
| O usuário descreve estrutura disponível |  |
| O usuário define capacidade de hospedagem |  |
| O usuário informa se oferece administração de medicamentos |  |
| O usuário envia documentos necessários |  |
| O usuário envia fotos da hospedagem |  |
|  | O sistema registra o perfil de host com status "pendente de validação" e o envia à fila de validação (HPET20b) |
|  | O sistema informa que o perfil não aparecerá na busca até a aprovação |

**Fluxo alternativo**

| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o usuário não preencha algum campo de forma correta, emitir mensagem de erro |
|  | Caso o cadastro seja rejeitado, o usuário recebe o feedback do Administrador e pode corrigir e reenviar (HPET37) |
### HPET10a – Editar Perfil de Host/Hotel

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o host altere os dados de seu perfil (estrutura, capacidade, documentos e oferta de administração de medicamentos). |
| Atores | Host |
| Pré-Condições | O host deve possuir perfil previamente cadastrado (HPET10). |

**Fluxo principal**

| Ações do Ator | Ações do Sistema |
| --- | --- |
| O host acessa a opção "alterar" |  |
|  | O sistema habilita os campos para edição |
| O host altera os campos pretendidos |  |
| O host pode ativar ou desativar a oferta de administração de medicamentos |  |
|  | O sistema valida os dados alterados |
|  | O sistema apresenta os dados atualizados |

**Fluxo alternativo**

| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o host não preencha algum campo de forma correta, emitir mensagem de erro |
|  | Caso a alteração envolva documentos ou dados sujeitos a validação, o sistema envia o perfil para revalidação (HPET20b) |
### HPET10b – Inativar Perfil de Host

| Campo | Conteúdo |
| --- | --- |
| Revisão | Sem alteração |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o host inative seu perfil na plataforma. |
| Atores | Host |
| Pré-Condições | O host deve possuir perfil previamente cadastrado (HPET10). |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O host seleciona a opção "inativar" |  |
|  | O sistema apresenta o perfil a inativar |
| O host confirma a inativação |  |
|  | O sistema inativa o registro do host |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso ocorra falha ao processar a inativação, emitir mensagem de erro |

### HPET11 – Gerenciar Agenda

| Campo | Conteúdo |
| --- | --- |
| Revisão | Sem alteração |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o host controle disponibilidade de hospedagem. |
| Atores | Host |
| Pré-Condições | O host deve estar autenticado na plataforma. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O host acessa sua agenda |  |
| O host define datas disponíveis |  |
| O host bloqueia períodos indisponíveis |  |
|  | O sistema atualiza disponibilidade para os tutores |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o host não preencha algum campo de forma correta, emitir mensagem de erro |

### HPET11a – Definir Preços e Tabela de Serviços

| Campo | Conteúdo |
| --- | --- |
| Revisão | Sem alteração |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o host defina os preços dos serviços oferecidos, utilizados como filtro na busca de serviços do tutor (HPET03 – filtro "preço"). |
| Atores | Host |
| Pré-Condições | O host deve possuir perfil previamente cadastrado (HPET10). |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O host acessa a área de preços e serviços |  |
| O host informa o valor de cada serviço oferecido |  |
|  | O sistema valida os valores informados |
|  | O sistema atualiza os preços exibidos na busca |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o host não preencha algum campo de forma correta, emitir mensagem de erro |

### HPET12 – Receber Solicitação de Reserva

| Campo | Conteúdo |
| --- | --- |
| Revisão | Sem alteração |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o host visualize e gerencie solicitações de hospedagem. |
| Atores | Host |
| Pré-Condições | Um tutor deve ter enviado uma solicitação de reserva (HPET04). |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | O sistema notifica o host sobre nova solicitação |
| O host visualiza dados do pet e do tutor |  |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso não existam solicitações pendentes, informar que não há novas solicitações |

### HPET12a – Aceitar/Recusar Solicitação de Reserva

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o host aceite ou recuse a solicitação de hospedagem recebida. |
| Atores | Host |
| Pré-Condições | Deve existir uma solicitação de reserva pendente (HPET12). |

**Fluxo principal**

| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | O sistema exibe a solicitação e os medicamentos informados pelo tutor, quando houver |
| O host aceita ou recusa a solicitação |  |
| Ao aceitar uma solicitação com medicamentos, o host confirma que assume a responsabilidade pela administração |  |
|  | O sistema atualiza o status da reserva para "aguardando pagamento" quando aceita |
|  | O sistema notifica o tutor para realizar o pagamento (HPET07a) |

**Fluxo alternativo**

| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o host recuse a solicitação, notificar o tutor e encerrar o fluxo |
|  | Caso o host não responda em 1 dia útil (segunda a sexta, sem feriados), a solicitação é cancelada automaticamente, as datas são liberadas e o tutor é notificado |
### HPET12b – Confirmar Check-in

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o host registre a chegada do pet no início da hospedagem, dando início ao acompanhamento de rotina (HPET09/HPET12c). |
| Atores | Host |
| Pré-Condições | A reserva deve estar confirmada (pagamento aprovado – HPET07a) e a data de início ter chegado. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O host acessa a reserva confirmada |  |
| O host confirma a chegada do pet |  |
|  | O sistema registra o início da hospedagem |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso a data de check-in ainda não tenha chegado, emitir mensagem de erro |

### HPET12c – Registrar Atividades do Pet

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite registrar atividades durante a hospedagem (obrigatório pelo menos 1 registro ao dia), incluindo cada administração dos medicamentos informados pelo tutor e o registro de incidentes com gravidade, que gera uma ocorrência para o Administrador. |
| Atores | Host |
| Pré-Condições | A hospedagem deve estar ativa. |

**Fluxo principal**

| Ações do Ator | Ações do Sistema |
| --- | --- |
| O host acessa hospedagem ativa |  |
| O host registra alimentação, recreação, medicação (incluindo cada aplicação dos medicamentos informados pelo tutor) etc. |  |
| O host pode anexar fotos |  |
|  | O sistema registra a atividade e a disponibiliza ao tutor (HPET09) |

**Fluxo alternativo**

| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o host não preencha algum campo de forma correta, emitir mensagem de erro |
| O host marca o registro como incidente e informa tipo (com o pet ou no imóvel), descrição, ação tomada e gravidade (leve, média ou grave), avaliada no momento do ocorrido | O sistema notifica o tutor com prioridade e gera uma ocorrência para análise do Administrador, que revisa a gravidade (HPET25) |
| Caso o incidente seja leve ou médio, o host alerta o tutor e aguarda o retorno antes de agir; se indicar veterinário, propõe o atendimento | O sistema envia a proposta para o tutor autorizar ou recusar (HPET09) |
| Caso o incidente seja grave, o host segue o procedimento padrão e registra a ação tomada | O sistema marca a ocorrência para análise de responsabilidade e custos pelo Administrador (HPET25) |
### HPET12d – Confirmar Check-out

| Campo | Conteúdo |
| --- | --- |
| Revisão | Sem alteração |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o host registre a saída do pet ao término da hospedagem. |
| Atores | Host |
| Pré-Condições | A hospedagem deve estar ativa (HPET12b). |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O host acessa a hospedagem ativa |  |
| O host confirma a saída do pet |  |
|  | O sistema registra o encerramento da hospedagem |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso ocorra falha ao registrar o encerramento, emitir mensagem de erro |

### HPET12e – Consultar Financeiro

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o prestador (host, pet sitter ou pet walker) acompanhe os ganhos obtidos pela plataforma e o saldo disponível para repasse. |
| Atores | Host / Pet Sitter / Pet Walker |
| Pré-Condições | O prestador deve estar autenticado na plataforma. |

**Fluxo principal**

| Ações do Ator | Ações do Sistema |
| --- | --- |
| O prestador acessa o painel financeiro |  |
|  | O sistema exibe ganhos e transações com valor bruto, taxa da plataforma vigente (HPET41) e valor líquido |
| O prestador visualiza relatórios de pagamentos recebidos |  |

**Fluxo alternativo**

| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso não existam transações registradas, informar que o painel está vazio |

### HPET12f – Solicitar Repasse

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o prestador (host, pet sitter ou pet walker) solicite o repasse dos valores recebidos, que é processado em Repassar Valores (HPET27). O repasse só é feito mediante esta solicitação. |
| Atores | Host / Pet Sitter / Pet Walker |
| Pré-Condições | O prestador deve possuir saldo disponível no painel financeiro (HPET12e). |

**Fluxo principal**

| Ações do Ator | Ações do Sistema |
| --- | --- |
| O prestador acessa o painel financeiro |  |
| O prestador solicita o repasse do saldo disponível |  |
|  | O sistema processa o repasse (HPET27) |

**Fluxo alternativo**

| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o saldo disponível seja insuficiente, emitir mensagem de erro |
|  | Caso o repasse falhe no processamento, notificar o prestador e disponibilizar a transação para tratamento pelo Administrador (HPET43) |
### HPET06b – Configurar Assistente de IA do Chat

| Campo | Conteúdo |
| --- | --- |
| Revisão | Sem alteração |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o host cadastre e mantenha a base de informações pré-definidas (FAQ) usada pelo Assistente de IA para responder tutores automaticamente, complementando o chat com IA definido em Conversar com Host (HPET06). |
| Atores | Host |
| Pré-Condições | O host deve possuir perfil previamente cadastrado (HPET10). |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O host acessa a área "Assistente de IA" |  |
|  | Sistema exibe a base de perguntas e respostas atual |
| O host cadastra ou edita perguntas frequentes e respectivas respostas |  |
|  | Sistema valida e salva o conteúdo informado |
| O host define se aceita atendimento humano direto antes de existir reserva (opt-in) |  |
|  | Sistema atualiza a configuração de disponibilidade do chat |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso algum campo não seja preenchido corretamente, emitir mensagem de erro |
|  | Caso o conteúdo cadastrado exceda o limite de caracteres permitido, emitir mensagem de erro |

### HPET06c – Gerenciar Conversas Escaladas pela IA

| Campo | Conteúdo |
| --- | --- |
| Revisão | Sem alteração |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o host visualize e responda conversas que o Assistente de IA não conseguiu resolver, mantendo o histórico completo trocado com o tutor, conforme fluxo de escalonamento definido em Conversar com Host (HPET06). |
| Atores | Host |
| Pré-Condições | Deve existir ao menos uma conversa escalada automaticamente pelo Assistente de IA (HPET06). |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Sistema notifica o host sobre nova conversa escalada |
| O host acessa a fila de conversas pendentes |  |
|  | Sistema exibe o histórico completo da conversa, incluindo as trocas com a IA |
| O host responde diretamente ao tutor |  |
|  | Sistema entrega a resposta em tempo real ao tutor via WebSocket |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso não existam conversas pendentes, informar que a fila está vazia |
|  | Caso a conexão via WebSocket seja perdida, sistema tenta reconexão automática e mantém a mensagem em fila |

* * *

Módulos Pet Sitter e Pet Walker (Mobile)
----------------------------------------

### HPET16 – Cadastrar-se como Pet Sitter

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que um profissional se cadastre para oferecer serviços de cuidado domiciliar. O perfil só é publicado após validação do Administrador (HPET20b). |
| Atores | Pet Sitter |
| Pré-Condições | O usuário deve estar autenticado com sua conta da plataforma (papel Tutor) e não deve possuir cadastro prévio como prestador. |

**Fluxo principal**

| Ações do Ator | Ações do Sistema |
| --- | --- |
| O usuário acessa cadastro de prestador |  |
| O usuário seleciona a função Pet Sitter |  |
| O usuário informa dados pessoais e área de atuação |  |
| O usuário informa se oferece administração de medicamentos |  |
|  | O sistema registra o perfil com status "pendente de validação" e o envia à fila de validação (HPET20b) |

**Fluxo alternativo**

| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o usuário não preencha algum campo de forma correta, emitir mensagem de erro |
|  | Caso o cadastro seja rejeitado, o usuário recebe o feedback do Administrador e pode corrigir e reenviar (HPET37) |
### HPET16a – Editar Perfil de Pet Sitter

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o pet sitter altere os dados de seu perfil (dados pessoais, área de atuação e oferta de administração de medicamentos). |
| Atores | Pet Sitter |
| Pré-Condições | O pet sitter deve possuir perfil previamente cadastrado (HPET16). |

**Fluxo principal**

| Ações do Ator | Ações do Sistema |
| --- | --- |
| O pet sitter acessa a opção "alterar" |  |
|  | O sistema habilita os campos para edição |
| O pet sitter altera os campos pretendidos |  |
| O pet sitter pode ativar ou desativar a oferta de administração de medicamentos |  |
|  | O sistema valida os dados alterados |

**Fluxo alternativo**

| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o pet sitter não preencha algum campo de forma correta, emitir mensagem de erro |
|  | Caso a alteração envolva documentos ou dados sujeitos a validação, o sistema envia o perfil para revalidação (HPET20b) |
### HPET16b – Definir Área de Atuação/Disponibilidade (Pet Sitter)

| Campo | Conteúdo |
| --- | --- |
| Revisão | Sem alteração |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o pet sitter defina sua área de atuação e disponibilidade, conforme dado informado no cadastro (HPET16). |
| Atores | Pet Sitter |
| Pré-Condições | O pet sitter deve possuir perfil previamente cadastrado (HPET16). |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O pet sitter acessa a área de disponibilidade |  |
| O pet sitter define a região de atuação e datas/horários disponíveis |  |
|  | O sistema atualiza a disponibilidade para os tutores |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o pet sitter não preencha algum campo de forma correta, emitir mensagem de erro |

### HPET17 – Receber Solicitação de Visita

| Campo | Conteúdo |
| --- | --- |
| Revisão | Sem alteração |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o pet sitter receba solicitações de cuidado domiciliar. |
| Atores | Pet Sitter |
| Pré-Condições | Um tutor deve ter enviado uma solicitação de serviço (HPET05). |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | O sistema envia solicitação |
| O profissional visualiza detalhes do serviço |  |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso não existam solicitações pendentes, informar que não há novas solicitações |

### HPET17a – Aceitar/Recusar Solicitação de Visita

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o pet sitter aceite ou recuse a solicitação recebida. |
| Atores | Pet Sitter |
| Pré-Condições | Deve existir uma solicitação de visita pendente (HPET17). |

**Fluxo principal**

| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | O sistema exibe a solicitação e os medicamentos informados pelo tutor, quando houver |
| O profissional aceita ou recusa a solicitação |  |
| Ao aceitar uma solicitação com medicamentos, o pet sitter confirma que assume a responsabilidade pela administração |  |
|  | O sistema atualiza o status para "aguardando pagamento" quando aceita e notifica o tutor para realizar o pagamento (HPET07a) |

**Fluxo alternativo**

| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o profissional recuse a solicitação, notificar o tutor e encerrar o fluxo |
|  | Caso o pet sitter não responda em 1 dia útil (segunda a sexta, sem feriados), a solicitação é cancelada automaticamente, as datas são liberadas e o tutor é notificado |
### HPET17b – Registrar Execução do Serviço

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o pet sitter registre a execução do serviço de cuidado domiciliar prestado. Inclui a administração dos medicamentos informados e o registro de incidentes com gravidade, que gera uma ocorrência para o Administrador. |
| Atores | Pet Sitter |
| Pré-Condições | A solicitação de visita deve ter sido aceita (HPET17a) e paga (HPET07a). |

**Fluxo principal**

| Ações do Ator | Ações do Sistema |
| --- | --- |
| O pet sitter acessa a solicitação aceita |  |
| O pet sitter registra a execução do serviço |  |
| O pet sitter registra cada aplicação dos medicamentos informados, quando houver |  |
|  | O sistema atualiza o histórico de quem cuidou/passeou |

**Fluxo alternativo**

| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o pet sitter não preencha algum campo de forma correta, emitir mensagem de erro |
| O pet sitter registra um incidente e informa tipo (com o pet ou no imóvel), descrição, ação tomada e gravidade (leve, média ou grave), avaliada no momento do ocorrido | O sistema notifica o tutor com prioridade e gera uma ocorrência para análise do Administrador, que revisa a gravidade (HPET25) |
| Caso o incidente seja leve ou médio, o pet sitter alerta o tutor e aguarda o retorno antes de agir; se indicar veterinário, propõe o atendimento | O sistema envia a proposta para o tutor autorizar ou recusar (HPET09) |
| Caso o incidente seja grave, o pet sitter segue o procedimento padrão e registra a ação tomada | O sistema marca a ocorrência para análise de responsabilidade e custos pelo Administrador (HPET25) |
### HPET18 – Cadastrar-se como Pet Walker

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite cadastro de profissionais que realizam passeios com pets. O perfil só é publicado após validação do Administrador (HPET20b). |
| Atores | Pet Walker |
| Pré-Condições | O usuário deve estar autenticado com sua conta da plataforma (papel Tutor) e não deve possuir cadastro prévio como prestador. |

**Fluxo principal**

| Ações do Ator | Ações do Sistema |
| --- | --- |
| O usuário acessa cadastro de prestador |  |
| O usuário seleciona a função Pet Walker |  |
| O usuário informa dados e região de atuação |  |
| O usuário informa se oferece administração de medicamentos |  |
|  | O sistema registra o perfil com status "pendente de validação" e o envia à fila de validação (HPET20b) |

**Fluxo alternativo**

| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o usuário não preencha algum campo de forma correta, emitir mensagem de erro |
|  | Caso o cadastro seja rejeitado, o usuário recebe o feedback do Administrador e pode corrigir e reenviar (HPET37) |
### HPET18a – Editar Perfil de Pet Walker

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o pet walker altere os dados de seu perfil (dados pessoais, região de atuação e oferta de administração de medicamentos). |
| Atores | Pet Walker |
| Pré-Condições | O pet walker deve possuir perfil previamente cadastrado (HPET18). |

**Fluxo principal**

| Ações do Ator | Ações do Sistema |
| --- | --- |
| O pet walker acessa a opção "alterar" |  |
|  | O sistema habilita os campos para edição |
| O pet walker altera os campos pretendidos |  |
| O pet walker pode ativar ou desativar a oferta de administração de medicamentos |  |
|  | O sistema valida os dados alterados |

**Fluxo alternativo**

| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o pet walker não preencha algum campo de forma correta, emitir mensagem de erro |
|  | Caso a alteração envolva documentos ou dados sujeitos a validação, o sistema envia o perfil para revalidação (HPET20b) |
### HPET18b – Definir Área de Atuação/Disponibilidade (Pet Walker)

| Campo | Conteúdo |
| --- | --- |
| Revisão | Sem alteração |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o pet walker defina sua região de atuação e disponibilidade, conforme dado informado no cadastro (HPET18). |
| Atores | Pet Walker |
| Pré-Condições | O pet walker deve possuir perfil previamente cadastrado (HPET18). |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O pet walker acessa a área de disponibilidade |  |
| O pet walker define a região de atuação e horários disponíveis |  |
|  | O sistema atualiza a disponibilidade para os tutores |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o pet walker não preencha algum campo de forma correta, emitir mensagem de erro |

### HPET19 – Aceitar/Recusar Passeio
| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o walker aceite ou recuse solicitações de passeio. |
| Atores | Pet Walker |
| Pré-Condições | Deve existir uma solicitação de passeio pendente (HPET05). |

**Fluxo principal**

| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | O sistema envia solicitação de passeio |
|  | O sistema exibe os detalhes e os medicamentos informados pelo tutor, quando houver |
| O walker visualiza detalhes |  |
| O walker aceita ou recusa o serviço |  |
| Ao aceitar uma solicitação com medicamentos, o walker confirma que assume a responsabilidade pela administração |  |
|  | O sistema atualiza o status da solicitação para "aguardando pagamento" quando aceita e notifica o tutor para realizar o pagamento (HPET07a) |

**Fluxo alternativo**

| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o profissional recuse a solicitação, notificar o tutor e encerrar o fluxo |
|  | Caso o walker não responda em 1 dia útil (segunda a sexta, sem feriados), a solicitação é cancelada automaticamente, as datas são liberadas e o tutor é notificado |

### HPET19b – Registrar Execução do Passeio

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o pet walker registre a execução do passeio realizado. Inclui a administração dos medicamentos informados e o registro de incidentes com gravidade, que gera uma ocorrência para o Administrador. |
| Atores | Pet Walker |
| Pré-Condições | A solicitação de passeio deve ter sido aceita (HPET19) e paga (HPET07a). |

**Fluxo principal**

| Ações do Ator | Ações do Sistema |
| --- | --- |
| O pet walker acessa a solicitação aceita |  |
| O pet walker registra a execução do passeio |  |
| O pet walker registra cada aplicação dos medicamentos informados, quando houver |  |
|  | O sistema atualiza o histórico de quem cuidou/passeou |

**Fluxo alternativo**

| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o pet walker não preencha algum campo de forma correta, emitir mensagem de erro |
| O pet walker registra um incidente e informa tipo (com o pet ou no imóvel), descrição, ação tomada e gravidade (leve, média ou grave), avaliada no momento do ocorrido | O sistema notifica o tutor com prioridade e gera uma ocorrência para análise do Administrador, que revisa a gravidade (HPET25) |
| Caso o incidente seja leve ou médio, o pet walker alerta o tutor e aguarda o retorno antes de agir; se indicar veterinário, propõe o atendimento | O sistema envia a proposta para o tutor autorizar ou recusar (HPET09) |
| Caso o incidente seja grave, o pet walker segue o procedimento padrão e registra a ação tomada | O sistema marca a ocorrência para análise de responsabilidade e custos pelo Administrador (HPET25) |
* * *

Comum a todos os perfis do app (Mobile)
---------------------------------------

### HPET36 – Denunciar Usuário ou Conteúdo

| Campo | Conteúdo |
| --- | --- |
| Revisão | Novo |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que qualquer usuário do app denuncie um usuário ou conteúdo (perfil, avaliação, foto ou mensagem de chat) que descumpra as regras da plataforma, gerando uma ocorrência para o Administrador (HPET25) e, quando for conteúdo, um item de moderação (HPET42). |
| Atores | Tutor / Host / Pet Sitter / Pet Walker |
| Pré-Condições | O usuário deve estar autenticado e visualizando o perfil, a avaliação, a foto ou a mensagem a denunciar. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O usuário seleciona a opção "Denunciar" |  |
|  | O sistema exibe os motivos de denúncia disponíveis |
| O usuário seleciona o motivo e, opcionalmente, descreve o ocorrido |  |
| O usuário confirma a denúncia |  |
|  | O sistema registra a denúncia com o item denunciado, o denunciante e a data |
|  | O sistema gera a ocorrência para análise do Administrador e confirma o envio ao usuário |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso nenhum motivo seja selecionado, emitir mensagem de erro |
|  | Caso o usuário já tenha denunciado o mesmo item, informar que a denúncia já está em análise |
|  | Caso a denúncia seja de mensagem de chat, registrar somente a mensagem denunciada para a moderação, sem expor o restante da conversa (DA-13) |
|  | Caso ocorra falha ao registrar, emitir mensagem de erro e permitir nova tentativa |

### HPET37 – Corrigir e Reenviar Cadastro Rejeitado

| Campo | Conteúdo |
| --- | --- |
| Revisão | Novo |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o host ou prestador cujo cadastro foi rejeitado consulte o feedback do Administrador, corrija os dados ou documentos e reenvie o cadastro para nova validação (HPET20b). |
| Atores | Host / Pet Sitter / Pet Walker |
| Pré-Condições | O cadastro do usuário deve ter sido rejeitado com feedback registrado (HPET20b). |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O usuário acessa a notificação de rejeição ou a área do seu perfil de prestador |  |
|  | O sistema exibe o status "rejeitado" e o feedback do Administrador, indicando os itens a ajustar |
| O usuário corrige os dados e/ou envia novos documentos |  |
|  | O sistema valida os campos alterados |
| O usuário confirma o reenvio |  |
|  | O sistema altera o status para "pendente de validação", marca o cadastro como reenvio e o devolve à fila do Administrador |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o usuário não preencha algum campo de forma correta, emitir mensagem de erro |
|  | Caso o usuário reenvie sem alterar nenhum item apontado no feedback, solicitar confirmação antes do envio |

### HPET38 – Visualizar Aviso de Penalidade

| Campo | Conteúdo |
| --- | --- |
| Revisão | Novo |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o usuário visualize a penalidade recebida (advertência, suspensão com prazo ou banimento), com motivo e prazo, garantindo a comunicação prevista em Gerenciar Penalidades (HPET25). |
| Atores | Tutor / Host / Pet Sitter / Pet Walker |
| Pré-Condições | O Administrador deve ter aplicado uma penalidade ao usuário (HPET25). |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | O sistema notifica o usuário sobre a penalidade aplicada (HPET29) |
| O usuário acessa a notificação ou o aplicativo |  |
|  | O sistema exibe o aviso com tipo de penalidade, motivo e, quando houver, data de fim |
| O usuário confirma a ciência do aviso |  |
|  | O sistema registra a ciência e libera a navegação permitida ao estado da conta |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso a penalidade seja suspensão ou banimento, o aviso é exibido na tentativa de login e o acesso permanece bloqueado (HPET01a) |
|  | Caso a penalidade seja revertida ou encerrada (HPET25a), notificar o usuário |

### HPET39 – Inativar Própria Conta

| Campo | Conteúdo |
| --- | --- |
| Revisão | Novo |
| Canal | Mobile |
| Finalidade/Objetivo | Permite que o usuário inative sua própria conta (estado "inativa" da máquina de estados da conta), conforme a opção de exclusão de conta do perfil (tela 31B). A inativação é lógica: dados e histórico são preservados. |
| Atores | Tutor / Host / Pet Sitter / Pet Walker |
| Pré-Condições | O usuário deve estar autenticado na plataforma. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O usuário acessa Privacidade e segurança e seleciona a opção de inativar a conta |  |
|  | O sistema informa os efeitos da inativação e solicita confirmação |
| O usuário confirma a inativação |  |
|  | O sistema altera o estado da conta para "inativa", retira os perfis de prestador da busca e encerra a sessão |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o usuário possua reserva, solicitação ou serviço ativo ou futuro, impedir a inativação e informar o motivo |
|  | Caso o usuário possua saldo a receber como prestador, informar a pendência antes de confirmar |

> ⚠️ **A confirmar com PO:** inclusão deste caso de uso.

* * *

Área Comercial Pública (Web)
----------------------------

### HPET34 – Consultar Informações Institucionais

| Campo | Conteúdo |
| --- | --- |
| Revisão | Novo |
| Canal | Web (área pública) |
| Finalidade/Objetivo | Permite que qualquer visitante conheça a HOSPETSE pelo site: quem somos, serviços oferecidos (hospedagem, pet sitter e pet walker) e como funciona. A Web não oferece busca, reserva nem cadastro de tutor ou prestador; essas jornadas são do aplicativo (DF-01/DF-02). |
| Atores | Visitante |
| Pré-Condições | Nenhuma (área pública, sem autenticação). |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O visitante acessa o site da HOSPETSE |  |
|  | O sistema exibe a página inicial com menu para Quem somos, Serviços, Como funciona e Sobre o app |
| O visitante seleciona uma seção |  |
|  | O sistema exibe o conteúdo institucional da seção |
| O visitante seleciona a chamada para usar a plataforma |  |
|  | O sistema direciona à seção Sobre o app (HPET35) |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso a página solicitada não exista, exibir página de erro com retorno à página inicial |
|  | O site não exibe link para o login administrativo, que fica em rota separada (HPET20a) |

> ⚠️ **A confirmar com PO:** inclusão de Termos de uso e Política de privacidade (telas 00D/00E, recomendadas).

### HPET35 – Conhecer e Obter o Aplicativo

| Campo | Conteúdo |
| --- | --- |
| Revisão | Novo |
| Canal | Web (área pública) |
| Finalidade/Objetivo | Permite que o visitante entenda o que o aplicativo oferece e seja direcionado à loja de aplicativos compatível com seu sistema operacional, sem download direto pelo site (DA-05). |
| Atores | Visitante |
| Pré-Condições | Nenhuma (área pública, sem autenticação). |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O visitante acessa a seção Sobre o app |  |
|  | O sistema exibe a explicação do aplicativo e dos perfis disponíveis (Tutor, Host, Pet Sitter e Pet Walker) |
| O visitante seleciona a opção de obter o aplicativo |  |
|  | O sistema identifica o sistema operacional do dispositivo e redireciona para a loja compatível |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o sistema operacional não seja identificado (ex.: computador), exibir as opções de loja disponíveis para o visitante escolher |
|  | Caso o aplicativo ainda não esteja publicado na loja, exibir aviso de disponibilidade em breve |

* * *

Módulo Administrativo (Web)
---------------------------

### HPET20 – Gerenciar Usuários

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Web (admin) |
| Finalidade/Objetivo | Permite que o administrador consulte os usuários cadastrados na plataforma (com abas por papel: Tutor, Host, Pet Sitter e Pet Walker) e reative contas. Suspensão e banimento não são feitos aqui: são sempre penalidades com motivo (HPET25); a validação de cadastro é feita em HPET20b. |
| Atores | Administrador |
| Pré-Condições | O administrador deve estar autenticado na plataforma (HPET20a). |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O administrador acessa o painel administrativo |  |
|  | O sistema exibe a lista de usuários cadastrados com estado da conta (ativa, suspensa com prazo, banida ou inativa) e filtros por papel e estado |
| O administrador visualiza detalhes do usuário |  |
|  | O sistema exibe dados do usuário, papéis, histórico de contratações, ocorrências e penalidades |
| O administrador seleciona "Reativar conta" e informa o motivo |  |
|  | O sistema registra a ação realizada (autor, data e motivo na trilha de auditoria) e notifica o usuário |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o usuário selecionado não seja encontrado, emitir mensagem de erro |
|  | Caso a conta já esteja ativa, a ação de reativar não é oferecida |
|  | Caso a conta esteja suspensa ou banida por penalidade, a reativação é feita pela reversão da penalidade (HPET25a) |
|  | Caso o motivo não seja informado, emitir mensagem de erro |

### HPET20a – Autenticar-se (Administrador)

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Web (admin, rota separada `/admin`) |
| Finalidade/Objetivo | Permite que o administrador do sistema acesse o painel administrativo Web mediante validação de suas credenciais. Há um único papel, Administrador (DF-03); a separação de cargos poderá ser feita no futuro por permissões. |
| Atores | Administrador |
| Pré-Condições | O administrador deve possuir conta administrativa criada via seed (DA-06). |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O administrador acessa a tela de login pela rota administrativa (sem link na área comercial) |  |
|  | Solicitar e-mail e senha |
| O administrador informa e-mail e senha |  |
|  | O sistema valida as credenciais e o papel de Administrador |
|  | O sistema autentica o administrador e libera o acesso ao Dashboard (HPET44); provisoriamente, à fila de validações (HPET20b) |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso as credenciais informadas sejam inválidas, emitir mensagem de erro sem indicar qual campo falhou |
|  | Caso o usuário autenticado não possua papel de Administrador, exibir Acesso negado (tela 01D) |
|  | Caso a conta administrativa esteja inativa, negar o acesso (tela 01F) |
|  | Caso a sessão expire, exigir novo login (tela 01E) |
|  | Não há "esqueci minha senha" na Web: a recuperação de senha do administrador é feita pelo seed (DA-07) |

### HPET20b – Validar Cadastro de Host/Prestador

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Web (admin) |
| Finalidade/Objetivo | Permite que o administrador valide o cadastro de hosts e prestadores (Pet Sitter e Pet Walker), incluindo cadastros novos, revalidações e reenvios após rejeição. |
| Atores | Administrador |
| Pré-Condições | Deve existir um cadastro de host ou prestador pendente de validação. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O administrador acessa o painel administrativo |  |
|  | O sistema exibe a fila de validações com filtro por tipo (novo, revalidação, reenvio) e por papel |
| O administrador seleciona o cadastro pendente |  |
| O administrador analisa os documentos enviados |  |
| O administrador valida ou rejeita o cadastro |  |
|  | O sistema registra a ação realizada na trilha de auditoria e notifica o usuário |
|  | Quando aprovado, o perfil passa a ativo e pode aparecer na busca |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O administrador rejeita o cadastro e informa o feedback (itens a ajustar) | O sistema registra a rejeição com o feedback e notifica o usuário, que poderá corrigir e reenviar (HPET37) |
|  | Caso o feedback não seja informado na rejeição, emitir mensagem de erro |
|  | Caso o cadastro já tenha sido decidido, exibir aviso (tela 03I) e não aplicar segunda decisão |

### HPET25 – Gerenciar Penalidades

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Web (admin) |
| Finalidade/Objetivo | Permite analisar ocorrências e aplicar penalidades a usuários que descumprirem regras da plataforma. As ocorrências se originam de denúncias no app (HPET36) e de incidentes registrados pelos prestadores (HPET12c, HPET17b e HPET19b). Penalidades possíveis: advertência, suspensão com prazo e banimento. |
| Atores | Administrador |
| Pré-Condições | O administrador deve estar autenticado na plataforma. |

**Fluxo principal**

| Ações do Ator | Ações do Sistema |
| --- | --- |
| O administrador acessa o histórico de ocorrências |  |
|  | O sistema exibe as ocorrências com origem (denúncia ou incidente), status e usuários envolvidos |
| O administrador seleciona um usuário envolvido |  |
| O administrador avalia o ocorrido |  |
| O administrador aplica advertência, suspensão com prazo ou banimento, informando o motivo (e a data de fim, no caso de suspensão) |  |
|  | O sistema registra a penalidade aplicada na trilha de auditoria, atualiza o estado da conta (suspensa ou banida) quando aplicável e notifica o usuário (HPET38) |

**Fluxo alternativo**

| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o usuário selecionado não seja encontrado, emitir mensagem de erro |
| O administrador conclui que não houve descumprimento e arquiva a ocorrência com justificativa | O sistema arquiva a ocorrência sem aplicar penalidade (tela 04M) |
|  | Caso o motivo ou o prazo da suspensão não seja informado, emitir mensagem de erro |
|  | Caso a ocorrência envolva conteúdo, o administrador pode tratá-lo pela moderação (HPET42) |
| Em ocorrência de incidente, o administrador revisa a gravidade informada pelo prestador e define a gravidade final | O sistema registra a gravidade final na ocorrência e na trilha de auditoria |
| Caso a ocorrência seja de incidente grave, o administrador analisa a responsabilidade e os custos adicionais, inclusive de atendimento veterinário, com base no ocorrido | O sistema registra a conclusão na trilha de auditoria e notifica o tutor e o prestador; se o custo for do tutor, ele entra como custo extra (HPET07a) |
### HPET25a – Acompanhar e Reverter Penalidades

| Campo | Conteúdo |
| --- | --- |
| Revisão | Novo |
| Canal | Web (admin) |
| Finalidade/Objetivo | Permite que o administrador acompanhe as penalidades ativas e reverta uma penalidade, e que o sistema encerre automaticamente as suspensões ao fim do prazo definido na aplicação (DA-01/DA-10). |
| Atores | Administrador / Sistema |
| Pré-Condições | Deve existir ao menos uma penalidade aplicada (HPET25). |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O administrador acessa Penalidades ativas (tela 04L) |  |
|  | O sistema exibe as penalidades com tipo, usuário, motivo, data de início e data de fim |
| O administrador seleciona uma penalidade e a opção "Reverter" |  |
| O administrador informa o motivo da reversão e confirma |  |
|  | O sistema encerra a penalidade, restaura o estado "ativa" da conta quando aplicável, registra nova entrada de auditoria sem apagar o registro original e notifica o usuário |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Ao atingir a data de fim de uma suspensão, o sistema a encerra automaticamente, restaura a conta e notifica o usuário |
|  | Caso o motivo da reversão não seja informado, emitir mensagem de erro |
|  | Caso não existam penalidades ativas, informar que a lista está vazia |

### HPET40 – Gerenciar Cupons de Desconto

| Campo | Conteúdo |
| --- | --- |
| Revisão | Novo |
| Canal | Web (admin) |
| Finalidade/Objetivo | Permite que o administrador crie, edite e inative cupons de desconto utilizados pelo tutor no pagamento (HPET32), conforme DA-04. |
| Atores | Administrador |
| Pré-Condições | O administrador deve estar autenticado na plataforma. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O administrador acessa a área de cupons |  |
|  | O sistema exibe os cupons cadastrados com código, desconto, vigência, usos e situação |
| O administrador cria ou edita um cupom informando código, tipo e valor do desconto, vigência e limite de uso |  |
|  | O sistema valida os dados e salva o cupom |
| O administrador pode inativar um cupom |  |
|  | O sistema inativa o cupom, que deixa de ser aceito em novos pagamentos, e registra a ação na auditoria |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o código já exista, emitir mensagem de erro |
|  | Caso algum campo não seja preenchido corretamente (ex.: vigência final anterior à inicial, valor inválido), emitir mensagem de erro |

### HPET41 – Configurar Taxa da Plataforma

| Campo | Conteúdo |
| --- | --- |
| Revisão | Novo |
| Canal | Web (admin) |
| Finalidade/Objetivo | Permite que o administrador defina a taxa cobrada pela plataforma sobre os serviços, usada no cálculo do repasse (HPET27) e exibida no financeiro do prestador (HPET12e), conforme DA-04. |
| Atores | Administrador |
| Pré-Condições | O administrador deve estar autenticado na plataforma. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O administrador acessa a área de taxa da plataforma |  |
|  | O sistema exibe a taxa vigente e o histórico de alterações |
| O administrador informa o novo percentual e a data de início de vigência |  |
|  | O sistema valida o valor, salva a nova taxa e registra a alteração na auditoria |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o valor seja negativo, maior que 100% ou não numérico, emitir mensagem de erro |
|  | A nova taxa não altera contratações já solicitadas ou confirmadas, que mantêm a taxa vigente no momento da contratação |

### HPET42 – Moderar Conteúdo Denunciado

| Campo | Conteúdo |
| --- | --- |
| Revisão | Novo |
| Canal | Web (admin) |
| Finalidade/Objetivo | Permite que o administrador analise conteúdos denunciados (perfis, avaliações, fotos e mensagens de chat) e decida mantê-los, ocultá-los ou removê-los, conforme DA-04. No chat, o administrador vê somente a mensagem denunciada (DA-13). |
| Atores | Administrador |
| Pré-Condições | Deve existir ao menos um conteúdo denunciado (HPET36). |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O administrador acessa a fila de moderação |  |
|  | O sistema exibe os conteúdos denunciados com tipo, motivo, denunciante e data |
| O administrador seleciona um item |  |
|  | O sistema exibe o conteúdo denunciado (no caso de chat, apenas a mensagem denunciada) |
| O administrador decide manter, ocultar ou remover o conteúdo, informando a justificativa |  |
|  | O sistema aplica a decisão, registra na auditoria e notifica o autor do conteúdo quando houver remoção |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso a análise exija contexto da conversa, o administrador solicita acesso à conversa completa (HPET42a) |
|  | Caso o conteúdo indique descumprimento de regras, o administrador pode aplicar penalidade (HPET25) |
|  | Caso a fila esteja vazia, informar que não há itens pendentes |

### HPET42a – Solicitar Acesso à Conversa Completa

| Campo | Conteúdo |
| --- | --- |
| Revisão | Novo |
| Canal | Web (admin) |
| Finalidade/Objetivo | Permite que o administrador solicite permissão para ver a conversa completa de um chat denunciado quando a mensagem isolada não basta para a decisão (DA-13; telas 14G/14H/14I). |
| Atores | Administrador / Responsável pela concessão |
| Pré-Condições | Deve existir uma mensagem de chat denunciada em análise (HPET42). |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O administrador seleciona "Solicitar acesso à conversa completa" e informa a justificativa |  |
|  | O sistema registra o pedido e o encaminha a quem concede a permissão |
| O responsável concede ou nega o acesso |  |
|  | Se concedido, o sistema libera a conversa completa ao administrador por acesso restrito e registra o acesso na auditoria |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o acesso seja negado, o administrador decide apenas com base na mensagem denunciada |
|  | Caso a justificativa não seja informada, emitir mensagem de erro |

> ⚠️ **A confirmar com PO (DA-15):** quem é o responsável pela concessão do acesso.

### HPET43 – Tratar Transação Financeira com Falha

| Campo | Conteúdo |
| --- | --- |
| Revisão | Novo |
| Canal | Web (admin) |
| Finalidade/Objetivo | Permite que o administrador resolva estornos (DA-09) e repasses (DA-12) que falharam no fluxo padrão, sempre por meio de novo lançamento no registro financeiro, sem alterar o original (telas 09I/09J). |
| Atores | Administrador |
| Pré-Condições | Deve existir um estorno (HPET28) ou repasse (HPET27) com status de falha. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O administrador acessa as transações com falha |  |
|  | O sistema exibe as transações com tipo, valor, envolvidos, motivo da falha e data |
| O administrador seleciona uma transação |  |
| O administrador reprocessa a transação ou registra a resolução manual, com justificativa |  |
|  | O sistema executa a ação, registra novo lançamento vinculado ao original (HPET28a), atualiza o status e notifica o tutor ou o prestador |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o reprocessamento falhe novamente, manter o status de falha e registrar a tentativa |
|  | Caso a justificativa não seja informada, emitir mensagem de erro |

### HPET44 – Consultar Dashboard Administrativo

| Campo | Conteúdo |
| --- | --- |
| Revisão | Novo |
| Canal | Web (admin) |
| Finalidade/Objetivo | Permite que o administrador veja, após o login, um resumo da operação: validações pendentes, ocorrências abertas, moderação pendente, transações com falha e indicadores de contratações (WJ05). |
| Atores | Administrador |
| Pré-Condições | O administrador deve estar autenticado na plataforma. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O administrador acessa o Dashboard |  |
|  | O sistema exibe os totais pendentes por fila e os indicadores do período |
| O administrador seleciona um indicador ou fila |  |
|  | O sistema direciona à área correspondente (HPET20b, HPET25, HPET42, HPET43) |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso não existam dados no período, exibir indicadores zerados |

### HPET45 – Consultar Contratações

| Campo | Conteúdo |
| --- | --- |
| Revisão | Novo |
| Canal | Web (admin) |
| Finalidade/Objetivo | Permite que o administrador consulte reservas e solicitações de serviço de toda a plataforma, com status e detalhes, para apoiar a análise de ocorrências e de transações (WJ08). |
| Atores | Administrador |
| Pré-Condições | O administrador deve estar autenticado na plataforma. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O administrador acessa Contratações |  |
|  | O sistema exibe a lista com tipo de serviço, tutor, prestador, pet, datas, valor e status, com filtros |
| O administrador seleciona uma contratação |  |
|  | O sistema exibe os detalhes, o histórico de status e os pagamentos vinculados |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso nenhuma contratação corresponda aos filtros, informar ausência de resultados |

### HPET46 – Consultar Financeiro da Plataforma

| Campo | Conteúdo |
| --- | --- |
| Revisão | Novo |
| Canal | Web (admin) |
| Finalidade/Objetivo | Permite que o administrador acompanhe pagamentos, repasses, estornos e taxas arrecadadas pela plataforma (WJ09), com acesso às transações com falha (HPET43). |
| Atores | Administrador |
| Pré-Condições | O administrador deve estar autenticado na plataforma. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O administrador acessa o Financeiro |  |
|  | O sistema exibe totais do período e a lista de transações com tipo, valor, status e envolvidos |
| O administrador filtra e seleciona uma transação |  |
|  | O sistema exibe o detalhe e os lançamentos vinculados (HPET28a) |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso não existam transações no período, informar que não há registros |

### HPET47 – Consultar Trilha de Auditoria

| Campo | Conteúdo |
| --- | --- |
| Revisão | Novo |
| Canal | Web (admin) |
| Finalidade/Objetivo | Permite que o administrador consulte o registro das ações administrativas (validações, penalidades, reversões, reativações, moderação, cupons, taxa, tratamento de transações e acessos a conversas) com autor, data e motivo (WJ10). |
| Atores | Administrador |
| Pré-Condições | O administrador deve estar autenticado na plataforma. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O administrador acessa Auditoria |  |
|  | O sistema exibe os registros em ordem cronológica, com filtros por tipo de ação, autor, usuário afetado e período |
| O administrador seleciona um registro |  |
|  | O sistema exibe o detalhe da ação, sem permitir edição |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso nenhum registro corresponda aos filtros, informar ausência de resultados |

### HPET48 – Gerenciar Conta do Administrador

| Campo | Conteúdo |
| --- | --- |
| Revisão | Novo |
| Canal | Web (admin) |
| Finalidade/Objetivo | Permite que o administrador consulte seus dados de acesso, altere a própria senha e encerre a sessão (WJ11). |
| Atores | Administrador |
| Pré-Condições | O administrador deve estar autenticado na plataforma. |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O administrador acessa Minha conta |  |
|  | O sistema exibe nome e e-mail da conta administrativa |
| O administrador altera a senha informando a senha atual e a nova |  |
|  | O sistema valida e atualiza a senha e registra a ação na auditoria |
| O administrador seleciona "Sair" |  |
|  | O sistema encerra a sessão e retorna ao login administrativo |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso a senha atual esteja incorreta ou a nova não atenda à política, emitir mensagem de erro |

* * *

Sistema de Pagamento (Backend)
------------------------------

### HPET26 – Processar Pagamento

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Backend |
| Finalidade/Objetivo | Permite processar pagamentos realizados pelos tutores para reservas e serviços já aceitos pelo prestador. |
| Atores | Sistema de Pagamento |
| Pré-Condições | O tutor deve ter confirmado o pagamento na tela de pagamento (HPET07a) de uma solicitação com status "aguardando pagamento". |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
| O tutor confirma pagamento |  |
|  | O sistema envia dados ao gateway de pagamento |
|  | O pagamento é validado |
|  | A transação é registrada (HPET28a) |
|  | O sistema altera o status da contratação para "confirmada" |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o pagamento seja recusado pelo gateway, emitir mensagem de erro e manter a contratação em "aguardando pagamento" para nova tentativa |

### HPET27 – Repassar Valores

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Backend |
| Finalidade/Objetivo | Permite repassar valores aos prestadores de serviço (Host, Pet Sitter e Pet Walker). O repasse não é automático: só ocorre mediante solicitação do prestador (HPET12f). |
| Atores | Sistema de Pagamento |
| Pré-Condições | O serviço deve estar concluído e o prestador deve ter solicitado o repasse (HPET12f). |

**Fluxo principal**

| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | O sistema recebe a solicitação de repasse (HPET12f) e confirma que o serviço está concluído |
|  | O sistema calcula repasse ao prestador descontando a taxa da plataforma vigente na contratação (HPET41) |
|  | O sistema registra a transação financeira |

**Fluxo alternativo**

| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso ocorra falha no cálculo do repasse, emitir mensagem de erro |
|  | Caso a transferência falhe, registrar o repasse com status de falha, sem transferir valor incorreto, e disponibilizá-lo ao Administrador (HPET43) |
### HPET28 – Processar Cancelamento / Estorno

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Backend |
| Finalidade/Objetivo | Permite processar o cancelamento e estorno de pagamentos realizados. |
| Atores | Sistema de Pagamento |
| Pré-Condições | Deve existir uma reserva cancelada com pagamento processado (HPET04a). |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | O sistema recebe a solicitação de cancelamento |
|  | O sistema calcula o valor do estorno |
|  | O sistema envia o estorno ao gateway de pagamento |
|  | A transação de estorno é registrada |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso o estorno seja recusado pelo gateway, emitir mensagem de erro e registrar o estorno com status de falha, sem declarar devolução inexistente, disponibilizando-o ao Administrador (HPET43) |

### HPET28a – Registrar Transação Financeira

| Campo | Conteúdo |
| --- | --- |
| Revisão | Alterado |
| Canal | Backend |
| Finalidade/Objetivo | Permite registrar, para fins de auditoria, as transações financeiras processadas na plataforma (pagamento, repasse e estorno, e as resoluções manuais do Administrador), conforme etapa "a transação é registrada" (HPET26/HPET27/HPET28). Os registros são imutáveis: correções geram novo lançamento vinculado ao original. |
| Atores | Sistema de Pagamento |
| Pré-Condições | Deve existir uma transação processada (pagamento, repasse ou estorno) ou uma resolução manual (HPET43). |
**Fluxo principal**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | O sistema identifica a transação processada |
|  | O sistema registra os dados da transação (tipo, valor, partes, status, data e vínculo com a contratação e com o lançamento original, quando houver) |
|  | O sistema disponibiliza o registro no financeiro do tutor/host e no financeiro administrativo (HPET46) |
**Fluxo alternativo**
| Ações do Ator | Ações do Sistema |
| --- | --- |
|  | Caso ocorra falha ao registrar a transação, emitir mensagem de erro e reter o evento para nova tentativa, sem confirmar movimento sem registro |

* * *

Casos novos criados (17)
------------------------

| ID | Caso de uso | Canal | Origem | Por que foi necessário |
| --- | --- | --- | --- | --- |
| HPET34 | Consultar Informações Institucionais | Web | DF-02 | A Web ganhou área comercial pública (quem somos, serviços, como funciona) sem caso de uso. |
| HPET35 | Conhecer e Obter o Aplicativo | Web | DF-02, DA-05, DA-14 | A seção Sobre o app redireciona à loja compatível, sem download direto; publicação na loja fica para depois. |
| HPET36 | Denunciar Usuário ou Conteúdo | Mobile | DA-02, DA-13 | A denúncia no app é uma das origens de ocorrência e não tinha caso; também alimenta a moderação. |
| HPET37 | Corrigir e Reenviar Cadastro Rejeitado | Mobile | DA-08 | Prestador rejeitado passou a receber feedback e poder reenviar; antes o fluxo só "encerrava". |
| HPET38 | Visualizar Aviso de Penalidade | Mobile | DA-01 | Penalidades precisam ser comunicadas; advertência não bloqueia login e precisava de aviso próprio. |
| HPET39 | Inativar Própria Conta | Mobile | DA-01 | O estado "inativa" existe na máquina de estados e a tela 31B (excluir conta) não tinha caso. Precisa de confirmação da PO. |
| HPET25a | Acompanhar e Reverter Penalidades | Web | DA-01, DA-10 | Prazos definidos na aplicação, fim automático e reversão pelo admin (tela 04L) não cabiam em HPET25. |
| HPET40 | Gerenciar Cupons de Desconto | Web | DA-04 | O tutor aplicava cupom (HPET32), mas ninguém os criava. |
| HPET41 | Configurar Taxa da Plataforma | Web | DA-04 | A taxa é usada no repasse e no financeiro do prestador e não tinha quem a definisse. |
| HPET42 | Moderar Conteúdo Denunciado | Web | DA-04, DA-13 | Moderação de conteúdo foi atribuída ao admin, com visão restrita à mensagem denunciada. |
| HPET42a | Solicitar Acesso à Conversa Completa | Web | DA-13, DA-15 | Acesso à conversa completa depende de permissão (telas 14G–14I); quem concede ainda está aberto. |
| HPET43 | Tratar Transação Financeira com Falha | Web | DA-09, DA-12 | Estornos e repasses com falha passaram a ser resolvidos pelo admin (telas 09I/09J). |
| HPET44 | Consultar Dashboard Administrativo | Web | Web v4 (WJ05) | Interface complementar adiada para a etapa de UC; é o destino desejado após o login. |
| HPET45 | Consultar Contratações | Web | Web v4 (WJ08) | Interface complementar adiada para a etapa de UC. |
| HPET46 | Consultar Financeiro da Plataforma | Web | Web v4 (WJ09) | Interface complementar adiada para a etapa de UC; dá acesso às transações com falha. |
| HPET47 | Consultar Trilha de Auditoria | Web | Web v4 (WJ10) | Várias decisões exigem auditoria (reativação, penalidade, reversão); faltava onde consultá-la. |
| HPET48 | Gerenciar Conta do Administrador | Web | Web v4 (WJ11) | Interface complementar adiada para a etapa de UC. |
