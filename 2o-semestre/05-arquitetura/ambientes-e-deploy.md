# Hospetse — Documento de Ambientes

> Documento técnico para o System Design e o Relatório Final.
>
> **Fonte:** configurações versionadas, `.env.example`, READMEs, Dockerfiles, pipelines Azure DevOps e configuração Railway dos repositórios locais.
>
> **Regra:** valores secretos, URLs privadas, tokens, senhas e chaves reais não aparecem neste documento.

## 1. Visão geral

O Hospetse é composto por três repositórios principais:

| Repositório | Responsabilidade | Execução principal |
| --- | --- | --- |
| `hospetse-mobile` | Aplicativo para Tutor, Host, Pet Sitter e Pet Walker | Expo SDK 58 / React Native |
| `hospetse-auth-service` | Cadastro, PIN, senha, JWT e refresh token | Node.js / Express / Prisma |
| `hospetse-backend` | API principal e regras do domínio | Node.js / Express / Prisma |

O Auth Service e o Backend possuem bancos PostgreSQL separados. O Auth Service provisiona o usuário no Backend por uma rota interna autenticada com segredo compartilhado.

## 2. Ambientes identificados

| Ambiente | Mobile | Auth Service | Backend | Banco | Estado |
| --- | --- | --- | --- | --- | --- |
| Desenvolvimento local | Expo Go ou Expo Web | `localhost:3001` | `localhost:3000` | PostgreSQL do Auth e PostgreSQL do Backend | Configurado |
| CI | Não executado neste pipeline | Container temporário PostgreSQL 16 | Container temporário PostgreSQL 16 | Bancos descartáveis por job | Configurado |
| Railway publicado | App aponta para URL pública configurada | Serviço `auth-service` | Serviço `backend` | PostgreSQL hospedado no Railway | Configurado |
| Staging separado | Não identificado | Não identificado | Não identificado | Não identificado | **Pendente** |

O pipeline registra que o Railway possui atualmente um único ambiente publicado. Não há um ambiente de staging separado identificado nos arquivos analisados.

## 3. Ambiente de desenvolvimento local

### 3.1 Pré-requisitos

- Node.js 22 é usado nos pipelines e nas imagens Docker.
- O Backend declara Node.js `>=20.12.0` no `package.json`.
- npm.
- Dois bancos PostgreSQL acessíveis:
  - um banco exclusivo do Auth Service;
  - um banco exclusivo do Backend.
- Dependências instaladas em cada repositório.

Não é necessário instalar PostgreSQL ou Docker para usar o banco de desenvolvimento hospedado no Railway, conforme o README do Backend. Para desenvolvimento isolado, os dois serviços também aceitam PostgreSQL local.

### 3.2 Ordem de configuração

1. Instalar dependências no `hospetse-auth-service`, `hospetse-backend` e `hospetse-mobile`.
2. Criar o `.env` do Auth a partir de `.env.example`.
3. Criar o `.env` do Backend a partir de `.env.example`.
4. Configurar bancos separados para Auth e Backend.
5. Gerar o Prisma Client em cada serviço.
6. Aplicar as migrations em cada banco.
7. Iniciar o Backend na porta `3000`.
8. Iniciar o Auth Service na porta `3001`.
9. Criar `.env.local` no Mobile a partir de `.env.example`.
10. Iniciar o Expo com `npx expo start -c`.

O cadastro depende do Backend estar disponível, porque o Auth Service chama a rota interna de provisionamento do USER.

### 3.3 Portas e URLs locais

| Componente | Endereço padrão | Uso |
| --- | --- | --- |
| Backend | `http://localhost:3000` | API principal |
| Auth Service | `http://localhost:3001` | Cadastro, login e tokens |
| Expo Web | `http://localhost:8081` | Execução web do Mobile |
| PostgreSQL Auth | Definido em `DATABASE_URL` | Banco exclusivo do Auth |
| PostgreSQL Backend | Definido em `DATABASE_URL` | Banco exclusivo do Backend |

O Mobile usa atualmente `EXPO_PUBLIC_AUTH_URL` para autenticação. A configuração padrão do Backend é:

