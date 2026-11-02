Documento de Requisitos Funcionais e Não Funcionais HOSPETSE — Revisão v2
=========================================================================

1. Identificação do documento
-----------------------------

Este documento consolida 119 requisitos funcionais e 47 não funcionais do HOSPETSE, derivados das decisões da PO.

| Campo | Conteúdo |
| --- | --- |
| Projeto | HOSPETSE — Plataforma de Hospedagem e Serviços para Pets |
| Disciplina | Experiência Profissional Fábrica de Software — Escola de TI, Unicesumar |
| Artefato | Documento de Requisitos Funcionais e Não Funcionais |
| Versão | 2.0 |
| Responsável | Brenda (Product Owner) |
| Equipe | Ana Clara, Amanda, Brenda, Lucas, Raul e Murilo |

2. Introdução
-------------

O HOSPETSE é um marketplace que conecta tutores de pets a prestadores de hospedagem (Host), visitas domiciliares (Pet Sitter) e passeios (Pet Walker), com um módulo administrativo Web para validação, moderação e financeiro.

### 2.1 Escopo por canal

| Canal | Módulos | Atores |
| --- | --- | --- |
| Mobile (app) | Tutor, Host, Pet Sitter, Pet Walker e funcionalidades comuns | Tutor, Host, Pet Sitter, Pet Walker, Assistente de IA |
| Web: área pública | Quem somos, Serviços, Como funciona, Sobre o app, Termos de Uso e Política de Privacidade | Visitante |
| Web: admin (rota /admin) | Usuários, validação, penalidades, moderação, cupons, taxa, contratações, financeiro, auditoria | Administrador |
| Back-end | Sistema de Pagamento (pagamento, repasse, estorno, registro financeiro) | Sistema de Pagamento, rotinas agendadas |

### 2.2 Convenções

*   RF-XXX-nn: requisito funcional, agrupado por módulo (ACE, TUT, PET, BUS, CON, EXE, CHT, PAG, FIN, AVA, NOT, PRE, COM, WEB, ADM).
*   RNF-XXX-nn: requisito não funcional, agrupado por categoria.
*   Prioridade (MoSCoW): Must = obrigatório para a entrega à banca (20/11/2026) ou exigido pelo regulamento; Should = importante, pode ir para a Sprint seguinte; Could = desejável.
*   Origem: decisão da PO, normas, documento ou fonte técnica que gerou o requisito; "Escopo do projeto" indica requisito do escopo base da plataforma.

3. Requisitos funcionais
------------------------

São 119 requisitos funcionais em 15 grupos: 63 Must, 53 Should e 3 Could.

### 3.1 Conta e acesso (ACE) — Mobile, comum a todos os perfis

| ID | Requisito | Prioridade | Origem |
| --- | --- | --- | --- |
| RF-ACE-01 | O sistema deve permitir o cadastro inicial com nome, e-mail, senha e aceite obrigatório dos Termos de Uso e da Política de Privacidade, registrando data e versão aceitas. | Must | Escopo do projeto |
| RF-ACE-02 | O sistema deve confirmar o cadastro por PIN enviado ao e-mail, de uso único e com validade, com no máximo 5 tentativas por código e novo envio após 1 minuto. | Must | decisão 24/10 |
| RF-ACE-03 | O sistema deve garantir um e-mail por conta. | Must | Glossário (Conta) |
| RF-ACE-04 | O sistema deve autenticar o usuário por e-mail e senha, verificando o estado da conta e liberando o acesso conforme os papéis. | Must | Escopo do projeto |
| RF-ACE-05 | O sistema deve negar o login de conta suspensa (informando motivo e data de fim), banida ou inativa (orientando a reativação). | Must | DA-01 |
| RF-ACE-06 | O sistema deve bloquear o login por 30 minutos após 5 tentativas erradas seguidas. | Should | decisão PO 01/11 |
| RF-ACE-07 | O sistema deve informar, no login, quando o cadastro não foi concluído e permitir pedir novo PIN para terminá-lo. | Should | ADR-011 |
| RF-ACE-08 | O sistema deve permitir redefinir a senha por código ou link de uso único com validade limitada (cerca de 10 min), invalidando a senha anterior, sem alterar o estado da conta. | Should | Escopo do projeto |
| RF-ACE-09 | O sistema deve permitir encerrar a sessão (sair). | Must | Escopo do projeto |
| RF-ACE-10 | O sistema deve aplicar o modelo de papel aditivo: toda conta começa como Tutor e pode acrescentar um papel de prestador sem perder o papel Tutor. | Must | DER |
| RF-ACE-11 | O sistema deve permitir que o usuário inative a própria conta, impedindo a ação se houver contratação ativa ou futura e avisando sobre saldo a receber. | Should | Escopo do projeto |

### 3.2 Perfil do Tutor (TUT) — Mobile

| ID | Requisito | Prioridade | Origem |
| --- | --- | --- | --- |
| RF-TUT-01 | O sistema deve permitir completar o perfil com CPF, data de nascimento, telefone, endereço e contato de emergência, recusando CPF inválido ou já cadastrado. | Must | decisões 25/10 e 01/11 (telefone) |
| RF-TUT-02 | O sistema deve exigir maioridade (mais de 18 anos) pela data de nascimento. | Must | DEF item 2 |
| RF-TUT-03 | O sistema deve permitir navegar e buscar com perfil incompleto, mas bloquear a contratação até o perfil estar completo e a conta sem restrição. | Must | Escopo do projeto |
| RF-TUT-04 | O sistema deve permitir editar os dados do perfil, validando os campos alterados e exigindo nova confirmação quando o e-mail mudar. | Should | Escopo do projeto |

