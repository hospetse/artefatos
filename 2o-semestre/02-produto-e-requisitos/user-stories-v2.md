User Story — HOSPETSE (Revisão v2)
==================================


Sobre esta revisão
------------------

O conjunto passa de 131 para 186 User Stories: 55 novas, 48 alteradas, 3 sincronizadas com a versão detalhada e 80 sem alteração. Nenhum identificador existente foi alterado ou reaproveitado.

**Coluna Revisão:**
*   **Novo** — US criada para cobrir UC novo ou fluxo novo de UC alterado.
*   **Alterado** — texto ou critérios de aceite ajustados às novas regras.
*   **Sincronizado** — a lista estava defasada em relação à versão detalhada; o texto foi igualado ao detalhado, sem mudança de regra.
*   **Sem alteração** — mantida como estava.

Módulo Tutor (Mobile)
---------------------

| ID<br> | Caso de Uso<br> | Tipo<br> | User Story<br> | Prioridade<br> | Revisão<br> |
| --- | --- | --- | --- | --- | --- |
| US-HPET01-01<br> | HPET01 – Cadastrar-se<br> | Principal<br> | Como visitante, quero criar uma conta com dados mínimos, para acessar a plataforma e escolher como quero atuar nela.<br> | Alta<br> | Sincronizado<br> |
| US-HPET01-02<br> | HPET01 – Cadastrar-se<br> | Alternativa<br> | Como Tutor, quero receber uma mensagem quando algum dado do cadastro estiver incorreto ou incompleto, para corrigi-lo antes de concluir o cadastro.<br> | Alta<br> | Sem alteração<br> |
| US-HPET01-03<br> | HPET01 – Cadastrar-se<br> | Principal<br> | Como Tutor, quero completar meu perfil após o cadastro inicial, para poder contratar serviços na plataforma.<br> | Alta<br> | Sincronizado<br> |
| US-HPET01A-01<br> | HPET01a – Autenticar-se (Login)<br> | Principal<br> | Como usuário do app (Tutor, Host, Pet Sitter ou Pet Walker), quero acessar a plataforma mediante validação das minhas credenciais e do estado da minha conta, para utilizar os recursos dos meus papéis.<br> | Alta<br> | Alterado<br> |
| US-HPET01A-02<br> | HPET01a – Autenticar-se (Login)<br> | Alternativa<br> | Como usuário do app, quero receber uma mensagem genérica quando minhas credenciais forem inválidas, para corrigir os dados e tentar novamente sem que a existência da conta seja revelada.<br> | Alta<br> | Alterado<br> |
| US-HPET01A-03<br> | HPET01a – Autenticar-se (Login)<br> | Alternativa<br> | Como usuário do app, quero ser informado quando minha conta estiver suspensa, banida ou inativa, para entender por que meu acesso foi negado e o que posso fazer.<br> | Alta<br> | Novo<br> |
| US-HPET01B-01<br> | HPET01b – Recuperar Senha<br> | Principal<br> | Como usuário do app, quero redefinir minha senha de acesso para recuperar minha conta caso a esqueça.<br> | Alta<br> | Alterado<br> |
| US-HPET01B-02<br> | HPET01b – Recuperar Senha<br> | Alternativa<br> | Como usuário do app, quero ser informado quando o e-mail ou telefone informado não estiver cadastrado, para saber por que não posso iniciar a recuperação.<br> | Média<br> | Alterado<br> |
| US-HPET01C-01<br> | HPET01c – Editar Perfil do Tutor<br> | Principal<br> | Como Tutor, quero alterar meus dados pessoais cadastrados para manter meu perfil atualizado.<br> | Média<br> | Sem alteração<br> |
| US-HPET01C-02<br> | HPET01c – Editar Perfil do Tutor<br> | Alternativa<br> | Como Tutor, quero receber uma mensagem quando algum dado alterado estiver incorreto, para corrigi-lo antes de salvar.<br> | Média<br> | Sem alteração<br> |
| US-HPET02-01<br> | HPET02 – Cadastrar Pet<br> | Principal<br> | Como Tutor, quero registrar um ou mais pets no sistema para utilizar os serviços da plataforma.<br> | Alta<br> | Sem alteração<br> |
| US-HPET02-02<br> | HPET02 – Cadastrar Pet<br> | Alternativa<br> | Como Tutor, quero receber uma mensagem quando os dados do pet estiverem incorretos ou incompletos, para corrigi-los antes de concluir o cadastro.<br> | Alta<br> | Sem alteração<br> |
| US-HPET02A-01<br> | HPET02a – Editar Dados do Pet<br> | Principal<br> | Como Tutor, quero alterar os dados cadastrados do meu pet para manter as informações atualizadas.<br> | Média<br> | Sem alteração<br> |
| US-HPET02A-02<br> | HPET02a – Editar Dados do Pet<br> | Alternativa<br> | Como Tutor, quero receber uma mensagem quando os dados alterados do pet forem inválidos, para corrigi-los.<br> | Média<br> | Sem alteração<br> |
| US-HPET02B-01<br> | HPET02b – Inativar/Remover Pet<br> | Principal<br> | Como Tutor, quero inativar o cadastro de um pet, preservando seu histórico, para retirá-lo da minha lista de pets ativos e da seleção na busca e na reserva.<br> | Média<br> | Alterado<br> |
| US-HPET02B-02<br> | HPET02b – Inativar/Remover Pet<br> | Exceção<br> | Como Tutor, quero ser informado quando ocorrer uma falha ao inativar meu pet, para saber que o cadastro continua ativo e tentar novamente.<br> | Média<br> | Sem alteração<br> |
| US-HPET03-01<br> | HPET03 – Buscar Serviços<br> | Principal<br> | Como Tutor, quero buscar hospedagens por localização, preço e disponibilidade, podendo selecionar meu pet para aplicar espécie e porte, para encontrar opções compatíveis.<br> | Alta<br> | Alterado<br> |
| US-HPET03-02<br> | HPET03 – Buscar Serviços<br> | Alternativa<br> | Como Tutor, quero ser informado quando nenhum host atender aos filtros escolhidos, para ajustar minha busca.<br> | Alta<br> | Sem alteração<br> |
| US-HPET03-03<br> | HPET03 – Buscar Serviços<br> | Alternativa<br> | Como Tutor sem pet ativo cadastrado, quero ser avisado e direcionado ao cadastro de pet, para conseguir buscar opções compatíveis com ele.<br> | Média<br> | Novo<br> |
| US-HPET04-01<br> | HPET04 – Solicitar Reserva<br> | Principal<br> | Como Tutor, quero solicitar a hospedagem do meu pet em um host disponível, sabendo que o pagamento só será solicitado após o aceite do host.<br> | Alta<br> | Alterado<br> |
| US-HPET04-02<br> | HPET04 – Solicitar Reserva<br> | Alternativa<br> | Como Tutor, quero receber uma mensagem quando algum campo da solicitação estiver preenchido incorretamente, para corrigi-lo antes de enviar.<br> | Alta<br> | Sem alteração<br> |
| US-HPET04-03<br> | HPET04 – Solicitar Reserva<br> | Alternativa<br> | Como Tutor, quero ser informado quando o pet selecionado não for aceito pelo host devido à espécie ou porte, para escolher outro host.<br> | Alta<br> | Sem alteração<br> |
| US-HPET04-04<br> | HPET04 – Solicitar Reserva<br> | Alternativa<br> | Como Tutor, quero ser informado quando o host não estiver disponível nas datas escolhidas, para selecionar outro período ou host.<br> | Alta<br> | Sem alteração<br> |
| US-HPET04-05<br> | HPET04 – Solicitar Reserva<br> | Exceção<br> | Como Tutor, quero receber uma mensagem quando ocorrer uma falha ao processar minha solicitação, para saber que a reserva não foi enviada e tentar novamente.<br> | Alta<br> | Sem alteração<br> |
| US-HPET04A-01<br> | HPET04a – Cancelar Reserva/Solicitação<br> | Principal<br> | Como Tutor, quero cancelar uma reserva ou solicitação ativa para desistir do agendamento, com estorno automático quando o pagamento já tiver sido aprovado.<br> | Alta<br> | Alterado<br> |
| US-HPET04A-02<br> | HPET04a – Cancelar Reserva/Solicitação<br> | Alternativa<br> | Como Tutor, quero ser informado quando tentar cancelar uma reserva já concluída, para entender por que o cancelamento não pode ser realizado.<br> | Média<br> | Sem alteração<br> |
| US-HPET05-01<br> | HPET05 – Solicitar Serviço (Pet Sitter/Walker)<br> | Principal<br> | Como Tutor, quero solicitar o serviço de pet sitter ou pet walker para meu pet a um prestador validado, pagando somente após o aceite.<br> | Alta<br> | Alterado<br> |
| US-HPET05-02<br> | HPET05 – Solicitar Serviço (Pet Sitter/Walker)<br> | Alternativa<br> | Como Tutor, quero receber uma mensagem quando os dados da solicitação estiverem incorretos ou incompletos, para corrigi-los.<br> | Alta<br> | Sem alteração<br> |
| US-HPET05-03<br> | HPET05 – Solicitar Serviço (Pet Sitter/Walker)<br> | Alternativa<br> | Como Tutor sem pet ativo cadastrado, quero ser avisado e direcionado ao cadastro de pet ao solicitar um serviço, para conseguir concluir a solicitação.<br> | Média<br> | Novo<br> |
| US-HPET06-01<br> | HPET06 – Conversar com Host<br> | Principal<br> | Como Tutor, quero conversar com o host — diretamente ou por meio do Assistente de IA — para tirar dúvidas antes e durante a reserva.<br> | Média<br> | Alterado<br> |
| US-HPET06-02<br> | HPET06 – Conversar com Host<br> | Alternativa<br> | Como Tutor, quero ser impedido de enviar novas mensagens diretas ao host quando a reserva não estiver mais ativa, para entender que o atendimento humano se encerrou (mantendo acesso à IA para dúvidas gerais, se o host permitir).<br> | Média<br> | Sem alteração<br> |
| US-HPET06-03<br> | HPET06 – Conversar com Host<br> | Alternativa<br> | Como Tutor, quero que minha conversa seja encaminhada automaticamente ao host quando a IA não conseguir responder à minha dúvida, para não ficar sem solução.<br> | Média<br> | Sem alteração<br> |
| US-HPET06-04<br> | HPET06 – Conversar com Host<br> | Alternativa<br> | Como Tutor, quero solicitar atendimento humano a qualquer momento, para ser atendido pelo host quando a resposta da IA não for suficiente, mantendo o histórico da conversa.<br> | Média<br> | Sem alteração<br> |
| US-HPET06-05<br> | HPET06 – Conversar com Host<br> | Exceção<br> | Como Tutor, quero que o chat tente se reconectar automaticamente após uma perda de conexão, para não perder minhas mensagens.<br> | Média<br> | Sem alteração<br> |
| US-HPET06-06<br> | HPET06 – Conversar com Host<br> | Exceção<br> | Como Tutor, quero que mensagens não entregues sejam mantidas em fila até a conexão ser restabelecida, para não perder o histórico da conversa.<br> | Média<br> | Sem alteração<br> |
| US-HPET07-01<br> | HPET07 – Cadastrar Forma de Pagamento<br> | Principal<br> | Como Tutor, quero cadastrar minha forma de pagamento para utilizá-la nas reservas realizadas na plataforma.<br> | Alta<br> | Sem alteração<br> |
| US-HPET07-02<br> | HPET07 – Cadastrar Forma de Pagamento<br> | Alternativa<br> | Como Tutor, quero receber uma mensagem quando os dados da forma de pagamento forem inválidos, para corrigi-los antes de salvar.<br> | Alta<br> | Sem alteração<br> |
| US-HPET07A-01<br> | HPET07a – Realizar Pagamento<br> | Principal<br> | Como Tutor, quero realizar o pagamento da reserva ou do serviço pela plataforma após o aceite do prestador, para confirmar a contratação.<br> | Alta<br> | Alterado<br> |
| US-HPET07A-02<br> | HPET07a – Realizar Pagamento<br> | Alternativa<br> | Como Tutor, quero ser informado quando meu pagamento for recusado, para tentar com outra forma de pagamento.<br> | Alta<br> | Alterado<br> |
| US-HPET07A-03<br> | HPET07a – Realizar Pagamento<br> | Alternativa<br> | Como Tutor, quero selecionar outra forma de pagamento após uma recusa, para tentar concluir a transação.<br> | Média<br> | Sem alteração<br> |
| US-HPET07B-01<br> | HPET07b – Consultar Extrato de Pagamentos<br> | Principal<br> | Como Tutor, quero consultar o histórico de pagamentos realizados para acompanhar meus gastos na plataforma.<br> | Média<br> | Sem alteração<br> |
| US-HPET07B-02<br> | HPET07b – Consultar Extrato de Pagamentos<br> | Alternativa<br> | Como Tutor, quero ser informado quando não houver transações registradas, para saber que meu extrato está vazio.<br> | Baixa<br> | Sem alteração<br> |
| US-HPET08-01<br> | HPET08 – Avaliar Host/Serviço<br> | Principal<br> | Como Tutor, quero avaliar uma vez cada contratação concluída com host ou prestador, para compartilhar minha experiência e ajudar outros tutores.<br> | Média<br> | Alterado<br> |
| US-HPET08-02<br> | HPET08 – Avaliar Host/Serviço<br> | Alternativa<br> | Como Tutor, quero receber uma mensagem quando não preencher algum campo obrigatório da avaliação, para completá-la corretamente antes de enviar.<br> | Baixa<br> | Sem alteração<br> |
| US-HPET08-03<br> | HPET08 – Avaliar Host/Serviço<br> | Alternativa<br> | Como Tutor, quero editar a avaliação que já publiquei, para atualizar minha opinião sem criar uma nova avaliação para a mesma contratação.<br> | Baixa<br> | Novo<br> |
| US-HPET08A-01<br> | HPET08a – Consultar Histórico de Reservas<br> | Principal<br> | Como Tutor, quero consultar as reservas realizadas anteriormente para acompanhar meu histórico na plataforma.<br> | Média<br> | Sem alteração<br> |
| US-HPET08A-02<br> | HPET08a – Consultar Histórico de Reservas<br> | Alternativa<br> | Como Tutor, quero ser informado quando não houver reservas registradas, para saber que meu histórico está vazio.<br> | Baixa<br> | Sem alteração<br> |
| US-HPET09-01<br> | HPET09 – Acompanhar Rotina do Pet<br> | Principal<br> | Como Tutor, quero acompanhar as atualizações enviadas pelo host durante a hospedagem para saber como meu pet está.<br> | Média<br> | Sem alteração<br> |
| US-HPET09-02<br> | HPET09 – Acompanhar Rotina do Pet<br> | Alternativa<br> | Como Tutor, quero ser informado quando ainda não houver registros enviados pelo host, para saber que não existem novas atualizações.<br> | Baixa<br> | Sem alteração<br> |
| US-HPET29-01<br> | HPET29 – Consultar Central de Notificações<br> | Principal<br> | Como Tutor, quero visualizar as notificações sobre reservas, solicitações, pagamentos e penalidades e acessar a tela relacionada a cada uma, para me manter informado.<br> | Média<br> | Alterado<br> |
| US-HPET29-02<br> | HPET29 – Consultar Central de Notificações<br> | Alternativa<br> | Como Tutor, quero ser informado quando não houver novas notificações, para saber que a central está vazia.<br> | Baixa<br> | Sem alteração<br> |
| US-HPET30-01<br> | HPET30 – Favoritar Host/Serviço<br> | Principal<br> | Como Tutor, quero marcar um host ou prestador de serviço como favorito para acessá-lo rapidamente depois.<br> | Baixa<br> | Sem alteração<br> |
| US-HPET30-02<br> | HPET30 – Favoritar Host/Serviço<br> | Alternativa<br> | Como Tutor, quero remover um host ou prestador dos favoritos para manter minha lista atualizada.<br> | Baixa<br> | Sem alteração<br> |
| US-HPET31-01<br> | HPET31 – Compartilhar Perfil de Host<br> | Principal<br> | Como Tutor, quero compartilhar o perfil de um host para indicá-lo a outras pessoas.<br> | Baixa<br> | Sem alteração<br> |
| US-HPET31-02<br> | HPET31 – Compartilhar Perfil de Host<br> | Exceção<br> | Como Tutor, quero receber uma mensagem quando ocorrer falha ao compartilhar o perfil, para saber que o compartilhamento não foi concluído e tentar novamente.<br> | Baixa<br> | Sem alteração<br> |
| US-HPET32-01<br> | HPET32 – Aplicar Cupom de Desconto<br> | Principal<br> | Como Tutor, quero aplicar um cupom de desconto cadastrado pelo Administrador no momento do pagamento, para reduzir o valor da reserva ou do serviço.<br> | Baixa<br> | Alterado<br> |
| US-HPET32-02<br> | HPET32 – Aplicar Cupom de Desconto<br> | Alternativa<br> | Como Tutor, quero ser informado quando o cupom for inválido, inativo, expirado ou tiver atingido o limite de uso, para corrigir ou substituir o código.<br> | Baixa<br> | Alterado<br> |
| US-HPET33-01<br> | HPET33 – Consultar Avaliações<br> | Principal<br> | Como Tutor, quero consultar as notas e avaliações publicadas de hosts e prestadores para decidir com quem contratar o serviço.<br> | Baixa<br> | Alterado<br> |
| US-HPET33-02<br> | HPET33 – Consultar Avaliações<br> | Alternativa<br> | Como Tutor, quero ser informado quando um host ainda não possuir avaliações, para decidir se quero ser o primeiro a contratá-lo mesmo assim.<br> | Baixa<br> | Sem alteração<br> |