```env
# Desenvolvimento local
EXPO_PUBLIC_API_URL=http://localhost:3000

# Ambiente publicado
EXPO_PUBLIC_API_URL=<URL publicada do back-end, na Wiki #88>
```

A variável ainda não é consumida pelo código atual do Mobile, porque `searchHosts` e `getHostPage` usam dados de exemplo. Ela deve ser usada quando a integração com a API principal for ativada.

## 4. Variáveis de ambiente

### 4.1 Auth Service

Arquivo de referência: `hospetse-auth-service/.env.example`.

| Variável | Classificação | Finalidade |
| --- | --- | --- |
| `DATABASE_URL` | Secreta | Conexão com o PostgreSQL exclusivo do Auth |
| `PORT` | Pública | Porta HTTP do serviço, padrão `3001` |
| `PIN_HMAC_SECRET` | Secreta | HMAC dos PINs |
| `BACKEND_URL` | Configuração | URL do Backend usado no provisionamento |
| `CORS_ORIGINS` | Configuração | Origens permitidas no CORS |
| `INTERNAL_SERVICE_TOKEN` | Secreta | Autenticação entre Auth e Backend |
| `JWT_PRIVATE_KEY` | Secreta | Chave privada RS256, exclusiva do Auth |
| `JWT_PUBLIC_KEY` | Configuração sensível | Chave pública correspondente, compartilhada com o Backend |
| `SEED_ADMIN_PASSWORD` | Secreta | Senha temporária da conta de demonstração |
| `SEED_TUTOR_PASSWORD` | Secreta | Senha temporária das contas de demonstração |
| `RESEND_API_KEY` | Secreta | Envio de e-mail pelo Resend |
| `EMAIL_FROM` | Configuração | Remetente do e-mail do PIN |

Sem `RESEND_API_KEY`, o desenvolvimento usa o `ConsoleEmailSender`, que exibe o PIN no terminal. Essa configuração não deve ser usada em produção.

### 4.2 Backend

Arquivo de referência: `hospetse-backend/.env.example`.

| Variável | Classificação | Finalidade |
| --- | --- | --- |
| `PORT` | Pública | Porta HTTP do serviço, padrão `3000` |
| `DATABASE_URL` | Secreta | Conexão com o PostgreSQL exclusivo do Backend |
| `SMOKE_DATABASE_URL` | Secreta e opcional | Banco temporário dos testes de falha |
| `INTERNAL_SERVICE_TOKEN` | Secreta | Validação das chamadas internas do Auth |
| `JWT_PUBLIC_KEY` | Configuração sensível | Validação dos JWT emitidos pelo Auth |
| `CORS_ORIGINS` | Configuração | Origens permitidas no CORS |
| `STRIPE_SECRET_KEY` | Secreta | Chave restrita do Stripe em modo teste (`rk_test_...`), usada para criar cobranças, estornos e formas de pagamento |
| `STRIPE_WEBHOOK_SECRET` | Secreta | Segredo de assinatura do webhook (`whsec_...`), usado para validar o header `Stripe-Signature` em `POST /webhooks/stripe` |

O `INTERNAL_SERVICE_TOKEN` deve ter o mesmo valor no Auth e no Backend dentro do mesmo ambiente, mas valores diferentes entre ambientes.