### 3.3 Pets (PET) — Mobile

| ID | Requisito | Prioridade | Origem |
| --- | --- | --- | --- |
| RF-PET-01 | O sistema deve permitir cadastrar um ou mais pets, com nome, espécie e porte obrigatórios (espécie e porte em listas pré-definidas), além de raça, idade, foto, características comportamentais e informações médicas (vacinas, alergias, medicações). | Must | Escopo do projeto |
| RF-PET-02 | O sistema deve permitir anexar documentos ou comprovantes ao pet (opcional). | Could | Escopo do projeto |
| RF-PET-03 | O sistema deve permitir editar os dados do pet, validando os campos alterados. | Should | Escopo do projeto |
| RF-PET-04 | O sistema deve inativar o pet por inativação lógica, preservando o histórico, retirando-o da busca e da contratação, e impedir a inativação se houver contratação ativa ou futura. | Should | Escopo do projeto |

### 3.4 Busca (BUS) — Mobile

| ID | Requisito | Prioridade | Origem |
| --- | --- | --- | --- |
| RF-BUS-01 | O sistema deve permitir buscar Hosts por localização (cidade), preço, disponibilidade, porte e raça. | Must | decisões 24/10 e 29/10 |
| RF-BUS-02 | O sistema deve permitir buscar Pet Sitter ou Pet Walker por tipo de serviço, localização, preço e disponibilidade. | Must | Escopo do projeto |
| RF-BUS-03 | O sistema deve aplicar espécie, porte e raça do pet selecionado como filtros, recusando filtro manual simultâneo desses campos. | Must | COD |
| RF-BUS-04 | O sistema deve exibir somente prestadores com cadastro aprovado, perfil ativo e conta ativa. | Must | Escopo do projeto |
| RF-BUS-05 | O sistema deve permitir buscar sem pet cadastrado e exibir aviso, na busca e na solicitação de serviço, com atalho para o cadastro de pet e retorno ao fluxo com o novo pet disponível. | Must | Decisão 29/10 |
| RF-BUS-06 | O sistema deve paginar os resultados e permitir ordená-los por preço, distância e avaliação, com a menor diária como ordem padrão. | Should | COD |
| RF-BUS-07 | O sistema deve exibir os detalhes do prestador: estrutura, fotos, comodidades, espécies e portes aceitos, preços e nota média. | Must | Escopo do projeto |
| RF-BUS-08 | O sistema deve informar a ausência de resultados e sugerir ajuste dos filtros (ex.: ampliar raio ou período). | Must | Escopo do projeto |
| RF-BUS-09 | O sistema deve permitir favoritar e desfavoritar prestadores. | Could | Escopo do projeto |
| RF-BUS-10 | O sistema deve permitir compartilhar o perfil de um Host pelos canais do dispositivo. | Could | Escopo do projeto |

### 3.5 Contratação (CON) — Mobile

| ID | Requisito | Prioridade | Origem |
| --- | --- | --- | --- |
| RF-CON-01 | O sistema deve controlar o ciclo único da contratação: aguardando aceite → aguardando pagamento → confirmada → em andamento → concluída, além de recusada e cancelada. | Must | Decisão PO. |
| RF-CON-02 | O sistema deve permitir solicitar reserva de hospedagem informando Host, datas, pet ativo, serviços adicionais e observações, com status "aguardando aceite". | Must | Escopo do projeto |
| RF-CON-03 | O sistema deve permitir solicitar visita (Pet Sitter) ou passeio (Pet Walker) informando prestador, pet ativo, data e horário. | Must | Escopo do projeto |
| RF-CON-04 | O sistema deve validar no backend a compatibilidade de espécie e porte do pet com o prestador, exibindo "Pet não permitido" quando incompatível. | Must | Escopo do projeto |
| RF-CON-05 | O sistema deve verificar novamente a disponibilidade no envio da solicitação, exibindo "Data indisponível" quando não houver vaga. | Must | Escopo do projeto |
| RF-CON-06 | O sistema deve registrar o snapshot do preço e da taxa da plataforma vigentes no momento da solicitação. | Must | Escopo do projeto |
| RF-CON-07 | O sistema deve informar ao tutor que o pagamento só será pedido após o aceite e que o prestador tem 1 dia útil para responder. | Must | Escopo do projeto |
| RF-CON-08 | O sistema deve permitir informar os medicamentos do pet (tipo, horários, duração e forma de aplicação) somente quando o prestador oferece administração de medicamentos. | Should | DA-21 |
| RF-CON-09 | O sistema deve permitir, na visita e no passeio, informar instruções de acesso à residência: método, passo a passo, alarme, áreas liberadas e restrições. | Should | DA-22 |
| RF-CON-10 | O sistema deve notificar o prestador sobre nova solicitação e exibir os dados do pet, os medicamentos e os dados do tutor permitidos antes do serviço. | Must | DA-20 |
| RF-CON-11 | O sistema deve permitir que o prestador aceite ou recuse a solicitação, no aceite, passa a "aguardando pagamento", bloqueia a agenda no período e notifica o tutor; na recusa, passa a "recusada" e notifica o tutor. | Must | Escopo do projeto |
| RF-CON-12 | O sistema deve exigir que o prestador confirme a responsabilidade pela administração dos medicamentos para aceitar uma solicitação com medicamentos. | Should | DA-21 |
| RF-CON-13 | O sistema deve cancelar automaticamente a solicitação sem resposta em 1 dia útil, liberar a agenda e notificar o tutor, informando ao prestador que ela foi cancelada se ele tentar aceitá-la depois. | Must | Escopo do projeto |
| RF-CON-14 | O sistema deve cancelar automaticamente a contratação não paga em 1 dia útil após o aceite, liberar a agenda e notificar as partes, aguardando o retorno do gateway quando houver pagamento em processamento. | Must | DA-17 |
| RF-CON-15 | O sistema deve permitir que o tutor cancele a contratação aguardando aceite, aguardando pagamento ou confirmada antes do início, exibindo a política de cancelamento antes da confirmação, notificando o prestador e acionando o estorno só se já houver pagamento aprovado (integral com mais de 48 h de antecedência). | Must | decisão PO 01/11 |
| RF-CON-16 | O sistema deve impedir o cancelamento de contratação concluída. | Must | Escopo do projeto |
| RF-CON-17 | O sistema deve exibir o histórico de contratações do tutor com seus detalhes. | Should | Escopo do projeto |

