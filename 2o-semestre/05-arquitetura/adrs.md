# Architecture Decision Records (ADR) - Hospetse

Este documento reúne os registros de decisão arquitetural (ADR) do projeto. Cada ADR segue o mesmo padrão, para manter consistência e facilitar a leitura por qualquer pessoa que entre no projeto depois:

- **Título:** identificação da decisão (ADR-XXX)
- **Status:** Proposto, Aceito, Substituído, etc.
- **Contexto:** o problema ou cenário que motivou a decisão
- **Decisão:** o que foi decidido
- **Consequências:** efeitos positivos e negativos, aceitos conscientemente
- **Alternativas consideradas:** opções descartadas e por quê, quando aplicável

Ao adicionar uma nova decisão, basta copiar essa estrutura e criar um novo ADR-XXX.

---

## ADR-001: Uso de Node.js com Express no back-end

**Status:** Aceito

### Contexto

O back-end do sistema precisa ser desenvolvido com uma tecnologia que a equipe já domine, com baixa curva de aprendizado e fácil manutenção, já que parte do time está em formação e aprendeu essa stack em aula.

### Decisão

Utilizar **Node.js** com o framework **Express** para construção da API do back-end.

### Consequências

**Positivas**

- Equipe já possui familiaridade com a linguagem e o framework, reduzindo tempo de desenvolvimento.
- Curva de aprendizado baixa para novos integrantes do time.
- Sintaxe e conceitos de fácil interpretação, facilitando manutenção.
- Ecossistema NPM amplo, com grande quantidade de bibliotecas e documentação.

**Negativas**

- Express é minimalista: não impõe uma arquitetura própria, exigindo que a equipe defina e mantenha padrões de camadas, tratamento de erros e validações manualmente.
- Sem tipagem estática nativa, a menos que se adote TypeScript, o que pode aumentar erros em tempo de execução.

---

## ADR-002: Uso do Prisma como ORM

**Status:** Aceito

### Contexto

É necessária uma ferramenta de ORM para abstrair o acesso ao banco de dados, além de gerenciar migrations e o versionamento do schema SQL de forma organizada. A equipe já possui experiência prévia com essa ferramenta.

### Decisão

Adotar o **Prisma** como ORM para modelagem de dados, geração de migrations e versionamento do banco de dados.

### Consequências

**Positivas**

- Familiaridade prévia da equipe reduz o tempo de adaptação.
- Ferramentas nativas de migration facilitam o versionamento e a evolução do schema.
- Tipagem automática e autocomplete agilizam o desenvolvimento e reduzem erros.
- Boa documentação e curva de aprendizado acessível.

**Negativas**

- Cria acoplamento a um ORM específico, o que pode dificultar uma futura migração para outra ferramenta ou abordagem, como SQL puro ou outro ORM.
- Em queries muito complexas, a abstração do ORM pode exigir soluções alternativas, como raw queries, para manter a performance.

---

## ADR-003: Uso do PostgreSQL como banco de dados

**Status:** Aceito

### Contexto

O sistema de reservas precisa de um banco de dados relacional confiável, com baixo custo de implantação, boa documentação e facilidade de uso para a equipe.

### Decisão

Utilizar **PostgreSQL** como sistema de gerenciamento de banco de dados.

### Consequências

**Positivas**

- Open source, sem custo de licenciamento.
- Documentação extensa e grande comunidade, facilitando a resolução de problemas.
- Recursos avançados, como transações ACID, integridade referencial e constraints, adequados para um sistema de reservas, onde a consistência dos dados é crítica.
- Boa integração com o Prisma.

**Negativas**

- Exige administração e ajustes de performance para cenários de alta carga.
- Escalabilidade horizontal é mais trabalhosa do que em bancos NoSQL.

---

## ADR-004: Uso do Redis como camada de cache

**Status:** Aceito

### Contexto

Para reduzir o custo e a quantidade de requisições diretas ao banco de dados, especialmente em consultas frequentes, como disponibilidade de reservas, é necessária uma camada de cache.

### Decisão

Utilizar **Redis** como camada de cache para dados de leitura frequente.

### Consequências

**Positivas**

- Reduz a carga sobre o PostgreSQL, diminuindo o número de consultas repetidas.
- Baixa latência de leitura, melhorando a percepção de performance para o usuário final.
- Ferramenta madura, com boa documentação e ampla adoção no mercado.

**Negativas**

