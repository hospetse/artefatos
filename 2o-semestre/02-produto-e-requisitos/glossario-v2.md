Glossário de Termos do Negócio — HOSPETSE V2
============================================

| **Campo** | **Conteúdo** |
| --- | --- |
| **Projeto** | HOSPETSE — Plataforma de Hospedagem e Serviços para Pets |
| **Artefato** | Glossário de Termos do Negócio (artefato do regulamento, citado na DoD de Gestão) |
| **Work item** | #504 HSP-PBI-079-TSK-06 — PBI pai #245 HSP-PBI-079 |
| **Versão** | 2.0 |
| **Data** | 02/11/2026 |
| **Responsável** | Brenda (Product Owner) |
| **Status** | Em validação |



1. Objetivo
-----------

Reunir num só lugar o vocabulário do negócio do HOSPETSE, com **uma única definição por termo**, para que requisitos, casos de uso, User Stories, telas, API, banco de dados e testes usem as mesmas palavras com o mesmo significado.

2. Como usar este glossário
---------------------------

*   **Termo oficial** é o nome que deve aparecer nos artefatos, nas telas e nas mensagens ao usuário.
*   **Evitar** lista os sinônimos encontrados no projeto que não devem mais ser usados com esse sentido. Quando um sinônimo continuar valendo só como nome técnico (código, tabela ou enum), isso está indicado.
*   **Módulo** indica onde o termo vale: Tutor, Host, Pet Sitter, Pet Walker, Comum (todos os perfis do app), Web pública, Admin (Web) ou Back-end.
*   **Regras e estados** traz o comportamento ligado ao termo, de forma resumida. A regra completa está na fonte.
*   **Fonte** aponta de onde a definição vem (ver legenda na seção 3).
*   Quando uma fonte diverge de outra, vale a ordem de precedência da seção 3.

3. Fontes e precedência
-----------------------

| **Sigla** | **Fonte** | **Versão usada** |
| --- | --- | --- |
| **DF / DA** | Decisões da PO sobre o fluxo Web e o projeto (DF-01 a DF-03, DA-01 a DA-22) e decisões de priorização de 24/10, 25/10 e 27/10/2026 | out–nov/2026 |
| **REQ** | Documento de Requisitos Funcionais e Não Funcionais HOSPETSE — Revisão v2 | 02/11/2026 |
| **UC** | Casos de Uso HOSPETSE — Revisão v2 (HPETxx) | 01/11/2026 |
| **US** | User Stories Detalhadas — Revisão v2 (US-HPETxx-nn) | 01/11/2026 |
| **TERM** | Termos de Uso (Host, Pet Sitter, Pet Walker) e Política de Privacidade | out/2026 |
| **PWEB / PTUT** | Planilha de Fluxo Web Administrativo v4 e Planilha de Fluxo do Tutor Mobile. | out/2026 |
| **DER / ADR / API** | Wiki › Desenvolvimento: Banco de Dados (DER v3.1), Arquitetura (ADR-001 a 015) e APIs publicadas | out/2026 |
| **FIG** | Figma — Hospetse Design Telas | 01/11/2026 |
| **TAP / DoD** | Termo de Abertura do Projeto e Definition of Done (Wiki › Gestão) | ago–out/2026 |
| **COD** | Código no GitHub hospetse (espelho do Azure Repos), branch develop: hospetse-backend (schema Prisma, policies e services), hospetse-auth-service e hospetse-mobile (constantes, validações e memória técnica DT-01 a DT-18) | 01/11/2026 |
| **GH-MVP** | GitHub hospetse/artefatos — documentação do MVP da fase 1 (UC03 e UC04), plano de testes, DER e diagrama de classes. Histórico: usado só para identificar termos legados. | jun/2026 |

**Precedência:** DF/DA › REQ › UC v2 › US v2 › TERM › planilhas › DER/ADR/COD › FIG › GH-MVP. O código mostra o que está implementado; quando diverge das decisões, a divergência vai para a seção 13 e vale a decisão. Os Termos de Uso e a Política seguem a regra registrada em "Precedência e sobreposições com as Definições" (01/11/2026).

4. Perfis, papéis e identidade
------------------------------

| **Termo oficial** | **Definição** | **Evitar** | **Módulo** | **Regras e estados** | **Fonte** |
| --- | --- | --- | --- | --- | --- |
| **Visitante** | Pessoa que acessa a área comercial pública da Web ou inicia o cadastro no app sem estar autenticada. | Lead, usuário anônimo | Web pública, Comum | Não acessa busca, reserva nem cadastro pela Web; esses fluxos são só do app. | UC HPET34/35; US-HPET01-01 |
| **Usuário do app** | Pessoa com conta na plataforma que usa o aplicativo mobile, com um ou mais papéis. | Cliente | Comum | Sempre tem o papel Tutor; pode somar papéis de prestador. | UC HPET01a |
| **Conta** | Identidade única de uma pessoa na plataforma, ligada a um e-mail. Guarda credenciais, estado da conta e papéis. | Cadastro (no sentido de identidade), login | Comum | Um e-mail = uma conta. Estados: ver **Estado da conta**. | US-HPET01-01; ADR-011; DA-01 |
| **Papel** | Capacidade atribuída a uma conta: Tutor, Host, Pet Sitter, Pet Walker ou Administrador. | Função, tipo de usuário | Comum, Admin | Modelo aditivo para os papéis do app. O Administrador é um papel separado, sem acesso ao app como prestador. | UC HPET01a, HPET20a; DF-03 |
| **Papel aditivo** | Regra pela qual todo usuário do app começa como Tutor e pode acrescentar papéis de prestador, sem perder o papel Tutor. | Troca de perfil | Comum | Um prestador por conta. Host, Pet Sitter e Pet Walker são acrescentados, não substituídos. | US-HPET10-01; DER (providers) |
| **Perfil** | Conjunto de dados de um papel dentro da conta, como o perfil de Tutor ou o perfil de prestador. | Conta (quando se fala dos dados do papel) | Comum | Perfil de Tutor: **incompleto / completo**. Perfil de prestador: **estado de validação** e **ativo / inativo**. | US-HPET01-03; UC HPET10 |
| **Tutor** | Responsável pelo pet, que busca, contrata e acompanha os serviços. É o papel inicial de toda conta do app. | Cliente, dono, hóspede, usuário final | Tutor | Precisa de **perfil completo** e de **pet ativo** para contratar. Deve ser maior de 18 anos. | UC HPET01–09; US-HPET01-03 |
| **Prestador** | Usuário que oferece serviço pela plataforma: Host, Pet Sitter ou Pet Walker. | Profissional, parceiro, provider (só no código) | Host, Pet Sitter, Pet Walker | Precisa de cadastro aprovado pelo Administrador e conta ativa para aparecer na busca. | UC HPET12e, HPET20b; DER |
| **Host** | Prestador que oferece **hospedagem** de pets em sua residência ou estabelecimento. | Anfitrião, hotel, hotel pet, pet hotel (no código o tipo é hotel) | Host | Define capacidade, espécies e portes aceitos, agenda e preços (diária). | UC HPET10–12f; TERM Host; FIG |
| **Pet Sitter** | Prestador que faz **visitas** de cuidado ao pet na residência do tutor. | Petsitter, cuidador, pet sitting | Pet Sitter | Define área de atuação e disponibilidade. | UC HPET16–17b |
| **Pet Walker** | Prestador que faz **passeios** com o pet. | Petwalker, walker, passeador | Pet Walker | Define região de atuação e horários. | UC HPET18–19b |
| **Administrador** | Papel único de gestão da plataforma no módulo administrativo Web. | Back-office, Moderador, operador, analista, admin (na interface) | Admin | Papel único (DF-03). Contas criadas e senhas recuperadas pelo **seed**; acesso pela rota **/admin**. Separação futura de cargos por permissões no back-end. | DF-03; DA-06/07; UC HPET20a |
| **Sistema de Pagamento** | Ator de backend que processa pagamentos, repasses e estornos com o gateway e registra as transações. | — | Backend | Não tem interface própria. | UC HPET26–28a |
| **Assistente de IA** | Ator de sistema que responde ao tutor no chat com base na FAQ do Host. | Bot, chatbot, IA do hotel | Tutor, Host | Escalona para atendimento humano quando não sabe responder ou quando o tutor pede. | UC HPET06, 06b, 06c |
| **Equipe HOSPETSE** | Responsável pela plataforma neste projeto: Ana Clara, Amanda, Brenda, Lucas, Raul e Murilo. | Pessoa jurídica responsável | — | Não há CNPJ; projeto acadêmico. | TAP |

