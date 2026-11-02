      
HOSPETSE — User Stories Detalhadas (Revisão v2)
===============================================

Sobre esta revisão
------------------

As User Stories foram alinhadas aos Casos de Uso v2: 186 US no total, sendo 55 novas, 53 alteradas e 78 mantidas. Nenhum identificador existente foi alterado.
*   Cada US traz o campo **Revisão** (Novo, Alterado ou Sem alteração).
*   Nas US alteradas, a linha **Ajuste v2** resume o que mudou.

Módulo Tutor (Mobile)
---------------------

### HPET01 – Cadastrar-se

#### US-HPET01-01

Principal | Prioridade: Alta | HPET01: Cadastrar-se | Revisão: Sem alteração
Como visitante, quero criar uma conta com dados mínimos, para acessar a plataforma e escolher como quero atuar nela.
**Descrição:** Cadastro inicial único, comum a todos os usuários. O visitante informa dados mínimos de identificação, informa seu e-mail e cria uma senha.
O cadastro ocorre em duas etapas. Na primeira etapa, o front-end envia somente o e-mail informado ao back-end. O back-end valida o formato do e-mail, cria uma solicitação temporária de verificação, gera um código de confirmação (PIN) e envia o código para o e-mail informado. Nessa primeira etapa, nenhuma conta é criada na tabela USER, nenhum perfil de Tutor é criado e nenhum JWT é emitido.
Na segunda etapa, após receber o PIN, o front-end envia o PIN junto com o payload completo do cadastro. O back-end valida o PIN e os dados enviados. Somente após todas as validações serem concluídas, o sistema cria uma conta única, cria automaticamente o perfil inicial de Tutor com status incomplete e emite o JWT.
Depois de autenticado, o usuário pode navegar e buscar serviços. O perfil de Tutor permanece com status incomplete enquanto os dados obrigatórios não estiverem completos. A contratação permanece bloqueada até a conclusão do perfil de Tutor, conforme US-HPET01-03.
Após a criação da conta e a confirmação do e-mail, o usuário pode completar o perfil de Tutor ou solicitar um perfil adicional de prestador (Host, Pet Sitter ou Pet Walker) por meio dos fluxos próprios.
**Critérios de Aceite:**
*   Dado que o visitante informa um e-mail válido, quando enviar o e-mail ao back-end, então o sistema cria uma solicitação temporária de verificação e envia um PIN para o e-mail informado.
*   Dado que o visitante solicitou um PIN, quando essa etapa for concluída, então o sistema não cria uma conta USER, não cria um perfil TUTOR e não emite um JWT.
*   Dado que o visitante possui um PIN válido e informa nome, e-mail, senha, confirmação de senha e aceita os Termos de Uso e a política de privacidade, quando enviar o payload completo ao back-end, então o sistema valida os dados, cria uma conta única e cria automaticamente o perfil inicial de Tutor com status incomplete.
*   Dado que o PIN e os dados do cadastro foram validados corretamente, então o sistema confirma o e-mail e emite automaticamente o JWT.
*   Dado que o usuário informa um PIN inválido, expirado ou já utilizado, então o sistema rejeita o cadastro, não cria a conta e não emite o JWT.
*   Dado que o e-mail informado já está associado a uma conta, quando o visitante enviar o payload completo, então o sistema bloqueia o cadastro sem duplicar a identidade.
*   A senha deve atender aos critérios mínimos de segurança e ser armazenada com hash.
*   O aceite dos Termos de Uso e da política de privacidade (LGPD) é obrigatório antes da criação da conta.
*   Dado um usuário autenticado com perfil de Tutor incomplete, quando acessar a plataforma, então ele pode navegar e buscar serviços, mas não pode contratar.
*   Dado que existam campos inválidos ou ausentes no payload do cadastro, quando o visitante enviar os dados ao back-end, então o sistema informa todos os erros e não cria a conta.
**Regras de Negócio:**
*   O e-mail é obrigatório no cadastro inicial.
*   O e-mail informado deve ser único na plataforma.
*   O primeiro envio ao back-end serve somente para solicitar o PIN.
*   A conta só deve ser criada após o envio do payload completo e a validação correta do PIN.
*   Todo cadastro concluído recebe automaticamente o perfil inicial de Tutor.
*   O perfil inicial de Tutor começa com status incomplete.
*   O status incomplete não impede a autenticação do usuário após a confirmação do PIN.
*   Um usuário autenticado com perfil incomplete pode buscar informações, mas não pode contratar serviços.
*   Um PIN inválido, expirado ou reutilizado não deve resultar na criação da conta nem na emissão do JWT.
*   O perfil de Tutor deve passar para complete antes da contratação.
**Notas Técnicas:**
Primeira etapa — solicitação do PIN, por POST /auth/request-pin. Esse endpoint deve: receber somente o e-mail; validar o formato do e-mail; normalizar o e-mail; gerar o PIN; armazenar somente o hash do PIN; criar uma solicitação temporária em SIGNUP_VERIFICATION_REQUEST; definir o prazo de expiração; controlar tentativas e reenvios; enviar o PIN para o e-mail informado; retornar um identificador de confirmação, como challengeId; não criar registro em USER; não criar registro em TUTOR; não emitir o JWT.
Segunda etapa — criação da conta, por POST /auth/sign-up. Esse endpoint deve receber: challengeId; PIN informado pelo usuário; nome; e-mail; senha; confirmação de senha; aceite dos Termos de Uso; aceite da política de privacidade. Deve: localizar a solicitação temporária; validar o PIN; verificar se o PIN não expirou; verificar se o PIN ainda não foi utilizado; validar todos os dados do cadastro; verificar a unicidade do e-mail; criar o registro em USER; criar automaticamente o perfil inicial em TUTOR; definir o perfil como incomplete; marcar a solicitação de verificação como consumida; emitir o JWT; retornar o JWT e o estado atual do perfil do Tutor.
A criação da conta e o consumo do PIN devem ocorrer de forma transacional. Caso a criação da conta falhe, o PIN não deve ser marcado como consumido. O front-end pode validar os dados para melhorar a experiência do usuário, mas o back-end deve repetir todas as validações.
Fluxo esperado:
Tela de e-mail
→ usuário informa o e-mail
POST /auth/request-pin
→ back-end valida o e-mail
→ back-end gera e envia o PIN
→ nenhuma conta é criada
→ nenhum JWT é emitido
Tela de senha
→ usuário informa nome, senha e confirmação de senha
→ front-end mantém temporariamente o payload
Tela de PIN
→ usuário informa o PIN
POST /auth/sign-up
→ back-end recebe o payload completo e o PIN
→ back-end valida o PIN
→ back-end valida os dados
→ conta USER criada
→ perfil TUTOR criado como incomplete
→ JWT emitido
Tela de sucesso
→ front-end recebe o JWT
→ usuário pode navegar e buscar serviços
→ contratação permanece bloqueada até completar o perfil

#### US-HPET01-02

Alternativa | Prioridade: Alta | HPET01 – Cadastrar-se | Revisão: Sem alteração
Como Tutor, quero receber uma mensagem quando algum dado do cadastro estiver incorreto ou incompleto, para corrigi-lo antes de concluir o cadastro.
**Descrição:** Cobre a validação de formulário (client-side e server-side) do cadastro de Tutor.
**Critérios de Aceite:**
*   Dado um campo obrigatório vazio ou em formato inválido (ex.: e-mail malformado), quando o usuário tentar submeter, então o campo correspondente é destacado com mensagem específica do erro.
*   Dado múltiplos erros simultâneos, quando exibidos, então todos os campos inválidos aparecem destacados ao mesmo tempo (não um por vez).
*   O envio ao backend só ocorre após todas as validações client-side passarem; o backend revalida tudo novamente.
**Notas Técnicas:** Erros retornados em formato padronizado (ex.: 422 + lista de campos/mensagens).

#### US-HPET01-03

Principal | Prioridade: Alta | HPET01: Completar o perfil do Tutor | Revisão: Sem alteração
Como Tutor, quero completar meu perfil após o cadastro inicial, para poder contratar serviços na plataforma.
**Descrição:** Complementação dos dados necessários para contratação, incluindo CPF, data de nascimento, endereço completo, contato de emergência, dados obrigatórios do perfil e validação de identidade.
O telefone não é obrigatório no cadastro inicial. A necessidade de informar telefone para concluir o perfil permanece como decisão de produto a ser confirmada. Caso essa exigência seja aprovada, o sistema deverá solicitar e validar o telefone durante a conclusão do perfil.
O cadastro de pets segue fluxo próprio em HPET02 e pode ser um pré-requisito para contratação, conforme as regras do serviço. A forma de pagamento não faz parte da conclusão do perfil. O cadastro e a validação da forma de pagamento pertencem ao fluxo específico de pagamentos, conforme HPET07.
**Critérios de Aceite:**
*   Dado um Tutor com perfil incomplete, quando acessar a plataforma, então o sistema deve exibir os dados pendentes e permitir o acesso ao fluxo de conclusão do perfil.
*   Dado um Tutor que informa CPF, data de nascimento, endereço e demais dados obrigatórios válidos, quando salvar o perfil, então o sistema deve marcar o perfil como complete e liberar a contratação, desde que os demais pré-requisitos da contratação estejam atendidos.
*   Dado que a exigência de telefone para completar o perfil esteja aprovada, quando o Tutor não informar um telefone válido, então o sistema deve manter o perfil como incomplete e informar a pendência.
*   Dado um CPF inválido ou já cadastrado, quando o Tutor salvar o perfil, então o sistema deve bloquear a operação e informar o motivo.
*   Dado um usuário menor de 18 anos, quando informar a data de nascimento, então o sistema deve impedir a conclusão do perfil.
*   Dado um Tutor com perfil incomplete, quando tentar contratar um serviço, então o sistema deve bloquear a ação e orientar a conclusão do perfil.
*   O cadastro ou a validação da forma de pagamento não deve ser exigido para marcar o perfil como complete. Essa validação deve ocorrer no fluxo de pagamento.
**Regras de Negócio:**
*   O telefone não é obrigatório no cadastro inicial.
*   A exigência de telefone para concluir o perfil está pendente de decisão do produto.
*   O perfil deve permanecer incomplete enquanto houver dados obrigatórios pendentes.
*   O perfil complete é necessário para contratar serviços.
*   A forma de pagamento é tratada separadamente no fluxo de pagamentos.
*   O cadastro de pet pode ser obrigatório para contratação, mas segue o fluxo específico de HPET02.

### HPET01a – Autenticar-se (Login)

#### US-HPET01A-01

Principal | Prioridade: Alta | HPET01a – Autenticar-se (Login) | Revisão: Alterado
Como usuário do app (Tutor, Host, Pet Sitter ou Pet Walker), quero acessar a plataforma mediante validação das minhas credenciais e do estado da minha conta, para utilizar os recursos dos meus papéis.
**Ajuste v2:** ator ampliado para todos os perfis do app; login passa a verificar o estado da conta (DA-01) e liberar o acesso conforme os papéis.
**Descrição:** Fluxo padrão de autenticação (e-mail/telefone + senha) do usuário do app, com verificação do estado da conta (ativa, suspensa, banida ou inativa) e emissão de token para as chamadas REST subsequentes.
**Critérios de Aceite:**
*   Dado e-mail/telefone e senha corretos e conta ativa, quando o usuário submeter o login, então recebe um token de acesso válido, com os papéis da conta (Tutor e, quando houver, Host, Pet Sitter ou Pet Walker), e é redirecionado à tela inicial.
*   Dado credenciais corretas e conta suspensa, banida ou inativa, quando tentar logar, então o acesso é negado conforme US-HPET01A-03 (não um erro genérico de senha).
*   Dado conta ativa com penalidade ainda não visualizada (ex.: advertência), quando o login for concluído, então o aviso é exibido antes da navegação (US-HPET38-01).
*   Sessão/token deve ter expiração definida, exigindo renovação (stateless).
**Notas Técnicas:** REST stateless: autenticação via JWT (sem sessão em servidor); o token carrega os papéis do usuário.

#### US-HPET01A-02

Alternativa | Prioridade: Alta | HPET01a – Autenticar-se (Login) | Revisão: Alterado
Como usuário do app, quero receber uma mensagem genérica quando minhas credenciais forem inválidas, para corrigir os dados e tentar novamente sem que a existência da conta seja revelada.
**Ajuste v2:** ator ampliado para todos os perfis do app.
**Descrição:** Tratamento de erro de autenticação sem expor qual campo (e-mail ou senha) está incorreto, por segurança.
**Critérios de Aceite:**
*   Dado credenciais incorretas, quando o usuário submeter o login, então recebe mensagem genérica ('e-mail ou senha inválidos') sem indicar qual campo está errado.
*   Após N tentativas incorretas consecutivas, o sistema deve aplicar bloqueio temporário ou captcha (a definir com time de segurança).
**Regras de Negócio:**
*   Nunca revelar se o e-mail existe ou não na base (mitigação de enumeração de usuários).
*   A verificação do estado da conta só ocorre após credenciais válidas, para não revelar a existência da conta.

#### US-HPET01A-03

Alternativa | Prioridade: Alta | HPET01a – Autenticar-se (Login) | Revisão: Novo
Como usuário do app, quero ser informado quando minha conta estiver suspensa, banida ou inativa, para entender por que meu acesso foi negado e o que posso fazer.
**Descrição:** Tratamento dos estados de conta que impedem o acesso, conforme a máquina de estados única da conta (DA-01).
**Critérios de Aceite:**
*   Dado credenciais válidas e conta suspensa, quando o usuário tentar logar, então o acesso é negado e o sistema exibe o motivo resumido e a data de fim da suspensão.
*   Dado credenciais válidas e conta banida, quando o usuário tentar logar, então o acesso é negado e o sistema informa o banimento.
*   Dado credenciais válidas e conta inativa, quando o usuário tentar logar, então o acesso é negado e o sistema orienta sobre como solicitar a reativação.
*   Em nenhum desses casos o token de acesso é emitido.
**Regras de Negócio:**
*   Suspensão e banimento são sempre penalidades com motivo (HPET25); o aviso exibido é o mesmo de US-HPET38-02.
*   A reativação de conta inativa é feita pelo Administrador (HPET20); a de conta suspensa ou banida ocorre pelo fim do prazo ou pela reversão da penalidade (HPET25a).
**Notas Técnicas:** Resposta 403 com código do estado (ACCOUNT_SUSPENDED, ACCOUNT_BANNED, ACCOUNT_INACTIVE) e, para suspensão, a data de fim.

### HPET01b – Recuperar Senha

#### US-HPET01B-01

Principal | Prioridade: Alta | HPET01b – Recuperar Senha | Revisão: Alterado
Como usuário do app, quero redefinir minha senha de acesso para recuperar minha conta caso a esqueça.
**Ajuste v2:** ator ampliado para todos os perfis do app; recuperação do Administrador fora do escopo (DA-07); a redefinição não altera o estado da conta.
**Descrição:** Fluxo de recuperação via e-mail (ou telefone/SMS) com link/código de validade limitada. A recuperação de senha do Administrador não é coberta por esta US (feita pelo seed — HPET20a).
**Critérios de Aceite:**
*   Dado um e-mail/telefone cadastrado, quando o usuário solicitar recuperação, então recebe um link/código com validade limitada (ex.: 10 min).
*   Dado um link/código válido, quando o usuário definir nova senha, então a senha antiga é invalidada e a nova passa a ser exigida no próximo login.
*   Dado um link/código expirado ou já utilizado, quando acessado, então o sistema exibe mensagem de expiração e permite solicitar um novo.
*   Dado uma conta suspensa, banida ou inativa, quando a senha for redefinida, então o estado da conta não muda e o acesso continua negado (US-HPET01A-03).
**Notas Técnicas:** Token de recuperação de uso único, com expiração armazenada no backend.

#### US-HPET01B-02

Alternativa | Prioridade: Média | HPET01b – Recuperar Senha | Revisão: Alterado
Como usuário do app, quero ser informado quando o e-mail ou telefone informado não estiver cadastrado, para saber por que não posso iniciar a recuperação.
**Ajuste v2:** ator ampliado para todos os perfis do app.
**Descrição:** Tratamento de erro do fluxo de recuperação para dado não cadastrado.
**Critérios de Aceite:**
*   Dado um e-mail/telefone não cadastrado, quando o usuário solicitar recuperação, então recebe mensagem informando que o dado não foi encontrado.
**Regras de Negócio:**
*   Avaliar com segurança se a mensagem deve ser genérica (evitar enumeração) ou explícita, decisão de produto a confirmar.

### HPET01c – Editar Perfil do Tutor

#### US-HPET01C-01

Principal | Prioridade: Média | HPET01c – Editar Perfil do Tutor | Revisão: Sem alteração
Como Tutor, quero alterar meus dados pessoais cadastrados para manter meu perfil atualizado.
**Descrição:** Edição de dados de perfil (nome, telefone, foto, endereço) já autenticado.
**Critérios de Aceite:**
*   Dado o Tutor autenticado na tela de perfil, quando alterar um ou mais campos e salvar, então os dados são persistidos e refletidos imediatamente na interface.
*   Alteração de e-mail deve exigir reconfirmação (nova validação de unicidade e, opcionalmente, verificação por link).
**Notas Técnicas:** Endpoint PUT/PATCH /api/tutores/{id}.

#### US-HPET01C-02

Alternativa | Prioridade: Média | HPET01c – Editar Perfil do Tutor | Revisão: Sem alteração
Como Tutor, quero receber uma mensagem quando algum dado alterado estiver incorreto, para corrigi-lo antes de salvar.
**Descrição:** Validação de formulário de edição de perfil, mesmo padrão de US-HPET01-02.
**Critérios de Aceite:**
*   Dado um campo alterado com formato inválido, quando o Tutor tentar salvar, então o campo é destacado com a mensagem de erro específica e nada é persistido.

### HPET02 – Cadastrar Pet

#### US-HPET02-01

Principal | Prioridade: Alta | HPET02 – Cadastrar Pet | Revisão: Sem alteração
Como Tutor, quero registrar um ou mais pets no sistema para utilizar os serviços da plataforma.
**Descrição:** Cadastro de pet vinculado ao Tutor: nome, espécie, raça, porte, idade, restrições/necessidades especiais, foto. Base para busca de hosts compatíveis (US-HPET03) e reservas (US-HPET04).
**Critérios de Aceite:**
*   Dado o Tutor autenticado com o cadastro completo, quando preencher os dados obrigatórios do pet (nome, espécie, porte) e salvar, então o pet é registrado e vinculado à sua conta.
*   Um Tutor pode cadastrar múltiplos pets, cada um com identificador próprio.
*   Campos de espécie e porte devem usar listas pré-definidas (não texto livre) para viabilizar filtros de busca e regras de compatibilidade com hosts.
**Notas Técnicas:** Endpoint POST /api/tutores/{id}/pets.

#### US-HPET02-02

Alternativa | Prioridade: Alta | HPET02 – Cadastrar Pet | Revisão: Sem alteração
Como Tutor, quero receber uma mensagem quando os dados do pet estiverem incorretos ou incompletos, para corrigi-los antes de concluir o cadastro.
**Descrição:** Validação de formulário de cadastro de pet.
**Critérios de Aceite:**
*   Dado um campo obrigatório do pet vazio ou inválido, quando o Tutor tentar salvar, então o campo é destacado com mensagem específica e o cadastro não é concluído.

### HPET02a – Editar Dados do Pet

#### US-HPET02A-01

Principal | Prioridade: Média | HPET02a – Editar Dados do Pet | Revisão: Sem alteração
Como Tutor, quero alterar os dados cadastrados do meu pet para manter as informações atualizadas.
**Descrição:** Edição de dados de um pet já cadastrado (peso, idade, restrições, foto etc.).
**Critérios de Aceite:**
*   Dado um pet já cadastrado, quando o Tutor alterar campos e salvar, então os dados são persistidos.
*   Se o pet possuir reserva ativa/futura no momento da edição, avaliar se alterações críticas (ex.: espécie/porte) exigem revalidação de compatibilidade com o host já reservado.

#### US-HPET02A-02

Alternativa | Prioridade: Média | HPET02a – Editar Dados do Pet | Revisão: Sem alteração
Como Tutor, quero receber uma mensagem quando os dados alterados do pet forem inválidos, para corrigi-los.
**Descrição:** Validação de formulário de edição de pet.
**Critérios de Aceite:**
*   Dado um campo alterado inválido, quando o Tutor tentar salvar, então o campo é destacado com a mensagem de erro e nada é persistido.

### HPET02b – Inativar/Remover Pet

#### US-HPET02B-01

Principal | Prioridade: Média | HPET02b – Inativar/Remover Pet | Revisão: Alterado
Como Tutor, quero inativar o cadastro de um pet, preservando seu histórico, para retirá-lo da minha lista de pets ativos e da seleção na busca e na reserva.
**Ajuste v2:** pet com reserva ou solicitação ativa/futura passa a ter a inativação impedida (antes: alerta + confirmação com cancelamento da reserva); pet inativo sai da seleção da busca e da reserva.
**Descrição:** Soft-delete do pet (inativação lógica, preservando histórico de reservas/avaliações já realizadas).
**Critérios de Aceite:**
*   Dado um pet sem reservas ou solicitações ativas/futuras, quando o Tutor solicitar a inativação, então o pet deixa de aparecer na lista ativa e na seleção de pets da busca (HPET03/HPET05) e da reserva (HPET04), mas seu histórico permanece consultável.
*   Dado um pet com reserva ou solicitação ativa/futura, quando o Tutor tentar inativá-lo, então o sistema impede a inativação e informa o motivo, sem cancelar a contratação.
**Regras de Negócio:**
*   Inativação é lógica (soft delete), não exclusão física, necessário para auditoria e histórico.
*   Para inativar um pet com contratação em andamento, o Tutor deve primeiro cancelá-la (US-HPET04A-01).