- Adiciona complexidade ao sistema, incluindo estratégias de invalidação de cache e controle de consistência entre cache e banco.
- Adiciona mais um componente de infraestrutura para manter, monitorar e escalar.

---

## ADR-005: Arquitetura de front-end com React e React Native

**Status:** Aceito

### Contexto

O sistema precisa atender dois públicos e plataformas: uma interface desktop ou web e uma interface mobile. Busca-se reaproveitar conhecimento entre as duas frentes e manter baixa divergência de tecnologias, facilitando a adoção de toda a equipe.

### Decisão

Utilizar **React** para o front-end desktop ou web e **React Native** para o front-end mobile.

### Armazenamento de tokens por plataforma

Os tokens emitidos pelo Auth Server (ADR-011) são guardados de forma diferente em cada plataforma:

- **Mobile (React Native):** access token e refresh token no armazenamento seguro do sistema (Keychain no iOS, Keystore no Android), por exemplo via `expo-secure-store`. Não usar `AsyncStorage`, que não é criptografado.
- **Web (React):** refresh token em cookie `httpOnly`, `Secure` e `SameSite`, inacessível ao JavaScript da página. O access token fica só em memória.

### Consequências

**Positivas**

- Sintaxe, conceitos como componentes, hooks e estado, e boas práticas são compartilhados entre React e React Native, reduzindo a curva de aprendizado entre as duas plataformas.
- Parte da lógica de negócio, hooks e utilitários pode ser reaproveitada entre os projetos.
- Ambas as ferramentas possuem comunidades grandes e documentação extensa.
- Facilita a alocação de desenvolvedores entre as duas frentes, já que a base conceitual é a mesma.

**Negativas**

- Não há compartilhamento total de código: os componentes visuais precisam ser reescritos para cada plataforma, web e nativo.
- Exige atenção às diferenças de comportamento e componentes nativos entre plataformas, como navegação, elementos de UI e APIs específicas do dispositivo.
- Manutenção de duas bases de código distintas, ainda que conceitualmente próximas.
- O Auth Server precisa suportar duas formas de entrega do refresh token: no corpo da resposta (mobile) e em cookie (web).

---

## ADR-006: Estratégia de autenticação com JWT

**Status:** Aceito

### Contexto

O sistema precisa de um mecanismo de autenticação e autorização que funcione de forma consistente entre o back-end e as duas interfaces de front-end, web e mobile, preferencialmente sem exigir estado guardado no servidor.

### Decisão

Utilizar **JWT (JSON Web Token)** como estratégia de autenticação.

### Consequências

**Positivas**

- Abordagem stateless, o que facilita a escalabilidade do back-end, pois não é necessário guardar sessão no servidor.
- Funciona de forma uniforme para o cliente web e para o mobile.
- É amplamente suportado por bibliotecas do ecossistema Node.js e Express, com boa documentação.

**Negativas**

- Revogar um token antes do vencimento não é trivial, exigindo mecanismos adicionais, como blacklist ou refresh tokens de curta duração.
- Exige cuidado no armazenamento do token no cliente, especialmente no navegador, por risco de XSS, além de boas práticas de expiração e renovação.

---

## ADR-007: Hospedagem no Railway

**Status:** Aceito (revisado: o Railway passou a hospedar os serviços, não só o banco)

### Contexto

O projeto precisa de um ambiente publicado para o Auth Server, o back-end principal e os bancos, com deploy automático e baixo custo. A decisão inicial previa o Railway só para o PostgreSQL. Durante o desenvolvimento, os dois serviços também passaram a ser publicados nele.

### Decisão

Hospedar no **Railway**:

| Serviço no Railway | Origem | Conteúdo |
| --- | --- | --- |
| `auth-service` | `hospetse-auth-service` | Container Docker, porta 3001 |
| `backend` | `hospetse-backend` | Container Docker, porta 3000 |
| PostgreSQL do Auth | — | Banco exclusivo do Auth Server |
| PostgreSQL do back-end | — | Banco exclusivo do back-end principal |