### 3.6 Cadastro e oferta do prestador (PRE) — Mobile

| ID | Requisito | Prioridade | Origem |
| --- | --- | --- | --- |
| RF-PRE-01 | O sistema deve permitir o cadastro como Host com dados do local, estrutura, capacidade, espécies e portes aceitos, oferta de administração de medicamentos, documentos e fotos, com status "pendente de validação", sem aparecer na busca nem receber solicitações até a aprovação. | Must | Escopo do projeto |
| RF-PRE-02 | O sistema deve permitir o cadastro como Pet Sitter ou Pet Walker com dados pessoais, área de atuação, oferta de administração de medicamentos e documentos, com status "pendente de validação". | Must | Escopo do projeto |
| RF-PRE-03 | O sistema deve exigir os documentos conforme o tipo de pessoa (física, CPF; jurídica, CNPJ). | Should | DER; decisão PO. |
| RF-PRE-04 | O sistema deve permitir editar o perfil de prestador e enviar para revalidação, fora da busca, quando mudarem documentos ou dados sujeitos a validação. | Should | Escopo do projeto |
| RF-PRE-05 | O sistema deve permitir ativar ou desativar a oferta de administração de medicamentos no perfil. | Should | DA-21 |
| RF-PRE-06 | O sistema deve permitir inativar o perfil de Host, alertando sobre contratações futuras. | Should | Escopo do projeto |
| RF-PRE-07 | O sistema deve exibir o feedback da rejeição, manter o histórico de rejeições e permitir corrigir e reenviar o cadastro (só em rejeição não definitiva), marcado como reenvio, pedindo confirmação extra se nenhum item apontado foi alterado, com prazo de 48 h quando um documento específico for pedido. | Should | DA-08 |
| RF-PRE-08 | O sistema deve permitir que o Host gerencie a agenda, com dias livres, ocupados e bloqueados, refletindo automaticamente as contratações aceitas e impedindo bloquear período com contratação confirmada. | Must | Escopo do projeto |
| RF-PRE-09 | O sistema deve controlar as vagas pela capacidade do Host: ocupam vaga as contratações aguardando pagamento, confirmadas e em andamento; o dia de saída não conta; um pet não pode estar em duas hospedagens no mesmo período. | Must | COD (HSP-DEC-004) |
| RF-PRE-10 | O sistema deve permitir que Pet Sitter e Pet Walker definam área de atuação e datas e horários disponíveis. | Must | Escopo do projeto |
| RF-PRE-11 | O sistema deve permitir que o Host defina a tabela de preços (diária por porte) e os serviços adicionais. | Must | Escopo do projeto |
| RF-PRE-12 | O sistema deve permitir que Pet Sitter e Pet Walker definam o preço por visita e por passeio. | Should | Lacuna aberta (ver seção 6) |

### 3.7 Execução e acompanhamento (EXE) — Mobile