#### US-HPET02B-02

Exceção | Prioridade: Média | HPET02b – Inativar/Remover Pet | Revisão: Sem alteração
Como Tutor, quero ser informado quando ocorrer uma falha ao inativar meu pet, para saber que o cadastro continua ativo e tentar novamente.
**Descrição:** Tratamento de falha técnica (ex.: erro de comunicação com o backend) durante a inativação.
**Critérios de Aceite:**
*   Dado uma falha no processamento da inativação, quando ocorrer, então o Tutor recebe mensagem de erro e o pet permanece com status ativo (sem estado intermediário inconsistente).

### HPET03 – Buscar Serviços

#### US-HPET03-01

Principal | Prioridade: Alta | HPET03 – Buscar Serviços | Revisão: Alterado
Como Tutor, quero buscar hospedagens por localização, preço e disponibilidade, podendo selecionar meu pet para aplicar espécie e porte, para encontrar opções compatíveis.
**Ajuste v2:** porte manual fica nos filtros; e o pet selecionado também passa a ser o parâmetro de espécie e porte; resultado somente com perfis validados e conta ativa.
**Descrição:** Motor de busca/filtro de hosts. Um dos fluxos centrais da plataforma; alimenta a listagem que leva à solicitação de reserva.
**Critérios de Aceite:**
*   Dado filtros de localização, porte, tipo de pet, faixa de preço e período, quando o Tutor executar a busca, então a lista retornada contém apenas hosts compatíveis com todos os critérios.
*   Dado que o Tutor selecionou um pet ativo, quando executar a busca, então espécie e porte desse pet são aplicados como critério.
*   Resultados devem ser paginados/ordenáveis (ex.: por preço, distância, avaliação).
*   A busca deve considerar apenas hosts com perfil validado (US-HPET20B) e conta ativa (sem suspensão, banimento ou inativação).
**Notas Técnicas:** Endpoint GET /api/hosts/search?localizacao=&precoMin=&precoMax=&petId=&dataInicio=&dataFim=; considerar índice geoespacial para localização.

#### US-HPET03-02

Alternativa | Prioridade: Alta | HPET03 – Buscar Serviços | Revisão: Sem alteração
Como Tutor, quero ser informado quando nenhum host atender aos filtros escolhidos, para ajustar minha busca.
**Descrição:** Estado vazio da busca.
**Critérios de Aceite:**
*   Dado filtros sem nenhum resultado correspondente, quando a busca for executada, então é exibida mensagem de 'nenhum resultado encontrado' com sugestão de ajuste de filtros (ex.: ampliar raio, período).

#### US-HPET03-03

Alternativa | Prioridade: Média | HPET03 – Buscar Serviços | Revisão: Novo
Como Tutor sem pet ativo cadastrado, quero ser avisado e direcionado se assim escolher ao cadastro de pet, para conseguir buscar opções compatíveis com ele.
**Descrição:** Aviso da tela 08I para Tutor que não possui nenhum pet ativo.
**Critérios de Aceite:**
*   Dado um Tutor sem pet ativo, quando acessar a busca de hospedagens, então o sistema exibe o aviso da tela 08I com atalho para o cadastro de pet (US-HPET02-01).
*   Dado que o Tutor concluiu o cadastro do pet a partir do aviso, quando retornar, então volta à busca com o novo pet disponível para seleção.
**Regras de Negócio:**
*   Pets inativos (US-HPET02B-01) não contam como pet ativo.

### HPET04 – Solicitar Reserva

#### US-HPET04-01

Principal | Prioridade: Alta | HPET04 – Solicitar Reserva | Revisão: Alterado

Como Tutor, quero solicitar a hospedagem do meu pet em um host disponível, sabendo que o pagamento só será solicitado após o aceite do host.

**Ajuste v2:** status inicial passa a ser "aguardando aceite"; pagamento somente após o aceite (decisão de PO); exige pet ativo. Decisão PO 01/11/2026: tutor pode informar medicamentos quando o host oferece administração; host tem 1 dia útil para responder, senão a solicitação é cancelada automaticamente.

**Descrição:** Criação da solicitação de reserva (ainda não confirmada, depende de aceite do host, US-HPET12A). Envolve seleção de pet ativo, host, datas, serviços adicionais e, quando houver, os medicamentos do pet.

**Critérios de Aceite:**

- Dado um host e período disponíveis e um pet ativo compatível, quando o Tutor confirmar a solicitação, então uma reserva é criada com status 'aguardando aceite' e o host é notificado.
- Dado a solicitação enviada, quando confirmada, então o sistema informa ao Tutor que o pagamento será solicitado somente após o aceite do host (US-HPET07A-01) e que o host tem 1 dia útil para responder.
- Dado um host que oferece administração de medicamentos, quando o Tutor indicar que o pet usa medicamento, então deve informar tipo, horários e duração e forma de aplicação de cada um, e os dados seguem com a solicitação para o host.
- Dado um host que não oferece administração de medicamentos, quando o Tutor montar a solicitação, então a opção de medicamentos não é exibida.
- Dado uma solicitação sem resposta do host em 1 dia útil, quando o prazo vencer, então o status passa a 'cancelada' e o Tutor é notificado.
- A solicitação deve registrar snapshot do preço vigente no momento (para evitar impacto de alterações futuras de preço pelo host).
- Ver validações relacionadas: US-HPET04-02 (dados), 04-03 (compatibilidade pet), 04-04 (disponibilidade), 04-05 (falha técnica).

**Regras de Negócio:**

- Dia útil = segunda a sexta, sem feriados.

**Notas Técnicas:** Endpoint POST /api/reservas; status inicial AGUARDANDO\_ACEITE; lista medicamentos\[\] no payload; cancelamento por prazo feito por rotina agendada.#### US-HPET04-02

Alternativa | Prioridade: Alta | HPET04 – Solicitar Reserva | Revisão: Sem alteração
Como Tutor, quero receber uma mensagem quando algum campo da solicitação estiver preenchido incorretamente, para corrigi-lo antes de enviar.
**Descrição:** Validação de formulário de solicitação de reserva (datas, pet, serviços).
**Critérios de Aceite:**
*   Dado um campo obrigatório inválido (ex.: data de check-out anterior ao check-in), quando o Tutor tentar enviar, então o campo é destacado com a mensagem específica.

#### US-HPET04-03

Alternativa | Prioridade: Alta | HPET04 – Solicitar Reserva | Revisão: Sem alteração
Como Tutor, quero ser informado quando o pet selecionado não for aceito pelo host devido à espécie ou porte, para escolher outro host.
**Descrição:** Validação de compatibilidade entre o perfil do pet e as restrições do host (espécies/porte aceitos, definidas em US-HPET10A).
**Critérios de Aceite:**
*   Dado que o host não aceita a espécie/porte do pet selecionado, quando o Tutor tentar solicitar a reserva, então o envio é bloqueado com mensagem explicando o motivo.
*   Dado a mensagem de incompatibilidade, quando o Tutor optar por nova busca, então é redirecionado a US-HPET03 mantendo os filtros já aplicados quando possível.
**Regras de Negócio:**
*   Validação de compatibilidade deve ocorrer no backend, não apenas na UI.
**Notas Técnicas:** Erro padronizado (ex.: 422 PET_NOT_COMPATIBLE).

#### US-HPET04-04

Alternativa | Prioridade: Alta | HPET04 – Solicitar Reserva | Revisão: Sem alteração
Como Tutor, quero ser informado quando o host não estiver disponível nas datas escolhidas, para selecionar outro período ou host.
**Descrição:** Validação de disponibilidade da agenda do host (US-HPET11) no momento da solicitação.
**Critérios de Aceite:**
*   Dado um período que colide com a agenda do host (bloqueio ou reserva já aceita), quando o Tutor tentar solicitar, então o envio é bloqueado com mensagem informando a indisponibilidade.
*   A verificação de disponibilidade deve ser feita novamente no momento do envio (não confiar apenas no estado exibido na tela, para evitar condição de corrida).

#### US-HPET04-05

Exceção | Prioridade: Alta | HPET04 – Solicitar Reserva | Revisão: Sem alteração
Como Tutor, quero receber uma mensagem quando ocorrer uma falha ao processar minha solicitação, para saber que a reserva não foi enviada e tentar novamente.
**Descrição:** Tratamento de falha técnica (timeout, erro de infraestrutura) na criação da reserva.
**Critérios de Aceite:**
*   Dado uma falha no processamento da solicitação, quando ocorrer, então o Tutor recebe mensagem de erro clara e nenhuma reserva parcial/inconsistente é criada.
*   A operação de criação deve ser atômica/idempotente para evitar reservas duplicadas em caso de reenvio.

### HPET04a – Cancelar Reserva/Solicitação

#### US-HPET04A-01

Principal | Prioridade: Alta | HPET04a – Cancelar Reserva/Solicitação | Revisão: Alterado
Como Tutor, quero cancelar uma reserva ou solicitação ativa para desistir do agendamento, com estorno automático quando o pagamento já tiver sido aprovado.
**Ajuste v2:** cobre reservas e solicitações de Pet Sitter/Walker; notificação ao host ou prestador; estados alinhados ao pagamento após o aceite.
**Descrição:** Cancelamento de reserva ou solicitação ainda não iniciada, respeitando política de cancelamento/estorno (integra com US-HPET28 no backend de pagamento).
**Critérios de Aceite:**
*   Dado uma reserva/solicitação 'aguardando aceite', 'aguardando pagamento' ou 'confirmada' e ainda não iniciada, quando o Tutor solicitar o cancelamento, então o status muda para 'cancelada' e o host ou prestador é notificado.
*   Se houver pagamento já aprovado, o cancelamento deve disparar o fluxo de estorno (US-HPET28) conforme política vigente (ex.: prazo mínimo para estorno integral).
*   Dado uma contratação sem pagamento aprovado, quando cancelada, então nenhum estorno é disparado.
**Regras de Negócio:**
*   Política de cancelamento (prazos, percentuais de estorno) deve ser definida e exibida ao Tutor antes da confirmação.
*   Não há cobrança de multa ao usuário (DA-11).

#### US-HPET04A-02

Alternativa | Prioridade: Média | HPET04a – Cancelar Reserva/Solicitação | Revisão: Sem alteração
Como Tutor, quero ser informado quando tentar cancelar uma reserva já concluída, para entender por que o cancelamento não pode ser realizado.
**Descrição:** Regra de negócio que impede cancelamento de reservas já finalizadas (checkout já realizado).
**Critérios de Aceite:**
*   Dado uma reserva com status 'concluída', quando o Tutor tentar cancelá-la, então o sistema bloqueia a ação e exibe mensagem explicativa.

### HPET05 – Solicitar Serviço (Pet Sitter/Walker)

#### US-HPET05-01

Principal | Prioridade: Alta | HPET05 – Solicitar Serviço (Pet Sitter/Walker) | Revisão: Alterado

Como Tutor, quero solicitar o serviço de pet sitter ou pet walker para meu pet a um prestador validado, pagando somente após o aceite.

**Ajuste v2:** status inicial "aguardando aceite" e pagamento somente após o aceite; pet selecionado aplica espécie e porte; somente prestadores validados e com conta ativa. Decisão PO 01/11/2026: tutor pode informar medicamentos quando o prestador oferece administração; prestador tem 1 dia útil para responder, senão a solicitação é cancelada automaticamente.

**Descrição:** Análogo ao fluxo de reserva de hospedagem (US-HPET04), porém para serviços domiciliares/passeio. Segue para aceite do profissional (US-HPET17A / US-HPET19) e, depois, para pagamento (US-HPET07A).

**Critérios de Aceite:**

- Dado filtros de localização, preço e disponibilidade e um pet ativo selecionado, quando o Tutor buscar prestadores, então são exibidos somente Pet Sitters/Walkers validados, com conta ativa e compatíveis com espécie e porte do pet.
- Dado um Pet Sitter/Walker disponível na área do Tutor, quando o Tutor solicitar o serviço com pet, data/hora e tipo de serviço, então a solicitação é criada com status 'aguardando aceite' e o profissional é notificado.
- Dado um prestador que oferece administração de medicamentos, quando o Tutor indicar que o pet usa medicamento, então deve informar tipo, horários e duração e forma de aplicação de cada um; se o prestador não oferece, a opção não é exibida.
- Dado uma solicitação sem resposta do prestador em 1 dia útil, quando o prazo vencer, então o status passa a 'cancelada' e o Tutor é notificado.
- O pagamento só é solicitado após o aceite do profissional (US-HPET07A-01).
- A solicitação deve registrar o preço vigente no momento (snapshot).

**Regras de Negócio:**

- Dia útil = segunda a sexta, sem feriados.

**Notas Técnicas:** Endpoint POST /api/solicitacoes-servico (tipo=SITTER|WALKER); status inicial AGUARDANDO\_ACEITE; lista medicamentos\[\] no payload.
#### US-HPET05-02

Alternativa | Prioridade: Alta | HPET05 – Solicitar Serviço (Pet Sitter/Walker) | Revisão: Sem alteração
Como Tutor, quero receber uma mensagem quando os dados da solicitação estiverem incorretos ou incompletos, para corrigi-los.
**Descrição:** Validação de formulário de solicitação de serviço.
**Critérios de Aceite:**
*   Dado um campo obrigatório inválido, quando o Tutor tentar enviar a solicitação, então o campo é destacado com a mensagem específica.

#### US-HPET05-03

Alternativa | Prioridade: Média | HPET05 – Solicitar Serviço (Pet Sitter/Walker) | Revisão: Novo
Como Tutor sem pet ativo cadastrado, quero ser avisado e direcionado ao cadastro de pet ao solicitar um serviço, para conseguir concluir a solicitação.
**Descrição:** Mesmo aviso da tela 08I aplicado à solicitação de Pet Sitter/Walker.
**Critérios de Aceite:**
*   Dado um Tutor sem pet ativo, quando acessar a solicitação de serviço, então o sistema exibe o aviso da tela 08I com atalho para o cadastro de pet (US-HPET02-01).
*   Dado que o Tutor concluiu o cadastro do pet a partir do aviso, quando retornar, então volta à solicitação com o novo pet disponível para seleção.

### HPET06 – Conversar com Host

#### US-HPET06-01

Principal | Prioridade: Média | HPET06 – Conversar com Host | Revisão: Alterado
Como Tutor, quero conversar com o host — diretamente ou por meio do Assistente de IA — para tirar dúvidas antes e durante a reserva.
**Ajuste v2:** inclusão da opção de denunciar mensagem recebida (HPET36, DA-02/DA-13).
**Descrição:** Caso de uso obrigatório de WebSocket do projeto. Chat com roteamento inicial para IA (respondendo com base na FAQ configurada pelo host, US-HPET06B) e escalonamento para humano quando necessário.
**Critérios de Aceite:**
*   Dado o Tutor autenticado, quando abrir o chat de um host, então uma conexão WebSocket é estabelecida e o histórico de mensagens anteriores é carregado.
*   Mensagens enviadas devem ser respondidas primeiramente pela IA (quando configurada), com opção de escalonamento (ver US-HPET06-03/06-04).
*   Mensagens devem ser persistidas no backend (não apenas em memória) para consulta posterior.
*   Dado uma mensagem recebida do host, quando o Tutor selecionar "Denunciar", então segue o fluxo de US-HPET36-01, e somente essa mensagem fica visível à moderação (US-HPET36-04).
**Notas Técnicas:** Canal WebSocket dedicado por conversa; fallback REST para histórico (GET /api/conversas/{id}/mensagens).

#### US-HPET06-02

Alternativa | Prioridade: Média | HPET06 – Conversar com Host | Revisão: Sem alteração
Como Tutor, quero ser impedido de enviar novas mensagens diretas ao host quando a reserva não estiver mais ativa, para entender que o atendimento humano se encerrou (mantendo acesso à IA para dúvidas gerais, se o host permitir).
**Descrição:** Regra de encerramento do canal humano vinculado ao ciclo de vida da reserva.
**Critérios de Aceite:**
*   Dado que a reserva associada à conversa não está mais ativa, quando o Tutor tentar enviar mensagem direta ao host, então o envio é bloqueado com mensagem explicativa.
*   Se o host permitir atendimento por IA fora do escopo de reserva ativa (US-HPET06B-02), o campo de mensagem permanece disponível apenas para a IA.

#### US-HPET06-03

Alternativa | Prioridade: Média | HPET06 – Conversar com Host | Revisão: Sem alteração
Como Tutor, quero que minha conversa seja encaminhada automaticamente ao host quando a IA não conseguir responder à minha dúvida, para não ficar sem solução.
**Descrição:** Escalonamento automático baseado em confiança/cobertura da resposta da IA.
**Critérios de Aceite:**
*   Dado que a IA não encontra resposta com confiança suficiente na base de FAQ, quando isso ocorrer, então a conversa é automaticamente marcada como 'escalonada' e encaminhada à fila do host (US-HPET06C).
*   O Tutor deve ser informado de que a conversa foi encaminhada a um atendente humano.

#### US-HPET06-04

Alternativa | Prioridade: Média | HPET06 – Conversar com Host | Revisão: Sem alteração
Como Tutor, quero solicitar atendimento humano a qualquer momento, para ser atendido pelo host quando a resposta da IA não for suficiente, mantendo o histórico da conversa.
**Descrição:** Escalonamento manual, iniciado pelo próprio Tutor.
**Critérios de Aceite:**
*   Dado uma conversa em andamento com a IA, quando o Tutor solicitar explicitamente falar com atendente humano, então a conversa é marcada como 'escalonada' preservando todo o histórico já trocado.

#### US-HPET06-05

Exceção | Prioridade: Média | HPET06 – Conversar com Host | Revisão: Sem alteração
Como Tutor, quero que o chat tente se reconectar automaticamente após uma perda de conexão, para não perder minhas mensagens.
**Descrição:** Resiliência da conexão WebSocket.
**Critérios de Aceite:**
*   Dado que a conexão WS cai durante uma conversa ativa, quando a rede for restabelecida, então o cliente reconecta automaticamente sem ação do usuário.
*   Deve haver indicador visual de status da conexão (ex.: 'reconectando…').
*   Após reconexão, o histórico permanece íntegro e sincronizado (sem duplicação/perda).
**Regras de Negócio:**
*   Reconexão com backoff (ex.: exponencial) e limite de tentativas antes de alertar falha permanente.
**Notas Técnicas:** Heartbeat/ping-pong para detecção de queda de conexão.

#### US-HPET06-06

Exceção | Prioridade: Média | HPET06 – Conversar com Host | Revisão: Sem alteração
Como Tutor, quero que mensagens não entregues sejam mantidas em fila até a conexão ser restabelecida, para não perder o histórico da conversa.
**Descrição:** Fila de reenvio no cliente e/ou servidor para mensagens enviadas durante instabilidade de rede.
**Critérios de Aceite:**
*   Dado uma mensagem enviada durante queda de conexão, quando a conexão for restabelecida, então a mensagem é reenviada automaticamente respeitando a ordem original.
*   Mensagens em fila devem ter indicador visual de 'enviando/pendente' até confirmação de entrega.
**Notas Técnicas:** Deduplicação por ID de mensagem no servidor para evitar duplicatas em reenvio.

### HPET07 – Cadastrar Forma de Pagamento

#### US-HPET07-01

Principal | Prioridade: Alta | HPET07 – Cadastrar Forma de Pagamento | Revisão: Sem alteração
Como Tutor, quero cadastrar minha forma de pagamento para utilizá-la nas reservas realizadas na plataforma.
**Descrição:** Tokenização de meio de pagamento via gateway, sem armazenar dados sensíveis de cartão na própria base.
**Critérios de Aceite:**
*   Dado dados válidos de cartão/meio de pagamento, quando o Tutor salvar, então o gateway retorna um token que é armazenado (nunca o número completo do cartão).
*   O Tutor pode cadastrar mais de uma forma de pagamento e definir uma como padrão.
**Regras de Negócio:**
*   Dados sensíveis (PAN, CVV) nunca trafegam nem são persistidos pelo backend da plataforma, apenas tokens do gateway (compliance PCI).
**Notas Técnicas:** Integração com API de pagamento sandbox (ex.: tokenização client-side).

#### US-HPET07-02

Alternativa | Prioridade: Alta | HPET07 – Cadastrar Forma de Pagamento | Revisão: Sem alteração
Como Tutor, quero receber uma mensagem quando os dados da forma de pagamento forem inválidos, para corrigi-los antes de salvar.
**Descrição:** Validação de formulário/retorno do gateway na tokenização.
**Critérios de Aceite:**
*   Dado dados de cartão inválidos (ex.: número, validade, CVV), quando o Tutor tentar salvar, então recebe mensagem de erro específica retornada pelo gateway ou validação local.

### HPET07a – Realizar Pagamento

#### US-HPET07A-01

Principal | Prioridade: Alta | HPET07a – Realizar Pagamento | Revisão: Alterado

Como Tutor, quero realizar o pagamento da reserva ou do serviço pela plataforma após o aceite do prestador, para confirmar a contratação.

