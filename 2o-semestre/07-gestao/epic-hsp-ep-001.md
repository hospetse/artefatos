# HSP-EP-001: Identidade, conta e acesso

  

**Tipo:** Epic

**Status:** Proposed

**Proveniência:** proposed

**Responsável/Release:** [TBD]

  

## Resultado esperado

  

Permitir que pessoas criem, recuperem e mantenham uma identidade e acessem somente as capacidades autorizadas para seus papéis e estado de conta.

  

## Escopo

  

**Incluído:** conta/perfil do Tutor, recuperação, edição, autenticação de Tutor e Administrador e autorização por papel/status.

**Fora de escopo:** perfis comerciais de prestadores, gestão administrativa da conta e validação documental.

**Métrica:** [TBD].

  

## Features

  

| Feature | Resultado | PBIs | Estado |

|---|---|---|---|

| HSP-FT-001 — Conta do Tutor | Criar, completar, recuperar e manter conta/perfil | HSP-US-0001..0004 | Proposed |

| HSP-FT-002 — Autenticação e autorização | Autenticar e aplicar acesso por papel/status | HSP-US-0005..0007 | Proposed |

  

## HSP-FT-001 — Conta do Tutor

  

**Status:** Proposed

**Dependências:** serviço de comunicação; HSP-DEC-002 e HSP-DEC-003.

  

### HSP-US-0001 — Criar uma conta na plataforma

  

**Tipo:** User Story | **Feature:** HSP-FT-001 | **Status:** Proposed | **Proveniência:** explicit + proposed

**Prioridade/Estimativa/Owner/Iteração:** [TBD]

  

**Narrativa:** Como visitante, quero criar uma conta com dados de identificação e contato, para acessar a plataforma e iniciar meu perfil.

  

**Fontes:** HSP-SRC-UC-001 (`UC01`, `HPET01`); Wiki `US-HPET01-01`, `US-HPET01-02`.

**Incluído:** dados mínimos, unicidade, confirmação de contato, aceite de termos e validações.

**Fora de escopo:** completar perfil, cadastrar prestador e escolher arquitetura de token.

  

**Pré-condição:** não existir conta conflitante segundo a regra de unicidade.

**Pós-condição:** conta persistida em estado definido por HSP-DEC-003, com trilha do aceite.

  

**Critérios de aceitação**

  

- **HSP-US-0001-AC-01:** Dado um visitante com nome, contato e senha válidos e aceite dos termos, quando concluir o cadastro, então o sistema cria uma única conta e inicia a confirmação de e-mail ou telefone.

- **HSP-US-0001-AC-02:** Dado e-mail já associado a uma conta, quando o visitante tentar cadastrar-se, então a criação é bloqueada sem duplicar a identidade.

- **HSP-US-0001-AC-03:** Dado campos ausentes ou inválidos, quando submeter o cadastro, então todos os campos inválidos são identificados e nenhuma conta parcial é criada.

  

**Regras/TBD:** conta comum e Tutor automático dependem de HSP-DEC-002; autenticação automática após cadastro conflita com confirmação da fonte local e depende de HSP-DEC-003.

**Dependências:** nenhuma interna.

**Evidência:** `implemented_by=[não identificado]`; `verified_by=[não identificado]`; `coverage_status=source_only`.

  

### HSP-US-0002 — Completar o perfil do Tutor para contratar

  

**Tipo:** User Story | **Feature:** HSP-FT-001 | **Status:** Proposed | **Proveniência:** explicit no Wiki detalhado

**Prioridade/Estimativa/Owner/Iteração:** [TBD]

  

**Narrativa:** Como Tutor com conta criada, quero completar os dados necessários à contratação, para solicitar serviços com identidade verificável.

  

**Fontes:** HSP-SRC-UC-001; Wiki `US-HPET01-03`.

**Incluído:** pendências do perfil, dados complementares, bloqueio da contratação enquanto incompleto.