| ID | Requisito | Prioridade | Origem |
| --- | --- | --- | --- |
| RF-EXE-01 | O sistema deve permitir que o Host confirme o check-in de contratação confirmada na data de início, passando-a para "em andamento" e notificando o tutor; antes da data ou sem pagamento aprovado, a ação é bloqueada. | Must | Escopo do projeto |
| RF-EXE-02 | O sistema deve permitir registrar atividades da hospedagem somente entre check-in e check-out, com mínimo de 1 registro por dia; descrição, alimentação e comportamento são obrigatórios, passeio e banho só quando incluídos no serviço, e fotos podem ser anexadas com limite de formato e tamanho. | Must | Escopo do projeto |
| RF-EXE-03 | O sistema deve alertar o Host quando um dia de hospedagem ficar sem registro. | Should | Escopo do projeto |
| RF-EXE-04 | O sistema deve exigir o registro de cada administração de medicamento, com data e hora, quando houver medicamentos na solicitação. | Should | DA-21 |
| RF-EXE-05 | O sistema deve permitir que o Host confirme o check-out, concluindo a contratação, notificando o tutor e liberando o valor líquido como saldo do prestador; em caso de falha, a contratação continua em andamento. | Must | Escopo do projeto |
| RF-EXE-06 | O sistema deve permitir que Pet Sitter e Pet Walker registrem check-in e check-out com localização e foto, e observações da execução, somente em contratação confirmada; o encerramento conclui a contratação e notifica o tutor. | Must | Escopo do projeto |
| RF-EXE-07 | O sistema deve registrar o trajeto do passeio e calcular sua duração. | Should | Escopo do projeto |
| RF-EXE-08 | O sistema deve exigir, no check-out de visita ou passeio com tutor ausente, a lista de verificação de encerramento (portas e janelas, alarme, devolução da chave). | Should | DA-22 |
| RF-EXE-09 | O sistema deve permitir registrar incidente com tipo (com o pet ou no imóvel), descrição, ação tomada e gravidade (leve, média ou grave), notificando o tutor com prioridade e gerando ocorrência para o Administrador. | Must | DA-18 |
| RF-EXE-10 | O sistema deve permitir que o prestador proponha atendimento veterinário e que o tutor o autorize ou recuse; a autorização cria uma contratação adicional vinculada à original. | Should | DA-19 |
| RF-EXE-11 | O sistema deve permitir que o tutor acompanhe, em tempo real ou quase real e em ordem cronológica, a rotina do pet na hospedagem, visita ou passeio: registros, medicação, fotos, vídeos e incidentes. | Must | Escopo do projeto |
| RF-EXE-12 | O sistema deve liberar ao prestador endereço, telefone e instruções de acesso do tutor somente após o pagamento e no dia do check-in ou do serviço, e ocultá-los após o check-out. | Must | DA-20, DA-22 |

### 3.8 Chat e Assistente de IA (CHT) — Mobile

| ID | Requisito | Prioridade | Origem |
| --- | --- | --- | --- |
| RF-CHT-01 | O sistema deve oferecer chat em tempo real entre tutor e Host via WebSocket, com envio de mensagens e mídia, histórico persistido e carregado ao abrir a conversa. | Must | regulamento |
| RF-CHT-02 | O sistema deve oferecer o Assistente de IA, que responde ao tutor antes e durante a reserva com base na FAQ do Host. | Must | decisão 24/10 |
| RF-CHT-03 | O sistema deve escalonar a conversa da IA para o Host quando a IA não souber responder ou o tutor pedir atendimento humano, mantendo o histórico, notificando o Host e avisando o tutor do encaminhamento. | Must | Escopo do projeto |
| RF-CHT-04 | O sistema deve permitir contato humano direto com o Host antes da reserva somente quando o Host tiver habilitado essa opção (opt-in). | Should | decisão PO 01/11 |
| RF-CHT-05 | O sistema deve restringir novas mensagens diretas ao Host quando a reserva não estiver mais ativa, mantendo a IA disponível se o Host permitir. | Should | Escopo do projeto |
| RF-CHT-06 | O sistema deve permitir que o Host cadastre, edite e remova itens da FAQ (categorias-base sugeridas e itens livres, com limite de caracteres). | Must | Escopo do projeto |
| RF-CHT-07 | O sistema deve oferecer ao Host a fila de conversas escaladas, com o histórico completo e resposta em tempo real. | Must | Escopo do projeto |

### 3.9 Pagamento (PAG) — Mobile e Backend

| ID | Requisito | Prioridade | Origem |
| --- | --- | --- | --- |
| RF-PAG-01 | O sistema deve permitir cadastrar uma ou mais formas de pagamento, guardadas como token do gateway, e definir uma como padrão. | Must | Escopo do projeto |
| RF-PAG-02 | O sistema deve permitir pagar somente contratações "aguardando pagamento", cobrando o snapshot de preço menos o cupom aplicado, e, com pagamento aprovado, passar a contratação para "confirmada", notificar o prestador e emitir comprovante. | Must | Escopo do projeto |
| RF-PAG-03 | O sistema deve, na recusa do gateway, manter "aguardando pagamento" e permitir nova tentativa com outra forma de pagamento sem refazer a solicitação. | Must | Escopo do projeto |
| RF-PAG-04 | O sistema deve impedir cobrança em duplicidade da mesma contratação. | Must | DEF item 7 |
| RF-PAG-05 | O sistema deve permitir aplicar cupom de desconto, validando situação, vigência e limite de uso, e atualizar o total. | Should | Escopo do projeto |
| RF-PAG-06 | O sistema deve cobrar do tutor, ao fim do serviço, os custos extras registrados (pagamento complementar). | Should | Escopo do projeto |
| RF-PAG-07 | O sistema deve exibir ao tutor o extrato de pagamentos e estornos. | Should | Escopo do projeto |
| RF-PAG-08 | O sistema deve processar o estorno pelo gateway e, em caso de falha, registrá-lo como "com falha" sem declarar devolução inexistente, encaminhando-o ao Administrador. | Must | DA-09 |
| RF-PAG-09 | O sistema deve registrar cada pagamento, repasse, estorno e resolução manual como lançamento imutável (tipo, valor, partes, status, data e vínculos); correção gera novo lançamento vinculado. | Must | Escopo do projeto |

### 3.10 Financeiro do prestador (FIN) — Mobile e Backend