**Ajuste v2:** pagamento ocorre somente após o aceite do host/prestador (status "aguardando pagamento"); cobre também Pet Sitter/Walker; cupom opcional (HPET32). Decisão PO 01/11/2026: prazo de 1 dia útil após o aceite para pagar, senão a solicitação é cancelada; custos extras (ex.: veterinário causado pelo tutor) pagos ao fim do serviço.

**Descrição:** Cobrança efetiva de uma contratação já aceita pelo prestador, usando forma de pagamento cadastrada. Integra com o backend de pagamento (US-HPET26). Inclui o pagamento complementar de custos extras ao fim do serviço.

**Critérios de Aceite:**

- Dado uma reserva ou serviço com status 'aguardando pagamento' e uma forma de pagamento válida, quando o Tutor confirmar o pagamento, então a transação é enviada ao gateway e, se aprovada, a contratação passa para status 'confirmada' e o prestador é notificado.
- Dado uma contratação ainda 'aguardando aceite', quando o Tutor tentar pagar, então a ação não é oferecida.
- O valor cobrado deve corresponder exatamente ao snapshot de preço registrado na solicitação (US-HPET04-01/US-HPET05-01), descontado o cupom aplicado, quando houver (US-HPET32-01).
- Após a aprovação, o Tutor recebe confirmação e comprovante.
- Dado uma contratação aceita há mais de 1 dia útil sem pagamento, quando o prazo vencer, então o status passa a 'cancelada', a agenda do prestador é liberada e as partes são notificadas.
- Dado um pagamento em processamento no gateway quando o prazo vencer, então a contratação só é cancelada se o gateway recusar o pagamento.
- Dado uma contratação cancelada por prazo, quando o Tutor tentar pagar, então o sistema informa que o prazo venceu.
- Dado custos extras registrados na contratação (ex.: atendimento veterinário causado pelo tutor, US-HPET09-01), quando o serviço for encerrado (check-out ou encerramento da visita/passeio), então o Tutor é solicitado a pagar o valor complementar.

**Regras de Negócio:**

- Dia útil = segunda a sexta, sem feriados.

**Notas Técnicas:** Chamada síncrona/assíncrona ao Sistema de Pagamento (US-HPET26-01); idempotência da requisição para evitar cobrança duplicada; cancelamento por prazo feito por rotina agendada.
#### US-HPET07A-02

Alternativa | Prioridade: Alta | HPET07a – Realizar Pagamento | Revisão: Alterado
Como Tutor, quero ser informado quando meu pagamento for recusado, para tentar com outra forma de pagamento.
**Ajuste v2:** status de permanência renomeado para "aguardando pagamento".
**Descrição:** Tratamento do retorno de recusa do gateway (ver US-HPET26-02).
**Critérios de Aceite:**
*   Dado que o gateway recusa a transação, quando isso ocorrer, então o Tutor recebe mensagem informando a recusa (motivo genérico, sem dados sensíveis do gateway) e a contratação permanece 'aguardando pagamento'.

#### US-HPET07A-03

Alternativa | Prioridade: Média | HPET07a – Realizar Pagamento | Revisão: Sem alteração
Como Tutor, quero selecionar outra forma de pagamento após uma recusa, para tentar concluir a transação.
**Descrição:** Nova tentativa de pagamento sem necessidade de refazer a solicitação de reserva.
**Critérios de Aceite:**
*   Dado um pagamento recusado, quando o Tutor selecionar outra forma de pagamento já cadastrada (ou cadastrar uma nova), então uma nova tentativa de cobrança é disparada para a mesma reserva.

### HPET07b – Consultar Extrato de Pagamentos

#### US-HPET07B-01

Principal | Prioridade: Média | HPET07b – Consultar Extrato de Pagamentos | Revisão: Sem alteração
Como Tutor, quero consultar o histórico de pagamentos realizados para acompanhar meus gastos na plataforma.
**Descrição:** Listagem de transações do Tutor (pagamentos, estornos).
**Critérios de Aceite:**
*   Dado o Tutor autenticado, quando acessar o extrato, então visualiza a lista de transações com data, valor, status e reserva associada, ordenadas da mais recente para a mais antiga.
**Notas Técnicas:** Endpoint GET /api/tutores/{id}/transacoes, com paginação.

#### US-HPET07B-02

Alternativa | Prioridade: Baixa | HPET07b – Consultar Extrato de Pagamentos | Revisão: Sem alteração
Como Tutor, quero ser informado quando não houver transações registradas, para saber que meu extrato está vazio.
**Descrição:** Estado vazio do extrato.
**Critérios de Aceite:**
*   Dado nenhuma transação registrada, quando o Tutor acessar o extrato, então é exibida mensagem indicando que não há transações.

### HPET08 – Avaliar Host/Serviço

#### US-HPET08-01

Principal | Prioridade: Média | HPET08 – Avaliar Host/Serviço | Revisão: Alterado
Como Tutor, quero avaliar uma vez cada contratação concluída com host ou prestador, para compartilhar minha experiência e ajudar outros tutores.
**Ajuste v2:** avaliação sujeita à moderação de conteúdo (HPET42); nova avaliação para a mesma contratação é bloqueada e direcionada à edição (US-HPET08-03), resolvendo o conflito da tela 23.
**Descrição:** Avaliação (nota + comentário) disponível somente após conclusão do serviço (checkout/execução confirmados). Alimenta US-HPET33 (consulta de avaliações).
**Critérios de Aceite:**
*   Dado um serviço concluído (checkout realizado) e ainda não avaliado, quando o Tutor acessar a reserva, então tem a opção de avaliar (nota de 1 a 5 + comentário opcional).
*   Cada reserva/serviço concluído pode ser avaliado apenas uma vez; dado uma contratação já avaliada, quando o Tutor tentar avaliar de novo, então o sistema impede a nova avaliação e oferece a edição (US-HPET08-03).
*   A avaliação deve atualizar a nota média exibida no perfil do host/prestador.
*   A avaliação publicada fica sujeita a denúncia e moderação (HPET36/HPET42).
**Notas Técnicas:** Endpoint POST /api/reservas/{id}/avaliacoes.

#### US-HPET08-02

Alternativa | Prioridade: Baixa | HPET08 – Avaliar Host/Serviço | Revisão: Sem alteração
Como Tutor, quero receber uma mensagem quando não preencher algum campo obrigatório da avaliação, para completá-la corretamente antes de enviar.
**Descrição:** Validação de formulário de avaliação (ex.: nota obrigatória).
**Critérios de Aceite:**
*   Dado o campo de nota não preenchido, quando o Tutor tentar enviar a avaliação, então recebe mensagem indicando que a nota é obrigatória.

#### US-HPET08-03

Alternativa | Prioridade: Baixa | HPET08 – Avaliar Host/Serviço | Revisão: Novo
Como Tutor, quero editar a avaliação que já publiquei, para atualizar minha opinião sem criar uma nova avaliação para a mesma contratação.
**Descrição:** Edição da avaliação existente, mantendo a regra de uma avaliação por contratação (UC HPET08 v2).
**Critérios de Aceite:**
*   Dado uma avaliação publicada pelo Tutor, quando ele selecionar "editar" e salvar nova nota e/ou comentário, então a avaliação é atualizada, sem criar novo registro, e a nota média é recalculada.
*   Dado uma avaliação removida pela moderação (HPET42), quando o Tutor tentar editá-la, então a edição não é permitida.
*   A edição segue as mesmas validações de US-HPET08-02.
**Notas Técnicas:** Endpoint PATCH /api/avaliacoes/{id}; guardar data da última edição.

### HPET08a – Consultar Histórico de Reservas

#### US-HPET08A-01

Principal | Prioridade: Média | HPET08a – Consultar Histórico de Reservas | Revisão: Sem alteração
Como Tutor, quero consultar as reservas realizadas anteriormente para acompanhar meu histórico na plataforma.
**Descrição:** Listagem de reservas do Tutor (todos os status: pendente, confirmada, concluída, cancelada).
**Critérios de Aceite:**
*   Dado o Tutor autenticado, quando acessar 'Minhas reservas', então visualiza a lista com status, host/profissional, pet, datas e valor.
*   A lista deve permitir filtro por tipo de serviço, status e período.
**Notas Técnicas:** Endpoint GET /api/tutores/{id}/reservas.

#### US-HPET08A-02

Alternativa | Prioridade: Baixa | HPET08a – Consultar Histórico de Reservas | Revisão: Sem alteração
Como Tutor, quero ser informado quando não houver reservas registradas, para saber que meu histórico está vazio.
**Descrição:** Estado vazio do histórico de reservas.
**Critérios de Aceite:**
*   Dado nenhuma reserva registrada, quando o Tutor acessar o histórico, então é exibida mensagem indicando lista vazia, com atalho para busca (US-HPET03).

### HPET09 – Acompanhar Rotina do Pet

#### US-HPET09-01

Principal | Prioridade: Média | HPET09 – Acompanhar Rotina do Pet | Revisão: Alterado

Como Tutor, quero acompanhar as atualizações enviadas pelo prestador durante a hospedagem, a visita ou o passeio, e decidir sobre atendimento veterinário quando proposto, para saber como meu pet está.

**Ajuste v2:** Decisão PO 01/11/2026: passa a valer também para visita (Pet Sitter) e passeio (Pet Walker); inclui incidentes e a autorização ou recusa de atendimento veterinário.

**Descrição:** Visualização, pelo Tutor, dos registros de atividade (US-HPET12C) e de execução (US-HPET17B / US-HPET19B), incluindo incidentes e propostas de atendimento veterinário.

**Critérios de Aceite:**

- Dado uma hospedagem, visita ou passeio em andamento, quando o prestador registrar atividades (com ou sem fotos) ou um incidente, então o Tutor visualiza essas atualizações em tempo real ou near-real-time na tela da contratação.
- Atualizações devem ser ordenadas cronologicamente.
- Dado uma proposta de atendimento veterinário após incidente leve ou médio, quando o Tutor autorizar, então o atendimento é registrado como contratação adicional vinculada à original e o prestador é notificado.
- Dado a mesma proposta, quando o Tutor recusar, então a recusa é registrada e o prestador é notificado para seguir a orientação do Tutor.

**Regras de Negócio:**

- Atendimento causado pelo Tutor, sem responsabilidade do prestador: o valor entra como custo extra, pago ao fim do serviço (US-HPET07A-01).
- Incidente grave com possível responsabilidade do prestador: não há autorização prévia; o Administrador decide quem arca com o custo, com base no ocorrido (US-HPET25-01).

**Notas Técnicas:** Pode reutilizar canal WebSocket do chat (US-HPET06) ou notificação push/polling, decisão técnica a alinhar com o time. Endpoint POST /api/incidentes/{id}/autorizacao-veterinaria.
#### US-HPET09-02

Alternativa | Prioridade: Baixa | HPET09 – Acompanhar Rotina do Pet | Revisão: Alterado

Como Tutor, quero ser informado quando ainda não houver registros enviados pelo prestador, para saber que não existem novas atualizações.

**Ajuste v2:** Decisão PO 01/11/2026: vale também para visita e passeio ("host" passa a "prestador").

**Descrição:** Estado vazio da timeline de atividades.

**Critérios de Aceite:**

- Dado nenhuma atividade registrada ainda pelo prestador, quando o Tutor acessar a tela de acompanhamento, então é exibida mensagem indicando que ainda não há atualizações.
### HPET29 – Consultar Central de Notificações

#### US-HPET29-01

Principal | Prioridade: Média | HPET29 – Consultar Central de Notificações | Revisão: Alterado
Como Tutor, quero visualizar as notificações sobre reservas, solicitações, pagamentos e penalidades e acessar a tela relacionada a cada uma, para me manter informado.
**Ajuste v2:** novos eventos (pagamento pendente após aceite, penalidade aplicada ou encerrada) e direcionamento à tela relacionada.
**Descrição:** Central de notificações do Tutor, agregando eventos de reserva, pagamento, chat e penalidades.
**Critérios de Aceite:**
*   Dado eventos relevantes (aceite/recusa de reserva, pagamento pendente após aceite, confirmação de pagamento, mensagem escalonada, penalidade aplicada, revertida ou encerrada), quando ocorridos, então uma notificação é criada e exibida na central, com indicador de não lida.
*   Dado uma notificação selecionada, quando o Tutor a abrir, então é direcionado à tela relacionada (ex.: pagamento pendente – US-HPET07A-01; aviso de penalidade – US-HPET38-01).
*   O Tutor deve poder marcar notificações como lidas.
**Notas Técnicas:** Pode usar WebSocket para push em tempo real, com fallback REST GET /api/tutores/{id}/notificacoes.

#### US-HPET29-02

Alternativa | Prioridade: Baixa | HPET29 – Consultar Central de Notificações | Revisão: Sem alteração
Como Tutor, quero ser informado quando não houver novas notificações, para saber que a central está vazia.
**Descrição:** Estado vazio da central de notificações.
**Critérios de Aceite:**
*   Dado nenhuma notificação registrada, quando o Tutor acessar a central, então é exibida mensagem indicando lista vazia.

### HPET30 – Favoritar Host/Serviço

#### US-HPET30-01

Principal | Prioridade: Baixa | HPET30 – Favoritar Host/Serviço | Revisão: Sem alteração
Como Tutor, quero marcar um host ou prestador de serviço como favorito para acessá-lo rapidamente depois.
**Descrição:** Lista de favoritos do Tutor.
**Critérios de Aceite:**
*   Dado o perfil de um host/prestador, quando o Tutor clicar em favoritar, então o item passa a constar na sua lista de favoritos, com feedback visual imediato.
**Notas Técnicas:** Endpoint POST /api/tutores/{id}/favoritos.

#### US-HPET30-02

Alternativa | Prioridade: Baixa | HPET30 – Favoritar Host/Serviço | Revisão: Sem alteração
Como Tutor, quero remover um host ou prestador dos favoritos para manter minha lista atualizada.
**Descrição:** Remoção de item da lista de favoritos.
**Critérios de Aceite:**
*   Dado um item já favoritado, quando o Tutor desmarcar o favorito, então o item é removido da lista imediatamente.

### HPET31 – Compartilhar Perfil de Host

#### US-HPET31-01

Principal | Prioridade: Baixa | HPET31 – Compartilhar Perfil de Host | Revisão: Sem alteração
Como Tutor, quero compartilhar o perfil de um host para indicá-lo a outras pessoas.
**Descrição:** Geração de link compartilhável (ou integração com share nativo do dispositivo/navegador) do perfil público do host.
**Critérios de Aceite:**
*   Dado o perfil de um host, quando o Tutor acionar 'compartilhar', então um link público (ou ação de compartilhamento do SO) é gerado/disparado com sucesso.

#### US-HPET31-02

Exceção | Prioridade: Baixa | HPET31 – Compartilhar Perfil de Host | Revisão: Sem alteração
Como Tutor, quero receber uma mensagem quando ocorrer falha ao compartilhar o perfil, para saber que o compartilhamento não foi concluído e tentar novamente.
**Descrição:** Tratamento de falha técnica no compartilhamento.
**Critérios de Aceite:**
*   Dado uma falha ao gerar/disparar o compartilhamento, quando ocorrer, então o Tutor recebe mensagem de erro e pode tentar novamente.

### HPET32 – Aplicar Cupom de Desconto

#### US-HPET32-01

Principal | Prioridade: Baixa | HPET32 – Aplicar Cupom de Desconto | Revisão: Alterado
Como Tutor, quero aplicar um cupom de desconto cadastrado pelo Administrador no momento do pagamento, para reduzir o valor da reserva ou do serviço.
**Ajuste v2:** cupons passam a ser criados e mantidos pelo Administrador (HPET40, DA-04); validação por situação, vigência e limite de uso.
**Descrição:** Aplicação de cupom promocional na tela de pagamento, antes da confirmação (US-HPET07A).
**Critérios de Aceite:**
*   Dado um cupom ativo, dentro da vigência e com limite de uso disponível, quando o Tutor aplicá-lo antes de confirmar o pagamento, então o valor final é recalculado com o desconto e exibido antes da confirmação.
**Regras de Negócio:**
*   A validação usa as regras cadastradas pelo Administrador em US-HPET40-01: situação ativa, vigência e limite de uso.
*   Demais regras de elegibilidade (valor mínimo, categoria, uso único por Tutor etc.) a definir.

#### US-HPET32-02

Alternativa | Prioridade: Baixa | HPET32 – Aplicar Cupom de Desconto | Revisão: Alterado
Como Tutor, quero ser informado quando o cupom for inválido, inativo, expirado ou tiver atingido o limite de uso, para corrigir ou substituir o código.
**Ajuste v2:** inclusão dos casos de cupom inativo e limite de uso atingido.
**Descrição:** Validação de cupom.
**Critérios de Aceite:**
*   Dado um código de cupom inválido, inativo, expirado ou que atingiu o limite de uso, quando o Tutor tentar aplicá-lo, então recebe mensagem específica e o valor permanece sem desconto.

### HPET33 – Consultar Avaliações

#### US-HPET33-01

Principal | Prioridade: Baixa | HPET33 – Consultar Avaliações | Revisão: Alterado
Como Tutor, quero consultar as notas e avaliações publicadas de hosts e prestadores para decidir com quem contratar o serviço.
**Ajuste v2:** avaliações removidas pela moderação ficam fora da lista e da média; inclusão da opção de denunciar avaliação (HPET36).
**Descrição:** Exibição pública de nota média e comentários (originados de US-HPET08) no perfil do host/prestador.
**Critérios de Aceite:**
*   Dado um perfil de host/prestador com avaliações, quando o Tutor visualizar o perfil, então vê a nota média e a lista de comentários, ordenados por data (mais recentes primeiro).
*   Avaliações ocultadas ou removidas pela moderação (HPET42) não aparecem na lista nem entram no cálculo da nota média e do número de avaliações.
*   Dado uma avaliação exibida, quando o Tutor selecionar "Denunciar", então segue o fluxo de US-HPET36-01.

#### US-HPET33-02

Alternativa | Prioridade: Baixa | HPET33 – Consultar Avaliações | Revisão: Sem alteração
Como Tutor, quero ser informado quando um host ainda não possuir avaliações, para decidir se quero ser o primeiro a contratá-lo mesmo assim.
**Descrição:** Estado vazio de avaliações no perfil.
**Critérios de Aceite:**
*   Dado um perfil sem avaliações, quando visualizado pelo Tutor, então é exibida mensagem indicando ausência de avaliações (ex.: 'ainda sem avaliações').

Módulo Host / Hotel (Mobile)
----------------------------

### HPET10 – Cadastrar Perfil de Host/Hotel

#### US-HPET10-01

Principal | Prioridade: Alta | HPET10 – Cadastrar Perfil de Host/Hotel | Revisão: Alterado

Como Host, quero me cadastrar na plataforma a partir da minha conta e enviar o perfil para validação, para oferecer hospedagem de pets após a aprovação.

**Ajuste v2:** cadastro feito por usuário já autenticado (papel Tutor, modelo aditivo de papéis); usuário é informado de que o perfil não aparece na busca até a aprovação; rejeição tratada em HPET37. Decisão PO 01/11/2026: Host informa se oferece administração de medicamentos.

**Descrição:** Cadastro do prestador de hospedagem: dados pessoais/empresa, endereço, estrutura, capacidade, espécies/portes aceitos, oferta de administração de medicamentos, fotos da dependência e documentação para validação (US-HPET20B).

**Critérios de Aceite:**

- Dado um usuário autenticado sem perfil de Host, quando acessar o cadastro de host, então o formulário é exibido; dado que já possui perfil de Host, então a opção de novo cadastro não é oferecida.
- Dado o preenchimento dos dados obrigatórios (identificação, endereço, capacidade, documentos), quando o Host submeter o cadastro, então o perfil é criado com status 'pendente de validação' e enviado à fila de validação (US-HPET20B-01).
- Dado que o Host marcou que oferece administração de medicamentos, quando o perfil for aprovado, então os Tutores podem informar medicamentos nas solicitações feitas a ele (US-HPET04-01).
- Após o envio, o sistema informa que o perfil não aparecerá na busca até a aprovação.
- O Host não pode receber solicitações de reserva enquanto o perfil não for validado pelo Administrador (US-HPET20B).
- Dado um cadastro rejeitado, o usuário recebe o feedback e pode corrigir e reenviar (US-HPET37-01).

**Regras de Negócio:**

- O papel de Host é adicionado à conta existente; o papel de Tutor é mantido.

**Notas Técnicas:** Endpoint POST /api/hosts; upload de documentos (armazenamento de arquivos); flag administraMedicamentos no perfil.
#### US-HPET10-02

Alternativa | Prioridade: Alta | HPET10 – Cadastrar Perfil de Host/Hotel | Revisão: Sem alteração
Como Host, quero receber uma mensagem quando os dados do cadastro estiverem incorretos ou incompletos, para corrigi-los.
**Descrição:** Validação de formulário de cadastro de Host.
**Critérios de Aceite:**
*   Dado um campo obrigatório inválido/incompleto, quando o Host tentar submeter, então o campo é destacado com mensagem específica.

### HPET10a – Editar Perfil de Host/Hotel

#### US-HPET10A-01

Principal | Prioridade: Média | HPET10a – Editar Perfil de Host/Hotel | Revisão: Alterado

Como Host, quero alterar os dados do meu perfil (estrutura, capacidade, documentos e oferta de administração de medicamentos) para mantê-lo atualizado, sabendo que alterações sujeitas a validação passam por revalidação.