**Fora de escopo:** cadastro do pet e método de validação de identidade.

  

**Critérios de aceitação**

  

- **HSP-US-0002-AC-01:** Dado um Tutor com perfil incompleto, quando acessar a plataforma, então visualiza os dados pendentes e um caminho para completá-los.

- **HSP-US-0002-AC-02:** Dado dados complementares válidos, quando salvar, então o perfil é marcado como completo e a elegibilidade para contratar é atualizada.

- **HSP-US-0002-AC-03:** Dado perfil incompleto, quando tentar solicitar serviço, então a contratação é bloqueada sem impedir navegação permitida.

  

**Regras/TBD:** CPF único, maioridade, endereço e contato de emergência foram adicionados pelo Wiki detalhado e exigem aprovação/necessidade LGPD; HSP-DEC-002.

**Dependências:** HSP-US-0001.

**Evidência:** `coverage_status=source_only`.

  

### HSP-US-0003 — Recuperar acesso por redefinição de senha

  

**Tipo:** User Story | **Feature:** HSP-FT-001 | **Status:** Proposed | **Proveniência:** explicit

**Prioridade/Estimativa/Owner/Iteração:** [TBD]

  

**Narrativa:** Como titular de uma conta, quero redefinir minha senha por um contato verificado, para recuperar o acesso com segurança.

  

**Fontes:** HSP-SRC-UC-003 (`UC01b`, `HPET01b`); Wiki `US-HPET01B-01`, `US-HPET01B-02`.

  

**Critérios de aceitação**

  

- **HSP-US-0003-AC-01:** Dado um contato elegível, quando solicitar recuperação, então o sistema envia um código ou link de uso único com validade configurada.

- **HSP-US-0003-AC-02:** Dado token válido e nova senha válida, quando confirmar a redefinição, então a senha anterior deixa de autenticar e o token não pode ser reutilizado.

- **HSP-US-0003-AC-03:** Dado token expirado, inválido ou usado, quando tentar redefinir, então nenhuma senha é alterada e é oferecido novo fluxo seguro.

  

**Regras/TBD:** a resposta para contato inexistente depende de HSP-DEC-003 para evitar enumeração; duração e política de senha `[TBD]`.

**Dependências:** HSP-US-0001; serviço de comunicação.

**Evidência:** `coverage_status=source_only`.

  

### HSP-US-0004 — Atualizar dados do perfil do Tutor

  

**Tipo:** User Story | **Feature:** HSP-FT-001 | **Status:** Proposed | **Proveniência:** explicit

**Prioridade/Estimativa/Owner/Iteração:** [TBD]

  

**Narrativa:** Como Tutor autenticado, quero atualizar meus dados pessoais, para manter meu perfil correto e contatável.

  

**Fontes:** HSP-SRC-UC-004 (`UC01c`, `HPET01c`); Wiki `US-HPET01C-01`, `US-HPET01C-02`.

  

**Critérios de aceitação**

  

- **HSP-US-0004-AC-01:** Dado um Tutor autenticado, quando salvar alterações válidas em dados autorizados, então somente seu perfil é atualizado e os novos valores são apresentados.

- **HSP-US-0004-AC-02:** Dado um valor inválido ou contato já utilizado, quando salvar, então nada inválido é persistido e o campo é identificado.

  

**Regras/TBD:** mudança de e-mail/telefone deve exigir reverificação `[TBD]`; alteração de senha pode ser separada após decisão de segurança.

**Dependências:** HSP-US-0005.

**Evidência:** `coverage_status=source_only`.

  

## HSP-FT-002 — Autenticação e autorização

  

**Status:** Proposed

**Dependências:** HSP-FT-001; estados administrativos de HSP-FT-024.

  

### HSP-US-0005 — Autenticar uma conta de plataforma

  

**Tipo:** User Story | **Feature:** HSP-FT-002 | **Status:** Proposed | **Proveniência:** explicit + derived