- Cada serviço é publicado a partir do `Dockerfile` do próprio repositório (`node:22-slim`, usuário `node`).
- O container roda `npx prisma migrate deploy` antes de subir. Se a migration falhar, o container não sobe e o deploy falha.
- O Railway confere `GET /health` (`railway.json`) antes de liberar a nova versão.
- Variáveis e segredos ficam no painel do Railway, nunca na imagem nem no repositório.
- O deploy é feito pelo estágio CD do Azure Pipelines, só a partir da `develop` e nunca de Pull Request, com `railway up --ci --service <serviço>`. O `RAILWAY_TOKEN` fica no grupo de variáveis `railway`.
- URLs publicadas: Auth em `https://auth-service-production-36e0.up.railway.app` e back-end em `https://backend-production-7298.up.railway.app`. Os contratos ficam em `/docs`.
- Existe um único ambiente publicado. Não há staging.
- O app mobile não roda no Railway: é executado pelo Expo.

### Consequências

**Positivas**

- Serviços e bancos no mesmo provedor, com um só painel.
- Deploy automático e repetível a partir da `develop`.
- Migration e health check impedem que uma versão quebrada receba tráfego.
- Custo baixo, adequado ao estágio do projeto.

**Negativas**

- Ambiente único: o que a `develop` publica é usado por todos, inclusive nos testes.
- Dependência do provedor. Os Dockerfiles reduzem o custo de uma troca futura.
- Limites do plano (memória, conexões, horas) podem exigir revisão se a carga crescer.

### Alternativas consideradas

- **Railway só para o banco** (decisão inicial): substituída quando os serviços passaram a ser publicados no Railway.

---

## ADR-008: Comunicação entre back-end e front-ends via API REST

**Status:** Aceito

### Contexto

O back-end precisa expor dados e funcionalidades para os dois front-ends, web e mobile, de forma padronizada.

### Decisão

Utilizar **API REST** como padrão de comunicação entre o back-end e os clientes web e mobile.

### Consequências

**Positivas**

- Padrão amplamente conhecido pela equipe, com curva de aprendizado baixa.
- Fácil de documentar, por exemplo, com Swagger ou OpenAPI, e de testar, por exemplo, com Postman ou Insomnia.
- Boa compatibilidade com Express e com clientes HTTP tanto em React quanto em React Native.

**Negativas**

- Pode gerar overfetching ou underfetching de dados em certos cenários, em comparação a alternativas como GraphQL.
- Endpoints podem se multiplicar conforme o sistema cresce, exigindo boa organização de rotas.

---

## ADR-009: Padronização para ES Modules em vez de CommonJS

**Status:** Aceito

### Contexto

O back-end foi iniciado utilizando **CommonJS**, com `require` e `module.exports`, padrão adotado automaticamente pelo Node.js quando o projeto não especifica outro.

Os front-ends do projeto, web em React e mobile em React Native, conforme a ADR-005, utilizam ES Modules nativamente.

Manter dois sistemas de módulo diferentes entre back-end e front-end gera troca de sintaxe desnecessária para quem transita entre as duas partes, além de divergir da direção mais atual do ecossistema JavaScript.

### Decisão

Utilizar **ES Modules**, com `import` e `export`, e `"type": "module"` no `package.json`, como padrão de módulos no back-end, substituindo o CommonJS usado inicialmente.

### Consequências

**Positivas**

- Sintaxe de import e export consistente entre back-end e front-end, reduzindo a troca de contexto para quem desenvolve nas duas frentes.
- Padrão amplamente adotado pela comunidade e pelos frameworks mais modernos do ecossistema JavaScript.
- Alinhado com a sintaxe já utilizada em React e React Native, facilitando a transição de desenvolvedores entre front-end e back-end.

**Negativas**

- Exige extensão `.js` explícita em todo import relativo, como `import x from './arquivo.js'`, diferente do CommonJS, onde a extensão é opcional.
- Algumas bibliotecas mais antigas do ecossistema Node ainda assumem CommonJS por padrão, podendo exigir configuração extra ou pacotes de interoperabilidade.
- `require` e `module.exports` deixam de estar disponíveis. A equipe precisa ter atenção para não misturar os dois sistemas de módulo no mesmo projeto.

### Alternativas consideradas

- Manter CommonJS: é o padrão histórico do Node.js e o mais comum em tutoriais e código legado, mas mantém divergência de sintaxe em relação ao front-end.

---

## ADR-010: Estratégia de testes com o test runner nativo do Node

**Status:** Aceito

### Contexto

O back-end precisa de testes automatizados desde o início, mas ainda não havia decisão sobre qual framework usar nem que tipo de teste escrever primeiro.

A equipe já vem priorizando recursos nativos do Node quando suficientes, como ES Modules e variáveis de ambiente via `process.loadEnvFile`, evitando dependências desnecessárias.

### Decisão