**Ajuste v2:** regra de revalidação, antes "a confirmar com PO", definida no UC HPET10a v2. Decisão PO 01/11/2026: Host pode ativar ou desativar a oferta de administração de medicamentos.

**Descrição:** Edição de dados do perfil já cadastrado.

**Critérios de Aceite:**

- Dado um Host autenticado, quando alterar dados de estrutura/capacidade/documentos e salvar, então as alterações são persistidas.
- Dado uma alteração em documentos ou dados sujeitos a validação, quando salva, então o perfil é enviado para revalidação (US-HPET20B-01, tipo "revalidação") e o Host é informado e não fica visível nas buscas até a nova aprovação.
- Dado que o Host ativou ou desativou a oferta de administração de medicamentos, quando salvar, então a opção passa a aparecer ou deixa de aparecer nas novas solicitações feitas a ele.
#### US-HPET10A-02

Alternativa | Prioridade: Média | HPET10a – Editar Perfil de Host/Hotel | Revisão: Sem alteração
Como Host, quero receber uma mensagem quando algum dado alterado estiver inválido, para corrigi-lo.
**Descrição:** Validação de formulário de edição de perfil de Host.
**Critérios de Aceite:**
*   Dado um campo alterado inválido, quando o Host tentar salvar, então o campo é destacado com a mensagem de erro.

### HPET10b – Inativar Perfil de Host

#### US-HPET10B-01

Principal | Prioridade: Média | HPET10b – Inativar Perfil de Host | Revisão: Sem alteração
Como Host, quero inativar meu perfil na plataforma para deixar de oferecer serviços temporária ou definitivamente.
**Descrição:** Inativação lógica do perfil, removendo-o das buscas (US-HPET03) sem apagar histórico de reservas/avaliações.
**Critérios de Aceite:**
*   Dado um Host sem reservas ativas/futuras pendentes, quando solicitar a inativação, então o perfil deixa de aparecer em buscas e não recebe novas solicitações.
*   Dado reservas ativas/futuras existentes, quando o Host tentar inativar, então o sistema deve alertar sobre o impacto e definir tratamento (ex.: concluir reservas em andamento antes da inativação efetiva).

#### US-HPET10B-02

Exceção | Prioridade: Média | HPET10b – Inativar Perfil de Host | Revisão: Sem alteração
Como Host, quero receber uma mensagem quando ocorrer falha ao processar a inativação, para saber que meu perfil continua ativo e tentar novamente.
**Descrição:** Tratamento de falha técnica na inativação.
**Critérios de Aceite:**
*   Dado uma falha no processamento, quando ocorrer, então o Host recebe mensagem de erro e o perfil permanece ativo, sem estado inconsistente.

### HPET11 – Gerenciar Agenda

#### US-HPET11-01

Principal | Prioridade: Alta | HPET11 – Gerenciar Agenda | Revisão: Sem alteração
Como Host, quero controlar minha disponibilidade de hospedagem para organizar as reservas recebidas.
**Descrição:** Agenda/calendário do Host, base para a verificação de disponibilidade em US-HPET04-04 e US-HPET03.
**Critérios de Aceite:**
*   Dado o Host autenticado, quando visualizar sua agenda, então vê datas disponíveis, bloqueadas e ocupadas por reservas confirmadas, de forma visualmente distinta.
*   A agenda deve refletir automaticamente reservas aceitas (US-HPET12A), sem exigir bloqueio manual duplicado.
**Notas Técnicas:** Endpoint GET/PUT /api/hosts/{id}/agenda.

#### US-HPET11-02

Alternativa | Prioridade: Alta | HPET11 – Gerenciar Agenda | Revisão: Sem alteração
Como Host, quero bloquear períodos indisponíveis para impedir solicitações em datas nas quais não posso atender.
**Descrição:** Bloqueio manual de datas pelo Host (ex.: manutenção, período pessoal).
**Critérios de Aceite:**
*   Dado um período livre, quando o Host marcar como bloqueado, então esse período deixa de aparecer como disponível na busca (US-HPET03) e novas solicitações nele são impedidas (US-HPET04-04).
*   Não deve ser possível bloquear um período que já possua reserva confirmada, sem antes tratá-la.

#### US-HPET11-03

Alternativa | Prioridade: Média | HPET11 – Gerenciar Agenda | Revisão: Sem alteração
Como Host, quero receber uma mensagem quando os dados de disponibilidade estiverem incorretos, para corrigi-los.
**Descrição:** Validação de datas na agenda (ex.: data final anterior à inicial).
**Critérios de Aceite:**
*   Dado um intervalo de datas inválido, quando o Host tentar salvar, então recebe mensagem de erro específica.

### HPET11a – Definir Preços e Tabela de Serviços

#### US-HPET11A-01

Principal | Prioridade: Alta | HPET11a – Definir Preços e Tabela de Serviços | Revisão: Sem alteração
Como Host, quero definir os preços dos serviços oferecidos para que sejam exibidos aos tutores na busca.
**Descrição:** Tabela de preços por tipo de serviço/porte de pet, usada na busca (US-HPET03) e no cálculo da reserva (US-HPET04).
**Critérios de Aceite:**
*   Dado o Host autenticado, quando definir/alterar preços por serviço e porte, então os valores atualizados passam a ser exibidos em novas buscas e solicitações.
*   Alterações de preço não afetam reservas já solicitadas/confirmadas (snapshot registrado em US-HPET04-01).

#### US-HPET11A-02

Alternativa | Prioridade: Média | HPET11a – Definir Preços e Tabela de Serviços | Revisão: Sem alteração
Como Host, quero receber uma mensagem quando um preço ou dado informado for inválido, para corrigi-lo.
**Descrição:** Validação de valores da tabela de preços (ex.: negativo, não numérico).
**Critérios de Aceite:**
*   Dado um valor de preço inválido, quando o Host tentar salvar, então recebe mensagem de erro específica.

### HPET12 – Receber Solicitação de Reserva

#### US-HPET12-01

Principal | Prioridade: Alta | HPET12 – Receber Solicitação de Reserva | Revisão: Sem alteração
Como Host, quero visualizar e gerenciar as solicitações de hospedagem recebidas para decidir sobre cada uma.
**Descrição:** Fila/lista de solicitações pendentes recebidas pelo Host, oriundas de US-HPET04-01.
**Critérios de Aceite:**
*   Dado solicitações pendentes, quando o Host acessar sua fila, então visualiza dados do Tutor, pet, período e valor, com opção de aceitar/recusar (US-HPET12A).
**Notas Técnicas:** Endpoint GET /api/hosts/{id}/reservas?status=PENDENTE.

#### US-HPET12-02

Alternativa | Prioridade: Baixa | HPET12 – Receber Solicitação de Reserva | Revisão: Sem alteração
Como Host, quero ser informado quando não houver solicitações pendentes, para saber que não existem novos pedidos aguardando análise.
**Descrição:** Estado vazio da fila de solicitações.
**Critérios de Aceite:**
*   Dado nenhuma solicitação pendente, quando o Host acessar a fila, então é exibida mensagem indicando lista vazia.

### HPET12a – Aceitar/Recusar Solicitação de Reserva

#### US-HPET12A-01

Principal | Prioridade: Alta | HPET12a – Aceitar/Recusar Solicitação de Reserva | Revisão: Alterado

Como Host, quero aceitar ou recusar uma solicitação de hospedagem para confirmar minha disponibilidade e liberar o pagamento ao tutor.

**Ajuste v2:** status após o aceite definido como "aguardando pagamento" (antes em aberto), com notificação ao Tutor para pagar. Decisão PO 01/11/2026: prazo de 1 dia útil (segunda a sexta, sem feriados) para responder; ao aceitar solicitação com medicamentos, o Host assume a responsabilidade pela administração.

**Descrição:** Decisão do Host sobre uma solicitação 'aguardando aceite'; ao aceitar, a agenda (US-HPET11) é automaticamente atualizada e segue-se para pagamento (US-HPET07A).

**Critérios de Aceite:**

- Dado uma solicitação 'aguardando aceite', quando o Host aceitar, então o status muda para 'aguardando pagamento', a agenda do Host é bloqueada automaticamente para o período e o Tutor é notificado para realizar o pagamento (US-HPET07A-01).
- Dado uma solicitação 'aguardando aceite', quando o Host recusar, então o status muda para 'recusada' e o Tutor é notificado (ver US-HPET12A-02).
- Dado uma solicitação com medicamentos informados (US-HPET04-01), quando o Host abrir a solicitação, então vê os medicamentos; e, para aceitar, precisa confirmar que assume a responsabilidade pela administração, senão o aceite é bloqueado.
- A reserva só passa a 'confirmada' após o pagamento aprovado (US-HPET26-01).
- Prazo para aceite de reserva é de 1 dia útil (segunda a sexta, sem feriados); sem nenhuma ação, a solicitação é cancelada automaticamente, as datas são liberadas e o Tutor é notificado.
- Dado uma solicitação cancelada por prazo, quando o Host tentar aceitar, então o sistema informa que ela foi cancelada.

**Notas Técnicas:** Campo responsabilidadeMedicamentos no aceite.
#### US-HPET12A-02

Alternativa | Prioridade: Alta | HPET12a – Aceitar/Recusar Solicitação de Reserva | Revisão: Sem alteração
Como Host, quero que o tutor seja notificado automaticamente quando eu recusar uma solicitação, para que ele busque outra opção sem precisar me contatar.
**Descrição:** Notificação automática ao Tutor no evento de recusa.
**Critérios de Aceite:**
*   Dado uma solicitação recusada pelo Host, quando a ação for confirmada, então uma notificação é enviada automaticamente ao Tutor (central de notificações US-HPET29 e, se aplicável, push).

### HPET12b – Confirmar Check-in

#### US-HPET12B-01

Principal | Prioridade: Alta | HPET12b – Confirmar Check-in | Revisão: Alterado
Como Host, quero registrar a chegada do pet no início de uma hospedagem já paga, para dar início ao acompanhamento da rotina.
**Ajuste v2:** check-in exige reserva confirmada com pagamento aprovado.
**Descrição:** Marco que habilita o registro de atividades (US-HPET12C) e o acompanhamento pelo Tutor (US-HPET09).
**Critérios de Aceite:**
*   Dado uma reserva 'confirmada' (pagamento aprovado) na data de início, quando o Host registrar o check-in, então o status da reserva muda para 'em andamento' e o Tutor é notificado.
*   Dado uma reserva ainda 'aguardando pagamento', quando o Host tentar o check-in, então a ação é bloqueada com mensagem explicativa.
**Notas Técnicas:** Endpoint PATCH /api/reservas/{id}/checkin.

#### US-HPET12B-02

Alternativa | Prioridade: Média | HPET12b – Confirmar Check-in | Revisão: Sem alteração
Como Host, quero ser impedido de registrar o check-in antes da data prevista, para evitar inconsistências na hospedagem.
**Descrição:** Regra de negócio de data mínima para check-in.
**Critérios de Aceite:**
*   Dado uma reserva cuja data de início ainda não chegou, quando o Host tentar registrar check-in, então a ação é bloqueada com mensagem explicativa.

### HPET12c – Registrar Atividades do Pet

#### US-HPET12C-01

Principal | Prioridade: Alta | HPET12c – Registrar Atividades do Pet | Revisão: Alterado

Como Host, quero registrar as atividades diárias do pet durante a hospedagem, para manter o tutor informado e comprovar o cuidado prestado.

**Ajuste v2:** RN06 passa a gerar ocorrência para o Administrador quando houver incidente (DA-02), detalhado em US-HPET12C-04. Decisão PO 01/11/2026: RN03 passa a usar os medicamentos informados pelo tutor na solicitação; RN06 inclui gravidade e incidente no imóvel.

**Descrição:** Diário de acompanhamento da estadia. O Host publica de 1 a 2 registros por dia com a descrição das atividades, alimentação, medicação (se aplicável), passeio, banho (se incluso), relato de comportamento e eventuais incidentes. Alimenta o histórico da reserva (US-HPET09) e fica visível ao Tutor.

**Critérios de Aceite:**

- RN01 – Mínimo 1 a 2 registros por dia de hospedagem (do check-in ao check-out).
- RN02 – Obrigatórios sempre: descrição das atividades, alimentação e relato de comportamento.
- RN03 – Medicação obrigatória quando houver medicamentos informados pelo tutor na solicitação (US-HPET04-01): cada aplicação é registrada com data e hora; caso contrário, campo não exibido.
- RN04 – Passeio obrigatório quando incluso no serviço contratado; senão, opcional.
- RN05 – Banho exibido/exigido apenas quando incluso no pacote, na data prevista.
- RN06 – Incidente é opcional; se marcado, exige tipo (com o pet ou no imóvel), descrição, ação tomada e gravidade, notifica o Tutor imediatamente, gera ocorrência para análise do Administrador (US-HPET12C-04) e não conta no limite diário.
- RN07 – Registros só podem ser criados entre check-in confirmado e check-out.
- RN08 – Tutor tem acesso somente leitura; Host recebe alerta de pendência enquanto o dia não tiver registro.
- Dado uma reserva com check-in confirmado, quando o Host registrar uma atividade (texto + tipo), então o registro é salvo e disponibilizado ao Tutor em tempo hábil.

**Notas Técnicas:** Endpoint POST /api/reservas/{id}/atividades.
#### US-HPET12C-02

Alternativa | Prioridade: Média | HPET12c – Registrar Atividades do Pet | Revisão: Sem alteração
Como Host, quero anexar fotos às atividades registradas para complementar as informações da rotina do pet.
**Descrição:** Upload de imagem junto ao registro de atividade.
**Critérios de Aceite:**
*   Dado um registro de atividade, quando o Host anexar uma ou mais fotos, então as imagens são salvas e exibidas junto ao registro para o Tutor.
*   Deve haver limite de tamanho/formato de arquivo aceito (ex.: JPEG/PNG até X MB).
**Notas Técnicas:** Armazenamento de arquivos (storage externo); geração de thumbnail recomendada para performance.

#### US-HPET12C-03

Alternativa | Prioridade: Média | HPET12c – Registrar Atividades do Pet | Revisão: Sem alteração
Como Host, quero receber uma mensagem quando os dados da atividade estiverem incorretos ou incompletos, para corrigi-los.
**Descrição:** Validação de formulário de registro de atividade.
**Critérios de Aceite:**
*   Dado um campo obrigatório inválido/vazio, quando o Host tentar salvar, então recebe mensagem de erro específica.

#### US-HPET12C-04

Alternativa | Prioridade: Alta | HPET12c – Registrar Atividades do Pet | Revisão: Novo

Como Host, quero marcar um registro como incidente, informando tipo, descrição, ação tomada e gravidade, para que o tutor seja avisado com prioridade e o Administrador analise a ocorrência.

**Descrição:** O incidente do relatório diário é uma das origens de ocorrência para o Administrador (DA-02), ao lado da denúncia (HPET36). Decisão PO 01/11/2026: a gravidade (leve, média ou grave) é avaliada pelo Host no momento do ocorrido e revisada depois pelo Administrador, que define a gravidade final.

**Critérios de Aceite:**

- Dado uma hospedagem ativa, quando o Host marcar o registro como incidente e informar tipo (com o pet ou no imóvel), descrição, ação tomada e gravidade, então o registro é salvo, o Tutor recebe notificação prioritária e uma ocorrência com origem "incidente" é criada para o Administrador (US-HPET25-01).
- Dado o incidente marcado sem tipo, descrição, ação tomada ou gravidade, quando o Host tentar salvar, então recebe mensagem indicando os campos obrigatórios.
- Dado um incidente leve ou médio, quando registrado, então o Host aguarda o retorno do Tutor antes de agir; se indicar veterinário, a proposta é enviada para o Tutor autorizar ou recusar (US-HPET09-01).
- Dado um incidente grave, quando registrado, então o Host segue o procedimento padrão, registra a ação tomada e a ocorrência chega ao Administrador marcada para análise de responsabilidade e custos.
- O registro de incidente não conta no limite diário de registros (RN06).

**Notas Técnicas:** Ocorrência vinculada à reserva e ao registro de atividade (origem=INCIDENTE); campos gravidadeInformada e gravidadeFinal.
### HPET12d – Confirmar Check-out

#### US-HPET12D-01

Principal | Prioridade: Alta | HPET12d – Confirmar Check-out | Revisão: Sem alteração
Como Host, quero registrar a saída do pet ao término da hospedagem para encerrar o serviço prestado.
**Descrição:** Marco que conclui a reserva, habilitando avaliação (US-HPET08) e repasse financeiro (US-HPET12F / US-HPET27).
**Critérios de Aceite:**
*   Dado uma reserva 'em andamento', quando o Host registrar o check-out, então o status muda para 'concluída' e o Tutor é notificado.
*   A conclusão da reserva deve disparar o cálculo de repasse ao Host (US-HPET27).
**Notas Técnicas:** Endpoint PATCH /api/reservas/{id}/checkout.

#### US-HPET12D-02

Exceção | Prioridade: Média | HPET12d – Confirmar Check-out | Revisão: Sem alteração
Como Host, quero receber uma mensagem quando ocorrer falha ao registrar o check-out, para saber que a hospedagem ainda não foi encerrada corretamente.
**Descrição:** Tratamento de falha técnica no encerramento da reserva.
**Critérios de Aceite:**
*   Dado uma falha no processamento do check-out, quando ocorrer, então o Host recebe mensagem de erro e a reserva permanece 'em andamento' até nova tentativa bem-sucedida.

### HPET12e – Consultar Financeiro

#### US-HPET12E-01

Principal | Prioridade: Média | HPET12e – Consultar Financeiro | Revisão: Alterado

Como prestador (Host, Pet Sitter ou Pet Walker), quero acompanhar os ganhos obtidos pela plataforma, com valor bruto, taxa da plataforma e valor líquido, para controlar minhas receitas e saber quanto posso solicitar de repasse.

**Ajuste v2:** taxa exibida é a configurada pelo Administrador e vigente na contratação (HPET41). Decisão PO 01/11/2026: painel disponível também para Pet Sitter e Pet Walker, já que o repasse só ocorre mediante solicitação.

**Descrição:** Painel financeiro do prestador: valores recebidos, saldo disponível para repasse, taxas da plataforma.

**Critérios de Aceite:**

- Dado o prestador autenticado, quando acessar o painel financeiro, então visualiza extrato de serviços concluídos com valor bruto, taxa da plataforma descontada, valor líquido e saldo disponível para repasse.
- A taxa exibida em cada transação é a vigente no momento da contratação (US-HPET41-01), não a taxa atual.
- Dado um Pet Sitter ou Pet Walker com serviços concluídos, quando acessar o painel, então vê as mesmas informações e a opção de solicitar repasse (US-HPET12F-01).

**Notas Técnicas:** Endpoint GET /api/prestadores/{id}/financeiro.
#### US-HPET12E-02

Alternativa | Prioridade: Baixa | HPET12e – Consultar Financeiro | Revisão: Alterado

Como prestador, quero ser informado quando não houver transações registradas, para saber que meu painel financeiro está vazio.

**Ajuste v2:** Decisão PO 01/11/2026: vale para Host, Pet Sitter e Pet Walker.

**Descrição:** Estado vazio do painel financeiro.

**Critérios de Aceite:**

- Dado nenhuma transação registrada, quando o prestador acessar o painel, então é exibida mensagem indicando ausência de dados.
### HPET12f – Solicitar Repasse

#### US-HPET12F-01

Principal | Prioridade: Média | HPET12f – Solicitar Repasse | Revisão: Alterado

Como prestador (Host, Pet Sitter ou Pet Walker), quero solicitar o repasse dos valores recebidos para receber o pagamento pelos serviços prestados.

**Ajuste v2:** Decisão PO 01/11/2026: o repasse só ocorre mediante esta solicitação (não é automático) e vale também para Pet Sitter e Pet Walker.

**Descrição:** Saque/transferência do saldo disponível para conta bancária do prestador; integra com US-HPET27 no backend de pagamento.

**Critérios de Aceite:**

- Dado um saldo disponível suficiente, quando o prestador solicitar o repasse, então a solicitação é enviada ao sistema de pagamento e o saldo fica reservado até confirmação da transferência.
- Dado um serviço concluído, quando o prestador não solicitar repasse, então o valor permanece como saldo disponível e nenhuma transferência é feita.

**Notas Técnicas:** Endpoint POST /api/prestadores/{id}/repasses; integração com API de pagamento sandbox.
#### US-HPET12F-02

Alternativa | Prioridade: Média | HPET12f – Solicitar Repasse | Revisão: Alterado

Como prestador, quero ser informado quando meu saldo disponível for insuficiente para o repasse solicitado, para ajustar o valor pedido.

**Ajuste v2:** Decisão PO 01/11/2026: vale para Host, Pet Sitter e Pet Walker.

**Descrição:** Validação de saldo antes do envio da solicitação de repasse.

**Critérios de Aceite:**

- Dado um valor solicitado maior que o saldo disponível, quando o prestador tentar solicitar o repasse, então a ação é bloqueada com mensagem informando o saldo disponível.
#### US-HPET12F-03

Exceção | Prioridade: Média | HPET12f – Solicitar Repasse | Revisão: Novo
Exceção | Prioridade: Média | HPET12f – Solicitar Repasse | Revisão: Novo