Módulo Host / Hotel (Mobile)
----------------------------

| ID<br> | Caso de Uso<br> | Tipo<br> | User Story<br> | Prioridade<br> | Revisão<br> |
| --- | --- | --- | --- | --- | --- |
| US-HPET10-01<br> | HPET10 – Cadastrar Perfil de Host/Hotel<br> | Principal<br> | Como Host, quero me cadastrar na plataforma a partir da minha conta e enviar o perfil para validação, para oferecer hospedagem de pets após a aprovação.<br> | Alta<br> | Alterado<br> |
| US-HPET10-02<br> | HPET10 – Cadastrar Perfil de Host/Hotel<br> | Alternativa<br> | Como Host, quero receber uma mensagem quando os dados do cadastro estiverem incorretos ou incompletos, para corrigi-los.<br> | Alta<br> | Sem alteração<br> |
| US-HPET10A-01<br> | HPET10a – Editar Perfil de Host/Hotel<br> | Principal<br> | Como Host, quero alterar os dados do meu perfil (estrutura, capacidade e documentos) para mantê-lo atualizado, sabendo que alterações sujeitas a validação passam por revalidação.<br> | Média<br> | Alterado<br> |
| US-HPET10A-02<br> | HPET10a – Editar Perfil de Host/Hotel<br> | Alternativa<br> | Como Host, quero receber uma mensagem quando algum dado alterado estiver inválido, para corrigi-lo.<br> | Média<br> | Sem alteração<br> |
| US-HPET10B-01<br> | HPET10b – Inativar Perfil de Host<br> | Principal<br> | Como Host, quero inativar meu perfil na plataforma para deixar de oferecer serviços temporária ou definitivamente.<br> | Média<br> | Sem alteração<br> |
| US-HPET10B-02<br> | HPET10b – Inativar Perfil de Host<br> | Exceção<br> | Como Host, quero receber uma mensagem quando ocorrer falha ao processar a inativação, para saber que meu perfil continua ativo e tentar novamente.<br> | Média<br> | Sem alteração<br> |
| US-HPET11-01<br> | HPET11 – Gerenciar Agenda<br> | Principal<br> | Como Host, quero controlar minha disponibilidade de hospedagem para organizar as reservas recebidas.<br> | Alta<br> | Sem alteração<br> |
| US-HPET11-02<br> | HPET11 – Gerenciar Agenda<br> | Alternativa<br> | Como Host, quero bloquear períodos indisponíveis para impedir solicitações em datas nas quais não posso atender.<br> | Alta<br> | Sem alteração<br> |
| US-HPET11-03<br> | HPET11 – Gerenciar Agenda<br> | Alternativa<br> | Como Host, quero receber uma mensagem quando os dados de disponibilidade estiverem incorretos, para corrigi-los.<br> | Média<br> | Sem alteração<br> |
| US-HPET11A-01<br> | HPET11a – Definir Preços e Tabela de Serviços<br> | Principal<br> | Como Host, quero definir os preços dos serviços oferecidos para que sejam exibidos aos tutores na busca.<br> | Alta<br> | Sem alteração<br> |
| US-HPET11A-02<br> | HPET11a – Definir Preços e Tabela de Serviços<br> | Alternativa<br> | Como Host, quero receber uma mensagem quando um preço ou dado informado for inválido, para corrigi-lo.<br> | Média<br> | Sem alteração<br> |
| US-HPET12-01<br> | HPET12 – Receber Solicitação de Reserva<br> | Principal<br> | Como Host, quero visualizar e gerenciar as solicitações de hospedagem recebidas para decidir sobre cada uma.<br> | Alta<br> | Sem alteração<br> |
| US-HPET12-02<br> | HPET12 – Receber Solicitação de Reserva<br> | Alternativa<br> | Como Host, quero ser informado quando não houver solicitações pendentes, para saber que não existem novos pedidos aguardando análise.<br> | Baixa<br> | Sem alteração<br> |
| US-HPET12A-01<br> | HPET12a – Aceitar/Recusar Solicitação de Reserva<br> | Principal<br> | Como Host, quero aceitar ou recusar uma solicitação de hospedagem para confirmar minha disponibilidade e liberar o pagamento ao tutor.<br> | Alta<br> | Alterado<br> |
| US-HPET12A-02<br> | HPET12a – Aceitar/Recusar Solicitação de Reserva<br> | Alternativa<br> | Como Host, quero que o tutor seja notificado automaticamente quando eu recusar uma solicitação, para que ele busque outra opção sem precisar me contatar.<br> | Alta<br> | Sem alteração<br> |
| US-HPET12B-01<br> | HPET12b – Confirmar Check-in<br> | Principal<br> | Como Host, quero registrar a chegada do pet no início de uma hospedagem já paga, para dar início ao acompanhamento da rotina.<br> | Alta<br> | Alterado<br> |
| US-HPET12B-02<br> | HPET12b – Confirmar Check-in<br> | Alternativa<br> | Como Host, quero ser impedido de registrar o check-in antes da data prevista, para evitar inconsistências na hospedagem.<br> | Média<br> | Sem alteração<br> |
| US-HPET12C-01<br> | HPET12c – Registrar Atividades do Pet<br> | Principal<br> | Como Host, quero registrar as atividades diárias do pet durante a hospedagem, para manter o tutor informado e comprovar o cuidado prestado.<br> | Alta<br> | Alterado<br> |
| US-HPET12C-02<br> | HPET12c – Registrar Atividades do Pet<br> | Alternativa<br> | Como Host, quero anexar fotos às atividades registradas para complementar as informações da rotina do pet.<br> | Média<br> | Sem alteração<br> |
| US-HPET12C-03<br> | HPET12c – Registrar Atividades do Pet<br> | Alternativa<br> | Como Host, quero receber uma mensagem quando os dados da atividade estiverem incorretos ou incompletos, para corrigi-los.<br> | Média<br> | Sem alteração<br> |
| US-HPET12C-04<br> | HPET12c – Registrar Atividades do Pet<br> | Alternativa<br> | Como Host, quero marcar um registro como incidente, informando tipo, descrição e ação tomada, para que o tutor seja avisado com prioridade e o Administrador analise a ocorrência.<br> | Alta<br> | Novo<br> |
| US-HPET12D-01<br> | HPET12d – Confirmar Check-out<br> | Principal<br> | Como Host, quero registrar a saída do pet ao término da hospedagem para encerrar o serviço prestado.<br> | Alta<br> | Sem alteração<br> |
| US-HPET12D-02<br> | HPET12d – Confirmar Check-out<br> | Exceção<br> | Como Host, quero receber uma mensagem quando ocorrer falha ao registrar o check-out, para saber que a hospedagem ainda não foi encerrada corretamente.<br> | Média<br> | Sem alteração<br> |
| US-HPET12E-01<br> | HPET12e – Consultar Financeiro<br> | Principal<br> | Como Host, quero acompanhar os ganhos obtidos pela plataforma, com valor bruto, taxa da plataforma e valor líquido, para controlar minhas receitas.<br> | Média<br> | Alterado<br> |
| US-HPET12E-02<br> | HPET12e – Consultar Financeiro<br> | Alternativa<br> | Como Host, quero ser informado quando não houver transações registradas, para saber que meu painel financeiro está vazio.<br> | Baixa<br> | Sem alteração<br> |
| US-HPET12F-01<br> | HPET12f – Solicitar Repasse<br> | Principal<br> | Como Host, quero solicitar o repasse dos valores recebidos para receber o pagamento pelos serviços prestados.<br> | Média<br> | Sem alteração<br> |
| US-HPET12F-02<br> | HPET12f – Solicitar Repasse<br> | Alternativa<br> | Como Host, quero ser informado quando meu saldo disponível for insuficiente para o repasse solicitado, para ajustar o valor pedido.<br> | Média<br> | Sem alteração<br> |
| US-HPET12F-03<br> | HPET12f – Solicitar Repasse<br> | Exceção<br> | Como Host, quero ser notificado quando meu repasse falhar no processamento, para saber que o valor não foi transferido e que a falha será tratada pelo Administrador.<br> | Média<br> | Novo<br> |
| US-HPET06B-01<br> | HPET06b – Configurar Assistente de IA do Chat<br> | Principal<br> | Como Host, quero preencher uma base de FAQ com categorias padrão sugeridas pelo sistema e acrescentar itens livres, para que o Assistente de IA responda automaticamente às dúvidas frequentes dos tutores com informações do meu estabelecimento.<br> | Média<br> | Sincronizado<br> |
| US-HPET06B-02<br> | HPET06b – Configurar Assistente de IA do Chat<br> | Alternativa<br> | Como Host, quero habilitar ou desabilitar o atendimento humano direto antes de existir uma reserva, para controlar quando os tutores podem falar comigo.<br> | Média<br> | Sem alteração<br> |
| US-HPET06B-03<br> | HPET06b – Configurar Assistente de IA do Chat<br> | Alternativa<br> | Como Host, quero receber uma mensagem quando algum campo da base de informações não for preenchido corretamente, para corrigi-lo antes de salvar.<br> | Média<br> | Sem alteração<br> |
| US-HPET06B-04<br> | HPET06b – Configurar Assistente de IA do Chat<br> | Alternativa<br> | Como Host, quero ser avisado quando o conteúdo cadastrado exceder o limite de caracteres permitido, para ajustar o texto antes de salvar.<br> | Baixa<br> | Sem alteração<br> |
| US-HPET06C-01<br> | HPET06c – Gerenciar Conversas Escaladas pela IA<br> | Principal<br> | Como Host, quero visualizar e responder às conversas que o Assistente de IA não conseguiu resolver para dar continuidade ao atendimento do tutor.<br> | Média<br> | Sem alteração<br> |
| US-HPET06C-02<br> | HPET06c – Gerenciar Conversas Escaladas pela IA<br> | Alternativa<br> | Como Host, quero ser informado quando não houver conversas pendentes na fila de escalonamento, para saber que estou em dia com os atendimentos.<br> | Baixa<br> | Sem alteração<br> |
| US-HPET06C-03<br> | HPET06c – Gerenciar Conversas Escaladas pela IA<br> | Exceção<br> | Como Host, quero que as mensagens sejam mantidas em fila e reenviadas automaticamente quando a conexão cair, para não perder o atendimento ao tutor.<br> | Média<br> | Sem alteração<br> |
| US-HPET06-H01<br> | HPET06 – Conversar com Host<br> | Principal<br> | Como Host, quero conversar com o tutor — diretamente ou com apoio do Assistente de IA — para esclarecer dúvidas antes e durante a reserva, assumindo o atendimento quando a IA escalonar a conversa.<br> | Média<br> | Alterado<br> |
| US-HPET29-H01<br> | HPET29 – Consultar Central de Notificações<br> | Principal<br> | Como Host, quero visualizar as notificações sobre reservas, solicitações, pagamentos, resultado da validação do meu cadastro e penalidades, para me manter informado.<br> | Média<br> | Alterado<br> |