Utilizar o **test runner nativo do Node**, com `node:test` e `node:assert`, executado via `node --test`, como framework de testes, sem instalar bibliotecas externas de teste, como Jest ou Mocha.

Os primeiros testes são de integração via HTTP real. O `app` Express é separado da inicialização do servidor. O arquivo `app.js` exporta o app sem chamar `.listen()`, enquanto apenas o `index.js` chama o método `.listen()`.

Essa separação permite que os testes importem o `app`, iniciem o servidor em uma porta efêmera com `app.listen(0)` e façam requisições reais contra ele utilizando Axios.

Os arquivos de teste ficam na pasta `test/`, com o sufixo `.test.js`.

**Escopo:** esta decisão vale para o back-end principal e o Auth Server. O app mobile usa Jest com `jest-expo` e `@testing-library/react-native`, que é o padrão do Expo.

### Consequências

**Positivas**

- Não adiciona dependência de framework de teste externo, reduzindo a superfície de manutenção e aprendizado.
- Separar `app.js` de `index.js` evita que importar o código de produção dispare efeitos colaterais de rede durante os testes.
- Usar uma porta efêmera com `listen(0)` evita conflitos de porta entre testes, com o servidor de desenvolvimento ou entre execuções paralelas, como em CI.

**Negativas**

- Testes de integração via HTTP são mais lentos e menos isolados que testes unitários puros. Eles não substituem testes isolados de regras de negócio quando elas existirem, como em uma camada de serviços.
- `node:test` tem menos recursos de ergonomia, como mocks avançados e relatórios visuais, do que frameworks como Jest ou Vitest. Essa decisão pode ser revisitada se o projeto crescer em complexidade de testes.

### Alternativas consideradas

- Jest ou Vitest: mais populares e com mais recursos, mas adicionam dependência e configuração extra para um projeto que ainda está no estágio inicial.

---

## ADR-011: Separação do Auth Server do back-end principal

**Status:** Aceito (revisado na HSP-PBI-078: o nome passou a ser informado no cadastro)

### Contexto

O Hospetse precisa realizar cadastro, confirmação de e-mail, proteção de senha e emissão de tokens de autenticação.

Essas responsabilidades serão separadas do back-end principal para permitir a experimentação de uma arquitetura distribuída no projeto acadêmico.

O back-end principal continuará responsável pelos dados de negócio, pelo perfil de Tutor, pela completude do cadastro e pelas regras de contratação.

### Problema

Manter autenticação e regras de negócio no mesmo serviço dificultaria a demonstração de uma separação clara de responsabilidades.

Também faria com que o back-end principal precisasse armazenar senhas e controlar diretamente:

- códigos de confirmação;
- proteção de credenciais;
- emissão de JWT;
- validação da identidade;
- recuperação de acesso.

### Decisão

Criar um serviço separado chamado **Auth Server**.

O Auth Server será responsável por:

- receber o e-mail do cadastro;
- gerar e enviar o PIN de confirmação;
- armazenar temporariamente a solicitação de verificação;
- validar o PIN;
- armazenar a senha protegida;
- criar a identidade autenticada;
- emitir o access token (JWT) e o refresh token;
- renovar o access token a partir de um refresh token válido;
- comunicar a criação da identidade ao back-end principal, repassando o nome informado no cadastro.

O back-end principal será responsável por:

- criar o registro local em `USER`, com o nome informado no cadastro;
- criar o registro em `TUTOR` e receber os demais dados na conclusão do perfil (`PUT /api/v1/me/tutor-profile`);
- controlar CPF, endereço e demais dados de negócio;
- aplicar as regras de contratação.

O telefone não fará parte do cadastro inicial nem do login.

### Fluxo definido

```text
1. O front-end envia o e-mail ao Auth Server
2. O Auth Server cria uma solicitação temporária
3. O Auth Server envia o PIN por e-mail
4. O usuário informa o PIN e o payload do cadastro (nome, senha, confirmação de senha e aceites)
5. O Auth Server valida o PIN e os dados
6. O Auth Server cria a identidade autenticada
7. O Auth Server comunica a criação ao back-end principal (auth_user_id, e-mail e nome)
8. O back-end principal cria o registro USER (o TUTOR nasce depois, na conclusão do perfil)
9. O Auth Server emite o access token (JWT) e o refresh token
10. O front-end recebe os tokens
```

A solicitação do PIN não cria `USER`, não cria `TUTOR` e não emite JWT.