| ID | Requisito | Prioridade | Origem |
| --- | --- | --- | --- |
| RF-FIN-01 | O sistema deve exibir ao prestador o painel financeiro com valor bruto, taxa da plataforma, valor líquido e saldo disponível. | Should | Escopo do projeto |
| RF-FIN-02 | O sistema deve permitir que o prestador solicite o repasse do saldo disponível; o repasse nunca é automático, só inclui contratações concluídas e é bloqueado quando o valor pedido supera o saldo. | Should | Escopo do projeto |
| RF-FIN-03 | O sistema deve calcular o repasse descontando a taxa vigente na contratação e, em caso de falha, registrar o repasse como "com falha" sem transferir valor incorreto, encaminhando-o ao Administrador e notificando o prestador. | Should | DA-12 |
| RF-FIN-04 | O sistema deve reservar o saldo enquanto houver repasse em andamento ou com falha. | Should | Escopo do projeto |

### 3.11 Avaliações (AVA) — Mobile

| ID | Requisito | Prioridade | Origem |
| --- | --- | --- | --- |
| RF-AVA-01 | O sistema deve permitir avaliar contratação concluída com nota de 1 a 5 e comentário opcional, uma por contratação, permitindo apenas a edição da avaliação existente, exceto quando removida pela moderação. | Should | Escopo do projeto |
| RF-AVA-02 | O sistema deve exibir a nota média, o número de avaliações e a lista completa, desconsiderando avaliações ocultadas ou removidas pela moderação. | Should | Escopo do projeto |

### 3.12 Notificações (NOT) — Mobile

| ID | Requisito | Prioridade | Origem |
| --- | --- | --- | --- |
| RF-NOT-01 | O sistema deve notificar o usuário sobre mudanças de status da contratação, pagamentos, resultado da validação, penalidades aplicadas, revertidas ou encerradas, repasses com falha, mensagens escalonadas e incidentes (estes com prioridade). | Must | Escopo do projeto |
| RF-NOT-02 | O sistema deve oferecer a central de notificações com marcação de lida e direcionamento à tela relacionada. | Should | Escopo do projeto |

### 3.13 Comum a todos os perfis do app (COM) — Mobile

| ID | Requisito | Prioridade | Origem |
| --- | --- | --- | --- |
| RF-COM-01 | O sistema deve permitir denunciar usuário ou conteúdo (perfil, avaliação, foto ou mensagem) com motivo, gerando ocorrência e, se for conteúdo, item de moderação, com motivos de lista pré-definida, sem revelar o denunciante e sem aceitar denúncia repetida do mesmo item nem do próprio conteúdo. | Should | DA-02 |
| RF-COM-02 | O sistema deve exibir o aviso de penalidade com tipo, motivo e data de fim, registrar a ciência, mostrar a advertência após o login e notificar quando a penalidade for revertida ou encerrada. | Should | Escopo do projeto |

### 3.14 Área comercial pública (WEB) — Web

| ID | Requisito | Prioridade | Origem |
| --- | --- | --- | --- |
| RF-WEB-01 | O sistema deve exibir, sem autenticação, a página inicial com as seções Quem somos, Serviços (hospedagem, pet sitter, pet walker), Como funciona e Sobre o app. | Must | decisão 25/10 |
| RF-WEB-02 | O sistema deve publicar os Termos de Uso e a Política de Privacidade (telas 00D/00E). | Must | decisão PO 01/11 |
| RF-WEB-03 | O sistema deve identificar o sistema operacional do visitante e redirecionar à loja compatível, exibindo as opções de loja quando não identificar e o aviso "disponível em breve" enquanto o app não estiver publicado; os links das lojas devem ser configuráveis sem nova publicação do site. | Must | DA-05, DA-14 |
| RF-WEB-04 | O sistema não deve oferecer na Web busca, reserva, cadastro de usuário nem link para o login administrativo. | Must | DF-01/02 |
| RF-WEB-05 | O sistema deve exibir página de erro com retorno à página inicial para endereços inexistentes. | Should | Escopo do projeto |

### 3.15 Módulo administrativo (ADM) — Web