Módulo Pet Sitter (Mobile)
--------------------------

| ID<br> | Caso de Uso<br> | Tipo<br> | User Story<br> | Prioridade<br> | Revisão<br> |
| --- | --- | --- | --- | --- | --- |
| US-HPET16-01<br> | HPET16 – Cadastrar-se como Pet Sitter<br> | Principal<br> | Como Pet Sitter, quero me cadastrar na plataforma a partir da minha conta e enviar o perfil para validação, para oferecer serviços de cuidado domiciliar após a aprovação.<br> | Média<br> | Alterado<br> |
| US-HPET16-02<br> | HPET16 – Cadastrar-se como Pet Sitter<br> | Alternativa<br> | Como Pet Sitter, quero receber uma mensagem quando meus dados estiverem incorretos ou incompletos, para corrigi-los.<br> | Média<br> | Sem alteração<br> |
| US-HPET16A-01<br> | HPET16a – Editar Perfil de Pet Sitter<br> | Principal<br> | Como Pet Sitter, quero alterar os dados do meu perfil para mantê-los atualizados, sabendo que alterações sujeitas a validação passam por revalidação.<br> | Média<br> | Alterado<br> |
| US-HPET16A-02<br> | HPET16a – Editar Perfil de Pet Sitter<br> | Alternativa<br> | Como Pet Sitter, quero receber uma mensagem quando algum dado alterado estiver incorreto, para corrigi-lo.<br> | Média<br> | Sem alteração<br> |
| US-HPET16B-01<br> | HPET16b – Definir Área de Atuação/Disponibilidade<br> | Principal<br> | Como Pet Sitter, quero definir minha área de atuação e disponibilidade para receber solicitações compatíveis.<br> | Média<br> | Sem alteração<br> |
| US-HPET16B-02<br> | HPET16b – Definir Área de Atuação/Disponibilidade<br> | Alternativa<br> | Como Pet Sitter, quero receber uma mensagem quando minha área ou disponibilidade estiver inválida, para corrigi-la.<br> | Baixa<br> | Sem alteração<br> |
| US-HPET17-01<br> | HPET17 – Receber Solicitação de Visita<br> | Principal<br> | Como Pet Sitter, quero receber as solicitações de cuidado domiciliar para avaliar e responder a cada pedido.<br> | Média<br> | Sem alteração<br> |
| US-HPET17-02<br> | HPET17 – Receber Solicitação de Visita<br> | Alternativa<br> | Como Pet Sitter, quero ser informado quando não houver solicitações pendentes, para saber que não existem novos atendimentos.<br> | Baixa<br> | Sem alteração<br> |
| US-HPET17A-01<br> | HPET17a – Aceitar/Recusar Solicitação de Visita<br> | Principal<br> | Como Pet Sitter, quero aceitar ou recusar uma solicitação recebida para confirmar minha disponibilidade e liberar o pagamento ao tutor.<br> | Média<br> | Alterado<br> |
| US-HPET17A-02<br> | HPET17a – Aceitar/Recusar Solicitação de Visita<br> | Alternativa<br> | Como Pet Sitter, quero que o tutor seja notificado automaticamente quando eu recusar uma solicitação, para que ele busque outro profissional.<br> | Média<br> | Sem alteração<br> |
| US-HPET17B-01<br> | HPET17b – Registrar Execução do Serviço<br> | Principal<br> | Como Pet Sitter, quero registrar o check-in e o check-out do atendimento com localização e foto de comprovação, para comprovar ao tutor o serviço efetivamente prestado.<br> | Média<br> | Alterado<br> |
| US-HPET17B-02<br> | HPET17b – Registrar Execução do Serviço<br> | Alternativa<br> | Como Pet Sitter, quero receber uma mensagem quando os dados da execução estiverem incorretos ou incompletos, para corrigi-los.<br> | Baixa<br> | Sem alteração<br> |
| US-HPET29-S01<br> | HPET29 – Consultar Central de Notificações<br> | Principal<br> | Como Pet Sitter, quero visualizar as notificações sobre solicitações, pagamentos, resultado da validação do meu cadastro e penalidades, para me manter informado.<br> | Baixa<br> | Alterado<br> |