O Auth Server e o back-end principal têm, cada um, o seu próprio banco PostgreSQL. Não existe chave estrangeira entre eles: o vínculo é o valor `users.auth_user_id = user_auth.id`, mantido pela rota interna.

A conta só é criada após a validação correta do PIN e do payload completo.

O e-mail usado no cadastro é o registrado na solicitação de verificação, não um e-mail enviado novamente pelo front-end.

### Integração entre os serviços

O Auth Server deve comunicar ao back-end principal somente:

- `auth_user_id`;
- e-mail confirmado;
- nome informado no cadastro.

A senha não é enviada.

O nome só passa pelo Auth Server: ele é repassado ao back-end principal e não é armazenado no Auth Server.

A chamada é feita por uma rota interna idempotente (`PUT /api/v1/internal/users/:authUserId`), para que o Auth Server possa repetir a chamada em caso de falha sem duplicar o `USER`.

O JWT deve conter um identificador estável da identidade no campo `sub`.

O back-end principal deve usar esse identificador para localizar o `USER` correspondente.

A comunicação entre os serviços deve ser autenticada. O back-end principal não deve confiar em um identificador de usuário enviado diretamente pelo front-end.

### Tokens

- **Access token (JWT):** vida curta (15 min, ver ADR-012), enviado em toda requisição ao back-end principal e validado sem consulta ao Auth Server.
- **Assinatura do JWT: RS256.** A chave privada fica somente no Auth Server e é a única capaz de emitir tokens. O back-end principal recebe somente a chave pública e apenas valida. Se a chave pública vazar, nenhum token pode ser forjado. Nenhuma das chaves é versionada.
- **Refresh token:** vida longa, usado somente para obter um novo access token. É armazenado no Auth Server apenas como hash e pode ser revogado.
- No cliente, os tokens são guardados conforme a plataforma (ver ADR-005): armazenamento seguro no mobile e cookie `httpOnly` na web.

### Padrão de rotas

As rotas do Auth Server seguem as convenções da API do projeto: prefixo `/api/v1/`, envelope de resposta e formato de erro padronizado.

### Senhas e credenciais

A senha deve ser armazenada somente pelo Auth Server, utilizando hash seguro (Argon2id).

O back-end principal não deve receber nem armazenar a senha.

O código PIN não deve ser armazenado em texto puro e não deve aparecer em logs.

### Perfil incompleto

Após a confirmação do e-mail, o back-end principal cria o registro em `USER`, com o nome informado no cadastro e `account_type = user`. O `account_type` aceita só `user` e `admin`: ser Tutor ou prestador é deduzido pelos registros em `TUTOR` e `PROVIDER`, e um mesmo `USER` pode ter os dois.

O registro em `TUTOR` é criado na conclusão do perfil (`PUT /api/v1/me/tutor-profile`), junto com CPF, data de nascimento, telefone, endereço e contato de emergência, direto no back-end principal.

Perfil incompleto significa que o `USER` existe com `profile_status = incomplete`: o `TUTOR` ainda não existe ou ainda não tem todos os dados obrigatórios.

Um usuário autenticado com perfil incompleto pode:

- acessar a plataforma;
- navegar;
- buscar serviços.

Um usuário com perfil incompleto não pode contratar serviços até concluir os dados obrigatórios do perfil.

Ao fazer login com perfil incompleto, o usuário é direcionado para a conclusão do perfil.

### Consequências

**Positivas**

- Separação clara entre autenticação e regras de negócio.
- Nenhuma senha armazenada no back-end principal.
- Auth Server reutilizável para Tutor, prestadores e administradores.
- Possibilidade de testar uma arquitetura distribuída.
- Responsabilidades mais fáceis de testar isoladamente.
- JWT emitido por um único serviço responsável pela identidade.
- O Auth Server não guarda dados de perfil: o nome só é repassado.
- Toda conta já nasce com nome, o que simplifica o tratamento no back-end e no front-end.

**Negativas**

- Dois serviços precisam ser executados e configurados.
- Existe comunicação interna entre Auth Server e back-end principal.
- Falhas de comunicação podem impedir a criação local do `USER`.
- Será necessário validar assinatura, emissor e identificador do JWT no back-end principal.
- O projeto terá maior complexidade operacional.
- Como o Auth Server não guarda o nome, uma nova tentativa de cadastro após falha do back-end precisa receber o nome de novo do front-end.
- O RS256 exige gerar e distribuir um par de chaves (privada no Auth Server, pública no back-end principal).