5. Conta, acesso e cadastro
---------------------------

| **Termo oficial** | **Definição** | **Evitar** | **Módulo** | **Regras e estados** | **Fonte** |
| --- | --- | --- | --- | --- | --- |
| **Cadastro inicial** | Criação da conta com nome, e-mail, senha e aceite dos Termos e da Política, confirmada por PIN enviado ao e-mail. | Registro, sign-up (na interface) | Comum | Só e-mail + PIN. A conta só existe após PIN e dados validados. | US-HPET01-01; ADR-011; decisão 24/10 |
| **PIN** | Código de confirmação enviado ao e-mail no cadastro. | Token, código de verificação (genérico) | Comum | Uso único, com validade. Guardado só como hash. No máximo 5 tentativas por código; novo pedido só depois de 1 minuto. PIN inválido, expirado ou usado não cria conta. | US-HPET01-01; DER (email_verification); COD (auth-service, mobile) |
| **Cadastro não concluído** | Conta criada no Auth Server cujo registro de usuário ainda não foi confirmado pelo back-end principal. | Cadastro pela metade, conta pendente | Comum | O login responde que o cadastro não foi concluído e pede um novo PIN para terminar. No código: pending_provisioning. | ADR-011; API; COD (auth-service, DT-13) |
| **Política de senha** | Regras mínimas que a senha precisa cumprir. | Senha forte | Comum | Hoje só o app valida (isStrongPassword); o Auth Server ainda não tem política (#252). | US-HPET01-01; COD (mobile DT-14) |
| **Perfil incompleto** | Perfil de Tutor sem todos os dados obrigatórios. | Cadastro incompleto | Tutor | Pode autenticar, navegar e buscar; **não pode contratar**. | US-HPET01-01/03; DER (incomplete) |
| **Perfil completo** | Perfil de Tutor com CPF, data de nascimento, telefone, endereço, contato de emergência e demais dados obrigatórios válidos. | Cadastro completo | Tutor | Libera a contratação. **Telefone é obrigatório**. CPF deve ser válido e único na plataforma. | US-HPET01-03; API (canContract); COD (profile-policy.js); decisão PO 01/11/2026; REQ RF-TUT-01 |
| **Pode contratar** | Situação do usuário que tem perfil de Tutor completo e conta sem restrição. | Liberado para contratar | Tutor | Conta restrita é verificada antes do perfil. | US-HPET01-03; API; COD (profile-policy.js) |
| **Contato de emergência** | Pessoa indicada pelo tutor para ser acionada em emergência quando ele não responder. | — | Tutor | Faz parte do perfil completo. | US-HPET01-03; DEF item 9 |
| **Maioridade** | Exigência de ter mais de 18 anos, verificada pela data de nascimento. | — | Comum | Vale para Tutor, Host, Pet Sitter e Pet Walker. | DEF item 2; Precedência TERM |
| **Aceite dos Termos e da Política** | Concordância obrigatória com os Termos de Uso e a Política de Privacidade antes da criação da conta. | — | Comum | Registrado com data e versão dos termos (versão atual no app: 2026-10-01). | US-HPET01-01; DER (terms_version); COD (mobile terms.ts) |
| **Autenticação (login)** | Validação de e-mail e senha e do estado da conta para liberar o acesso conforme os papéis. | Logon | Comum, Admin | Mensagem genérica para credencial inválida. Conta suspensa, banida ou inativa não entra. Bloqueio temporário de 30 minutos após 5 erros seguidos. | UC HPET01a, 20a; US-HPET01A-02; FIG; decisão PO |
| **Recuperação de senha** | Redefinição da senha do usuário do app por código ou link de uso único. | — | Comum | Código ou link válido por cerca de 10 min; a senha anterior deixa de valer. Não muda o estado da conta. Não existe para o Administrador (feita pelo seed). | UC HPET01b; DA-07; REQ RF-ACE-08 |
| **Sessão expirada** | Situação em que o token de acesso venceu e o usuário precisa entrar de novo. | Sessão caída, logout automático | Comum, Admin | Exige novo login. A ação em andamento não é aplicada. No Admin, exibe a tela 01E. | REQ RF-ADM-01, RNF-SEG-03 |
| **Estado da conta** | Situação da conta na máquina de estados única: **ativa**, **suspensa (com prazo)**, **banida** ou **inativa**. | Status do usuário, bloqueada, pendente | Comum, Admin | Suspensão e banimento só por penalidade. Advertência não muda o estado. | DA-01; UC HPET01a, 20, 25 |
| **Conta ativa** | Conta que pode acessar a plataforma normalmente. | Liberada | Comum | Estado padrão após o cadastro. | DA-01 |
| **Conta suspensa** | Conta bloqueada temporariamente por penalidade de suspensão, com motivo e data de fim. | Bloqueada, congelada | Comum | Login negado com motivo e data de fim. Volta a ativa no fim automático ou por reversão. | DA-01/10; UC HPET25a |
| **Conta banida** | Conta bloqueada definitivamente por penalidade de banimento. | Excluída, encerrada definitivamente | Comum | Login negado. Só volta por reversão da penalidade. | DA-01; UC HPET25, 25a |
| **Conta inativa** | Conta desativada por inativação lógica, pelo próprio usuário ou registrada assim. | Excluída, deletada | Comum | Dados e histórico preservados. Reativação feita pelo Administrador (HPET20). | UC HPET39, 20 |
| **Inativar própria conta** | Ação do usuário que leva a conta ao estado inativa. | Excluir conta | Comum | Bloqueada se houver contratação ativa ou futura. Avisa saldo a receber. | UC HPET39 |
| **Reativação de conta** | Ação do Administrador que devolve uma conta inativa ao estado ativa, com motivo. | Desbloquear | Admin | Única ação de estado da gestão de usuários. Registrada na auditoria. | UC HPET20; DA-01 |
| **Inativação lógica** | Desativação de um registro sem exclusão física, preservando dados e histórico. | Exclusão, deletar, remover, soft delete (só técnico) | Comum | Vale para pet, conta, perfil de Host e cupom. | Decisão PO; UC HPET02b, 39 |
| **Seed** | Carga inicial de dados que cria as contas administrativas e Hosts já validados para testes. | — | Admin, Backend | Recupera a senha do Administrador. O seed cria Hosts já validados (27/10/2026). | DA-06/07; decisão 27/10 |

6. Pet
------

| **Termo oficial** | **Definição** | **Evitar** | **Módulo** | **Regras e estados** | **Fonte** |
| --- | --- | --- | --- | --- | --- |
| **Pet** | Animal doméstico cadastrado pelo tutor para usar os serviços. | Animal (na interface), hóspede pet | Tutor | Pertence a um tutor. Estados: **ativo / inativo**. | UC HPET02 |
| **Pet ativo** | Pet disponível para seleção na busca e na contratação. | — | Tutor | Exigido para solicitar reserva ou serviço. Tutor sem pet ativo vê aviso (tela 08I). | UC HPET03–05; US-HPET03-03 |
| **Inativação do pet** | Inativação lógica do pet, que sai da busca e da contratação, mantendo o histórico. | Excluir pet, remover pet | Tutor | Bloqueada se houver contratação ativa ou futura. Tela 29A renomeada. | UC HPET02b; US-HPET02B-01 |
| **Espécie** | Tipo de animal do pet, escolhido em lista pré-definida. | Tipo de pet | Tutor, Host | Valores atuais: cão e gato (provisório). Aplicado como filtro e na compatibilidade. | US-HPET02-01; DER (#369) |
| **Porte** | Tamanho do pet, escolhido em lista pré-definida. | Tamanho | Tutor, Host | Valores atuais: pequeno, médio, grande. Faixas de peso do protótipo (até 10 kg, 10–20 kg, +20 kg). | US-HPET02-01; DER; FIG |
| **Raça** | Raça do pet, informada no cadastro. | — | Tutor | Pode ser usada como filtro de busca (29/10/2026). | Decisão PO 29/10; DER |
| **Informações médicas do pet** | Vacinas, alergias, medicações e observações de saúde no cadastro do pet. | Histórico médico | Tutor | Base para o cuidado; não substitui os medicamentos informados na solicitação. | UC HPET02 |
| **Características comportamentais** | Descrição do comportamento do pet (sociabilidade, ansiedade etc.). | Temperamento | Tutor | Informadas no cadastro do pet. | UC HPET02 |
| **Compatibilidade** | Verificação de que a espécie e o porte do pet são aceitos pelo prestador. | — | Tutor, Host | Pet incompatível bloqueia a solicitação ("Pet não permitido"). Validada no backend. | US-HPET04-03 |
| **Pet como parâmetro da busca** | Uso do pet selecionado para aplicar espécie e porte como filtros da busca. | — | Tutor | Pet cadastrado não é obrigatório para buscar, só para contratar. | Decisão PO 29/10; UC HPET03 |

7. Oferta do prestador
----------------------

| **Termo oficial** | **Definição** | **Evitar** | **Módulo** | **Regras e estados** | **Fonte** |
| --- | --- | --- | --- | --- | --- |
| **Cadastro de prestador** | Envio dos dados, documentos e fotos para atuar como Host, Pet Sitter ou Pet Walker. | Credenciamento, homologação | Host, Pet Sitter, Pet Walker | Entra como **pendente de validação**. Documentos exigidos. | UC HPET10, 16, 18; DEF item 2 |
| **Tipo de pessoa** | Indica se o prestador se cadastra como pessoa física (CPF) ou jurídica (CNPJ). | PF/PJ | Host, Pet Sitter, Pet Walker | O documento exigido muda conforme o tipo. Lista de documentos por tipo. | GH-MVP (diagrama de classes); DER (cpf_cnpj) |
| **Capacidade** | Quantidade máxima de pets que o Host atende ao mesmo tempo. | Vagas, lotação | Host | Definida no perfil. Hoje, Host sem capacidade cadastrada atende 1 pet por vez (provisório; o cadastro deve exigir). Critérios mínimos do ambiente de hospedagem. | UC HPET10; DEF itens 4–5; COD (availability.js) |
| **Vaga** | Lugar livre para mais um pet num dia, considerando a capacidade do Host e as contratações que ocupam o dia. | — | Host, Tutor | Ocupam vaga as contratações aguardando pagamento, confirmadas e em andamento. A que está aguardando aceite **não ocupa**. O dia de saída não conta. Um pet não pode estar em duas hospedagens no mesmo período. | COD (availability.js, HSP-DEC-004) |
| **Situação do dia na agenda** | Estado de cada dia na agenda do Host: **livre**, **ocupado** (sem vaga) ou **bloqueado**. | Disponível, indisponível | Host | Bloqueado prevalece sobre ocupado. Dia bloqueado não recebe solicitação nova, e bloqueio feito depois impede o aceite. | COD (agenda.service.js) |
| **Host elegível** | Host que pode aparecer na busca e receber solicitações: perfil de hospedagem, ativo e com cadastro aprovado. | — | Host, Tutor | Pendente, rejeitado ou inativo não aparece nem recebe solicitação. | UC HPET03; COD (host-policy.js) |
| **Espécies e portes aceitos** | Lista de espécies e portes que o prestador atende. | Restrições do host | Host, Pet Sitter, Pet Walker | Lista vazia = aceita todos. Base da compatibilidade. | US-HPET10-01; DER |
| **Área de atuação** | Região (raio ou bairros) em que o Pet Sitter ou o Pet Walker atende. | Raio, região de atendimento | Pet Sitter, Pet Walker | Usada para filtrar as solicitações. | UC HPET16b, 18b |
| **Agenda** | Calendário do prestador com datas ou horários disponíveis, bloqueados e ocupados. | Calendário, disponibilidade (como tela) | Host, Pet Sitter, Pet Walker | Atualizada automaticamente no aceite. Base da verificação de disponibilidade. | UC HPET11, 16b, 18b |
| **Bloqueio de agenda** | Marcação manual de período indisponível pelo prestador. | Data bloqueada | Host | Período com contratação confirmada não pode ser bloqueado sem tratá-la. | US-HPET11-02; DER |
| **Disponibilidade** | Situação de um período livre na agenda para nova contratação. | — | Comum | Verificada de novo no envio da solicitação. | US-HPET04-04 |
| **Tabela de preços** | Valores que o prestador cobra por serviço (e porte, no Host). | Tarifário | Host | Existe só para o Host. Pet Sitter e Pet Walker a definir. | UC HPET11a; questão aberta |
| **Diária** | Unidade de preço da hospedagem, por dia. | Noite, estadia | Host | Snapshot da diária registrado na solicitação. O período vai do dia de entrada até o dia de saída, sem contar o dia de saída. | DER (daily_price); FIG; COD |
| **Unidade de cobrança** | Base sobre a qual o preço do serviço é calculado: **diária** (hospedagem), **visita** (Pet Sitter) e **passeio** (Pet Walker). | /noite, /caminhada, hora | Host, Pet Sitter, Pet Walker | O MVP previa também cobrança por hora. Unidade oficial de Pet Sitter e Pet Walker. | GH-MVP (enum Unidade: DIARIA, HORA, PASSEIO); FIG |
| **Tipo de acomodação** | Categoria do local de hospedagem do Host: hotel, casa, apartamento ou chácara. | Categoria do local | Host, Tutor | filtro no MVP e aparece no Figma. | GH-MVP; FIG |
| **Comodidades** | Recursos do local de hospedagem (área verde, piscina, climatização etc.) exibidos no perfil do Host. | Amenidades | Host, Tutor | o app já filtra por área verde, "apenas castrados" e "aceita medicação oral" com dados de exemplo (o Host precisa ter todas as escolhidas). | GH-MVP; FIG; US-HPET06B-01; COD (mobile DT-16/17) |
| **Requisitos de admissão** | Condições que o Host exige do pet para aceitá-lo (ex.: vacinação em dia, controle de pulgas, pet sociável, castrado). | Restrições do host, regras da casa | Host, Tutor | Hoje a compatibilidade cobre só espécie e porte. Requisitos extras como bloqueio da solicitação a incluir. | GH-MVP (FA03 "restrições específicas definidas pelo host"); FIG |
| **Meia diária** | Hospedagem só durante o dia (ex.: horário de trabalho do tutor). | Creche, day care | Host | Aparece na descrição do projeto e no protótipo ("creche"). | Descrição do projeto; FIG |
| **Serviço adicional** | Serviço extra oferecido junto à contratação (banho, tosa, passeio extra, transporte, adestramento, atendimento veterinário). | Opcional, upgrade | Host | Escolhido na solicitação. Veterinário entra como contratação adicional. | UC HPET04; US-HPET06B-01; DA-19 |
| **Administração de medicamentos** | Serviço opcional em que o prestador aceita dar ao pet os medicamentos informados pelo tutor. | Medicação (como serviço) | Host, Pet Sitter, Pet Walker | O prestador ativa ou desativa no perfil. Se desativado, a opção não aparece na solicitação. Busca esconder quem não oferece. | DA-21; UC HPET10a, 16a, 18a |
| **Validação do cadastro** | Análise do cadastro do prestador pelo Administrador, que aprova ou rejeita. | Credenciamento, homologação, triagem, aprovação cadastral | Admin | Estados: **pendente de validação → aprovado** ou **rejeitado**. Só aprovado aparece na busca. | UC HPET20b; DER |
| **Pendente de validação** | Estado do cadastro de prestador enviado e ainda não decidido. | Aguardando análise, em análise | Host, Pet Sitter, Pet Walker, Admin | O perfil não aparece na busca nem recebe solicitações. | UC HPET10, 37 |
| **Fila de validação** | Lista de cadastros pendentes no painel do Administrador, filtrável por tipo e papel. | Fila de triagem, fila de homologação | Admin | Tipos: **novo**, **revalidação**, **reenvio**. | UC HPET20b; PWEB (03H) |
| **Revalidação** | Nova validação exigida quando o prestador altera documentos ou dados sujeitos a validação. | — | Host, Pet Sitter, Pet Walker | O perfil sai da busca até nova aprovação. | UC HPET10a, 16a, 18a |
| **Rejeição com feedback** | Decisão do Administrador que recusa o cadastro informando os itens a ajustar. | Reprovação | Admin | Feedback obrigatório. Pode ser **com reenvio** ou **definitiva**. | DA-08, DA-16; UC HPET20b |
| **Rejeição definitiva** | Rejeição sem possibilidade de reenvio. | Banimento (não é penalidade) | Admin | Só quando o cadastro fere política definida ou há fraude. | DA-16 |
| **Reenvio** | Novo envio do cadastro rejeitado depois de corrigido pelo prestador. | Resubmissão | Host, Pet Sitter, Pet Walker | Volta a pendente de validação, marcado como reenvio. Quando o Administrador pede um documento específico, o prestador tem **48 horas** para reenviá-lo. | UC HPET37; FIG; decisão PO 01/11/2026 |
| **Inativação do perfil de Host** | Inativação lógica do perfil de prestador, que sai da busca. | Excluir perfil | Host | Tratamento com contratações futuras. (US pede alerta). | UC HPET10b; US-HPET10B-01 |

8. Busca e contratação
----------------------

| **Termo oficial** | **Definição** | **Evitar** | **Módulo** | **Regras e estados** | **Fonte** |
| --- | --- | --- | --- | --- | --- |
| **Busca** | Pesquisa de prestadores por localização, preço, disponibilidade, porte, raça e pet selecionado. | Pesquisa, vitrine, catálogo | Tutor | Mostra só prestadores **aprovados** e com **conta ativa**. Filtros de cidade e preço mantidos. Com pet selecionado, espécie, porte e raça vêm dele (filtro manual junto é recusado). Resultados paginados e ordenáveis por preço, distância e avaliação; a ordem padrão é a menor diária. | UC HPET03, 05; decisões 24/10 e 29/10; COD (host-search-policy.js); REQ RF-BUS-06 |
| **Contratação** | Termo geral para um serviço contratado entre tutor e prestador (hospedagem, visita ou passeio), do pedido até o encerramento. | Agendamento, pedido, booking | Comum, Admin | Ciclo de vida: ver **Estados da contratação**. É o nome usado na Web admin (HPET45). | UC HPET45; DA (WJ08) |
| **Reserva** | Contratação de **hospedagem** com um Host. | Agendamento, estadia, booking | Tutor, Host | Segue os estados da contratação. | UC HPET04, 12–12d |
| **Solicitação de serviço** | Contratação de **visita** (Pet Sitter) ou de **passeio** (Pet Walker). | Agendamento, pedido | Tutor, Pet Sitter, Pet Walker | Segue os estados da contratação. | UC HPET05, 17, 19 |
| **Solicitação** | Pedido enviado pelo tutor ao prestador, enquanto aguarda aceite. Também é o nome da ação ("solicitar reserva", "solicitar serviço"). | Proposta, pedido | Tutor, prestadores | Depois do aceite, o objeto passa a ser chamado de **reserva** ou **solicitação de serviço**. | UC HPET04, 05, 12 |
| **Tipo de serviço** | Classificação da contratação: **hospedagem**, **visita** ou **passeio**. | Categoria, modalidade, Pet Hotel (como tipo) | Comum, Admin | Define qual prestador atende e qual fluxo de execução vale. | UC HPET05, 45; GH-MVP (enum TipoServico) |
| **Observações da solicitação** | Texto livre do tutor com informações complementares para o prestador, enviado com a solicitação. | Notas, comentários | Tutor | Opcional. Não substitui os medicamentos da solicitação nem as instruções de acesso. | GH-MVP (UC04); DER (reservations.notes) |
| **Horário de check-in e check-out** | Horário a partir do qual o Host recebe o pet e até o qual deve devolvê-lo. | — | Host, Tutor | Check-in e check-out definido no momento da contratação. | GH-MVP; FIG |
| **Hospedagem** | Serviço em que o pet fica na residência ou estabelecimento do Host por um período, do check-in ao check-out. | Estadia, hotelzinho | Host | Exige pelo menos 1 registro de atividade por dia. | UC HPET04, 12b–12d |
| **Visita** | Serviço do Pet Sitter na residência do tutor, com início e fim registrados. | Cuidado domiciliar (como nome), pet sitting | Pet Sitter | Pode exigir instruções de acesso à residência. | UC HPET05, 17–17b |
| **Passeio** | Serviço do Pet Walker de levar o pet para passear, com início, fim e trajeto registrados. | Caminhada | Pet Walker | Duração e trajeto calculados pelo sistema. | UC HPET05, 19–19b; US-HPET19B-01 |
| **Estados da contratação** | Ciclo único: **aguardando aceite → aguardando pagamento → confirmada → em andamento → concluída**, além de **recusada** e **cancelada**. | Pendente, aceita, agendada, finalizada | Comum | Pagamento só depois do aceite. Recusa do gateway mantém aguardando pagamento. | DEF item 6; UC HPET04, 07a, 12a |
| **Aguardando aceite** | Contratação enviada e ainda sem decisão do prestador. | Pendente, solicitada (requested no código) | Comum | Prazo de 1 dia útil para o prestador responder. | UC HPET04, 05 |
| **Aguardando pagamento** | Contratação aceita pelo prestador e ainda não paga. | Aceita (awaiting_payment no código) | Comum | Prazo de 1 dia útil para o tutor pagar. | UC HPET12a; DA-17 |
| **Confirmada** | Contratação paga, com pagamento aprovado. | Agendada, aceita | Comum | Libera check-in ou início do serviço na data. | UC HPET07a, 26 |
| **Em andamento** | Contratação com check-in feito e ainda sem check-out. | Ativa, em execução | Comum | Período em que se registram atividades e incidentes. | US-HPET12B-01 |
| **Concluída** | Contratação encerrada pelo check-out ou pelo fim da visita ou do passeio. | Finalizada, encerrada | Comum | Habilita avaliação e cálculo do repasse. | US-HPET12D-01, 17B-01 |
| **Recusada** | Contratação que o prestador não aceitou. | Rejeitada | Comum | Tutor é notificado. Motivo da recusa [TBD]: existe só no código e ainda está em decisão. | UC HPET12a, 17a, 19; COD (decide-host-reservation); REQ RF-CON-11 |
| **Cancelada** | Contratação encerrada antes do início, pelo tutor ou automaticamente por prazo. | Desistência | Comum | Estorno automático se já estava paga. **Não há multa.** | UC HPET04a; DA-11 |
| **Dia útil** | Segunda a sexta, sem feriados. | Dia corrido | Comum | Base dos prazos de aceite e pagamento. | US-HPET04-01 |
| **Prazo de aceite** | Até 1 dia útil para o prestador aceitar ou recusar a solicitação. | SLA de resposta | Comum | Vencido o prazo, cancelamento automático e tutor notificado. | UC HPET12a, 17a, 19 |
| **Prazo de pagamento** | Até 1 dia útil após o aceite para o tutor pagar. | — | Tutor | Vencido o prazo, cancelamento automático, agenda liberada e partes notificadas. Pagamento em processamento aguarda o gateway. | DA-17; UC HPET07a |
| **Cancelamento automático** | Cancelamento feito pelo sistema quando vence o prazo de aceite ou de pagamento. | Expiração | Backend | Executado por rotina agendada. | US-HPET04-01, 07A-01 |
| **Cancelamento pelo tutor** | Cancelamento solicitado pelo tutor antes de o serviço começar. | — | Tutor | Contratação concluída não pode ser cancelada. Com mais de 48 horas de antecedência do início, o cancelamento é gratuito, com estorno integral. Fora disso, o estorno pode ser **integral ou parcial**, conforme o momento e a forma do cancelamento; os critérios ainda serão avaliados. Não há multa. | UC HPET04a; DEF item 8; FIG; decisão PO 01/11/2026 |
| **Política de cancelamento** | Regras de prazo e de estorno de uma contratação, exibidas ao tutor antes de ele confirmar o cancelamento. | Regras de reembolso, termos de cancelamento | Tutor | Mais de 48 horas antes do início: cancelamento gratuito, com estorno integral. Fora desse prazo: estorno **integral ou parcial**, com critérios ainda a avaliar. Não há multa. | REQ RF-CON-15, RN-03 |
| **Snapshot de preço** | Registro do preço e da taxa vigentes no momento da solicitação. | Preço congelado | Backend | Mudanças posteriores de preço ou taxa não afetam a contratação. | US-HPET04-01, 41-01 |
| **Responsabilidade pela administração de medicamentos** | Confirmação obrigatória do prestador, no aceite, de que assume dar os medicamentos informados. | — | Host, Pet Sitter, Pet Walker | Sem a confirmação, o aceite é bloqueado. | DA-21; US-HPET12A-01 |
| **Medicamentos da solicitação** | Lista de medicamentos do pet informada pelo tutor na solicitação: tipo, horários e duração, forma de aplicação. | Prescrição | Tutor | Só aparece se o prestador oferece administração de medicamentos. | DA-21; UC HPET04, 05 |
| **Instruções de acesso** | Orientações do tutor para o prestador entrar na residência: método, passo a passo, alarme e áreas liberadas. | Chave, senha do imóvel | Tutor, Pet Sitter, Pet Walker | Preenchidas na solicitação. Liberadas só após o pagamento e no dia do serviço; ocultas após o check-out. | DA-22 |
| **Contato antes da reserva** | Conversa do tutor com o prestador antes de existir solicitação. | — | Tutor, Host | Por padrão, só pelo Assistente de IA. **Exceção:** o Host pode liberar o atendimento humano antes da reserva, se quiser (opt-in do HPET06b). | Decisão 25/10; UC HPET06, 06b; decisão PO 01/11/2026 |

9. Execução e acompanhamento
----------------------------

| **Termo oficial** | **Definição** | **Evitar** | **Módulo** | **Regras e estados** | **Fonte** |
| --- | --- | --- | --- | --- | --- |
| **Check-in** | Registro da chegada do pet (hospedagem) ou do início da visita ou do passeio. | Entrada, início | Host, Pet Sitter, Pet Walker | Exige contratação confirmada e data de início chegada. Pet Sitter e Pet Walker registram localização e foto. | UC HPET12b; US-HPET17B-01, 19B-01 |
| **Check-out** | Registro da saída do pet (hospedagem) ou do fim da visita ou do passeio. | Saída, encerramento | Host, Pet Sitter, Pet Walker | Conclui a contratação. Na visita e no passeio com tutor ausente, inclui a **lista de verificação de encerramento**. | UC HPET12d; DA-22 |
| **Liberação de dados do tutor** | Regra que só mostra ao prestador o endereço, telefone e dados desse nível no dia do check-in ou do serviço, e só o necessário. | — | Host, Pet Sitter, Pet Walker | Vale também para as instruções de acesso. | DA-20 |
| **Registro de atividade** | Anotação do prestador sobre alimentação, recreação, medicação, comportamento e fotos durante o serviço. | Diário, rotina, post | Host | Na hospedagem: mínimo 1 registro por dia, só entre check-in e check-out. Descrição, alimentação e comportamento são obrigatórios; passeio e banho só quando incluídos no serviço. | UC HPET12c; US-HPET12C-01; REQ RF-EXE-02 |
| **Relatório diário** | Conjunto dos registros de atividade de um dia de hospedagem, visível ao tutor. | Diário de bordo, relatório de rotina | Host, Tutor | Host recebe alerta se o dia ficar sem registro. | US-HPET12C-01; DEF item 9 |
| **Registro de execução** | Registro da visita ou do passeio: check-in e check-out com localização e foto, observações, medicação e incidentes. | — | Pet Sitter, Pet Walker | Exige contratação confirmada. | UC HPET17b, 19b |
| **Registro de administração de medicamento** | Anotação de cada aplicação de medicamento, com data e hora, no registro de atividade ou de execução. | — | Host, Pet Sitter, Pet Walker | Obrigatória quando houver medicamentos na solicitação. | DA-21; US-HPET12C-01 (RN03) |
| **Acompanhamento de rotina** | Visualização, pelo tutor, dos registros e incidentes enviados pelo prestador durante o serviço. | Monitoramento, timeline | Tutor | Inclui autorizar ou recusar atendimento veterinário. | UC HPET09 |
| **Incidente** | Registro de algo grave ou fora do normal durante o serviço, com o pet ou no imóvel, feito pelo prestador. | Acidente, problema | Host, Pet Sitter, Pet Walker | Exige tipo, descrição, ação tomada e gravidade. Notifica o tutor com prioridade e gera **ocorrência** para o Administrador. Não conta no limite diário. | DA-02, DA-18; UC HPET12c, 17b, 19b |
| **Tipo de incidente** | Classificação do incidente: **com o pet** ou **no imóvel**. | — | Host, Pet Sitter, Pet Walker | Incidente no imóvel cobre problemas na residência do tutor. | DA-22; UC HPET12c |
| **Gravidade** | Classificação do incidente em **leve**, **média** ou **grave**. | Criticidade, prioridade | Host, Pet Sitter, Pet Walker, Admin | O prestador informa no momento (**gravidade informada**); o Administrador define a **gravidade final**. | DA-18; UC HPET25 |
| **Procedimento de emergência** | Conduta em acidente, doença, fuga ou óbito do pet. | — | Host, Pet Sitter, Pet Walker | Até caso médio: o prestador alerta e espera o tutor decidir. Caso grave: segue procedimento padrão e decide. Em ambos registra e relata. | DA-18 |
| **Atendimento veterinário** | Atendimento ao pet por veterinário durante o serviço, tratado como contratação adicional. | Garantia veterinária, plantão veterinário | Tutor, prestadores, Admin | Caso médio: precisa de autorização do tutor. Caso grave: o prestador age e o Administrador analisa responsabilidade e custos. | DA-19; UC HPET09, 25 |
| **Contratação adicional** | Contratação vinculada à original, criada quando o tutor autoriza atendimento veterinário. | — | Tutor | Registrada a partir do acompanhamento de rotina. | UC HPET09 |
| **Custo extra** | Valor adicional devido pelo tutor, como atendimento veterinário que ele causou. | Taxa extra, cobrança adicional, multa | Tutor | Pago ao fim do serviço como pagamento complementar. Não é multa. | UC HPET07a, 09; DA-11 |
| **Lista de verificação de encerramento** | Itens conferidos no check-out quando o tutor está ausente: portas e janelas trancadas, alarme ligado e onde a chave ficou. | Checklist (na interface) | Pet Sitter, Pet Walker | Faz parte do check-out da visita e do passeio. | DA-22 |

10. Financeiro
--------------

| **Termo oficial** | **Definição** | **Evitar** | **Módulo** | **Regras e estados** | **Fonte** |
| --- | --- | --- | --- | --- | --- |
| **Forma de pagamento** | Meio de pagamento do tutor guardado como token do gateway. | Cartão salvo, método de pagamento | Tutor | Plataforma nunca guarda número completo nem CVV. Tipos no banco: cartão de crédito, débito, Pix, boleto. | UC HPET07; US-HPET07-01; DER |
| **Forma de pagamento padrão** | Forma de pagamento que o tutor marca como preferida entre as que cadastrou. | Cartão principal | Tutor | O tutor pode ter várias formas de pagamento e só uma é a padrão. Ela vem pré-selecionada no pagamento e pode ser trocada. | REQ RF-PAG-01 |
| **Pagamento** | Cobrança da contratação aceita, feita pelo tutor na plataforma. | Checkout, cobrança | Tutor, Backend | Só depois do aceite. Aprovado: contratação confirmada. Recusado: segue aguardando pagamento. | Decisão PO; UC HPET07a, 26 |
| **Pagamento complementar** | Cobrança, ao fim do serviço, dos custos extras registrados na contratação. | — | Tutor | Ver **Custo extra**. | UC HPET07a |
| **Comprovante de pagamento** | Registro entregue ao tutor quando o pagamento de uma contratação é aprovado. | Recibo, nota | Tutor | Só é emitido com pagamento aprovado. Fica disponível no extrato de pagamentos. | REQ RF-PAG-02 |
| **Gateway de pagamento** | Serviço externo, em sandbox, que processa cobranças, estornos e transferências. | Adquirente, PSP | Backend | Exigência do regulamento (pagamento em ambiente de teste). | UC HPET26; TAP |
| **Cupom de desconto** | Código criado pelo Administrador que reduz o valor do pagamento. | Voucher, código promocional | Tutor, Admin | Tipo **percentual** ou **valor fixo**; vigência; limite de uso; situação **ativo / inativo**. Outras regras de elegibilidade. | UC HPET32, 40; US-HPET32-01 |
| **Taxa da plataforma** | Percentual que a plataforma desconta do valor bruto de cada contratação. | Comissão, taxa de serviço, receita Hospetse | Admin, prestadores | Configurada pelo Administrador com início de vigência. Vale a taxa vigente na contratação. | DA-04; UC HPET41; DEF item 7; FIG; decisão PO 01/11/2026 |
| **Valor bruto** | Valor total pago pelo tutor por uma contratação, antes da taxa. | — | Prestadores | — | UC HPET12e |
| **Valor líquido** | Valor bruto menos a taxa da plataforma; é o que o prestador recebe. | — | Prestadores | — | UC HPET12e, 27 |
| **Saldo disponível** | Soma dos valores líquidos de contratações concluídas ainda não repassados. | Carteira, crédito | Prestadores | Fica reservado durante um repasse em andamento ou com falha. | US-HPET12E-01, 12F-01 |
| **Repasse** | Transferência do saldo disponível ao prestador. | Saque, liquidação, payout (só no código) | Prestadores, Backend | **Só mediante solicitação do prestador**, nunca automático. | DEF; UC HPET12f, 27 |
| **Estorno** | Devolução ao tutor do valor pago, quando a contratação é cancelada. | Reembolso, refund (só no código) | Tutor, Backend | Automático no cancelamento de contratação paga. Pode ser **integral** ou **parcial**, conforme o momento e a forma do cancelamento (ver Cancelamento pelo tutor). Estorno com falha vai ao Administrador. | UC HPET04a, 28; DA-09; decisão PO 01/11/2026 |
| **Transação com falha** | Estorno ou repasse que não se concluiu no fluxo padrão. | Erro de pagamento, chargeback | Admin | O Administrador reprocessa ou registra resolução manual, com justificativa. | DA-09, DA-12; UC HPET43 |
| **Resolução manual** | Regularização de transação com falha registrada pelo Administrador, sempre como novo lançamento. | Ajuste, correção | Admin | Nunca altera o lançamento original. | UC HPET43, 28a |
| **Registro financeiro (ledger)** | Conjunto imutável de lançamentos de pagamentos, repasses, estornos e resoluções manuais. | Extrato (quando é o registro interno) | Backend | Só inserção. Correção = novo lançamento vinculado ao original. | UC HPET28a |
| **Status da transação** | Situação de um lançamento: **aprovada**, **recusada**, **estornada** ou **com falha**. | Pago, pendente | Backend, Admin | Lista consolidada: (as US usam os termos ao lado) | US-HPET26-01/02, 28-01/02; DER; GH-MVP |
| **Lançamento** | Cada registro do registro financeiro, com tipo, valor, data, partes, status e vínculo com a contratação. | Movimento | Backend | Imutável. | UC HPET28a |
| **Extrato de pagamentos** | Histórico de pagamentos e estornos do tutor. | — | Tutor | — | UC HPET07b |
| **Painel financeiro** | Tela do prestador com ganhos, valor bruto, taxa, valor líquido e saldo disponível. | Carteira | Host, Pet Sitter, Pet Walker | Ponto de partida para solicitar repasse. | UC HPET12e |
| **Condições por forma de pagamento** | Vantagens ligadas ao meio de pagamento, como desconto no Pix ou parcelamento no cartão. | — | Tutor | o MVP e o Figma mostram "Pix 5% OFF" e "até 6x sem juros" | GH-MVP; FIG |
| **Multa** | **Termo proibido.** Não existe multa nem cobrança punitiva na plataforma. | — | — | Penalidades são só advertência, suspensão e banimento. | DA-11 |

11. Comunicação
---------------

| **Termo oficial** | **Definição** | **Evitar** | **Módulo** | **Regras e estados** | **Fonte** |
| --- | --- | --- | --- | --- | --- |
| **Chat** | Conversa em tempo real entre tutor e Host, com o Assistente de IA, via WebSocket. | Mensageria, inbox | Tutor, Host | Caso de uso obrigatório de WebSocket. Mensagens persistidas. Estados da conversa: em atendimento pela IA, escalonada, em atendimento humano, encerrada. | UC HPET06; DER (conversations) |
| **FAQ do Host** | Base de perguntas e respostas pré-definidas cadastrada pelo Host para o Assistente de IA usar. | Base de conhecimento | Host | Categorias-base sugeridas pelo sistema mais itens livres. | UC HPET06b; US-HPET06B-01 |
| **Escalonamento** | Transferência da conversa da IA para o Host, automática (a IA não sabe responder) ou a pedido do tutor. | Transbordo, handoff | Tutor, Host | Mantém o histórico. Vai para a fila de conversas escaladas. | UC HPET06, 06c |
| **Atendimento humano** | Atendimento do tutor pelo próprio Host no chat. | Suporte, atendente | Host | Direto antes da reserva só se o Host habilitar (opt-in), como exceção à regra de contato só pela IA. | UC HPET06, 06b; decisão PO 01/11/2026 |
| **Fila de conversas escaladas** | Lista de conversas que a IA passou ao Host. | Fila de atendimento | Host | — | UC HPET06c |
| **Notificação** | Aviso ao usuário sobre um evento (contratação, pagamento, validação, penalidade, chat). | Alerta | Comum | Leva à tela relacionada. Incidente gera notificação prioritária. | UC HPET29 |
| **Central de notificações** | Lista das notificações do usuário, com marcação de lida. | Caixa de entrada | Comum | — | UC HPET29 |

12. Confiança, moderação e administração
----------------------------------------

| **Termo oficial** | **Definição** | **Evitar** | **Módulo** | **Regras e estados** | **Fonte** |
| --- | --- | --- | --- | --- | --- |
| **Avaliação** | Nota de 1 a 5, com comentário opcional, do tutor sobre uma contratação concluída. | Review, depoimento | Tutor | Uma por contratação; só é possível editar a existente. Sujeita a denúncia e moderação. | UC HPET08; US-HPET08-01/03 |
| **Nota média** | Média das avaliações publicadas de um prestador. | Rating, score | Tutor | Desconsidera avaliações ocultadas ou removidas. | UC HPET33 |
| **Favorito** | Prestador marcado pelo tutor para acesso rápido. | — | Tutor | — | UC HPET30 |
| **Denúncia** | Registro, por qualquer usuário do app, de usuário ou conteúdo (perfil, avaliação, foto ou mensagem) que descumpra as regras. | Reclamação, report | Comum | Gera **ocorrência** e, se for conteúdo, item de **moderação**. Motivos de lista pré-definida. Não aceita denúncia repetida do mesmo item nem do próprio conteúdo. Denunciante não é revelado. Prazo de análise: 5 dias úteis. | DA-02; UC HPET36; DEF item 12; REQ RF-COM-01 |
| **Ocorrência** | Caso para análise do Administrador, originado de **denúncia** ou de **incidente**. | Chamado, ticket | Admin | Termina em penalidade ou arquivamento. Tem gravidade informada e final (se for incidente). | DA-02; UC HPET25 |
| **Arquivamento de ocorrência** | Encerramento da ocorrência sem penalidade, com justificativa. | Descarte | Admin | Tela 04M. | UC HPET25 |
| **Análise de responsabilidade e custos** | Decisão do Administrador, em incidente grave, sobre quem arca com os custos (inclusive veterinário). | Mediação de conflitos | Admin | Se o custo for do tutor, vira custo extra. | DA-18/19; UC HPET25 |
| **Penalidade** | Sanção aplicada pelo Administrador a um usuário: **advertência**, **suspensão com prazo** ou **banimento**. | Sanção, punição, multa | Admin, Comum | Sempre com motivo. Não existe multa, restrição de funcionalidades nem suspensão só de novas reservas. | DA-01, DA-11; UC HPET25 |
| **Advertência** | Penalidade que não muda o estado da conta. | Aviso | Comum | Aviso exibido após o login, com confirmação de ciência. | UC HPET38 |
| **Suspensão com prazo** | Penalidade que deixa a conta suspensa até a data de fim. | Bloqueio temporário | Comum | Prazo definido na aplicação; fim automático. | DA-01/10; UC HPET25a |
| **Banimento** | Penalidade que deixa a conta banida. | Exclusão, encerramento definitivo | Comum | Só termina por reversão. | DA-01 |
| **Aviso de penalidade** | Comunicação ao usuário da penalidade recebida, com tipo, motivo e prazo. | — | Comum | O usuário confirma a ciência. | UC HPET38 |
| **Reversão de penalidade** | Encerramento de uma penalidade pelo Administrador, com motivo. | Anulação, perdão | Admin | Restaura a conta quando aplicável e cria nova entrada de auditoria. | DA-10; UC HPET25a |
| **Fim automático da suspensão** | Encerramento da suspensão pelo sistema na data de fim. | — | Backend | Registrado na auditoria com autor "Sistema". | UC HPET25a |
| **Moderação de conteúdo** | Análise pelo Administrador de conteúdo denunciado, com decisão de **manter**, **ocultar** ou **remover**, com justificativa. | — | Admin | No chat, vê só a mensagem denunciada. | DA-04, DA-13; UC HPET42 |
| **Acesso à conversa completa** | Permissão para o Administrador ver toda a conversa de um chat denunciado. | — | Admin | Pedido com justificativa, concedido por **outro** Administrador, nunca pelo solicitante. Acesso restrito e auditado. | DA-13, DA-15 (decidida em 01/11/2026); UC HPET42a |
| **Trilha de auditoria** | Registro imutável das ações administrativas com autor, data e motivo. | Logs | Admin | Cobre validações, penalidades, reversões, reativações, moderação, cupons, taxa, transações e acessos a conversas. | UC HPET47 |
| **Dashboard administrativo** | Tela inicial do Administrador com filas pendentes e indicadores. | Visão geral operacional, painel geral | Admin | Destino após o login; provisoriamente, a fila de validação. | UC HPET44; PWEB |
| **Área comercial pública** | Parte da Web aberta a visitantes: Quem somos, Serviços, Como funciona, Sobre o app, Termos de Uso e Política de Privacidade. | Site institucional, landing page | Web pública | Sem busca, reserva, cadastro nem link para o login admin. Indispensável para a entrega. | DF-01/02; UC HPET34; decisão 25/10 |
| **Sobre o app** | Seção da área pública que explica o app e leva à loja compatível com o sistema do visitante. | Download | Web pública | Sem download direto; publicação na loja é próxima etapa. | DA-05, DA-14; UC HPET35 |
| **Inativação de cupom** | Desativação lógica de um cupom, que deixa de valer em novos pagamentos. | Excluir cupom | Admin | — | UC HPET40 |

13. Siglas e identificadores do projeto
---------------------------------------

| **Sigla** | **Significado** |
| --- | --- |
| **HPETxx** | Identificador canônico dos casos de uso (ex.: HPET04 — Solicitar Reserva). |
| **RF-XXX-nn** | Requisito funcional, agrupado por módulo (ACE, TUT, PET, BUS, CON, PRE, EXE, CHT, PAG, FIN, AVA, NOT, COM, WEB, ADM). |
| **RNF-XXX-nn** | Requisito não funcional, agrupado por categoria (SEG, PRI, ARQ, CON, DES, USA, MAN). |
| **RN-nn** | Regra de negócio transversal. |
| **MoSCoW** | Prioridade dos requisitos: Must, Should, Could. |
| **UCxx** | Numeração antiga dos casos de uso, usada no MVP da fase 1 (UC03 = HPET03 Buscar Serviços; UC04 = HPET04 Solicitar Reserva). |
| **COD** | Referência ao código-fonte (back-end, Auth Server e app) usada como fonte neste glossário. |
| **HOS-xx** | Cards do Jira da fase 1 (legado; o backlog atual está no Azure DevOps). |
| **US-HPETxx-nn** | Identificador das User Stories, ligado ao caso de uso. |
| **PR** | Prefixo dos componentes de tela compartilhados entre prestadores. |
| **HSP-EP / HSP-FT / HSP-PBI / TSK** | Epic, Feature, PBI e Task no Azure DevOps. |
| **DF / DA** | Decisões da PO: DF (fluxo e canais), DA (ajustes e regras). |
| **WJxx** | Jornadas da Planilha de Fluxo Web (ex.: WJ08 Contratações). |
| **DoD** | Definition of Done. |
| **AC** | Critério de aceite (Dado / Quando / Então). |
| **TBD** | A definir: decisão ainda não tomada. |
| **LGPD** | Lei nº 13.709/2018, Lei Geral de Proteção de Dados. |
| **JWT** | Token de acesso emitido pelo Auth Server (validade de 15 min); renovado pelo refresh token. |
| **Auth Server** | Serviço separado que faz cadastro com PIN, login e emissão de tokens. |
| **WebSocket** | Canal de comunicação em tempo real usado no chat (exigência do regulamento). |
| **Sandbox** | Ambiente de testes do gateway de pagamento. |

14. Manutenção
--------------

*   Toda decisão da PO que crie ou mude um termo deve atualizar este glossário na mesma Sprint.
*   As tasks de validação com a PO (por exemplo, #271 HSP-PBI-001-TSK-13 e #279 HSP-PBI-002-TSK-08) passam a usar esta base.

15. Histórico de revisões
-------------------------

| **Versão** | **Data** | **Autor** | **Descrição** |
| --- | --- | --- | --- |
| 2.0 | 02/11/2026 | Brenda (PO) | Inclusão do Documento de Requisitos (REQ) como fonte e na precedência; novos termos: Sessão expirada, Política de cancelamento, Forma de pagamento padrão e Comprovante de pagamento; novas siglas RF, RNF, RN e MoSCoW; Busca, Recusada, Registro de atividade, Denúncia, Recuperação de senha e Perfil completo alinhados aos requisitos. |
| 1.0 | 01/11/2026 | Brenda (PO) | Primeira versão, consolidada a partir da Wiki, das Definições, dos UCs e USs v2, dos Termos e da Política, das planilhas de fluxo, do DER, Figma e GitHub hospetse. |