Módulo Pet Walker (Mobile)
--------------------------

| ID<br> | Caso de Uso<br> | Tipo<br> | User Story<br> | Prioridade<br> | Revisão<br> |
| --- | --- | --- | --- | --- | --- |
| US-HPET18-01<br> | HPET18 – Cadastrar-se como Pet Walker<br> | Principal<br> | Como Pet Walker, quero me cadastrar na plataforma a partir da minha conta e enviar o perfil para validação, para oferecer serviços de passeio com pets após a aprovação.<br> | Média<br> | Alterado<br> |
| US-HPET18-02<br> | HPET18 – Cadastrar-se como Pet Walker<br> | Alternativa<br> | Como Pet Walker, quero receber uma mensagem quando meus dados estiverem incorretos ou incompletos, para corrigi-los.<br> | Média<br> | Sem alteração<br> |
| US-HPET18A-01<br> | HPET18a – Editar Perfil de Pet Walker<br> | Principal<br> | Como Pet Walker, quero alterar os dados do meu perfil para mantê-los atualizados, sabendo que alterações sujeitas a validação passam por revalidação.<br> | Média<br> | Alterado<br> |
| US-HPET18A-02<br> | HPET18a – Editar Perfil de Pet Walker<br> | Alternativa<br> | Como Pet Walker, quero receber uma mensagem quando algum dado alterado estiver incorreto, para corrigi-lo.<br> | Baixa<br> | Sem alteração<br> |
| US-HPET18B-01<br> | HPET18b – Definir Área de Atuação/Disponibilidade<br> | Principal<br> | Como Pet Walker, quero definir minha região de atuação e disponibilidade para receber solicitações compatíveis.<br> | Média<br> | Sem alteração<br> |
| US-HPET18B-02<br> | HPET18b – Definir Área de Atuação/Disponibilidade<br> | Alternativa<br> | Como Pet Walker, quero receber uma mensagem quando minha região ou disponibilidade estiver inválida, para corrigi-la.<br> | Baixa<br> | Sem alteração<br> |
| US-HPET19-01<br> | HPET19 – Aceitar/Recusar Passeio<br> | Principal<br> | Como Pet Walker, quero aceitar ou recusar solicitações de passeio para confirmar minha disponibilidade e liberar o pagamento ao tutor.<br> | Média<br> | Alterado<br> |
| US-HPET19-02<br> | HPET19 – Aceitar/Recusar Passeio<br> | Alternativa<br> | Como Pet Walker, quero que o tutor seja notificado automaticamente quando eu recusar uma solicitação de passeio, para que ele busque outro profissional.<br> | Média<br> | Alterado<br> |
| US-HPET19B-01<br> | HPET19b – Registrar Execução do Passeio<br> | Principal<br> | Como Pet Walker, quero registrar o início e o fim do passeio com localização e foto de comprovação, para comprovar ao tutor o atendimento efetivamente prestado.<br> | Média<br> | Alterado<br> |
| US-HPET19B-02<br> | HPET19b – Registrar Execução do Passeio<br> | Alternativa<br> | Como Pet Walker, quero receber uma mensagem quando os dados da execução estiverem incorretos ou incompletos, para corrigi-los.<br> | Baixa<br> | Sem alteração<br> |
| US-HPET29-W01<br> | HPET29 – Consultar Central de Notificações<br> | Principal<br> | Como Pet Walker, quero visualizar as notificações sobre solicitações, pagamentos, resultado da validação do meu cadastro e penalidades, para me manter informado.<br> | Baixa<br> | Alterado<br> |
US-HPET19-02 mudou apenas o nome do caso de uso, renomeado no UC v2 para "Aceitar/Recusar Passeio".