### Alternativas consideradas

#### Manter autenticação no back-end principal

Foi considerada a manutenção de cadastro, senha, PIN e JWT no mesmo back-end.

Essa alternativa seria mais simples, mas não atende ao objetivo acadêmico de experimentar a separação de serviços e mistura responsabilidades de autenticação com regras de negócio.

#### Criar o JWT antes de confirmar o USER no back-end principal

Essa alternativa foi rejeitada porque poderia gerar um JWT válido para uma identidade que ainda não possui registro correspondente no back-end principal.

O Auth Server deve comunicar a criação da identidade ao back-end principal antes de finalizar o fluxo de emissão do JWT.

#### Pedir o nome só na conclusão do perfil, direto no back-end

Era a decisão inicial: o cadastro criava o `USER` sem nome, e o nome só era informado na conclusão do perfil.

Foi substituída durante a HSP-PBI-078: o nome passou a ser pedido no cadastro e é gravado no `USER`. O `TUTOR` continua sendo criado só na conclusão do perfil, e copia o nome do `USER`.

A preocupação original era que o Auth Server teria que guardar o nome para repetir a chamada ao back-end em caso de falha. Isso foi resolvido sem guardar o nome: a nova tentativa de cadastro recebe o nome de novo do front-end.

#### Assinar o JWT com HS256 (chave simétrica)

Rejeitada porque o mesmo segredo precisaria existir no Auth Server e no back-end principal. Um vazamento em qualquer um dos dois permitiria emitir tokens válidos para qualquer usuário.

---

## ADR-012: Emissão do access token (JWT) no Auth Server

**Status:** Aceito

### Contexto

A ADR-011 definiu que o Auth Server emite o access token assinado com RS256 e que o back-end principal apenas valida, com a chave pública. Faltava definir a biblioteca, o tempo de vida, o conteúdo do token e onde as chaves ficam.

Um JWT não pode ser revogado antes do vencimento (ADR-006): ele vale até o `exp`. Por isso o tempo de vida do access token limita o estrago de um token roubado.

### Decisão

- **Biblioteca:** `jose`, no Auth Server.
- **Tempo de vida do access token:** **15 minutos**. A renovação sem pedir a senha de novo é feita pelo refresh token.
- **Claims:**

| Claim | Valor |
| --- | --- |
| `sub` | `authUserId` (id da `user_auth`) |
| `iss` | `hospetse-auth-service` |
| `aud` | `hospetse-backend` |
| `iat` | momento da emissão |
| `exp` | `iat` + 15 min |

- **Sem dados pessoais no token:** e-mail, nome e outros dados não vão no payload, porque ele é só base64 e qualquer um consegue ler. O back-end busca o `USER` pelo `sub`.
- **Validação no back-end principal:** assinatura com a chave pública, depois `exp`, `iss` e `aud`. Se qualquer um falhar, responde 401.
- **Chaves:** par RSA de 2048 bits, gerado com OpenSSL. As duas chaves ficam em PEM nas variáveis de ambiente `JWT_PRIVATE_KEY` e `JWT_PUBLIC_KEY`, nunca versionadas. O back-end principal recebe somente a pública.

### Consequências

**Positivas**

- Token roubado tem uso limitado a no máximo 15 minutos.
- Token pequeno e sem dados pessoais.
- `iss` e `aud` impedem que um token emitido para outro fim seja aceito pelo back-end.
- `jose` não tem dependências e usa ES Modules e `async/await`, no mesmo estilo do projeto (ADR-009).
- Chaves em variável de ambiente seguem o padrão já usado (`DATABASE_URL`, `PIN_HMAC_SECRET`) e funcionam no Railway sem arquivos no servidor.

**Negativas**

- O cliente precisa renovar o token a cada 15 minutos usando o refresh token.
- Mudanças no usuário, como bloqueio da conta, só passam a valer quando o token atual vence.
- `jose` tem menos material na internet do que `jsonwebtoken`.
- Trocar o par de chaves exige atualizar a chave pública em todo serviço que valida o token.

### Alternativas consideradas

- **`jsonwebtoken`:** a biblioteca de JWT mais conhecida em Node, mas é CommonJS, traz várias dependências e tem manutenção mais lenta.
- **Tempo de vida maior (1 hora ou mais):** menos renovações, mas um token roubado vale por mais tempo.
- **Caminho do arquivo da chave no `.env`:** simples localmente, mas exige colocar o arquivo da chave no servidor no deploy.