Como prestador, quero ser notificado quando meu repasse falhar no processamento, para saber que o valor não foi transferido e que a falha será tratada pelo Administrador.

**Descrição:** Visão do prestador (Host, Pet Sitter ou Pet Walker) sobre o repasse com falha (US-HPET27-03), resolvido pelo Administrador (DA-12, HPET43).

**Critérios de Aceite:**

- Dado um repasse com falha na transferência, quando registrado, então o prestador recebe notificação (central de notificações do seu perfil) e o painel financeiro exibe o repasse com status de falha.
- O valor do repasse com falha não volta como saldo disponível para nova solicitação enquanto estiver em tratamento pelo Administrador.
- Dado que o Administrador resolveu a falha (US-HPET43-01), quando concluída a ação, então o prestador é notificado e o status é atualizado.
### HPET06b – Configurar Assistente de IA do Chat

#### US-HPET06B-01

Principal | Prioridade: Média | HPET06b – Configurar Assistente de IA do Chat | Revisão: Sem alteração
Como Host, quero preencher uma base de FAQ com categorias padrão sugeridas pelo sistema e acrescentar itens livres, para que o Assistente de IA responda automaticamente às dúvidas frequentes dos tutores com informações do meu estabelecimento.
**Descrição:** Configuração da base de conhecimento consumida pela IA em US-HPET06-01/06-03. O sistema apresenta um conjunto de perguntas-base pré-definidas (com pergunta pronta e resposta em branco) que o Host preenche, e permite adicionar itens livres ilimitados (pergunta + resposta próprias).
**Critérios de Aceite:**
Categorias-base sugeridas:
*   Estrutura e espaço – tamanho, ambiente coberto/aberto, climatização, separação por porte, capacidade máxima
*   Equipe – número de cuidadores, capacitação, presença 24h
*   Serviços adicionais – banho, tosa, passeio extra, transporte, adestramento e valores
*   Alimentação – ração fornecida ou do tutor, frequência, dietas especiais
*   Saúde e emergência – medicação, veterinário parceiro, procedimento em emergências, vacinas exigidas
*   Registros de atividade – frequência, conteúdo e fotos dos registros diários (integra HPET12c)
*   Regras e políticas – check-in/check-out, restrições de pets, cancelamento
*   Rotina – horários de alimentação, passeios, descanso e socialização
Demais critérios:
*   Dado o Host autenticado, quando cadastrar itens de FAQ (pergunta + resposta), então esses itens ficam disponíveis para a IA responder no chat.
*   O Host deve poder editar e remover itens da base a qualquer momento.
**Notas Técnicas:** Endpoint CRUD /api/hosts/{id}/faq; possível uso de busca semântica/embedding para o matching de perguntas dos Tutores.

#### US-HPET06B-02

Alternativa | Prioridade: Média | HPET06b – Configurar Assistente de IA do Chat | Revisão: Sem alteração
Como Host, quero habilitar ou desabilitar o atendimento humano direto antes de existir uma reserva, para controlar quando os tutores podem falar comigo.
**Descrição:** Configuração de disponibilidade de atendimento humano pré-reserva, referenciada em US-HPET06-02.
**Critérios de Aceite:**
*   Dado o Host autenticado, quando alternar a opção de atendimento humano pré-reserva, então essa configuração passa a valer imediatamente para novas conversas iniciadas por Tutores sem reserva ativa.

#### US-HPET06B-03

Alternativa | Prioridade: Média | HPET06b – Configurar Assistente de IA do Chat | Revisão: Sem alteração
Como Host, quero receber uma mensagem quando algum campo da base de informações não for preenchido corretamente, para corrigi-lo antes de salvar.
**Descrição:** Validação de formulário de cadastro de FAQ.
**Critérios de Aceite:**
*   Dado um campo obrigatório vazio, quando o Host tentar salvar um item de FAQ, então recebe mensagem de erro específica.

#### US-HPET06B-04

Alternativa | Prioridade: Baixa | HPET06b – Configurar Assistente de IA do Chat | Revisão: Sem alteração
Como Host, quero ser avisado quando o conteúdo cadastrado exceder o limite de caracteres permitido, para ajustar o texto antes de salvar.
**Descrição:** Validação de tamanho de campo na FAQ.
**Critérios de Aceite:**
*   Dado um texto que excede o limite definido, quando o Host tentar salvar, então recebe mensagem indicando o limite e a quantidade excedente.

### HPET06c – Gerenciar Conversas Escaladas pela IA

#### US-HPET06C-01

Principal | Prioridade: Média | HPET06c – Gerenciar Conversas Escaladas pela IA | Revisão: Sem alteração
Como Host, quero visualizar e responder às conversas que o Assistente de IA não conseguiu resolver para dar continuidade ao atendimento do tutor.
**Descrição:** Fila de atendimento humano do Host, alimentada por US-HPET06-03/06-04.
**Critérios de Aceite:**
*   Dado conversas escalonadas, quando o Host acessar sua fila de atendimento, então visualiza a lista com Tutor, histórico e pode responder diretamente via WebSocket.

#### US-HPET06C-02

Alternativa | Prioridade: Baixa | HPET06c – Gerenciar Conversas Escaladas pela IA | Revisão: Sem alteração
Como Host, quero ser informado quando não houver conversas pendentes na fila de escalonamento, para saber que estou em dia com os atendimentos.
**Descrição:** Estado vazio da fila de escalonamento.
**Critérios de Aceite:**
*   Dado nenhuma conversa escalonada pendente, quando o Host acessar a fila, então é exibida mensagem indicando lista vazia.

#### US-HPET06C-03

Exceção | Prioridade: Média | HPET06c – Gerenciar Conversas Escaladas pela IA | Revisão: Sem alteração
Como Host, quero que as mensagens sejam mantidas em fila e reenviadas automaticamente quando a conexão cair, para não perder o atendimento ao tutor.
**Descrição:** Mesma resiliência de US-HPET06-06, aplicada ao lado do Host.
**Critérios de Aceite:**
*   Dado uma mensagem enviada durante queda de conexão do Host, quando a conexão for restabelecida, então a mensagem é reenviada automaticamente na ordem correta.

### HPET06 – Conversar com Host

#### US-HPET06-H01

Principal | Prioridade: Média | HPET06 – Conversar com Host | Revisão: Alterado
Como Host, quero conversar com o tutor — diretamente ou com apoio do Assistente de IA — para esclarecer dúvidas antes e durante a reserva, assumindo o atendimento quando a IA escalonar a conversa.
**Ajuste v2:** inclusão da opção de denunciar mensagem recebida (HPET36).
**Descrição:** Visão do Host do mesmo caso de uso de chat (US-HPET06); contraparte de US-HPET06-01 do lado do Tutor.
**Critérios de Aceite:**
*   Dado uma conversa escalonada ou iniciada diretamente pelo Tutor, quando o Host acessar o chat, então pode visualizar histórico completo e enviar mensagens via WebSocket em tempo real.
*   Dado uma mensagem recebida do Tutor, quando o Host selecionar "Denunciar", então segue o fluxo de US-HPET36-01, e somente essa mensagem fica visível à moderação (US-HPET36-04).

### HPET29 – Consultar Central de Notificações

#### US-HPET29-H01

Principal | Prioridade: Média | HPET29 – Consultar Central de Notificações | Revisão: Alterado
Como Host, quero visualizar as notificações sobre reservas, solicitações, pagamentos, resultado da validação do meu cadastro e penalidades, para me manter informado.
**Ajuste v2:** novos eventos (resultado da validação com feedback, penalidades, repasse com falha) e direcionamento à tela relacionada.
**Descrição:** Central de notificações do Host, análoga à US-HPET29-01 do Tutor.
**Critérios de Aceite:**
*   Dado eventos relevantes (nova solicitação, pagamento confirmado, cancelamento, aprovação ou rejeição do cadastro, penalidade aplicada ou encerrada, repasse com falha), quando ocorridos, então uma notificação é criada e exibida na central do Host, com indicador de não lida.
*   Dado uma notificação selecionada, quando aberta, então o Host é direcionado à tela relacionada (ex.: feedback de rejeição – US-HPET37-01; aviso de penalidade – US-HPET38-01).

Módulo Pet Sitter (Mobile)
--------------------------

### HPET16 – Cadastrar-se como Pet Sitter

#### US-HPET16-01

Principal | Prioridade: Média | HPET16 – Cadastrar-se como Pet Sitter | Revisão: Alterado

Como Pet Sitter, quero me cadastrar na plataforma a partir da minha conta e enviar o perfil para validação, para oferecer serviços de cuidado domiciliar após a aprovação.

**Ajuste v2:** cadastro feito por usuário já autenticado (papel Tutor, modelo aditivo); rejeição tratada em HPET37. Decisão PO 01/11/2026: Pet Sitter informa se oferece administração de medicamentos.

**Descrição:** Cadastro do profissional de cuidado domiciliar, com dados pessoais, documentos, oferta de administração de medicamentos e comprovação de experiência (se aplicável), sujeito a validação (US-HPET20B).

**Critérios de Aceite:**

- Dado um usuário autenticado sem perfil de prestador, quando acessar o cadastro de prestador e selecionar Pet Sitter, então o formulário é exibido.
- Dado o preenchimento dos dados obrigatórios, quando o Pet Sitter submeter o cadastro, então o perfil é criado com status 'pendente de validação' e enviado à fila de validação (US-HPET20B-01).
- Dado que o Pet Sitter marcou que oferece administração de medicamentos, quando o perfil for aprovado, então os Tutores podem informar medicamentos nas solicitações feitas a ele (US-HPET05-01).
- O Pet Sitter não pode receber solicitações enquanto o perfil não for validado.
- Dado um cadastro rejeitado, o usuário recebe o feedback e pode corrigir e reenviar (US-HPET37-01).

**Notas Técnicas:** Endpoint POST /api/pet-sitters; flag administraMedicamentos no perfil.
#### US-HPET16-02

Alternativa | Prioridade: Média | HPET16 – Cadastrar-se como Pet Sitter | Revisão: Sem alteração
Como Pet Sitter, quero receber uma mensagem quando meus dados estiverem incorretos ou incompletos, para corrigi-los.
**Descrição:** Validação de formulário de cadastro.
**Critérios de Aceite:**
*   Dado um campo obrigatório inválido, quando o Pet Sitter tentar submeter, então o campo é destacado com mensagem específica.

### HPET16a – Editar Perfil de Pet Sitter

#### US-HPET16A-01

Principal | Prioridade: Média | HPET16a – Editar Perfil de Pet Sitter | Revisão: Alterado

Como Pet Sitter, quero alterar os dados do meu perfil para mantê-los atualizados, sabendo que alterações sujeitas a validação passam por revalidação.

**Ajuste v2:** inclusão da regra de revalidação (UC HPET16a v2). Decisão PO 01/11/2026: Pet Sitter pode ativar ou desativar a oferta de administração de medicamentos.

**Descrição:** Edição de dados de perfil já cadastrado.

**Critérios de Aceite:**

- Dado o Pet Sitter autenticado, quando alterar dados do perfil e salvar, então as alterações são persistidas.
- Dado uma alteração em documentos ou dados sujeitos a validação, quando salva, então o perfil é enviado para revalidação (US-HPET20B-01) e o Pet Sitter é informado.
- Dado que o Pet Sitter ativou ou desativou a oferta de administração de medicamentos, quando salvar, então a opção passa a aparecer ou deixa de aparecer nas novas solicitações feitas a ele.
#### US-HPET16A-02

Alternativa | Prioridade: Média | HPET16a – Editar Perfil de Pet Sitter | Revisão: Sem alteração
Como Pet Sitter, quero receber uma mensagem quando algum dado alterado estiver incorreto, para corrigi-lo.
**Descrição:** Validação de formulário de edição de perfil.
**Critérios de Aceite:**
*   Dado um campo alterado inválido, quando o Pet Sitter tentar salvar, então recebe mensagem de erro específica.

### HPET16b – Definir Área de Atuação/Disponibilidade

#### US-HPET16B-01

Principal | Prioridade: Média | HPET16b – Definir Área de Atuação/Disponibilidade | Revisão: Sem alteração
Como Pet Sitter, quero definir minha área de atuação e disponibilidade para receber solicitações compatíveis.
**Descrição:** Definição de raio/região de atendimento e agenda de disponibilidade, usadas na busca/match com solicitações (US-HPET05, US-HPET17).
**Critérios de Aceite:**
*   Dado o Pet Sitter autenticado, quando definir região e disponibilidade, então essas informações passam a ser usadas para filtrar quais solicitações lhe são direcionadas/exibidas.

#### US-HPET16B-02

Alternativa | Prioridade: Baixa | HPET16b – Definir Área de Atuação/Disponibilidade | Revisão: Sem alteração
Como Pet Sitter, quero receber uma mensagem quando minha área ou disponibilidade estiver inválida, para corrigi-la.
**Descrição:** Validação de dados de área/disponibilidade.
**Critérios de Aceite:**
*   Dado um dado de área ou disponibilidade inválido, quando o Pet Sitter tentar salvar, então recebe mensagem de erro específica.

### HPET17 – Receber Solicitação de Visita

#### US-HPET17-01

Principal | Prioridade: Média | HPET17 – Receber Solicitação de Visita | Revisão: Sem alteração
Como Pet Sitter, quero receber as solicitações de cuidado domiciliar para avaliar e responder a cada pedido.
**Descrição:** Fila de solicitações pendentes recebidas pelo Pet Sitter, oriundas de US-HPET05-01.
**Critérios de Aceite:**
*   Dado solicitações pendentes, quando o Pet Sitter acessar sua fila, então visualiza dados do Tutor, pet, data/hora e valor, com opção de aceitar/recusar (US-HPET17A).
*   Prazo para aceite de solicitação é de 1 dia útil, caso não seja feito nenhuma ação é cancelada automaticamente.

#### US-HPET17-02

Alternativa | Prioridade: Baixa | HPET17 – Receber Solicitação de Visita | Revisão: Sem alteração
Como Pet Sitter, quero ser informado quando não houver solicitações pendentes, para saber que não existem novos atendimentos.
**Descrição:** Estado vazio da fila de solicitações.
**Critérios de Aceite:**
*   Dado nenhuma solicitação pendente, quando o Pet Sitter acessar a fila, então é exibida mensagem indicando lista vazia.

### HPET17a – Aceitar/Recusar Solicitação de Visita

#### US-HPET17A-01

Principal | Prioridade: Média | HPET17a – Aceitar/Recusar Solicitação de Visita | Revisão: Alterado

Como Pet Sitter, quero aceitar ou recusar uma solicitação recebida para confirmar minha disponibilidade e liberar o pagamento ao tutor.

**Ajuste v2:** status após o aceite passa de "confirmada" para "aguardando pagamento"; confirmação somente após o pagamento. Decisão PO 01/11/2026: prazo de 1 dia útil (segunda a sexta, sem feriados); ao aceitar solicitação com medicamentos, o Pet Sitter assume a responsabilidade pela administração.

**Descrição:** Decisão sobre solicitação 'aguardando aceite'; ao aceitar, a disponibilidade do profissional é bloqueada para o horário.

**Critérios de Aceite:**

- Dado uma solicitação 'aguardando aceite', quando o Pet Sitter aceitar, então o status muda para 'aguardando pagamento', o horário correspondente é bloqueado em sua agenda e o Tutor é notificado para realizar o pagamento (US-HPET07A-01).
- Dado uma solicitação 'aguardando aceite', quando o Pet Sitter recusar, então o status muda para 'recusada' e o Tutor é notificado (US-HPET17A-02).
- Dado uma solicitação com medicamentos informados (US-HPET05-01), quando o Pet Sitter abrir a solicitação, então vê os medicamentos; e, para aceitar, precisa confirmar que assume a responsabilidade pela administração, senão o aceite é bloqueado.
- A solicitação só passa a 'confirmada' após o pagamento aprovado.
- Prazo para aceite da solicitação é de 1 dia útil (segunda a sexta, sem feriados); sem nenhuma ação, a solicitação é cancelada automaticamente, o horário é liberado e o Tutor é notificado.

**Notas Técnicas:** Campo responsabilidadeMedicamentos no aceite.
#### US-HPET17A-02

Alternativa | Prioridade: Média | HPET17a – Aceitar/Recusar Solicitação de Visita | Revisão: Sem alteração
Como Pet Sitter, quero que o tutor seja notificado automaticamente quando eu recusar uma solicitação, para que ele busque outro profissional.
**Descrição:** Notificação automática ao Tutor no evento de recusa.
**Critérios de Aceite:**
*   Dado uma solicitação recusada, quando confirmada, então o Tutor recebe notificação automática.

### HPET17b – Registrar Execução do Serviço

#### US-HPET17B-01

Principal | Prioridade: Média | HPET17b – Registrar Execução do Serviço | Revisão: Alterado

Como Pet Sitter, quero registrar o check-in e o check-out do atendimento com localização e foto de comprovação, para comprovar ao tutor o serviço efetivamente prestado.

**Ajuste v2:** execução exige solicitação aceita e paga. Decisão PO 01/11/2026: registro de cada aplicação dos medicamentos informados e de incidentes com gravidade (inclusive no imóvel), que geram ocorrência para o Administrador.

**Descrição:** Registro da execução do serviço de cuidado domiciliar. O Pet Sitter realiza check-in ao chegar e check-out ao encerrar, ambos com captura de localização e foto de comprovação, além de observações do atendimento, aplicação de medicamentos e eventuais incidentes. Análogo a US-HPET12C/12D da hospedagem. Ao concluir, habilita a avaliação (US-HPET08).

**Critérios de Aceite:**

- Dado uma solicitação 'confirmada' (aceita e paga – US-HPET07A-01) e no horário previsto, quando o Pet Sitter registrar a execução, então o status muda para 'concluído' e o Tutor é notificado, habilitando avaliação (US-HPET08).
- Dado uma solicitação ainda 'aguardando pagamento', quando o Pet Sitter tentar registrar a execução, então a ação é bloqueada.
- Dado uma visita com medicamentos informados, quando o Pet Sitter registrar a aplicação, então o registro aparece para o Tutor com data e hora.
- Dado uma visita em andamento, quando o Pet Sitter registrar um incidente com tipo (com o pet ou no imóvel), descrição, ação tomada e gravidade avaliada no momento, então o Tutor recebe notificação prioritária e uma ocorrência é criada para o Administrador, que revisa a gravidade (US-HPET25-01).
- Dado um incidente leve ou médio, o Pet Sitter aguarda o retorno do Tutor antes de agir; se indicar veterinário, a proposta vai para o Tutor autorizar ou recusar (US-HPET09-01). Dado um incidente grave, o Pet Sitter segue o procedimento padrão e a ocorrência é marcada para análise de responsabilidade e custos.

**Notas Técnicas:** Endpoint POST /api/solicitacoes-servico/{id}/incidentes (multipart para fotos).
#### US-HPET17B-02

Alternativa | Prioridade: Baixa | HPET17b – Registrar Execução do Serviço | Revisão: Sem alteração
Como Pet Sitter, quero receber uma mensagem quando os dados da execução estiverem incorretos ou incompletos, para corrigi-los.
**Descrição:** Validação de formulário de registro de execução.
**Critérios de Aceite:**
*   Dado um campo obrigatório inválido, quando o Pet Sitter tentar salvar, então recebe mensagem de erro específica.

### HPET29 – Consultar Central de Notificações

#### US-HPET29-S01

Principal | Prioridade: Baixa | HPET29 – Consultar Central de Notificações | Revisão: Alterado
Como Pet Sitter, quero visualizar as notificações sobre solicitações, pagamentos, resultado da validação do meu cadastro e penalidades, para me manter informado.
**Ajuste v2:** novos eventos (validação do cadastro, penalidades) e direcionamento à tela relacionada.
**Descrição:** Central de notificações do Pet Sitter, análoga à US-HPET29-01.
**Critérios de Aceite:**
*   Dado eventos relevantes (nova solicitação, pagamento/repasse, aprovação ou rejeição do cadastro, penalidade aplicada ou encerrada), quando ocorridos, então uma notificação é criada e exibida na central do Pet Sitter.
*   Dado uma notificação selecionada, quando aberta, então o Pet Sitter é direcionado à tela relacionada (US-HPET37-01, US-HPET38-01).

Módulo Pet Walker (Mobile)
--------------------------

### HPET18 – Cadastrar-se como Pet Walker

#### US-HPET18-01

Principal | Prioridade: Média | HPET18 – Cadastrar-se como Pet Walker | Revisão: Alterado

Como Pet Walker, quero me cadastrar na plataforma a partir da minha conta e enviar o perfil para validação, para oferecer serviços de passeio com pets após a aprovação.

**Ajuste v2:** cadastro feito por usuário já autenticado (papel Tutor, modelo aditivo); rejeição tratada em HPET37. Decisão PO 01/11/2026: Pet Walker informa se oferece administração de medicamentos.

**Descrição:** Cadastro do profissional de passeio, análogo ao cadastro de Pet Sitter (US-HPET16-01), sujeito a validação (US-HPET20B).

**Critérios de Aceite:**

- Dado um usuário autenticado sem perfil de prestador, quando acessar o cadastro de prestador e selecionar Pet Walker, então o formulário é exibido.
- Dado o preenchimento dos dados obrigatórios, quando o Pet Walker submeter o cadastro, então o perfil é criado com status 'pendente de validação' e enviado à fila de validação (US-HPET20B-01).
- Dado que o Pet Walker marcou que oferece administração de medicamentos, quando o perfil for aprovado, então os Tutores podem informar medicamentos nas solicitações feitas a ele (US-HPET05-01).
- Dado um cadastro rejeitado, o usuário recebe o feedback e pode corrigir e reenviar (US-HPET37-01).
#### US-HPET18-02