Comum a todos os perfis do app (Mobile)
---------------------------------------

| ID<br> | Caso de Uso<br> | Tipo<br> | User Story<br> | Prioridade<br> | Revisão<br> |
| --- | --- | --- | --- | --- | --- |
| US-HPET36-01<br> | HPET36 – Denunciar Usuário ou Conteúdo<br> | Principal<br> | Como usuário do app, quero denunciar um usuário ou conteúdo (perfil, avaliação, foto ou mensagem de chat) que descumpra as regras, para que o Administrador analise o caso.<br> | Alta<br> | Novo<br> |
| US-HPET36-02<br> | HPET36 – Denunciar Usuário ou Conteúdo<br> | Alternativa<br> | Como usuário do app, quero receber uma mensagem quando não selecionar o motivo da denúncia, para completá-la antes de enviar.<br> | Média<br> | Novo<br> |
| US-HPET36-03<br> | HPET36 – Denunciar Usuário ou Conteúdo<br> | Alternativa<br> | Como usuário do app, quero ser informado quando já tiver denunciado o mesmo item, para saber que a denúncia já está em análise.<br> | Baixa<br> | Novo<br> |
| US-HPET36-04<br> | HPET36 – Denunciar Usuário ou Conteúdo<br> | Alternativa<br> | Como usuário do app, quero que, ao denunciar uma mensagem de chat, somente a mensagem denunciada seja enviada à moderação, para preservar a privacidade do restante da conversa.<br> | Alta<br> | Novo<br> |
| US-HPET36-05<br> | HPET36 – Denunciar Usuário ou Conteúdo<br> | Exceção<br> | Como usuário do app, quero ser informado quando ocorrer falha ao registrar a denúncia, para tentar novamente.<br> | Média<br> | Novo<br> |
| US-HPET37-01<br> | HPET37 – Corrigir e Reenviar Cadastro Rejeitado<br> | Principal<br> | Como Host, Pet Sitter ou Pet Walker com cadastro rejeitado, quero consultar o feedback do Administrador, corrigir os itens apontados e reenviar o cadastro, para que ele seja validado novamente.<br> | Alta<br> | Novo<br> |
| US-HPET37-02<br> | HPET37 – Corrigir e Reenviar Cadastro Rejeitado<br> | Alternativa<br> | Como Host, Pet Sitter ou Pet Walker, quero receber uma mensagem quando algum dado corrigido estiver incorreto, para ajustá-lo antes do reenvio.<br> | Média<br> | Novo<br> |
| US-HPET37-03<br> | HPET37 – Corrigir e Reenviar Cadastro Rejeitado<br> | Alternativa<br> | Como Host, Pet Sitter ou Pet Walker, quero ser alertado quando tentar reenviar sem alterar nenhum item apontado no feedback, para confirmar se desejo reenviar assim mesmo.<br> | Baixa<br> | Novo<br> |
| US-HPET38-01<br> | HPET38 – Visualizar Aviso de Penalidade<br> | Principal<br> | Como usuário do app, quero visualizar a penalidade recebida, com tipo, motivo e prazo, e confirmar a ciência, para entender o que aconteceu e as consequências na minha conta.<br> | Alta<br> | Novo<br> |
| US-HPET38-02<br> | HPET38 – Visualizar Aviso de Penalidade<br> | Alternativa<br> | Como usuário do app com conta suspensa ou banida, quero ver o aviso da penalidade ao tentar entrar, para entender por que meu acesso está bloqueado e até quando.<br> | Alta<br> | Novo<br> |
| US-HPET38-03<br> | HPET38 – Visualizar Aviso de Penalidade<br> | Alternativa<br> | Como usuário do app, quero ser notificado quando minha penalidade for revertida ou encerrada, para saber que minha conta voltou ao normal.<br> | Média<br> | Novo<br> |
| US-HPET39-01<br> | HPET39 – Inativar Própria Conta<br> | Principal<br> | Como usuário do app, quero inativar minha própria conta, preservando meus dados e histórico, para deixar de usar a plataforma.<br> | Baixa<br> | Novo<br> |
| US-HPET39-02<br> | HPET39 – Inativar Própria Conta<br> | Alternativa<br> | Como usuário do app, quero ser impedido de inativar minha conta enquanto houver reserva, solicitação ou serviço ativo ou futuro, para não deixar contratações em aberto.<br> | Média<br> | Novo<br> |
| US-HPET39-03<br> | HPET39 – Inativar Própria Conta<br> | Alternativa<br> | Como prestador, quero ser avisado sobre saldo a receber antes de confirmar a inativação da conta, para decidir se solicito o repasse antes.<br> | Baixa<br> | Novo<br> |
HPET39 depende de confirmação da PO; as três US ficam condicionadas a essa decisão.