---

## ADR-013: Axios como cliente HTTP padrão

**Status:** Aceito

### Contexto

O projeto faz requisições HTTP em dois lugares:

- nos testes de integração (ADR-010), que chamam a API real;
- na comunicação entre serviços (ADR-011), em que o Auth Server chama a rota interna do back-end principal para criar o `USER`.

Sem um padrão, cada parte do código poderia usar um cliente diferente (`fetch`, `axios`, `got`), com tratamento de erro e configuração diferentes.

### Decisão

Utilizar **Axios** como cliente HTTP padrão no Auth Server, no back-end e no app mobile, tanto nos testes quanto no código de produção. No mobile, a instância do Auth Server fica em `src/services/api.ts` (`authApi`).

- Nos serviços que chamam outro serviço, criar uma instância com `axios.create({ baseURL, timeout })` uma vez, no construtor da classe que faz a chamada, e não a cada requisição.
- O Axios fica em `dependencies` quando o código de produção usa, e não só em `devDependencies`.

### Consequências

**Positivas**

- Mesmo cliente e mesma forma de tratar erros em testes e produção, reduzindo o que a equipe precisa aprender.
- Respostas com status 4xx e 5xx viram erro automaticamente, com o status em `error.response.status`, o que simplifica distinguir falha de negócio (como 409) de falha passageira (5xx ou sem resposta).
- `baseURL` e `timeout` configurados uma vez na instância.
- Biblioteca madura, com bastante documentação e amplamente adotada.

**Negativas**

- Adiciona uma dependência externa, sendo que o Node já tem `fetch` nativo.
- Precisa ser mantido atualizado, como qualquer dependência, inclusive por correções de segurança.

### Alternativas consideradas

- **`fetch` nativo do Node:** sem dependência, mas não lança erro em status 4xx e 5xx, e não tem `timeout` nem `baseURL` prontos, exigindo mais código manual para o mesmo resultado.

---

## ADR-014: Estratégia de branches e nomenclatura

**Status:** Aceito

### Contexto

O regulamento da Escola de TI exige que toda tarefa seja executada em uma branch separada, derivada de uma branch de desenvolvimento, e que o time defina a sua estratégia de merge e release. Também aponta como não-conformidade:

- branch fora do formato `"#1332 - efetuando implementações iniciais"`;
- merge sem a opção `--no-ff`;
- trabalhar em uma issue sem a branch correspondente.

O formato do regulamento não pode ser usado ao pé da letra, porque o Git não aceita espaços em nome de branch.

Até aqui, os repositórios usavam apenas `main` e branches no formato `<id>-<descricao>` (ex.: `251-solicitacao-de-pin-e-envio-de-email`, no Auth Server), sem distinguir produção de desenvolvimento nem o tipo da mudança. Os commits já seguem o padrão `feat:`, `fix:` e `docs:`.

### Decisão

**Branches permanentes:**

| Branch | Papel |
| --- | --- |
| `main` | Produção. Recebe código somente por Pull Request a partir da `develop`, com pelo menos 1 revisor que não seja o autor. Push direto bloqueado. |
| `develop` | Desenvolvimento e **branch padrão** do repositório. Toda branch de trabalho nasce dela e volta para ela por Pull Request. |

**Branches de trabalho:** `<tipo>/<id-do-work-item>-<titulo-da-task-em-kebab-case>`

| Tipo | Quando usar | Commit correspondente |
| --- | --- | --- |
| `feature/` | Nova funcionalidade | `feat:` |
| `fix/` | Correção de defeito | `fix:` |
| `docs/` | Documentação e artefatos | `docs:` |
| `chore/` | Configuração, dependências, build | `chore:` |
| `test/` | Somente testes | `test:` |

- O número é o ID do work item no Azure DevOps e vem logo depois do tipo.
- A descrição é o título da task em minúsculas, sem acentos, com palavras separadas por hífen.
- Exemplo: task #268 "Integrar o cadastro ao Auth Server" → `feature/268-integrar-o-cadastro-ao-auth-server`.

**Commits:** commit e push a cada dia trabalhado, no formato `<tipo>: <descrição> (#<id>, AB#<id>)`. As referências ao work item ligam o commit à task no Azure Boards.

Exemplo: `chore: adiciona cliente HTTP do Auth Server, secure store e dependências de teste (#268, AB#268)`.