| ID | Requisito | Prioridade | Origem |
| --- | --- | --- | --- |
| RF-ADM-01 | O sistema deve autenticar o Administrador pela rota /admin, com contas criadas pelo seed, sem "esqueci minha senha", exibindo Acesso negado (403) a quem não tem o papel, negando conta administrativa inativa e exigindo novo login quando a sessão expirar. | Must | DF-03; DA-06/07 |
| RF-ADM-02 | O sistema deve exibir a fila de validação de prestadores, filtrável por tipo (novo, revalidação, reenvio) e papel, com os documentos enviados. | Must | Escopo do projeto |
| RF-ADM-03 | O sistema deve permitir aprovar ou rejeitar o cadastro, com feedback obrigatório na rejeição, indicando se é com reenvio ou definitiva (só por violação de política ou fraude), e impedir segunda decisão sobre o mesmo envio. | Must | DA-08, DA-16 |
| RF-ADM-04 | O sistema deve listar os usuários com filtros por papel e estado da conta e exibir detalhes com papéis, contratações, ocorrências e penalidades. | Should | Escopo do projeto |
| RF-ADM-05 | O sistema deve permitir reativar conta inativa, com motivo obrigatório; suspensão e banimento não são feitos nesta tela. | Should | DA-01 |
| RF-ADM-06 | O sistema deve listar as ocorrências com origem (denúncia ou incidente), status, gravidade informada e envolvidos, e permitir aplicar advertência, suspensão com prazo ou banimento com motivo, ou arquivar com justificativa. | Should | DA-02 |
| RF-ADM-07 | O sistema deve permitir definir a gravidade final de incidentes e registrar a análise de responsabilidade e custos de incidentes graves; custo atribuído ao tutor vira custo extra. | Should | DA-18/19 |
| RF-ADM-08 | O sistema deve listar as penalidades ativas, permitir revertê-las com motivo e encerrar automaticamente as suspensões na data de fim, restaurando a conta; o login já considera encerrada a suspensão vencida antes de a rotina rodar. | Should | DA-10 |
| RF-ADM-09 | O sistema deve oferecer a fila de moderação para manter, ocultar ou remover conteúdo com justificativa, mostrando no chat só a mensagem denunciada. | Should | DA-13 |
| RF-ADM-10 | O sistema deve permitir pedir acesso à conversa completa com justificativa, concedido somente por outro Administrador, e registrar o acesso. | Should | DA-15 |
| RF-ADM-11 | O sistema deve permitir criar, editar e inativar cupons com código único, tipo (percentual ou valor fixo), valor, vigência e limite de uso. | Should | DA-04 |
| RF-ADM-12 | O sistema deve permitir definir a taxa da plataforma (0 a 100%) com data de início de vigência e histórico, sem alterar contratações já solicitadas. | Should | DA-04 |
| RF-ADM-13 | O sistema deve permitir reprocessar estornos e repasses com falha ou registrar resolução manual com justificativa, sempre como novo lançamento; nova falha mantém o status e registra a tentativa. | Should | DA-09, DA-12 |
| RF-ADM-14 | O sistema deve exibir o dashboard com filas pendentes (validações, ocorrências, moderação, transações com falha) e indicadores do período, como destino após o login. | Should | Escopo do projeto |
| RF-ADM-15 | O sistema deve permitir consultar todas as contratações com filtros e ver o histórico de status e os pagamentos de cada uma. | Should | Escopo do projeto |
| RF-ADM-16 | O sistema deve exibir o financeiro da plataforma: totais do período, pagamentos, repasses, estornos e taxas, com os lançamentos vinculados. | Should | Escopo do projeto |
| RF-ADM-17 | O sistema deve permitir consultar a trilha de auditoria com filtros por ação, autor, usuário afetado e período, sem permitir edição. | Should | Escopo do projeto |
| RF-ADM-18 | O sistema deve permitir que o Administrador veja seus dados, altere a própria senha e saia, sem que o token anterior permita novas ações. | Should | Escopo do projeto |

4. Requisitos não funcionais
----------------------------

São 47 requisitos não funcionais em 7 categorias. As metas de desempenho (4.5) são propostas da PO e ainda não foram medidas.

### 4.1 Segurança (SEG)