Área Comercial Pública (Web)
----------------------------

| ID<br> | Caso de Uso<br> | Tipo<br> | User Story<br> | Prioridade<br> | Revisão<br> |
| --- | --- | --- | --- | --- | --- |
| US-HPET34-01<br> | HPET34 – Consultar Informações Institucionais<br> | Principal<br> | Como visitante, quero conhecer a HOSPETSE pelo site (quem somos, serviços e como funciona), para entender a proposta da plataforma antes de baixar o app.<br> | Média<br> | Novo<br> |
| US-HPET34-02<br> | HPET34 – Consultar Informações Institucionais<br> | Alternativa<br> | Como visitante, quero ver uma página de erro com retorno à página inicial quando acessar uma página inexistente, para continuar navegando.<br> | Baixa<br> | Novo<br> |
| US-HPET35-01<br> | HPET35 – Conhecer e Obter o Aplicativo<br> | Principal<br> | Como visitante, quero entender o que o aplicativo oferece e ser direcionado à loja compatível com meu dispositivo, para instalar o app.<br> | Média<br> | Novo<br> |
| US-HPET35-02<br> | HPET35 – Conhecer e Obter o Aplicativo<br> | Alternativa<br> | Como visitante, quero escolher entre as lojas disponíveis quando o sistema operacional do meu dispositivo não for identificado, para obter o app mesmo assim.<br> | Baixa<br> | Novo<br> |
| US-HPET35-03<br> | HPET35 – Conhecer e Obter o Aplicativo<br> | Alternativa<br> | Como visitante, quero ser avisado de que o app estará disponível em breve quando ainda não estiver publicado na loja, para voltar depois.<br> | Baixa<br> | Novo<br> |