**Merge:** sempre por Pull Request, com merge sem fast-forward (`--no-ff`, opção *Merge (no fast-forward)* no Azure DevOps). A branch de trabalho é apagada depois do merge.

### Consequências

**Positivas**

- Atende às exigências do regulamento: branch por tarefa, derivada da branch de desenvolvimento, ID da issue no nome e merge `--no-ff`.
- O prefixo do tipo agrupa as branches em pastas no Azure DevOps e deixa o propósito claro antes de abrir o Pull Request.
- Padrão alinhado com o tipo do commit, sem regra nova para aprender.
- A `main` protegida garante que produção só recebe código revisado.

**Negativas**

- O nome difere do exemplo literal do regulamento (sem `#` e sem espaços). É preciso comunicar o padrão ao auditor para não gerar não-conformidade por interpretação.
- As branches já existentes no Auth Server ficam sem prefixo. O padrão vale para as novas branches.
- A política de revisor na `main` exige que outra pessoa do time esteja disponível para aprovar cada release.

### Alternativas consideradas

- **Formato literal do regulamento (`#268 - descrição`):** impossível no Git por causa dos espaços. A variação `#268-descricao` funciona, mas o `#` no início é interpretado como comentário no terminal e exige aspas em todo comando.
- **Sem prefixo de tipo (`268-descricao`):** padrão usado até então. Mais simples, mas não mostra o tipo da mudança nem agrupa as branches.
- **GitFlow completo (`release/` e `hotfix/`):** descartado por enquanto. O volume de releases do projeto não justifica a complexidade. Pode ser revisto quando houver deploy contínuo em produção.

---

## ADR-015: Organização do back-end por módulo e por caso de uso

**Status:** Aceito (o código já segue o padrão: `hosts/host-search.controller.js`, `hosts/host-search-policy.js`, `users/profile-policy.js`)

### Contexto

O Express não impõe estrutura de pastas nem de camadas (ADR-001). O back-end já segue, sem registro, um padrão: pastas por domínio (`users/`, `tutors/`) e um par controller/service por caso de uso (`me.controller.js`, `tutor-profile.controller.js`).

O módulo de Host vai receber várias tasks de pessoas diferentes em paralelo: detalhes públicos (#479), cadastro e submissão (#323 e #325) e agenda (#482). Com um único `host.controller.js`, todas editariam o mesmo arquivo. Rotas públicas e rotas do próprio Host também ficariam lado a lado, com regras de acesso diferentes.

### Decisão

- **Pasta por domínio** em `src/`: `users/`, `tutors/`, `hosts/` e assim por diante.
- **Um par de arquivos por caso de uso:** `<caso>.controller.js` (HTTP: lê a requisição, valida e traduz o resultado em status) e `<caso>.service.js` (regra de negócio e Prisma, sem conhecer HTTP).
  - Exemplo: `hosts/host-details.controller.js` e `hosts/host-details.service.js` para `GET /api/v1/hosts/:id`.
- **Regra compartilhada** entre casos de uso do mesmo domínio vai para `<dominio>-policy.js`, como `users/profile-policy.js`. Os controllers continuam separados.
- **Rotas registradas no `app.js`**, uma linha por rota, com o comentário da task.
- **Respostas públicas usam `select` explícito no Prisma.** Um caso de uso público nunca reaproveita a consulta de um caso de uso do próprio dono, para não expor dado privado (LGPD).

### Consequências

**Positivas**

- Cada task toca os seus próprios arquivos: menos conflito de merge e Pull Requests menores.
- Arquivos pequenos, mais fáceis de revisar e de testar.
- A regra de acesso de cada rota fica isolada no seu controller.
- Formaliza o padrão que o código já segue.

**Negativas**

- Mais arquivos por domínio.
- Código repetido entre casos de uso só é percebido na revisão. A equipe precisa extrair para o `-policy.js` quando a repetição aparecer.

### Alternativas consideradas

- **Um controller e um service por domínio** (`host.controller.js` com tudo): menos arquivos, mas concentra tasks paralelas no mesmo arquivo e mistura rotas públicas com rotas do dono.
- **`express.Router()` por domínio:** agrupa as rotas, mas fica para depois, se o `app.js` crescer demais. Hoje a proteção por padrão está no middleware de `/api/v1`.

---

## Próximos ADRs sugeridos

Conforme o projeto evoluir, pode ser interessante registrar decisões futuras como:

- Hospedagem da interface web (React).
- Ambiente de staging separado da `develop`.
- Qual branch publica produção (ver ADR-014).