Alternativa | Prioridade: Média | HPET18 – Cadastrar-se como Pet Walker | Revisão: Sem alteração
Como Pet Walker, quero receber uma mensagem quando meus dados estiverem incorretos ou incompletos, para corrigi-los.
**Descrição:** Validação de formulário de cadastro.
**Critérios de Aceite:**
*   Dado um campo obrigatório inválido, quando o Pet Walker tentar submeter, então o campo é destacado com mensagem específica.

### HPET18a – Editar Perfil de Pet Walker

#### US-HPET18A-01

Principal | Prioridade: Média | HPET18a – Editar Perfil de Pet Walker | Revisão: Alterado

Como Pet Walker, quero alterar os dados do meu perfil para mantê-los atualizados, sabendo que alterações sujeitas a validação passam por revalidação.

**Ajuste v2:** inclusão da regra de revalidação (UC HPET18a v2). Decisão PO 01/11/2026: Pet Walker pode ativar ou desativar a oferta de administração de medicamentos.

**Descrição:** Edição de dados de perfil já cadastrado.

**Critérios de Aceite:**

- Dado o Pet Walker autenticado, quando alterar dados do perfil e salvar, então as alterações são persistidas.
- Dado uma alteração em documentos ou dados sujeitos a validação, quando salva, então o perfil é enviado para revalidação (US-HPET20B-01) e o Pet Walker é informado.
- Dado que o Pet Walker ativou ou desativou a oferta de administração de medicamentos, quando salvar, então a opção passa a aparecer ou deixa de aparecer nas novas solicitações feitas a ele.
#### US-HPET18A-02

Alternativa | Prioridade: Baixa | HPET18a – Editar Perfil de Pet Walker | Revisão: Sem alteração
Como Pet Walker, quero receber uma mensagem quando algum dado alterado estiver incorreto, para corrigi-lo.
**Descrição:** Validação de formulário de edição de perfil.
**Critérios de Aceite:**
*   Dado um campo alterado inválido, quando o Pet Walker tentar salvar, então recebe mensagem de erro específica.

### HPET18b – Definir Área de Atuação/Disponibilidade

#### US-HPET18B-01

Principal | Prioridade: Média | HPET18b – Definir Área de Atuação/Disponibilidade | Revisão: Sem alteração
Como Pet Walker, quero definir minha região de atuação e disponibilidade para receber solicitações compatíveis.
**Descrição:** Definição de raio/região e agenda de disponibilidade, análoga à US-HPET16B-01.
**Critérios de Aceite:**
*   Dado o Pet Walker autenticado, quando definir região e disponibilidade, então essas informações passam a ser usadas no match de solicitações de passeio.

#### US-HPET18B-02

Alternativa | Prioridade: Baixa | HPET18b – Definir Área de Atuação/Disponibilidade | Revisão: Sem alteração
Como Pet Walker, quero receber uma mensagem quando minha região ou disponibilidade estiver inválida, para corrigi-la.
**Descrição:** Validação de dados de área/disponibilidade.
**Critérios de Aceite:**
*   Dado um dado de área ou disponibilidade inválido, quando o Pet Walker tentar salvar, então recebe mensagem de erro específica.

### HPET19 – Aceitar/Recusar Passeio

#### US-HPET19-01

Principal | Prioridade: Média | HPET19 – Aceitar/Recusar Passeio | Revisão: Alterado

Como Pet Walker, quero aceitar ou recusar solicitações de passeio para confirmar minha disponibilidade e liberar o pagamento ao tutor.

**Ajuste v2:** caso de uso renomeado para "Aceitar/Recusar Passeio"; status após o aceite passa de "confirmado" para "aguardando pagamento". Decisão PO 01/11/2026: prazo de 1 dia útil (segunda a sexta, sem feriados); ao aceitar solicitação com medicamentos, o Pet Walker assume a responsabilidade pela administração.

**Descrição:** Decisão sobre solicitação de passeio 'aguardando aceite' (oriunda de US-HPET05-01), análoga a US-HPET17A-01.

**Critérios de Aceite:**

- Dado uma solicitação de passeio 'aguardando aceite', quando o Pet Walker aceitar, então o status muda para 'aguardando pagamento', o horário é bloqueado em sua agenda e o Tutor é notificado para realizar o pagamento (US-HPET07A-01).
- Dado uma solicitação 'aguardando aceite', quando o Pet Walker recusar, então o status muda para 'recusado' e o Tutor é notificado (US-HPET19-02).
- Dado uma solicitação com medicamentos informados (US-HPET05-01), quando o Pet Walker abrir a solicitação, então vê os medicamentos; e, para aceitar, precisa confirmar que assume a responsabilidade pela administração, senão o aceite é bloqueado.
- O passeio só passa a 'confirmado' após o pagamento aprovado.
- Prazo para aceite da solicitação é de 1 dia útil (segunda a sexta, sem feriados); sem nenhuma ação, a solicitação é cancelada automaticamente, o horário é liberado e o Tutor é notificado.

**Notas Técnicas:** Campo responsabilidadeMedicamentos no aceite.
#### US-HPET19-02

Alternativa | Prioridade: Média | HPET19 – Aceitar/Recusar Passeio | Revisão: Alterado
Como Pet Walker, quero que o tutor seja notificado automaticamente quando eu recusar uma solicitação de passeio, para que ele busque outro profissional.
**Ajuste v2:** apenas o nome do caso de uso foi atualizado.
**Descrição:** Notificação automática ao Tutor no evento de recusa.
**Critérios de Aceite:**
*   Dado uma solicitação de passeio recusada, quando confirmada, então o Tutor recebe notificação automática.

### HPET19b – Registrar Execução do Passeio

#### US-HPET19B-01

Principal | Prioridade: Média | HPET19b – Registrar Execução do Passeio | Revisão: Alterado

Como Pet Walker, quero registrar o início e o fim do passeio com localização e foto de comprovação, para comprovar ao tutor o atendimento efetivamente prestado.

**Ajuste v2:** execução exige solicitação aceita e paga. Decisão PO 01/11/2026: registro de cada aplicação dos medicamentos informados e de incidentes com gravidade, que geram ocorrência para o Administrador.

**Descrição:** Registro da execução do passeio. O Pet Walker realiza check-in ao retirar o pet e check-out ao devolvê-lo, ambos com captura de localização e foto de comprovação, além de observações do passeio (comportamento, necessidades fisiológicas, ocorrências), aplicação de medicamentos e eventuais incidentes. O sistema calcula duração e trajeto percorrido. Ao concluir, habilita a avaliação (US-HPET08).

**Critérios de Aceite:**

- Dado uma solicitação 'confirmada' (aceita e paga – US-HPET07A-01) e no horário previsto, quando o Pet Walker registrar a execução, então o status muda para 'concluído' e o Tutor é notificado.
- Dado uma solicitação ainda 'aguardando pagamento', quando o Pet Walker tentar registrar a execução, então a ação é bloqueada.
- Dado um passeio com medicamentos informados, quando o Pet Walker registrar a aplicação, então o registro aparece para o Tutor com data e hora.
- Dado um passeio em andamento, quando o Pet Walker registrar um incidente com tipo (com o pet ou no imóvel), descrição, ação tomada e gravidade avaliada no momento, então o Tutor recebe notificação prioritária e uma ocorrência é criada para o Administrador, que revisa a gravidade (US-HPET25-01).
- Dado um incidente leve ou médio, o Pet Walker aguarda o retorno do Tutor antes de agir; se indicar veterinário, a proposta vai para o Tutor autorizar ou recusar (US-HPET09-01). Dado um incidente grave, o Pet Walker segue o procedimento padrão e a ocorrência é marcada para análise de responsabilidade e custos.

**Notas Técnicas:** Endpoint POST /api/solicitacoes-servico/{id}/incidentes (multipart para fotos).
#### US-HPET19B-02

Alternativa | Prioridade: Baixa | HPET19b – Registrar Execução do Passeio | Revisão: Sem alteração
Como Pet Walker, quero receber uma mensagem quando os dados da execução estiverem incorretos ou incompletos, para corrigi-los.
**Descrição:** Validação de formulário de registro de execução.
**Critérios de Aceite:**
*   Dado um campo obrigatório inválido, quando o Pet Walker tentar salvar, então recebe mensagem de erro específica.

### HPET29 – Consultar Central de Notificações

#### US-HPET29-W01

Principal | Prioridade: Baixa | HPET29 – Consultar Central de Notificações | Revisão: Alterado
Como Pet Walker, quero visualizar as notificações sobre solicitações, pagamentos, resultado da validação do meu cadastro e penalidades, para me manter informado.
**Ajuste v2:** novos eventos (validação do cadastro, penalidades) e direcionamento à tela relacionada.
**Descrição:** Central de notificações do Pet Walker, análoga à US-HPET29-01.
**Critérios de Aceite:**
*   Dado eventos relevantes (nova solicitação, pagamento/repasse, aprovação ou rejeição do cadastro, penalidade aplicada ou encerrada), quando ocorridos, então uma notificação é criada e exibida na central do Pet Walker.
*   Dado uma notificação selecionada, quando aberta, então o Pet Walker é direcionado à tela relacionada (US-HPET37-01, US-HPET38-01).

Comum a todos os perfis do app (Mobile)
---------------------------------------

### HPET36 – Denunciar Usuário ou Conteúdo

#### US-HPET36-01

Principal | Prioridade: Alta | HPET36 – Denunciar Usuário ou Conteúdo | Revisão: Novo
Como usuário do app, quero denunciar um usuário ou conteúdo (perfil, avaliação, foto ou mensagem de chat) que descumpra as regras, para que o Administrador analise o caso.
**Descrição:** Denúncia disponível para todos os perfis do app; é uma das origens de ocorrência para o Administrador (DA-02) e, quando o alvo é conteúdo, gera item de moderação (HPET42).
**Critérios de Aceite:**
*   Dado um perfil, avaliação, foto ou mensagem de chat visível ao usuário, quando ele selecionar "Denunciar", então o sistema exibe a lista de motivos disponíveis.
*   Dado um motivo selecionado (e descrição opcional), quando o usuário confirmar, então a denúncia é registrada com item denunciado, denunciante e data, e o usuário recebe confirmação do envio.
*   Dado uma denúncia registrada, então o sistema cria uma ocorrência com origem "denúncia" para o Administrador (US-HPET25-01) e, se o alvo for conteúdo, um item na fila de moderação (US-HPET42-01).
*   O usuário denunciado não é informado sobre quem fez a denúncia.
**Regras de Negócio:**
*   Motivos de denúncia vêm de lista pré-definida.
*   Usuário não pode denunciar o próprio conteúdo.
**Notas Técnicas:** Endpoint POST /api/denuncias (tipoAlvo=PERFIL|AVALIACAO|FOTO|MENSAGEM, alvoId, motivo, descricao).

#### US-HPET36-02

Alternativa | Prioridade: Média | HPET36 – Denunciar Usuário ou Conteúdo | Revisão: Novo
Como usuário do app, quero receber uma mensagem quando não selecionar o motivo da denúncia, para completá-la antes de enviar.
**Descrição:** Validação do formulário de denúncia.
**Critérios de Aceite:**
*   Dado nenhum motivo selecionado, quando o usuário tentar confirmar a denúncia, então recebe mensagem indicando que o motivo é obrigatório e nada é registrado.

#### US-HPET36-03

Alternativa | Prioridade: Baixa | HPET36 – Denunciar Usuário ou Conteúdo | Revisão: Novo
Como usuário do app, quero ser informado quando já tiver denunciado o mesmo item, para saber que a denúncia já está em análise.
**Descrição:** Prevenção de denúncias duplicadas do mesmo usuário para o mesmo item.
**Critérios de Aceite:**
*   Dado que o usuário já denunciou o item e a denúncia ainda está em análise, quando tentar denunciá-lo novamente, então o sistema informa que a denúncia já está em análise e não cria novo registro.

#### US-HPET36-04

Alternativa | Prioridade: Alta | HPET36 – Denunciar Usuário ou Conteúdo | Revisão: Novo
Como usuário do app, quero que, ao denunciar uma mensagem de chat, somente a mensagem denunciada seja enviada à moderação, para preservar a privacidade do restante da conversa.
**Descrição:** Regra de privacidade da moderação de chat (DA-13).
**Critérios de Aceite:**
*   Dado uma denúncia de mensagem de chat, quando registrada, então somente a mensagem denunciada (conteúdo, autor e data) fica disponível à moderação.
*   O restante da conversa só pode ser visto pelo Administrador mediante acesso concedido (US-HPET42A-01).

#### US-HPET36-05

Exceção | Prioridade: Média | HPET36 – Denunciar Usuário ou Conteúdo | Revisão: Novo
Como usuário do app, quero ser informado quando ocorrer falha ao registrar a denúncia, para tentar novamente.
**Descrição:** Tratamento de falha técnica no envio da denúncia.
**Critérios de Aceite:**
*   Dado uma falha ao registrar, quando ocorrer, então o usuário recebe mensagem de erro, nenhuma denúncia parcial é criada e o motivo informado é mantido para nova tentativa.

### HPET37 – Corrigir e Reenviar Cadastro Rejeitado

#### US-HPET37-01

Principal | Prioridade: Alta | HPET37 – Corrigir e Reenviar Cadastro Rejeitado | Revisão: Novo
Como Host, Pet Sitter ou Pet Walker com cadastro rejeitado, quero consultar o feedback do Administrador, corrigir os itens apontados e reenviar o cadastro, para que ele seja validado novamente.
**Descrição:** Fluxo de correção e reenvio após rejeição com feedback (DA-08).
**Critérios de Aceite:**
*   Dado um cadastro rejeitado, quando o usuário abrir a notificação de rejeição ou a área do seu perfil de prestador, então o sistema exibe o status 'rejeitado' e o feedback do Administrador com os itens a ajustar.
*   Dado que o usuário corrigiu dados e/ou enviou novos documentos, quando confirmar o reenvio, então o status volta para 'pendente de validação', o cadastro é marcado como "reenvio" e retorna à fila do Administrador (US-HPET20B-01).
*   O histórico de rejeições e feedbacks anteriores é mantido.
*   Só tem a opção de reenvio caso seja somente necessário ajustes, caso seja um cadastro negado definitivo não será permitido o reenvio de solicitação de cadastro.
**Notas Técnicas:** Endpoint POST /api/prestadores/{id}/reenvio.

#### US-HPET37-02

Alternativa | Prioridade: Média | HPET37 – Corrigir e Reenviar Cadastro Rejeitado | Revisão: Novo
Como Host, Pet Sitter ou Pet Walker, quero receber uma mensagem quando algum dado corrigido estiver incorreto, para ajustá-lo antes do reenvio.
**Descrição:** Validação dos campos alterados no reenvio.
**Critérios de Aceite:**
*   Dado um campo corrigido inválido, quando o usuário tentar reenviar, então o campo é destacado com mensagem específica e o cadastro não é reenviado.

#### US-HPET37-03

Alternativa | Prioridade: Baixa | HPET37 – Corrigir e Reenviar Cadastro Rejeitado | Revisão: Novo
Como Host, Pet Sitter ou Pet Walker, quero ser alertado quando tentar reenviar sem alterar nenhum item apontado no feedback, para confirmar se desejo reenviar assim mesmo.
**Descrição:** Confirmação extra para reenvio sem alterações.
**Critérios de Aceite:**
*   Dado que nenhum item apontado no feedback foi alterado, quando o usuário confirmar o reenvio, então o sistema solicita uma segunda confirmação antes de devolver o cadastro à fila.

### HPET38 – Visualizar Aviso de Penalidade

#### US-HPET38-01

Principal | Prioridade: Alta | HPET38 – Visualizar Aviso de Penalidade | Revisão: Novo
Como usuário do app, quero visualizar a penalidade recebida, com tipo, motivo e prazo, e confirmar a ciência, para entender o que aconteceu e as consequências na minha conta.
**Descrição:** Comunicação de penalidades aplicadas em HPET25 (advertência, suspensão com prazo ou banimento — DA-01/DA-11).
**Critérios de Aceite:**
*   Dado uma penalidade aplicada, quando registrada, então o usuário recebe notificação (US-HPET29).
*   Dado uma advertência não visualizada, quando o usuário acessar o app, então o aviso é exibido com tipo, motivo e data, e a navegação só segue após a confirmação de ciência.
*   Dado a confirmação de ciência, então o sistema registra a data da ciência e libera a navegação permitida ao estado da conta.
**Notas Técnicas:** Endpoints GET /api/usuarios/me/penalidades e POST /api/penalidades/{id}/ciencia.

#### US-HPET38-02

Alternativa | Prioridade: Alta | HPET38 – Visualizar Aviso de Penalidade | Revisão: Novo
Como usuário do app com conta suspensa ou banida, quero ver o aviso da penalidade ao tentar entrar, para entender por que meu acesso está bloqueado e até quando.
**Descrição:** Aviso exibido no login para penalidades que bloqueiam o acesso (integra US-HPET01A-03).
**Critérios de Aceite:**
*   Dado uma conta suspensa, quando o usuário tentar logar, então o aviso mostra motivo resumido e data de fim, e o acesso permanece bloqueado.
*   Dado uma conta banida, quando o usuário tentar logar, então o aviso informa o banimento e o acesso permanece bloqueado.

#### US-HPET38-03

Alternativa | Prioridade: Média | HPET38 – Visualizar Aviso de Penalidade | Revisão: Novo
Como usuário do app, quero ser notificado quando minha penalidade for revertida ou encerrada, para saber que minha conta voltou ao normal.
**Descrição:** Comunicação do fim da penalidade, por reversão do Administrador ou fim automático do prazo (HPET25a).
**Critérios de Aceite:**
*   Dado uma penalidade revertida (US-HPET25A-01) ou suspensão encerrada pelo prazo (US-HPET25A-02), quando isso ocorrer, então o usuário recebe notificação e, se a conta foi restaurada, volta a ter acesso no próximo login.

### HPET39 – Inativar Própria Conta

#### US-HPET39-01

Principal | Prioridade: Baixa | HPET39 – Inativar Própria Conta | Revisão: Novo
Como usuário do app, quero inativar minha própria conta, preservando meus dados e histórico, para deixar de usar a plataforma.
**Descrição:** Leva a conta ao estado "inativa" da máquina de estados (DA-01), a partir da opção de excluir conta do perfil (tela 31B). Inativação lógica.
**Critérios de Aceite:**
*   Dado o usuário autenticado em Privacidade e segurança, quando selecionar inativar a conta, então o sistema informa os efeitos da inativação e solicita confirmação.
*   Dado a confirmação, então o estado da conta passa a 'inativa', os perfis de prestador saem da busca, a sessão é encerrada. (US-HPET01A-03).
*   Dados e histórico são preservados (sem exclusão física).
**Notas Técnicas:** Endpoint POST /api/usuarios/me/inativacao.

#### US-HPET39-02

Alternativa | Prioridade: Média | HPET39 – Inativar Própria Conta | Revisão: Novo
Como usuário do app, quero ser impedido de inativar minha conta enquanto houver reserva, solicitação ou serviço ativo ou futuro, para não deixar contratações em aberto.
**Descrição:** Bloqueio da inativação com contratações pendentes, como Tutor ou como prestador.
**Critérios de Aceite:**
*   Dado uma contratação ativa ou futura vinculada ao usuário, quando ele tentar inativar a conta, então o sistema impede a inativação e informa o motivo e as contratações envolvidas.

#### US-HPET39-03

Alternativa | Prioridade: Baixa | HPET39 – Inativar Própria Conta | Revisão: Novo
Como prestador, quero ser avisado sobre saldo a receber antes de confirmar a inativação da conta, para decidir se solicito o repasse antes.
**Descrição:** Aviso de pendência financeira na inativação de conta com perfil de prestador.
**Critérios de Aceite:**
*   Dado um prestador com saldo disponível ou repasse pendente, quando iniciar a inativação, então o sistema informa a pendência e o valor antes de pedir a confirmação.

Área Comercial Pública (Web)
----------------------------

### HPET34 – Consultar Informações Institucionais

#### US-HPET34-01

Principal | Prioridade: Média | HPET34 – Consultar Informações Institucionais | Revisão: Novo
Como visitante, quero conhecer a HOSPETSE pelo site (quem somos, serviços e como funciona), para entender a proposta da plataforma antes de baixar o app.
**Descrição:** Área pública da Web, sem autenticação (DF-02). A Web não oferece busca, reserva nem cadastro de tutor ou prestador; essas jornadas são do aplicativo (DF-01).
**Critérios de Aceite:**
*   Dado um visitante no site, quando acessar a página inicial, então vê o menu com Quem somos, Serviços (hospedagem, pet sitter e pet walker), Como funciona e Sobre o app.
*   Dado uma seção selecionada, quando aberta, então o sistema exibe o conteúdo institucional correspondente.
*   Dado a chamada para usar a plataforma, quando selecionada, então o visitante é direcionado à seção Sobre o app (US-HPET35-01).
*   O site não exibe link para o login administrativo, que fica em rota separada (US-HPET20A-01).
*   O layout funciona em computador e celular.
**Notas Técnicas:** Páginas estáticas ou SSR, sem chamadas autenticadas à API.