Módulo Administrador (Web)
--------------------------

| ID<br> | Caso de Uso<br> | Tipo<br> | User Story<br> | Prioridade<br> | Revisão<br> |
| --- | --- | --- | --- | --- | --- |
| US-HPET20-01<br> | HPET20 – Gerenciar Usuários<br> | Principal<br> | Como Administrador, quero consultar os usuários cadastrados, com abas por papel e o estado da conta, para acompanhar contas, histórico, ocorrências e penalidades.<br> | Alta<br> | Alterado<br> |
| US-HPET20-02<br> | HPET20 – Gerenciar Usuários<br> | Principal<br> | Como Administrador, quero reativar uma conta inativa informando o motivo, para restabelecer o acesso do usuário com registro na auditoria.<br> | Alta<br> | Alterado<br> |
| US-HPET20-03<br> | HPET20 – Gerenciar Usuários<br> | Alternativa<br> | Como Administrador, quero ser informado quando o usuário selecionado não for encontrado, para evitar executar uma ação sobre uma conta inexistente.<br> | Média<br> | Sem alteração<br> |
| US-HPET20A-01<br> | HPET20a – Autenticar-se (Administrador)<br> | Principal<br> | Como Administrador, quero acessar o painel administrativo Web pela rota administrativa mediante validação das minhas credenciais, para realizar as atividades de gestão.<br> | Alta<br> | Alterado<br> |
| US-HPET20A-02<br> | HPET20a – Autenticar-se (Administrador)<br> | Alternativa<br> | Como Administrador, quero receber uma mensagem quando minhas credenciais forem inválidas, para corrigi-las e tentar novamente.<br> | Alta<br> | Sem alteração<br> |
| US-HPET20A-03<br> | HPET20a – Autenticar-se (Administrador)<br> | Alternativa<br> | Como Administrador, quero que o painel negue o acesso a usuários sem papel de Administrador e a contas administrativas inativas, para proteger as funções de gestão.<br> | Alta<br> | Novo<br> |
| US-HPET20A-04<br> | HPET20a – Autenticar-se (Administrador)<br> | Alternativa<br> | Como Administrador, quero ser direcionado a um novo login quando minha sessão expirar, para continuar o trabalho com segurança.<br> | Média<br> | Novo<br> |
| US-HPET20B-01<br> | HPET20b – Validar Cadastro de Host/Prestador<br> | Principal<br> | Como Administrador, quero validar cadastros novos, revalidações e reenvios de hosts e prestadores em uma fila filtrável, para garantir a qualidade e a segurança dos serviços oferecidos.<br> | Alta<br> | Alterado<br> |
| US-HPET20B-02<br> | HPET20b – Validar Cadastro de Host/Prestador<br> | Alternativa<br> | Como Administrador, quero rejeitar um cadastro informando o feedback com os itens a ajustar, para que o solicitante seja notificado e possa corrigir e reenviar.<br> | Alta<br> | Alterado<br> |
| US-HPET20B-03<br> | HPET20b – Validar Cadastro de Host/Prestador<br> | Alternativa<br> | Como Administrador, quero ser avisado quando o cadastro já tiver sido decidido, para não aplicar uma segunda decisão sobre ele.<br> | Média<br> | Novo<br> |
| US-HPET25-01<br> | HPET25 – Gerenciar Penalidades<br> | Principal<br> | Como Administrador, quero analisar as ocorrências (denúncias e incidentes) e aplicar advertência, suspensão com prazo ou banimento com motivo, para manter a integridade da plataforma.<br> | Média<br> | Alterado<br> |
| US-HPET25-02<br> | HPET25 – Gerenciar Penalidades<br> | Alternativa<br> | Como Administrador, quero ser informado quando o usuário selecionado para penalização não for encontrado, para corrigir a busca.<br> | Média<br> | Sem alteração<br> |
| US-HPET25-03<br> | HPET25 – Gerenciar Penalidades<br> | Alternativa<br> | Como Administrador, quero arquivar uma ocorrência com justificativa quando não houver descumprimento, para encerrá-la sem aplicar penalidade.<br> | Média<br> | Novo<br> |
| US-HPET25A-01<br> | HPET25a – Acompanhar e Reverter Penalidades<br> | Principal<br> | Como Administrador, quero acompanhar as penalidades ativas e reverter uma penalidade informando o motivo, para corrigir decisões e restaurar a conta quando aplicável.<br> | Média<br> | Novo<br> |
| US-HPET25A-02<br> | HPET25a – Acompanhar e Reverter Penalidades<br> | Técnica<br> | Como Sistema, preciso encerrar automaticamente as suspensões na data de fim, restaurar a conta e notificar o usuário, para que o prazo definido seja cumprido sem ação manual.<br> | Alta<br> | Novo<br> |
| US-HPET25A-03<br> | HPET25a – Acompanhar e Reverter Penalidades<br> | Alternativa<br> | Como Administrador, quero ser informado quando não houver penalidades ativas, para saber que a lista está vazia.<br> | Baixa<br> | Novo<br> |
| US-HPET40-01<br> | HPET40 – Gerenciar Cupons de Desconto<br> | Principal<br> | Como Administrador, quero criar e editar cupons de desconto com código, tipo e valor, vigência e limite de uso, para oferecer promoções aos tutores.<br> | Baixa<br> | Novo<br> |
| US-HPET40-02<br> | HPET40 – Gerenciar Cupons de Desconto<br> | Alternativa<br> | Como Administrador, quero inativar um cupom, para que ele deixe de ser aceito em novos pagamentos.<br> | Baixa<br> | Novo<br> |
| US-HPET40-03<br> | HPET40 – Gerenciar Cupons de Desconto<br> | Alternativa<br> | Como Administrador, quero receber uma mensagem quando o código já existir ou os dados do cupom forem inválidos, para corrigi-los antes de salvar.<br> | Baixa<br> | Novo<br> |
| US-HPET41-01<br> | HPET41 – Configurar Taxa da Plataforma<br> | Principal<br> | Como Administrador, quero definir o percentual da taxa da plataforma com data de início de vigência, para que seja aplicado no repasse e exibido no financeiro dos prestadores.<br> | Alta<br> | Novo<br> |
| US-HPET41-02<br> | HPET41 – Configurar Taxa da Plataforma<br> | Alternativa<br> | Como Administrador, quero receber uma mensagem quando o valor da taxa for inválido, para corrigi-lo antes de salvar.<br> | Média<br> | Novo<br> |
| US-HPET42-01<br> | HPET42 – Moderar Conteúdo Denunciado<br> | Principal<br> | Como Administrador, quero analisar os conteúdos denunciados e decidir mantê-los, ocultá-los ou removê-los com justificativa, para manter a plataforma segura.<br> | Média<br> | Novo<br> |
| US-HPET42-02<br> | HPET42 – Moderar Conteúdo Denunciado<br> | Alternativa<br> | Como Administrador, quero ser informado quando a fila de moderação estiver vazia, para saber que não há itens pendentes.<br> | Baixa<br> | Novo<br> |
| US-HPET42A-01<br> | HPET42a – Solicitar Acesso à Conversa Completa<br> | Principal<br> | Como Administrador, quero solicitar, com justificativa, acesso à conversa completa de um chat denunciado, para decidir quando a mensagem isolada não for suficiente.<br> | Baixa<br> | Novo<br> |
| US-HPET42A-02<br> | HPET42a – Solicitar Acesso à Conversa Completa<br> | Alternativa<br> | Como Administrador, quero ser informado quando o acesso à conversa completa for negado, para decidir apenas com base na mensagem denunciada.<br> | Baixa<br> | Novo<br> |
| US-HPET43-01<br> | HPET43 – Tratar Transação Financeira com Falha<br> | Principal<br> | Como Administrador, quero reprocessar ou registrar a resolução manual de estornos e repasses com falha, com justificativa, para regularizar a situação do tutor ou do prestador.<br> | Alta<br> | Novo<br> |
| US-HPET43-02<br> | HPET43 – Tratar Transação Financeira com Falha<br> | Alternativa<br> | Como Administrador, quero que uma nova falha no reprocessamento mantenha o status de falha e registre a tentativa, para acompanhar o histórico até a resolução.<br> | Média<br> | Novo<br> |
| US-HPET44-01<br> | HPET44 – Consultar Dashboard Administrativo<br> | Principal<br> | Como Administrador, quero ver após o login um resumo das filas pendentes e dos indicadores de contratações, para priorizar o trabalho.<br> | Média<br> | Novo<br> |
| US-HPET44-02<br> | HPET44 – Consultar Dashboard Administrativo<br> | Alternativa<br> | Como Administrador, quero ver indicadores zerados quando não houver dados no período, para não confundir ausência de dados com erro.<br> | Baixa<br> | Novo<br> |
| US-HPET45-01<br> | HPET45 – Consultar Contratações<br> | Principal<br> | Como Administrador, quero consultar as reservas e solicitações de serviço da plataforma com status e detalhes, para apoiar a análise de ocorrências e transações.<br> | Média<br> | Novo<br> |
| US-HPET45-02<br> | HPET45 – Consultar Contratações<br> | Alternativa<br> | Como Administrador, quero ser informado quando nenhuma contratação corresponder aos filtros, para ajustar a consulta.<br> | Baixa<br> | Novo<br> |
| US-HPET46-01<br> | HPET46 – Consultar Financeiro da Plataforma<br> | Principal<br> | Como Administrador, quero acompanhar pagamentos, repasses, estornos e taxas arrecadadas, para controlar o financeiro da plataforma.<br> | Média<br> | Novo<br> |
| US-HPET46-02<br> | HPET46 – Consultar Financeiro da Plataforma<br> | Alternativa<br> | Como Administrador, quero ser informado quando não houver transações no período, para ajustar os filtros.<br> | Baixa<br> | Novo<br> |
| US-HPET47-01<br> | HPET47 – Consultar Trilha de Auditoria<br> | Principal<br> | Como Administrador, quero consultar a trilha de auditoria das ações administrativas, com autor, data e motivo, para garantir a rastreabilidade das decisões.<br> | Média<br> | Novo<br> |
| US-HPET47-02<br> | HPET47 – Consultar Trilha de Auditoria<br> | Alternativa<br> | Como Administrador, quero ser informado quando nenhum registro corresponder aos filtros, para ajustar a consulta.<br> | Baixa<br> | Novo<br> |
| US-HPET48-01<br> | HPET48 – Gerenciar Conta do Administrador<br> | Principal<br> | Como Administrador, quero consultar meus dados de acesso e alterar minha senha, para manter minha conta administrativa segura.<br> | Média<br> | Novo<br> |
| US-HPET48-02<br> | HPET48 – Gerenciar Conta do Administrador<br> | Principal<br> | Como Administrador, quero encerrar minha sessão, para impedir o uso do painel por outra pessoa no mesmo dispositivo.<br> | Média<br> | Novo<br> |
| US-HPET48-03<br> | HPET48 – Gerenciar Conta do Administrador<br> | Alternativa<br> | Como Administrador, quero receber uma mensagem quando a senha atual estiver incorreta ou a nova não atender à política, para corrigir antes de salvar.<br> | Baixa<br> | Novo<br> |