**Prioridade/Estimativa/Owner/Iteração:** [TBD]

  

**Narrativa:** Como titular de uma conta, quero autenticar-me com minhas credenciais, para acessar capacidades permitidas ao meu perfil.

  

**Fontes:** HSP-SRC-UC-002 (`UC01a`, `HPET01a`); Wiki `US-HPET01A-01`.

  

**Critérios de aceitação**

  

- **HSP-US-0005-AC-01:** Dado credenciais válidas de uma conta apta, quando autenticar, então o sistema concede acesso com identidade e papéis correspondentes.

- **HSP-US-0005-AC-02:** Dado acesso expirado ou revogado, quando uma operação protegida for solicitada, então ela é negada e o fluxo de renovação ou novo login é exigido.

  

**Regras/TBD:** e-mail versus telefone, duração/renovação de acesso, MFA e tecnologia de token dependem de HSP-DEC-003 e HSP-DEC-015.

**Dependências:** HSP-US-0001.

**Evidência:** `coverage_status=source_only`.

  

### HSP-US-0006 — Tratar credencial inválida e conta restrita

  

**Tipo:** User Story | **Feature:** HSP-FT-002 | **Status:** Proposed | **Proveniência:** explicit + derived

**Prioridade/Estimativa/Owner/Iteração:** [TBD]

  

**Narrativa:** Como titular de uma conta, quero receber uma resposta segura quando o acesso falhar, para corrigir o problema sem expor dados da conta.

  

**Fontes:** HSP-SRC-UC-002; Wiki `US-HPET01A-01`, `US-HPET01A-02`; relação com HSP-SRC-UC-050.

  

**Critérios de aceitação**

  

- **HSP-US-0006-AC-01:** Dado credenciais inválidas, quando tentar autenticar, então o sistema nega acesso com mensagem genérica que não revela se a conta existe.

- **HSP-US-0006-AC-02:** Dado conta bloqueada, suspensa ou inativa, quando tentar autenticar, então o acesso é negado e a comunicação do estado segue a política aprovada sem confundi-lo com senha inválida.

  

**Regras/TBD:** limiar de tentativas, captcha, bloqueio temporário, auditoria e mensagem de status `[TBD]`.

**Dependências:** HSP-US-0005; HSP-US-0063.

**Evidência:** `coverage_status=source_only`.

  

### HSP-US-0007 — Autenticar e autorizar Administrador

  

**Tipo:** User Story | **Feature:** HSP-FT-002 | **Status:** Proposed | **Proveniência:** explicit

**Prioridade/Estimativa/Owner/Iteração:** [TBD]

  

**Narrativa:** Como Administrador, quero acessar o back-office com minhas permissões, para executar somente as operações autorizadas ao meu papel.

  

**Fontes:** HSP-SRC-UC-049 (`UC20a`, `HPET20a`); Wiki `US-HPET20A-01`, `US-HPET20A-02`.

  

**Critérios de aceitação**

  

- **HSP-US-0007-AC-01:** Dado Administrador válido, quando autenticar, então recebe acesso com o papel Back-office ou Moderador e as permissões correspondentes.

- **HSP-US-0007-AC-02:** Dado usuário sem papel administrativo, quando acessar recurso do back-office, então o recurso é negado independentemente de estar autenticado.

- **HSP-US-0007-AC-03:** Dado credenciais administrativas inválidas, quando autenticar, então o acesso é negado sem revelar qual campo falhou.

  

**Regras/TBD:** matriz de permissões Back-office/Moderador, MFA e segregação de funções `[TBD]`.

**Dependências:** HSP-US-0005.

**Evidência:** `coverage_status=source_only`.

  

## Decisões do Epic

  

- [ ] HSP-DEC-002 — modelo de identidade e múltiplos papéis.

- [ ] HSP-DEC-003 — senha, sessão, recuperação e bloqueio.

- [ ] HSP-DEC-015 — decisões técnicas de autenticação.