#### US-HPET34-02

Alternativa | Prioridade: Baixa | HPET34 – Consultar Informações Institucionais | Revisão: Novo
Como visitante, quero ver uma página de erro com retorno à página inicial quando acessar uma página inexistente, para continuar navegando.
**Descrição:** Página de erro 404 da área pública.
**Critérios de Aceite:**
*   Dado um endereço inexistente, quando acessado, então o sistema exibe página de erro com link para a página inicial.

### HPET35 – Conhecer e Obter o Aplicativo

#### US-HPET35-01

Principal | Prioridade: Média | HPET35 – Conhecer e Obter o Aplicativo | Revisão: Novo
Como visitante, quero entender o que o aplicativo oferece e ser direcionado à loja compatível com meu dispositivo, para instalar o app.
**Descrição:** Seção Sobre o app, com redirecionamento à loja do sistema operacional e sem download direto pelo site (DA-05).
**Critérios de Aceite:**
*   Dado um visitante na seção Sobre o app, quando a página carregar, então vê a explicação do app e dos perfis disponíveis (Tutor, Host, Pet Sitter e Pet Walker).
*   Dado um dispositivo Android ou iOS identificado, quando o visitante selecionar obter o app, então é redirecionado à loja compatível.
*   O site não oferece download direto de instalador.

#### US-HPET35-02

Alternativa | Prioridade: Baixa | HPET35 – Conhecer e Obter o Aplicativo | Revisão: Novo
Como visitante, quero escolher entre as lojas disponíveis quando o sistema operacional do meu dispositivo não for identificado, para obter o app mesmo assim.
**Descrição:** Tratamento para acesso por computador ou sistema não reconhecido.
**Critérios de Aceite:**
*   Dado um sistema operacional não identificado, quando o visitante selecionar obter o app, então o sistema exibe as opções de loja disponíveis para escolha.

#### US-HPET35-03

Alternativa | Prioridade: Baixa | HPET35 – Conhecer e Obter o Aplicativo | Revisão: Novo
Como visitante, quero ser avisado de que o app estará disponível em breve quando ainda não estiver publicado na loja, para voltar depois.
**Descrição:** A publicação na loja é próxima etapa, fora do escopo atual (DA-14).
**Critérios de Aceite:**
*   Dado que o app ainda não está publicado na loja correspondente, quando o visitante selecionar obter o app, então o sistema exibe aviso de disponibilidade em breve, sem redirecionar.
**Notas Técnicas:** Links das lojas configuráveis, sem necessidade de nova publicação do site.

Módulo Administrador (Web)
--------------------------

### HPET20 – Gerenciar Usuários

#### US-HPET20-01

Principal | Prioridade: Alta | HPET20 – Gerenciar Usuários | Revisão: Alterado
Como Administrador, quero consultar os usuários cadastrados, com abas por papel e o estado da conta, para acompanhar contas, histórico, ocorrências e penalidades.
**Ajuste v2:** abas por papel (antigas jornadas Clientes e Hosts/Prestadores unificadas em WJ02); estado da conta conforme DA-01; detalhe com histórico, ocorrências e penalidades.
**Descrição:** Painel administrativo com listagem de todos os usuários (Tutor, Host, Pet Sitter, Pet Walker), com filtros por papel e estado.
**Critérios de Aceite:**
*   Dado o Administrador autenticado, quando acessar a área de usuários, então visualiza a lista com abas por papel, estado da conta (ativa, suspensa com prazo, banida ou inativa) e data de cadastro, com busca e filtros por papel e estado.
*   Dado um usuário selecionado, quando aberto, então o sistema exibe dados, papéis, histórico de contratações, ocorrências e penalidades.
*   A lista deve ser paginada para suportar grande volume de usuários.
**Notas Técnicas:** Endpoint GET /api/admin/usuarios?papel=&estado=; acesso restrito ao papel Administrador.

#### US-HPET20-02

Principal | Prioridade: Alta | HPET20 – Gerenciar Usuários | Revisão: Alterado
Como Administrador, quero reativar uma conta inativa informando o motivo, para restabelecer o acesso do usuário com registro na auditoria.
**Ajuste v2:** gestão de usuários passa a apenas reativar (DA-01); bloquear e suspender saem daqui e viram penalidades com motivo (HPET25); validação de cadastro fica em HPET20b (telas 02C/02D removidas, 02E Reativar conta).
**Descrição:** Única ação de estado disponível na gestão de usuários, com registro de auditoria.
**Critérios de Aceite:**
*   Dado uma conta inativa, quando o Administrador selecionar "Reativar conta" e informar o motivo, então a conta volta a 'ativa', o usuário é notificado e a ação é registrada na trilha de auditoria (autor, data e motivo).
*   Dado uma conta já ativa, então a ação "Reativar conta" não é oferecida.
*   Dado uma conta suspensa ou banida por penalidade, então a reativação não é feita aqui: o sistema direciona à reversão da penalidade (US-HPET25A-01).
*   Dado o motivo não informado, quando o Administrador confirmar, então recebe mensagem de erro e nada é alterado.
**Regras de Negócio:**
*   Toda mudança de estado gera registro de auditoria (quem, quando, motivo).
*   Suspensão e banimento são sempre penalidades (HPET25).
**Notas Técnicas:** Endpoint POST /api/admin/usuarios/{id}/reativacao.

#### US-HPET20-03

Alternativa | Prioridade: Média | HPET20 – Gerenciar Usuários | Revisão: Sem alteração
Como Administrador, quero ser informado quando o usuário selecionado não for encontrado, para evitar executar uma ação sobre uma conta inexistente.
**Descrição:** Tratamento de erro para usuário inexistente/removido.
**Critérios de Aceite:**
*   Dado um ID de usuário inexistente, quando o Administrador tentar executar uma ação sobre ele, então recebe mensagem de erro informando que o usuário não foi encontrado.

### HPET20a – Autenticar-se (Administrador)

#### US-HPET20A-01

Principal | Prioridade: Alta | HPET20a – Autenticar-se (Administrador) | Revisão: Alterado
Como Administrador, quero acessar o painel administrativo Web pela rota administrativa mediante validação das minhas credenciais, para realizar as atividades de gestão.
**Ajuste v2:** papel único de Administrador (DF-03), substituindo Back-office e Moderador; rota separada /admin; contas criadas via seed (DA-06); sem recuperação de senha na Web (DA-07).
**Descrição:** Login do painel administrativo Web, com um único papel de Administrador. A separação de cargos poderá ser feita no futuro por permissões.
**Critérios de Aceite:**
*   Dado credenciais válidas de uma conta com papel de Administrador, quando submeter o login pela rota /admin, então recebe token de acesso e é direcionado ao Dashboard (US-HPET44-01); provisoriamente, à fila de validações (US-HPET20B-01).
*   A tela de login não tem link na área comercial pública nem opção "esqueci minha senha".
*   O painel administrativo deve ser inacessível para usuários sem papel de Administrador (US-HPET20A-03).
**Regras de Negócio:**
*   Contas administrativas são criadas via seed (podendo existir mais de uma); a recuperação de senha também é feita pelo seed.
**Notas Técnicas:** Autorização por permissões granulares no back-end associadas ao papel Administrador, permitindo separar cargos depois sem refazer as telas.

#### US-HPET20A-02

Alternativa | Prioridade: Alta | HPET20a – Autenticar-se (Administrador) | Revisão: Sem alteração
Como Administrador, quero receber uma mensagem quando minhas credenciais forem inválidas, para corrigi-las e tentar novamente.
**Descrição:** Tratamento de erro de autenticação administrativa, mesmo padrão de US-HPET01A-02.
**Critérios de Aceite:**
*   Dado credenciais inválidas, quando o Administrador submeter o login, então recebe mensagem genérica de erro, sem indicar qual campo está incorreto.

#### US-HPET20A-03

Alternativa | Prioridade: Alta | HPET20a – Autenticar-se (Administrador) | Revisão: Novo
Como Administrador, quero que o painel negue o acesso a usuários sem papel de Administrador e a contas administrativas inativas, para proteger as funções de gestão.
**Descrição:** Telas 01D (Acesso negado) e 01F (Conta administrativa inativa).
**Critérios de Aceite:**
*   Dado credenciais válidas de um usuário sem papel de Administrador, quando tentar acessar o painel, então o sistema exibe Acesso negado (tela 01D) e não emite token administrativo.
*   Dado uma conta administrativa inativa, quando tentar logar, então o acesso é negado (tela 01F).
*   Dado uma chamada à API administrativa sem papel de Administrador, então a resposta é 403.

#### US-HPET20A-04

Alternativa | Prioridade: Média | HPET20a – Autenticar-se (Administrador) | Revisão: Novo
Como Administrador, quero ser direcionado a um novo login quando minha sessão expirar, para continuar o trabalho com segurança.
**Descrição:** Tela 01E (Sessão expirada).
**Critérios de Aceite:**
*   Dado um token expirado, quando o Administrador realizar qualquer ação no painel, então o sistema exibe a tela de sessão expirada e exige novo login.
*   Ações não concluídas antes da expiração não são aplicadas.

### HPET20b – Validar Cadastro de Host/Prestador

#### US-HPET20B-01

Principal | Prioridade: Alta | HPET20b – Validar Cadastro de Host/Prestador | Revisão: Alterado
Como Administrador, quero validar cadastros novos, revalidações e reenvios de hosts e prestadores em uma fila filtrável, para garantir a qualidade e a segurança dos serviços oferecidos.
**Ajuste v2:** fila com filtro por tipo (novo, revalidação, reenvio — tela 03H) e por papel; decisão registrada na auditoria.
**Descrição:** Fila de validação de cadastros de Host/Pet Sitter/Pet Walker com status 'pendente de validação', com análise de documentos.
**Critérios de Aceite:**
*   Dado o Administrador na fila de validações, quando aplicar filtros, então vê os cadastros por tipo (novo, revalidação, reenvio) e por papel (Host, Pet Sitter, Pet Walker).
*   O Administrador deve poder visualizar os documentos enviados no cadastro para análise.
*   Dado um cadastro pendente, quando o Administrador aprovar, então o status muda para 'ativo', o prestador passa a aparecer nas buscas e pode receber solicitações, o usuário é notificado e a decisão é registrada na auditoria.
**Notas Técnicas:** Endpoint GET /api/admin/validacoes?tipo=&papel=; POST /api/admin/validacoes/{id}/decisao.

#### US-HPET20B-02

Alternativa | Prioridade: Alta | HPET20b – Validar Cadastro de Host/Prestador | Revisão: Alterado
Como Administrador, quero rejeitar um cadastro informando o feedback com os itens a ajustar, para que o solicitante seja notificado e possa corrigir e reenviar e quero poder rejeitar de forma definitiva também sem possibilidade de reenvio.
**Ajuste v2:** rejeição exige feedback com itens a ajustar e permite reenvio pelo prestador, caso não seja rejeição definitiva sem liberação de reenvio (DA-08, tela 03E).
**Descrição:** Rejeição de cadastro com feedback e notificação automática ao prestador.
**Critérios de Aceite:**
*   Dado um cadastro pendente, quando o Administrador rejeitar informando o feedback, então o status muda para 'rejeitado', o prestador recebe notificação com o feedback e pode corrigir e reenviar, caso a rejeição não seja definitiva (US-HPET37-01).
*   Dado o feedback não informado, quando o Administrador tentar rejeitar, então recebe mensagem de erro e a rejeição não é aplicada.
*   A rejeição é registrada na auditoria.

#### US-HPET20B-03

Alternativa | Prioridade: Média | HPET20b – Validar Cadastro de Host/Prestador | Revisão: Novo
Como Administrador, quero ser avisado quando o cadastro já tiver sido decidido, para não aplicar uma segunda decisão sobre ele.
**Descrição:** Tela 03I (Cadastro já decidido), útil quando mais de um Administrador atua na fila.
**Critérios de Aceite:**
*   Dado um cadastro já aprovado ou rejeitado, quando o Administrador tentar decidir sobre ele, então o sistema exibe o aviso com a decisão anterior e não aplica a segunda decisão.
**Notas Técnicas:** Controle de concorrência (ex.: versão do registro) na decisão.

### HPET25 – Gerenciar Penalidades

#### US-HPET25-01

Principal | Prioridade: Média | HPET25 – Gerenciar Penalidades | Revisão: Alterado

Como Administrador, quero analisar as ocorrências (denúncias e incidentes) e aplicar advertência, suspensão com prazo ou banimento com motivo, para manter a integridade da plataforma.

**Ajuste v2:** tipos de penalidade definidos (DA-01/DA-11, sem multa e sem restrição de funcionalidades); ocorrências com origem em denúncia (HPET36) ou incidente (HPET12c) (DA-02); estado da conta atualizado; notificação via HPET38. Decisão PO 01/11/2026: incidentes também de Pet Sitter e Pet Walker (HPET17b/19b); Administrador revisa e define a gravidade final; em incidente grave, decide responsabilidade e custos, inclusive de atendimento veterinário.

**Descrição:** Análise de ocorrências e aplicação de penalidades a qualquer tipo de usuário.

**Critérios de Aceite:**

- Dado o Administrador no histórico de ocorrências, quando acessar a lista, então vê origem (denúncia ou incidente), status, gravidade informada e usuários envolvidos.
- Dado um usuário envolvido em descumprimento, quando o Administrador aplicar advertência, suspensão com prazo ou banimento informando o motivo (e a data de fim, no caso de suspensão), então a penalidade passa a valer imediatamente, é registrada na auditoria e o usuário é notificado (US-HPET38-01).
- Dado suspensão ou banimento, então o estado da conta muda para 'suspensa' ou 'banida'; advertência não altera o estado.
- Dado o motivo ou o prazo da suspensão não informado, quando o Administrador confirmar, então recebe mensagem de erro e nada é aplicado.
- Dado uma ocorrência envolvendo conteúdo, o Administrador pode tratá-lo pela moderação (US-HPET42-01).
- Dado uma ocorrência de incidente, quando o Administrador revisar o caso, então define a gravidade final, que fica registrada na ocorrência e na auditoria.
- Dado um incidente grave com possível responsabilidade do prestador, quando o Administrador concluir a análise, então registra quem arca com os custos (inclusive veterinário); se for o Tutor, o valor entra como custo extra (US-HPET07A-01); Tutor e prestador são notificados.

**Regras de Negócio:**

- Penalidades possíveis: advertência, suspensão com prazo e banimento. Não há multa.
- O prazo da suspensão é definido na aplicação.
- Ocorrências vêm de denúncias (HPET36) e de incidentes registrados por Host, Pet Sitter e Pet Walker (HPET12c, 17b, 19b).

**Notas Técnicas:** Endpoints GET /api/admin/ocorrencias e POST /api/admin/penalidades; PATCH /api/admin/ocorrencias/{id} para gravidade final e conclusão de custos.

#### US-HPET25-02

Alternativa | Prioridade: Média | HPET25 – Gerenciar Penalidades | Revisão: Sem alteração
Como Administrador, quero ser informado quando o usuário selecionado para penalização não for encontrado, para corrigir a busca.
**Descrição:** Tratamento de erro para usuário inexistente na aplicação de penalidade.
**Critérios de Aceite:**
*   Dado um ID de usuário inexistente, quando o Administrador tentar aplicar penalidade, então recebe mensagem de erro informando que o usuário não foi encontrado.

#### US-HPET25-03

Alternativa | Prioridade: Média | HPET25 – Gerenciar Penalidades | Revisão: Novo
Como Administrador, quero arquivar uma ocorrência com justificativa quando não houver descumprimento, para encerrá-la sem aplicar penalidade.
**Descrição:** Tela 04M (Arquivar ocorrência).
**Critérios de Aceite:**
*   Dado uma ocorrência em análise, quando o Administrador selecionar arquivar e informar a justificativa, então a ocorrência passa a 'arquivada', sem penalidade, e a ação é registrada na auditoria.
*   Dado a justificativa não informada, então o arquivamento não é aplicado e o sistema exibe mensagem de erro.

### HPET25a – Acompanhar e Reverter Penalidades

#### US-HPET25A-01

Principal | Prioridade: Média | HPET25a – Acompanhar e Reverter Penalidades | Revisão: Novo
Como Administrador, quero acompanhar as penalidades ativas e reverter uma penalidade informando o motivo, para corrigir decisões e restaurar a conta quando aplicável.
**Descrição:** Tela 04L (Penalidades ativas); reversão sempre feita pelo Administrador (DA-10).
**Critérios de Aceite:**
*   Dado o Administrador em Penalidades ativas, quando acessar a lista, então vê tipo, usuário, motivo, data de início e data de fim.
*   Dado uma penalidade selecionada, quando o Administrador escolher "Reverter" e informar o motivo, então a penalidade é encerrada, a conta volta a 'ativa' quando aplicável, o usuário é notificado (US-HPET38-03) e uma nova entrada de auditoria é criada, sem apagar o registro original.
*   Dado o motivo da reversão não informado, então a reversão não é aplicada e o sistema exibe mensagem de erro.
**Notas Técnicas:** Endpoint POST /api/admin/penalidades/{id}/reversao.

#### US-HPET25A-02

Técnica | Prioridade: Alta | HPET25a – Acompanhar e Reverter Penalidades | Revisão: Novo
Como Sistema, preciso encerrar automaticamente as suspensões na data de fim, restaurar a conta e notificar o usuário, para que o prazo definido seja cumprido sem ação manual.
**Descrição:** Fim automático da suspensão (DA-01/DA-10).
**Critérios de Aceite:**
*   Dado uma suspensão cuja data de fim foi atingida, quando o processo agendado rodar, então a penalidade é encerrada, a conta volta a 'ativa', o usuário é notificado (US-HPET38-03) e o encerramento é registrado na auditoria com autor "Sistema".
*   Dado que o usuário tenta logar após a data de fim e antes da execução do processo, então o login considera a suspensão encerrada.
**Notas Técnicas:** Job agendado (ex.: a cada hora) + verificação da data de fim no login.

#### US-HPET25A-03

Alternativa | Prioridade: Baixa | HPET25a – Acompanhar e Reverter Penalidades | Revisão: Novo
Como Administrador, quero ser informado quando não houver penalidades ativas, para saber que a lista está vazia.
**Descrição:** Estado vazio da lista de penalidades ativas.
**Critérios de Aceite:**
*   Dado nenhuma penalidade ativa, quando o Administrador acessar a lista, então é exibida mensagem indicando lista vazia.

### HPET40 – Gerenciar Cupons de Desconto

#### US-HPET40-01

Principal | Prioridade: Baixa | HPET40 – Gerenciar Cupons de Desconto | Revisão: Novo
Como Administrador, quero criar e editar cupons de desconto com código, tipo e valor, vigência e limite de uso, para oferecer promoções aos tutores.
**Descrição:** Cupons usados pelo Tutor no pagamento (US-HPET32-01), conforme DA-04 (jornada WJ12).
**Critérios de Aceite:**
*   Dado o Administrador na área de cupons, quando acessar a lista, então vê código, desconto, vigência, usos e situação de cada cupom.
*   Dado código, tipo (percentual ou valor fixo), valor, vigência e limite de uso válidos, quando salvar, então o cupom é criado ou atualizado e a ação é registrada na auditoria.
*   Alterações em um cupom não afetam pagamentos já concluídos.
**Notas Técnicas:** Endpoint CRUD /api/admin/cupons; contador de usos atualizado na aprovação do pagamento.

#### US-HPET40-02

Alternativa | Prioridade: Baixa | HPET40 – Gerenciar Cupons de Desconto | Revisão: Novo
Como Administrador, quero inativar um cupom, para que ele deixe de ser aceito em novos pagamentos.
**Descrição:** Inativação lógica do cupom.
**Critérios de Aceite:**
*   Dado um cupom ativo, quando o Administrador inativá-lo, então o cupom deixa de ser aceito em novos pagamentos (US-HPET32-02) e a ação é registrada na auditoria.

#### US-HPET40-03

Alternativa | Prioridade: Baixa | HPET40 – Gerenciar Cupons de Desconto | Revisão: Novo
Como Administrador, quero receber uma mensagem quando o código já existir ou os dados do cupom forem inválidos, para corrigi-los antes de salvar.
**Descrição:** Validação do formulário de cupom.
**Critérios de Aceite:**
*   Dado um código já existente, quando o Administrador salvar, então recebe mensagem de erro e o cupom não é salvo.
*   Dado vigência final anterior à inicial, valor inválido ou limite de uso não numérico, quando salvar, então o campo é destacado com mensagem específica.

### HPET41 – Configurar Taxa da Plataforma

#### US-HPET41-01

Principal | Prioridade: Alta | HPET41 – Configurar Taxa da Plataforma | Revisão: Novo
Como Administrador, quero definir o percentual da taxa da plataforma com data de início de vigência, para que seja aplicado no repasse e exibido no financeiro dos prestadores.
**Descrição:** Parametrização da taxa usada em US-HPET27-01 e exibida em US-HPET12E-01 (DA-04, jornada WJ13).
**Critérios de Aceite:**
*   Dado o Administrador na área de taxa, quando acessar, então vê a taxa vigente e o histórico de alterações.
*   Dado um novo percentual e uma data de início de vigência válidos, quando salvar, então a nova taxa é registrada e a alteração entra na auditoria.
*   Contratações já solicitadas ou confirmadas mantêm a taxa vigente no momento da contratação.
**Regras de Negócio:**
*   A taxa é parametrizável, nunca fixada em código.
*   Cada contratação guarda a taxa aplicável no momento em que foi solicitada.
**Notas Técnicas:** Endpoint GET/POST /api/admin/taxa-plataforma; tabela de vigências.