Sistema de Pagamento (Backend)
------------------------------

| ID<br> | Caso de Uso<br> | Tipo<br> | User Story<br> | Prioridade<br> | Revisão<br> |
| --- | --- | --- | --- | --- | --- |
| US-HPET26-01<br> | HPET26 – Processar Pagamento<br> | Técnica<br> | Como Sistema de Pagamento, preciso processar os pagamentos de reservas e serviços já aceitos pelo prestador e registrar as transações aprovadas.<br> | Alta<br> | Alterado<br> |
| US-HPET26-02<br> | HPET26 – Processar Pagamento<br> | Exceção<br> | Como Sistema de Pagamento, preciso informar quando o gateway recusar uma transação, para impedir que um pagamento não aprovado seja confirmado.<br> | Alta<br> | Alterado<br> |
| US-HPET27-01<br> | HPET27 – Repassar Valores<br> | Técnica<br> | Como Sistema de Pagamento, preciso calcular e registrar o repasse aos prestadores após a conclusão do serviço, descontando a taxa vigente na contratação.<br> | Alta<br> | Alterado<br> |
| US-HPET27-02<br> | HPET27 – Repassar Valores<br> | Exceção<br> | Como Sistema de Pagamento, preciso informar quando ocorrer uma falha no cálculo do repasse, para evitar o envio de um valor incorreto.<br> | Alta<br> | Alterado<br> |
| US-HPET27-03<br> | HPET27 – Repassar Valores<br> | Exceção<br> | Como Sistema de Pagamento, preciso registrar com status de falha o repasse cuja transferência não se concluir, para que o Administrador o trate sem transferir valor incorreto.<br> | Alta<br> | Novo<br> |
| US-HPET28-01<br> | HPET28 – Processar Cancelamento / Estorno<br> | Técnica<br> | Como Sistema de Pagamento, preciso processar o estorno de pagamentos de reservas canceladas para devolver o valor correspondente ao tutor.<br> | Alta<br> | Sem alteração<br> |
| US-HPET28-02<br> | HPET28 – Processar Cancelamento / Estorno<br> | Exceção<br> | Como Sistema de Pagamento, preciso registrar com status de falha o estorno recusado pelo gateway, para que o Administrador o trate sem que uma devolução inexistente seja declarada.<br> | Alta<br> | Alterado<br> |
| US-HPET28A-01<br> | HPET28a – Registrar Transação Financeira<br> | Técnica<br> | Como Sistema de Pagamento, preciso registrar pagamentos, repasses, estornos e resoluções manuais do Administrador em lançamentos imutáveis, para fins de auditoria e consulta financeira.<br> | Alta<br> | Alterado<br> |
| US-HPET28A-02<br> | HPET28a – Registrar Transação Financeira<br> | Exceção<br> | Como Sistema de Pagamento, preciso informar quando ocorrer falha no registro de uma transação, para evitar que uma movimentação financeira fique sem rastro.<br> | Alta<br> | Sem alteração<br> |