| ID | Requisito | Prioridade | Origem |
| --- | --- | --- | --- |
| RNF-SEG-01 | Senhas devem ser armazenadas apenas como hash com algoritmo adaptativo (ex.: bcrypt ou Argon2), e a política de senha deve ser validada também no Auth Server, não só no app. | Must | COD (#252) |
| RNF-SEG-02 | PIN e códigos de recuperação devem ser guardados só como hash, ter uso único e expiração (recuperação: cerca de 10 min). | Must | Escopo do projeto |
| RNF-SEG-03 | A autenticação deve ser stateless por JWT com validade de 15 min, renovado por refresh token, carregando os papéis da conta. | Must | Glossário |
| RNF-SEG-04 | Todo endpoint do backend deve verificar o papel do usuário, com permissões granulares ligadas ao papel Administrador para permitir separar cargos depois; o papel Administrador é separado e não dá acesso ao app como prestador. | Must | DF-03 |
| RNF-SEG-05 | O login deve responder com mensagem genérica a credenciais inválidas e só verificar o estado da conta depois de credenciais válidas, sem revelar se o e-mail existe. | Must | Escopo do projeto |
| RNF-SEG-06 | Toda comunicação deve usar HTTPS/TLS, inclusive o WebSocket (wss). | Must | Boa prática; Política de Privacidade |
| RNF-SEG-07 | A plataforma não deve armazenar número completo de cartão nem CVV, apenas o token do gateway. | Must | DEF item 13 |
| RNF-SEG-08 | O backend deve repetir todas as validações feitas no app e na Web. | Must | Escopo do projeto |
| RNF-SEG-09 | A criação da conta e o consumo do PIN devem ser transacionais: se a criação falhar, o PIN não é consumido. | Must | Escopo do projeto |
| RNF-SEG-10 | O login administrativo deve ficar em rota separada (/admin), sem link na área pública. | Must | Escopo do projeto |
| RNF-SEG-11 | Chaves do gateway, do provedor de IA e demais segredos devem ficar fora do código-fonte (variáveis de ambiente ou cofre). | Should | Boa prática |

### 4.2 Privacidade e LGPD (PRI)

| ID | Requisito | Prioridade | Origem |
| --- | --- | --- | --- |
| RNF-PRI-01 | O prestador deve receber só os dados do tutor necessários ao serviço, liberados no dia do check-in ou do serviço (ver RF-EXE-12). | Must | DA-20; Política §4 |
| RNF-PRI-02 | Pet, conta, perfil de prestador e cupom devem usar inativação lógica, sem exclusão física, preservando o histórico. | Must | Decisão PO |
| RNF-PRI-03 | As ações administrativas devem gerar trilha de auditoria imutável com autor, data e motivo. | Must | Escopo do projeto |
| RNF-PRI-04 | A moderação de chat deve ver só a mensagem denunciada; a conversa completa só com liberação de outro Administrador, com acesso auditado. | Must | DA-13, DA-15 |
| RNF-PRI-05 | A identidade do denunciante não deve ser revelada ao denunciado. | Must | Glossário (Denúncia) |
| RNF-PRI-06 | A versão e a data do aceite dos Termos e da Política devem ser registradas, com novo aceite quando houver alteração relevante. | Should | Política §9 |
| RNF-PRI-07 | Os registros de acesso à aplicação devem ser guardados por 6 meses, sob sigilo. | Should | Política §6; Marco Civil art. 15 |
| RNF-PRI-08 | O tutor deve ser informado de que conversa com um Assistente de IA, e as mensagens enviadas à IA devem seguir a Política de Privacidade. | Should | Política §3–4 |
| RNF-PRI-09 | A plataforma deve atender aos direitos do titular (acesso, correção e eliminação), com prazos de retenção definidos por categoria. | Could | Política §6, §8 |

### 4.3 Arquitetura e regulamento (ARQ)

| ID | Requisito | Prioridade | Origem |
| --- | --- | --- | --- |
| RNF-ARQ-01 | O backend deve expor API REST stateless. | Must | Regulamento ETI 2026 |
| RNF-ARQ-02 | O app mobile e a Web devem ser construídos com HTML5 e JavaScript. | Must | Regulamento ETI 2026 |
| RNF-ARQ-03 | Ao menos uma funcionalidade deve usar WebSocket (chat entre tutor e Host com Assistente de IA); a configuração da FAQ e a fila de escalonamento não podem sair do escopo. | Must | Regulamento ETI 2026 |
| RNF-ARQ-04 | O pagamento deve ser integrado a um gateway em ambiente sandbox. | Must | Regulamento ETI 2026 |
| RNF-ARQ-05 | Cadastro com PIN, login e emissão de tokens devem ficar em um Auth Server separado do backend principal. | Must | ADR-011 |
| RNF-ARQ-06 | O sistema deve estar implantado e acessível para a apresentação à banca. | Must | Regulamento ETI 2026 |
| RNF-ARQ-07 | O código deve estar versionado no repositório oficial (Azure Repos, espelhado no GitHub hospetse). | Must | Regulamento; exceção aprovada |
| RNF-ARQ-08 | A API deve responder erros em formato padronizado (ex.: 422 com lista de campos; 403 com código do estado da conta). | Should | Escopo do projeto |

### 4.4 Confiabilidade (CON)

| ID | Requisito | Prioridade | Origem |
| --- | --- | --- | --- |
| RNF-CON-01 | Rotinas agendadas devem executar os cancelamentos automáticos por prazo e o fim automático das suspensões, contando dia útil como segunda a sexta, sem feriados. | Must | Escopo do projeto |
| RNF-CON-02 | O pagamento e a criação de solicitação devem ser atômicos e idempotentes (chave de idempotência): reenvios não geram nova cobrança nem solicitação duplicada ou parcial. | Must | Decisão PO |
| RNF-CON-03 | Se o registro financeiro falhar, o evento deve ser retido para nova tentativa e gerar alerta crítico de log, sem confirmar movimento sem registro. | Must | Escopo do projeto |
| RNF-CON-04 | O chat deve detectar queda por heartbeat, reconectar automaticamente com backoff e indicador de status, e manter em fila as mensagens não entregues, reenviadas na ordem e sem duplicar (ID de mensagem). | Should | Escopo do projeto |
| RNF-CON-05 | A verificação de vaga deve ser feita no envio e no aceite, de modo que duas solicitações simultâneas não ocupem a mesma última vaga. | Must | COD |

### 4.5 Desempenho (DES) — metas propostas

| ID | Requisito | Prioridade | Origem |
| --- | --- | --- | --- |
| RNF-DES-01 | Mensagens de chat devem chegar ao destinatário em até 2 s em condições normais de rede. | Should | Proposta (a confirmar) |
| RNF-DES-02 | A busca deve retornar os resultados em até 3 s. | Should | Proposta (a confirmar) |
| RNF-DES-03 | A notificação de incidente deve chegar ao tutor em até 1 min do registro. | Should | Proposta (a confirmar) |

### 4.6 Usabilidade (USA)

| ID | Requisito | Prioridade | Origem |
| --- | --- | --- | --- |
| RNF-USA-01 | Formulários devem destacar todos os campos inválidos de uma vez, com mensagem específica por campo. | Must | Escopo do projeto |
| RNF-USA-02 | Telas, mensagens e API devem usar os termos oficiais do Glossário v1.0. | Must | Glossário §2 |
| RNF-USA-03 | A interface deve estar em português do Brasil, com datas dd/mm/aaaa e valores em R$. | Must | Escopo do projeto |
| RNF-USA-04 | As telas devem seguir o Figma "Hospetse — Design Telas" e reutilizar os componentes compartilhados de prestador (prefixo PR). | Should | FIG |
| RNF-USA-05 | A área pública da Web deve ser responsiva para computador e celular. | Should | Escopo do projeto |
| RNF-USA-06 | As telas devem ter contraste adequado e texto alternativo em imagens, tendo a WCAG 2.1 AA como referência. | Could | Boa prática |

### 4.7 Qualidade e processo (MAN)

| ID | Requisito | Prioridade | Origem |
| --- | --- | --- | --- |
| RNF-MAN-01 | Cada PBI só é concluído com testes automatizados, conforme a DoD. | Must | Decisão 24/10; regulamento |
| RNF-MAN-02 | As regras críticas (estados da contratação, prazos, compatibilidade, vagas, taxa e repasse) devem ter testes unitários. | Must | DoD |
| RNF-MAN-03 | A documentação das APIs deve ficar publicada na Wiki › Desenvolvimento. | Should | Wiki (API) |
| RNF-MAN-04 | O processo deve seguir Scrum, com backlog e artefatos no Azure DevOps. | Must | Exceção aprovada pela coordenação |

5. Regras de negócio transversais
---------------------------------

Estas 18 regras valem para vários requisitos ao mesmo tempo; quando um requisito e uma regra parecerem divergir, vale a regra.

| ID | Regra | Requisitos afetados | Origem |
| --- | --- | --- | --- |
| RN-01 | O pagamento só ocorre depois do aceite do prestador. | RF-CON-11, RF-PAG-02 | Decisão PO |
| RN-02 | O prestador tem 1 dia útil para aceitar e o tutor tem 1 dia útil após o aceite para pagar; vencido o prazo, cancelamento automático. | RF-CON-13, RF-CON-14, RNF-CON-01 | DA-17; decisão PO |
| RN-03 | Cancelamento com mais de 48 h de antecedência é gratuito, com estorno integral; fora disso o estorno pode ser integral ou parcial. Não existe multa. | RF-CON-15, RF-PAG-08 | DA-11; decisão PO |
| RN-04 | Vale a taxa da plataforma vigente no momento da solicitação; nova taxa não altera contratações já feitas. | RF-CON-06, RF-ADM-12, RF-FIN-03 | decisão PO |
| RN-05 | O repasse só acontece por solicitação do prestador e só inclui contratações concluídas. | RF-FIN-02, RF-FIN-03 | Escopo do projeto |
| RN-06 | Contratar exige perfil de Tutor completo, conta sem restrição e pet ativo; buscar não exige pet. | RF-TUT-03, RF-BUS-05, RF-CON-02/03 | Decisões 29/10 e 01/11 |
| RN-07 | Na busca só aparecem prestadores com cadastro aprovado, perfil ativo e conta ativa. O seed cria Hosts já validados. | RF-BUS-04, RF-ADM-03 | decisão 27/10 |
| RN-08 | Rejeição definitiva só por violação de política definida ou fraude; nos demais casos o prestador pode corrigir e reenviar (48 h para documento específico). | RF-ADM-03, RF-PRE-07 | DA-08, DA-16 |
| RN-09 | Penalidades são advertência, suspensão com prazo e banimento, sempre com motivo. Suspensão e banimento só por penalidade; a gestão de usuários só reativa. | RF-ADM-05, RF-ADM-06, RF-ADM-08, RF-ACE-05 | DA-01, DA-10 |
| RN-10 | Uma avaliação por contratação concluída; só é permitido editar a existente. | RF-AVA-01 | Escopo do projeto |
| RN-11 | O prestador classifica a gravidade do incidente; o Administrador define a gravidade final. Até caso médio, o prestador alerta e espera o tutor decidir; em caso grave, segue o procedimento padrão e o caso vai para análise de responsabilidade e custos. | RF-EXE-09, RF-ADM-07 | DA-18 |
| RN-12 | Atendimento veterinário é contratação adicional; em caso médio exige autorização do tutor, que arca com os custos quando a responsabilidade não é do prestador. | RF-EXE-10, RF-PAG-06 | DA-19 |
| RN-13 | Administrar medicamentos é serviço opcional do prestador; ele assume a responsabilidade no aceite e cada aplicação é registrada. | RF-CON-08, RF-CON-12, RF-EXE-04, RF-PRE-05 | DA-21 |
| RN-14 | Endereço, telefone e instruções de acesso do tutor só são liberados ao prestador no dia do check-in ou do serviço, e só o necessário. | RF-EXE-12, RNF-PRI-01 | DA-20, DA-22 |
| RN-15 | Antes da solicitação, o contato com o Host é só pelo Assistente de IA, salvo quando o Host libera o atendimento humano (opt-in). | RF-CHT-02, RF-CHT-04 | Decisões 25/10 e 01/11 |
| RN-16 | Denúncias são analisadas em até 5 dias úteis. | RF-COM-01, RF-ADM-06 | DEF item 12 |
| RN-17 | Lançamentos financeiros e registros de auditoria são imutáveis; correção gera novo lançamento vinculado. | RF-PAG-09, RF-ADM-13, RNF-PRI-03 | Escopo do projeto |
| RN-18 | A hospedagem exige pelo menos 1 registro de atividade por dia entre check-in e check-out. | RF-EXE-02, RF-EXE-03 | Escopo do projeto |