#### US-HPET41-02

Alternativa | Prioridade: Média | HPET41 – Configurar Taxa da Plataforma | Revisão: Novo
Como Administrador, quero receber uma mensagem quando o valor da taxa for inválido, para corrigi-lo antes de salvar.
**Descrição:** Validação do percentual informado.
**Critérios de Aceite:**
*   Dado um valor negativo, maior que 100% ou não numérico, quando o Administrador salvar, então recebe mensagem de erro e a taxa não é alterada.

### HPET42 – Moderar Conteúdo Denunciado

#### US-HPET42-01

Principal | Prioridade: Média | HPET42 – Moderar Conteúdo Denunciado | Revisão: Novo
Como Administrador, quero analisar os conteúdos denunciados e decidir mantê-los, ocultá-los ou removê-los com justificativa, para manter a plataforma segura.
**Descrição:** Fila de moderação alimentada por US-HPET36-01 (DA-04, jornada WJ14). No chat, o Administrador vê somente a mensagem denunciada (DA-13).
**Critérios de Aceite:**
*   Dado a fila de moderação, quando o Administrador acessar, então vê os itens com tipo (perfil, avaliação, foto, mensagem), motivo, denunciante e data.
*   Dado um item de chat selecionado, então o sistema exibe apenas a mensagem denunciada.
*   Dado a decisão de manter, ocultar ou remover com justificativa, quando confirmada, então o sistema aplica a decisão, registra na auditoria e notifica o autor quando houver remoção.
*   Dado conteúdo que indica descumprimento de regras, o Administrador pode aplicar penalidade a partir do item (US-HPET25-01).
*   Avaliações ocultadas ou removidas deixam de contar na nota média (US-HPET33-01).
**Notas Técnicas:** Endpoints GET /api/admin/moderacao e POST /api/admin/moderacao/{id}/decisao.

#### US-HPET42-02

Alternativa | Prioridade: Baixa | HPET42 – Moderar Conteúdo Denunciado | Revisão: Novo
Como Administrador, quero ser informado quando a fila de moderação estiver vazia, para saber que não há itens pendentes.
**Descrição:** Estado vazio da fila de moderação.
**Critérios de Aceite:**
*   Dado nenhum item pendente, quando o Administrador acessar a fila, então é exibida mensagem indicando que não há itens pendentes.

### HPET42a – Solicitar Acesso à Conversa Completa

#### US-HPET42A-01

Principal | Prioridade: Baixa | HPET42a – Solicitar Acesso à Conversa Completa | Revisão: Novo
Como Administrador, quero solicitar, com justificativa, acesso à conversa completa de um chat denunciado, para decidir quando a mensagem isolada não for suficiente.
**Descrição:** Acesso restrito à conversa completa (DA-13; telas 14G/14H/14I). Liberado somente por admin 00 e com registro justificado.
**Critérios de Aceite:**
*   Dado uma mensagem de chat em análise, quando o Administrador selecionar "Solicitar acesso à conversa completa" e informar a justificativa, então o pedido é registrado e encaminhado ao responsável pela concessão.
*   Dado o acesso concedido, então a conversa completa fica disponível ao Administrador por acesso restrito, e o acesso é registrado na auditoria.
*   Dado a justificativa não informada, então o pedido não é registrado e o sistema exibe mensagem de erro.

#### US-HPET42A-02

Alternativa | Prioridade: Baixa | HPET42a – Solicitar Acesso à Conversa Completa | Revisão: Novo
Como Administrador, quero ser informado quando o acesso à conversa completa for negado, para decidir apenas com base na mensagem denunciada.
**Descrição:** Retorno da negativa do pedido de acesso.
**Critérios de Aceite:**
*   Dado um pedido negado, quando o Administrador retornar ao item, então vê a negativa e continua a análise apenas com a mensagem denunciada.

### HPET43 – Tratar Transação Financeira com Falha

#### US-HPET43-01

Principal | Prioridade: Alta | HPET43 – Tratar Transação Financeira com Falha | Revisão: Novo
Como Administrador, quero reprocessar ou registrar a resolução manual de estornos e repasses com falha, com justificativa, para regularizar a situação do tutor ou do prestador.
**Descrição:** Tratamento de estornos (DA-09) e repasses (DA-12) que falharam no fluxo padrão (telas 09I/09J), sempre por novo lançamento no registro financeiro.
**Critérios de Aceite:**
*   Dado o Administrador em transações com falha, quando acessar a lista, então vê tipo, valor, envolvidos, motivo da falha e data.
*   Dado uma transação selecionada, quando o Administrador reprocessar ou registrar a resolução manual com justificativa, então o sistema executa a ação, cria novo lançamento vinculado ao original (US-HPET28A-01), atualiza o status e notifica o tutor ou o prestador.
*   O lançamento original nunca é alterado.
*   Dado a justificativa não informada, então a ação não é executada e o sistema exibe mensagem de erro.
**Notas Técnicas:** Endpoint POST /api/admin/transacoes-falha/{id}/resolucao (acao=REPROCESSAR|RESOLUCAO_MANUAL).

#### US-HPET43-02

Alternativa | Prioridade: Média | HPET43 – Tratar Transação Financeira com Falha | Revisão: Novo
Como Administrador, quero que uma nova falha no reprocessamento mantenha o status de falha e registre a tentativa, para acompanhar o histórico até a resolução.
**Descrição:** Histórico de tentativas de reprocessamento.
**Critérios de Aceite:**
*   Dado um reprocessamento que falha novamente, quando concluído, então a transação permanece com status de falha e a tentativa (data, autor, resultado) é registrada.

### HPET44 – Consultar Dashboard Administrativo

#### US-HPET44-01

Principal | Prioridade: Média | HPET44 – Consultar Dashboard Administrativo | Revisão: Novo
Como Administrador, quero ver após o login um resumo das filas pendentes e dos indicadores de contratações, para priorizar o trabalho.
**Descrição:** Destino desejado após o login (jornada WJ05).
**Critérios de Aceite:**
*   Dado o Administrador autenticado, quando acessar o Dashboard, então vê os totais de validações pendentes, ocorrências abertas, moderação pendente e transações com falha, e os indicadores de contratações do período.
*   Dado um indicador ou fila selecionado, então o sistema direciona à área correspondente (US-HPET20B-01, US-HPET25-01, US-HPET42-01, US-HPET43-01).
**Notas Técnicas:** Endpoint GET /api/admin/dashboard?periodo=.

#### US-HPET44-02

Alternativa | Prioridade: Baixa | HPET44 – Consultar Dashboard Administrativo | Revisão: Novo
Como Administrador, quero ver indicadores zerados quando não houver dados no período, para não confundir ausência de dados com erro.
**Descrição:** Estado vazio do Dashboard.
**Critérios de Aceite:**
*   Dado nenhum dado no período, quando o Dashboard carregar, então os indicadores aparecem zerados.

### HPET45 – Consultar Contratações

#### US-HPET45-01

Principal | Prioridade: Média | HPET45 – Consultar Contratações | Revisão: Novo
Como Administrador, quero consultar as reservas e solicitações de serviço da plataforma com status e detalhes, para apoiar a análise de ocorrências e transações.
**Descrição:** Jornada WJ08 (antes "Reservas"), somente consulta.
**Critérios de Aceite:**
*   Dado o Administrador em Contratações, quando acessar, então vê a lista com tipo de serviço, tutor, prestador, pet, datas, valor e status, com filtros.
*   Dado uma contratação selecionada, então o sistema exibe detalhes, histórico de status e pagamentos vinculados.
**Notas Técnicas:** Endpoint GET /api/admin/contratacoes.

#### US-HPET45-02

Alternativa | Prioridade: Baixa | HPET45 – Consultar Contratações | Revisão: Novo
Como Administrador, quero ser informado quando nenhuma contratação corresponder aos filtros, para ajustar a consulta.
**Descrição:** Estado vazio da consulta.
**Critérios de Aceite:**
*   Dado filtros sem resultado, quando a consulta for executada, então é exibida mensagem de ausência de resultados.

### HPET46 – Consultar Financeiro da Plataforma

#### US-HPET46-01

Principal | Prioridade: Média | HPET46 – Consultar Financeiro da Plataforma | Revisão: Novo
Como Administrador, quero acompanhar pagamentos, repasses, estornos e taxas arrecadadas, para controlar o financeiro da plataforma.
**Descrição:** Jornada WJ09, com acesso às transações com falha (HPET43).
**Critérios de Aceite:**
*   Dado o Administrador no Financeiro, quando acessar, então vê os totais do período e a lista de transações com tipo, valor, status e envolvidos.
*   Dado uma transação selecionada, então o sistema exibe o detalhe e os lançamentos vinculados (US-HPET28A-01).
*   Transações com falha têm atalho para tratamento (US-HPET43-01).
**Notas Técnicas:** Endpoint GET /api/admin/financeiro.

#### US-HPET46-02

Alternativa | Prioridade: Baixa | HPET46 – Consultar Financeiro da Plataforma | Revisão: Novo
Como Administrador, quero ser informado quando não houver transações no período, para ajustar os filtros.
**Descrição:** Estado vazio do financeiro.
**Critérios de Aceite:**
*   Dado nenhum registro no período, quando o Financeiro carregar, então é exibida mensagem indicando que não há registros.

### HPET47 – Consultar Trilha de Auditoria

#### US-HPET47-01

Principal | Prioridade: Média | HPET47 – Consultar Trilha de Auditoria | Revisão: Novo
Como Administrador, quero consultar a trilha de auditoria das ações administrativas, com autor, data e motivo, para garantir a rastreabilidade das decisões.
**Descrição:** Jornada WJ10. Cobre validações, penalidades, reversões, reativações, moderação, cupons, taxa, tratamento de transações e acessos a conversas.
**Critérios de Aceite:**
*   Dado o Administrador em Auditoria, quando acessar, então vê os registros em ordem cronológica, com filtros por tipo de ação, autor, usuário afetado e período.
*   Dado um registro selecionado, então o sistema exibe o detalhe da ação, sem permitir edição ou exclusão.
**Regras de Negócio:**
*   Registros de auditoria são imutáveis.
**Notas Técnicas:** Endpoint GET /api/admin/auditoria.

#### US-HPET47-02

Alternativa | Prioridade: Baixa | HPET47 – Consultar Trilha de Auditoria | Revisão: Novo
Como Administrador, quero ser informado quando nenhum registro corresponder aos filtros, para ajustar a consulta.
**Descrição:** Estado vazio da auditoria.
**Critérios de Aceite:**
*   Dado filtros sem resultado, quando a consulta for executada, então é exibida mensagem de ausência de resultados.

### HPET48 – Gerenciar Conta do Administrador

#### US-HPET48-01

Principal | Prioridade: Média | HPET48 – Gerenciar Conta do Administrador | Revisão: Novo
Como Administrador, quero consultar meus dados de acesso e alterar minha senha, para manter minha conta administrativa segura.
**Descrição:** Jornada WJ11 (Minha conta).
**Critérios de Aceite:**
*   Dado o Administrador em Minha conta, quando acessar, então vê nome e e-mail da conta administrativa.
*   Dado a senha atual correta e uma nova senha que atende à política, quando salvar, então a senha é atualizada e a ação é registrada na auditoria.
**Notas Técnicas:** Endpoint PUT /api/admin/conta/senha.

#### US-HPET48-02

Principal | Prioridade: Média | HPET48 – Gerenciar Conta do Administrador | Revisão: Novo
Como Administrador, quero encerrar minha sessão, para impedir o uso do painel por outra pessoa no mesmo dispositivo.
**Descrição:** Logout do painel administrativo.
**Critérios de Aceite:**
*   Dado o Administrador autenticado, quando selecionar "Sair", então a sessão é encerrada e ele retorna ao login administrativo.
*   Após sair, o token anterior não permite novas ações no painel.

#### US-HPET48-03

Alternativa | Prioridade: Baixa | HPET48 – Gerenciar Conta do Administrador | Revisão: Novo
Como Administrador, quero receber uma mensagem quando a senha atual estiver incorreta ou a nova não atender à política, para corrigir antes de salvar.
**Descrição:** Validação da troca de senha.
**Critérios de Aceite:**
*   Dado a senha atual incorreta ou uma nova senha fora da política, quando o Administrador salvar, então recebe mensagem de erro e a senha não é alterada.

Sistema de Pagamento (Backend)
------------------------------

### HPET26 – Processar Pagamento

#### US-HPET26-01

Técnica | Prioridade: Alta | HPET26 – Processar Pagamento | Revisão: Alterado
Como Sistema de Pagamento, preciso processar os pagamentos de reservas e serviços já aceitos pelo prestador e registrar as transações aprovadas.
**Ajuste v2:** processamento somente para contratações em "aguardando pagamento" (pagamento após o aceite); cobre reservas e serviços.
**Descrição:** Núcleo da integração com o gateway de pagamento (sandbox); recebe solicitação de cobrança (US-HPET07A-01) e retorna aprovação/recusa.
**Critérios de Aceite:**
*   Dado uma requisição de pagamento válida (token de pagamento + valor + contratação em 'aguardando pagamento'), quando processada com sucesso pelo gateway, então a transação é registrada como 'aprovada' (US-HPET28A-01) e a contratação é atualizada para 'confirmada'.
*   Dado uma contratação em outro status, quando chegar uma requisição de pagamento, então a cobrança é rejeitada sem envio ao gateway.
*   A requisição deve ser idempotente (uso de chave de idempotência) para evitar cobrança duplicada em caso de reenvio/timeout.
**Notas Técnicas:** Integração com API de pagamento sandbox (ex.: Stripe/Mercado Pago/PagSeguro sandbox); comunicação REST assíncrona ou síncrona conforme provedor escolhido.

#### US-HPET26-02

Exceção | Prioridade: Alta | HPET26 – Processar Pagamento | Revisão: Alterado
Como Sistema de Pagamento, preciso informar quando o gateway recusar uma transação, para impedir que um pagamento não aprovado seja confirmado.
**Ajuste v2:** status de permanência renomeado para "aguardando pagamento", permitindo nova tentativa.
**Descrição:** Tratamento do retorno negativo do gateway, propagando o status correto para a contratação (US-HPET07A-02).
**Critérios de Aceite:**
*   Dado que o gateway retorna recusa, quando processado, então a transação é registrada como 'recusada' e a contratação permanece 'aguardando pagamento' para nova tentativa (nunca é confirmada indevidamente).
*   Toda tentativa (aprovada ou recusada) deve ser registrada para auditoria (US-HPET28A).

### HPET27 – Repassar Valores

#### US-HPET27-01

Técnica | Prioridade: Alta | HPET27 – Repassar Valores | Revisão: Alterado

Como Sistema de Pagamento, preciso calcular o valor disponível para repasse após a conclusão do serviço, descontando a taxa vigente na contratação, e transferi-lo somente quando o prestador solicitar.

**Ajuste v2:** taxa passa a ser configurada pelo Administrador (HPET41) e aplicada conforme a vigência no momento da contratação. Decisão PO 01/11/2026: a transferência não é automática, só ocorre mediante solicitação do prestador (US-HPET12F-01).

**Descrição:** Cálculo do valor líquido a repassar ao Host/Pet Sitter/Pet Walker após dedução da taxa da plataforma, feito na conclusão do serviço (US-HPET12D-01, US-HPET17B-01, US-HPET19B-01); a transferência acontece apenas quando o prestador solicita o repasse (US-HPET12F-01).

**Critérios de Aceite:**

- Dado um serviço concluído, quando o cálculo de repasse for executado, então o valor líquido (bruto − taxa da plataforma vigente na contratação) é registrado como saldo disponível para repasse ao prestador.
- Dado um saldo disponível, quando o prestador não solicitar o repasse, então nenhuma transferência é feita.
- Dado uma solicitação de repasse válida (US-HPET12F-01), quando processada, então o valor é transferido ao prestador via gateway e a transação é registrada (US-HPET28A-01).
- A taxa da plataforma aplicada deve ser rastreável (percentual/valor usado no cálculo, registrado junto à transação).

**Regras de Negócio:**

- Percentual de taxa da plataforma parametrizável pelo Administrador (US-HPET41-01), nunca fixado em código.
- Alteração de taxa não afeta contratações já solicitadas ou confirmadas.
- Repasse somente mediante solicitação do prestador.
#### US-HPET27-02

Exceção | Prioridade: Alta | HPET27 – Repassar Valores | Revisão: Alterado
Como Sistema de Pagamento, preciso informar quando ocorrer uma falha no cálculo do repasse, para evitar o envio de um valor incorreto.
**Ajuste v2:** falha na transferência separada em US-HPET27-03; esta US cobre apenas a falha de cálculo.
**Descrição:** Tratamento de falha no cálculo do repasse.
**Critérios de Aceite:**
*   Dado uma falha no cálculo do repasse, quando ocorrer, então nenhum valor é transferido, o erro é registrado e o prestador é informado de que o repasse não foi concluído.

#### US-HPET27-03

Exceção | Prioridade: Alta | HPET27 – Repassar Valores | Revisão: Novo
Como Sistema de Pagamento, preciso registrar com status de falha o repasse cuja transferência não se concluir, para que o Administrador o trate sem transferir valor incorreto.
**Descrição:** Repasse com falha disponibilizado ao Administrador (DA-12, US-HPET43-01).
**Critérios de Aceite:**
*   Dado uma falha na transferência ao prestador, quando retornada pelo gateway, então o repasse é registrado com status de falha (US-HPET28A-01), nenhum valor incorreto é transferido e o prestador é notificado (US-HPET12F-03).
*   O repasse com falha passa a constar na lista de transações com falha do Administrador.

### HPET28 – Processar Cancelamento / Estorno

#### US-HPET28-01

Técnica | Prioridade: Alta | HPET28 – Processar Cancelamento / Estorno | Revisão: Sem alteração
Como Sistema de Pagamento, preciso processar o estorno de pagamentos de reservas canceladas para devolver o valor correspondente ao tutor.
**Descrição:** Disparado pelo cancelamento de reserva (US-HPET04A-01), aplica a política de estorno vigente (integral/parcial conforme prazo).
**Critérios de Aceite:**
*   Dado uma reserva cancelada com pagamento já aprovado, quando o estorno for processado com sucesso, então o valor (integral ou parcial, conforme política) é devolvido ao Tutor e a transação é registrada como 'estornada'.
**Notas Técnicas:** Integração com endpoint de estorno/refund do gateway sandbox.

#### US-HPET28-02

Exceção | Prioridade: Alta | HPET28 – Processar Cancelamento / Estorno | Revisão: Alterado
Como Sistema de Pagamento, preciso registrar com status de falha o estorno recusado pelo gateway, para que o Administrador o trate sem que uma devolução inexistente seja declarada.
**Ajuste v2:** tratamento manual passa a ser feito em HPET43 (DA-09); proibido declarar devolução que não ocorreu.
**Descrição:** Tratamento de falha no estorno, exigindo intervenção do Administrador.
**Critérios de Aceite:**
*   Dado que o gateway recusa o estorno, quando processado, então a falha é registrada com status específico ('estorno com falha') e o estorno passa a constar na lista de transações com falha do Administrador (US-HPET43-01).
*   Enquanto não resolvido, o Tutor não recebe confirmação de devolução.

### HPET28a – Registrar Transação Financeira

#### US-HPET28A-01

Técnica | Prioridade: Alta | HPET28a – Registrar Transação Financeira | Revisão: Alterado
Como Sistema de Pagamento, preciso registrar pagamentos, repasses, estornos e resoluções manuais do Administrador em lançamentos imutáveis, para fins de auditoria e consulta financeira.
**Ajuste v2:** inclusão das resoluções manuais (HPET43), do vínculo com o lançamento original e da exibição no financeiro administrativo (HPET46).
**Descrição:** Camada de registro/ledger de todas as movimentações financeiras da plataforma, base para US-HPET07B (extrato do Tutor), US-HPET12E (financeiro do Host) e US-HPET46 (financeiro administrativo).
**Critérios de Aceite:**
*   Dado qualquer evento financeiro (pagamento, repasse, estorno ou resolução manual), quando processado (com sucesso ou falha), então um registro imutável é criado com tipo, valor, data, partes envolvidas, status e vínculo com a contratação.
*   Dado uma correção ou resolução manual, então é criado novo lançamento vinculado ao original, que permanece inalterado.
*   O registro fica disponível no financeiro do Tutor/Host e no financeiro administrativo.
**Regras de Negócio:**
*   Registros de transação devem ser imutáveis (apenas inserção; correções via novo registro de ajuste, nunca alteração do original) para garantir trilha de auditoria.

#### US-HPET28A-02

Exceção | Prioridade: Alta | HPET28a – Registrar Transação Financeira | Revisão: Sem alteração
Como Sistema de Pagamento, preciso informar quando ocorrer falha no registro de uma transação, para evitar que uma movimentação financeira fique sem rastro.
**Descrição:** Tratamento de falha na própria camada de registro/persistência.
**Critérios de Aceite:**
*   Dado uma falha ao persistir o registro de uma transação, quando ocorrer, então o sistema deve reter/reenfileirar o evento para nova tentativa e alertar a operação (ex.: log crítico/monitoramento), evitando perda de rastro financeiro.