As variáveis do Stripe seguem o Documento de Integração de Pagamento (#511):

- O Backend usa chave restrita (`rk_`), não a chave secreta completa (`sk_`).
- O Railway tem um único ambiente, então usa só chaves de teste.
- O `STRIPE_WEBHOOK_SECRET` é diferente por ambiente. O endpoint publicado tem um segredo; o `stripe listen` local gera outro.
- O webhook publicado fica em `<URL publicada do back-end>/webhooks/stripe` ([Wiki #88](https://dev.azure.com/escola-ti/Hospetse/_wiki/wikis/Hospetse.wiki/88)), fora do `/api/v1`, porque não usa JWT.
- Em desenvolvimento, a Stripe CLI encaminha os eventos para o Backend local:

```bash
stripe login
stripe listen --forward-to localhost:3000/webhooks/stripe
```

### 4.3 Mobile

Arquivo de referência: `hospetse-mobile/.env.example`.

| Variável | Classificação | Finalidade |
| --- | --- | --- |
| `EXPO_PUBLIC_AUTH_URL` | Pública | URL do Auth Service usada no cadastro, login e renovação de tokens |
| `EXPO_PUBLIC_API_URL` | Pública | URL padrão do Backend: `http://localhost:3000` local ou a URL publicada ([Wiki #88](https://dev.azure.com/escola-ti/Hospetse/_wiki/wikis/Hospetse.wiki/88)) |
| `EXPO_PUBLIC_STRIPE_PUBLISHABLE_KEY` | Pública | Chave publicável do Stripe em modo teste (`pk_test_...`), usada pelo `StripeProvider` para abrir o PaymentSheet |

A chave publicável é a única chave do Stripe que pode ficar no App. A chave secreta e o segredo do webhook ficam só no Backend.

Variáveis com prefixo `EXPO_PUBLIC_` entram no aplicativo e podem ser lidas por qualquer usuário. Nunca colocar senha, token privado ou chave nesse arquivo.

O Mobile armazena tokens no `expo-secure-store`. Login e cadastro funcionam no Expo Go; na web existem limitações relacionadas ao SecureStore e ao CORS.

## 5. Health checks

### Auth Service

- `GET /health`
- Railway usa `/health` como `healthcheckPath`.
- A rota verifica o processo e a conexão com o banco.
- `200`: servidor e banco disponíveis.
- `503`: servidor ativo, banco indisponível.

### Backend

- `GET /health`
- `GET /health/ready`
- Railway usa `/health` como `healthcheckPath`.
- `/health` verifica liveness sem acessar o banco.
- `/health/ready` verifica a conexão com o banco com limite de 2 segundos.
- `200` em `/health/ready`: banco disponível.
- `503` em `/health/ready`: banco indisponível ou timeout.

## 6. Containers e execução publicada

Auth Service e Backend usam Dockerfiles equivalentes:

- Imagem `node:22-slim`.
- OpenSSL instalado para o Prisma.
- Dependências instaladas com `npm ci`.
- Prisma Client gerado durante o build.
- Processo executado como usuário não-root `node`.
- Variáveis recebidas do ambiente de execução.
- Migrations aplicadas antes da inicialização do servidor.
- Falha de migration impede o container de subir.

| Serviço | Porta exposta | Comando de inicialização |
| --- | ---: | --- |
| Auth Service | `3001` | `npx prisma migrate deploy && node index.js` |
| Backend | `3000` | `npx prisma migrate deploy && node index.js` |

As imagens não carregam `.env` dentro do container. Os valores vêm do Railway ou de `--env-file` em execução local.

### 6.1 Exemplo de Dockerfile

Auth Service e Backend usam a mesma estratégia. A diferença principal é a porta exposta e o nome do serviço no deploy.

```dockerfile
FROM node:22-slim

RUN apt-get update -y \
  && apt-get install -y openssl \
  && rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY package*.json prisma.config.ts ./
COPY prisma ./prisma
RUN npm ci

# Gera o Prisma Client sem conectar a um banco real durante o build.
RUN DATABASE_URL="postgresql://build:build@localhost:5432/build" \
  npx prisma generate

COPY . .
USER node

# Auth Service: 3001 | Backend: 3000
EXPOSE 3001

# A migration falha antes de o servidor subir.
CMD ["sh", "-c", "npx prisma migrate deploy && node index.js"]
```

O Dockerfile do Backend troca `EXPOSE 3001` por `EXPOSE 3000`. As variáveis reais continuam fora da imagem.

## 7. CI e CD

### Auth Service

Arquivo: `hospetse-auth-service/azure-pipelines.yml`.

- Executa em push para `develop` e `main`.
- Validação de Pull Request ocorre pela branch policy.
- Usa `ubuntu-latest`.
- Usa Node.js 22.
- Sobe PostgreSQL 16 descartável com banco `auth_test`.
- Executa `npm ci`.
- Executa lint.
- Gera o Prisma Client.
- Aplica migrations.
- Executa os testes.
- CD executa somente em `develop` fora de Pull Request.
- Deploy usa Railway CLI no serviço `auth-service`.

### Backend

Arquivo: `hospetse-backend/azure-pipelines.yml`.

- Executa em push para `develop` e `main`.
- Validação de Pull Request ocorre pela branch policy.
- Usa `ubuntu-latest`.
- Usa Node.js 22 no CI.
- Sobe PostgreSQL 16 descartável com banco `backend_test`.
- Gera o Prisma Client.
- Executa lint.
- Executa testes sem banco.
- Aplica migrations.
- Executa testes de integração.
- CD executa somente em `develop` fora de Pull Request.
- Deploy usa Railway CLI no serviço `backend`.

### Exemplo resumido de pipeline Azure DevOps

Os dois serviços usam o mesmo desenho. O Auth executa os testes do Auth e publica `auth-service`; o Backend executa testes unitários e de integração e publica `backend`.

```yaml
trigger:
  branches:
    include: [develop, main]

resources:
  containers:
    - container: postgres
      image: postgres:16
      env:
        POSTGRES_USER: postgres
        POSTGRES_PASSWORD: postgres
        POSTGRES_DB: service_test

stages:
  - stage: CI
    jobs:
      - job: lint_e_testes
        pool:
          vmImage: ubuntu-latest
        services:
          postgres: postgres
        steps:
          - task: UseNode@1
            inputs:
              version: 22.x
          - script: npm ci
          - script: npm run lint
          - script: npx prisma generate
          - script: npx prisma migrate deploy
          - script: npm test

  - stage: CD
    dependsOn: CI
    condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/develop'), ne(variables['Build.Reason'], 'PullRequest'))
    jobs:
      - job: deploy_railway
        steps:
          - script: npm install -g @railway/cli
          - script: railway up --ci --service auth-service
            env:
              RAILWAY_TOKEN: $(RAILWAY_TOKEN)
```

No Backend, o serviço publicado é `backend` e o pipeline também executa `npm run test:integration`. O `RAILWAY_TOKEN` vem do grupo seguro `railway`; ele não fica no YAML.

### Mobile

O repositório Mobile possui scripts locais para Expo, Jest e TypeScript, mas não foi identificado um pipeline Azure DevOps equivalente nos arquivos analisados.

Comandos documentados:

```bash
npm test
npx tsc --noEmit
npx expo start -c
```

Builds nativos e distribuição por loja ainda não estão descritos neste documento como um pipeline publicado.

## 8. Seeds e dados de demonstração

Os seeds são manuais e separados por banco:

1. Executar o seed do Backend.
2. Executar o seed do Auth Service.
3. Usar os mesmos UUIDs e e-mails reservados nos dois repositórios.
4. Validar o login pelo Auth.
5. Validar `/api/v1/me` e os pets pelo Backend.

Não existe transação distribuída entre os dois bancos. Uma falha no segundo seed exige correção e nova execução.

Seeds não entram no Dockerfile nem no pipeline de deploy.

## 9. Segurança por ambiente

- `.env` e `.env.local` ficam fora do versionamento.
- Segredos não devem aparecer em código, logs, documentação ou variáveis `EXPO_PUBLIC_`.
- A chave privada JWT existe somente no Auth Service.
- O Backend recebe apenas a chave pública JWT.
- O token interno entre Auth e Backend é compartilhado somente dentro do mesmo ambiente.
- Cada ambiente deve ter seu próprio par de chaves JWT.
- O `RESEND_API_KEY` é obrigatório para envio real de PIN em produção.
- O `STRIPE_SECRET_KEY` e o `STRIPE_WEBHOOK_SECRET` ficam só no Backend. O App recebe apenas a chave publicável.
- O Backend confere a assinatura de todo evento do webhook do Stripe antes de alterar uma reserva ou transação.
- O ambiente publicado usa somente chaves de teste do Stripe, sem movimentar dinheiro.
- O Mobile usa SecureStore para tokens nativos.
- O Auth e o Backend usam migrations no início do container.

