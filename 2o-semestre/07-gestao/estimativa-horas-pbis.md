# Estimativa inicial em horas por PBI: Hospetse

  

Fonte dos PBIs: Azure DevOps (escola-ti/Hospetse), consulta de 2026-10-18T12:49Z. Estimativa inicial para o time revisar; não foi gravada no Azure.

  

## Como ler

  

- Horas de esforço (pessoa-hora), não prazo de calendário.

- **Provável:** soma das horas de cada parte do PBI (back-end, interface, testes, revisão de PR e validação da PO). A composição está no fim do documento.

- **Otimista e pessimista:** faixa conforme a confiança. Alta: −25% e +40%. Média: −35% e +60%. Baixa: −40% e +90%.

- **Confiança:** o quanto o PBI está claro e o quanto já existe no código e nas decisões, pela análise feita no backlog.

- **(restante):** PBI em andamento; as horas cobrem só o que falta, conforme as Tasks no Azure.

- **precisa de ajuste:** o PBI precisa ser refinado ou reescrito. Quando há horas, elas já usam o escopo esperado depois do ajuste.

  

## Premissas

  

- Time júnior usando agentes de IA para pesquisa e geração de trechos de código. A IA acelera código repetitivo (rotas simples, formulários, testes), mas revisar e depurar o que ela gera também consome tempo. Integração, concorrência e segurança aceleram pouco.

- Interface no Mobile para Tutor, Host, Pet Sitter e Pet Walker, e na Web para o Administrador, conforme as decisões DF/DA (Tasks #247 e #248).

- Inclui o que a Definition of Done pede: testes automatizados, revisão de PR e validação da PO.

- Não inclui o trabalho que ainda não tem PBI (apps base, base do back-end, armazenamento de arquivos, deploy). Ele está na tabela separada.

- Quando uma decisão de produto está aberta, as horas usam o padrão mais simples registrado na análise do PBI.

  

## Horas por PBI

  

### HSP-EP-001 Fundação da plataforma, identidade e confiança

  

| PBI | Otimista | Provável | Pessimista | Confiança |

|---|---:|---:|---:|---|

| #103 HSP-PBI-001 Criar conta | 34 | 53 | 85 | Média · precisa de ajuste |

| #104 HSP-PBI-002 Completar perfil do Tutor | 19 | 29 | 46 | Média |

| #105 HSP-PBI-003 Redefinir senha | 20 | 30 | 48 | Média |

| #106 HSP-PBI-004 Atualizar perfil do Tutor | 10 | 16 | 26 | Média |

| #107 HSP-PBI-005 Autenticar conta | 18 | 28 | 45 | Média |

| #108 HSP-PBI-006 Credencial inválida e conta restrita | 15 | 23 | 37 | Média |

| #109 HSP-PBI-007 Autenticar Administrador | 16 | 26 | 49 | Baixa · precisa de ajuste |

| #244 HSP-PBI-078 Auth Server (restante) | 21 | 28 | 39 | Alta |

| #110 HSP-PBI-008 Submeter perfil de Host | 30 | 50 | 95 | Baixa |

| #111 HSP-PBI-009 Atualizar perfil de Host | 12 | 20 | 38 | Baixa |

| #112 HSP-PBI-010 Inativar perfil de Host | 10 | 17 | 32 | Baixa |

| #113 HSP-PBI-011 Submeter perfil de Sitter | 18 | 27 | 43 | Média |

| #114 HSP-PBI-012 Atualizar perfil de Sitter | 8 | 12 | 19 | Média |

| #115 HSP-PBI-013 Submeter perfil de Walker | 12 | 19 | 30 | Média |

| #116 HSP-PBI-014 Atualizar perfil de Walker | 6 | 10 | 16 | Média |

| #117 HSP-PBI-015 Serviços e preços | 19 | 32 | 61 | Baixa |

| #118 HSP-PBI-016 Cadastrar pet | 18 | 28 | 45 | Média |

| #119 HSP-PBI-017 Atualizar pet | 11 | 19 | 36 | Baixa |

| #120 HSP-PBI-018 Inativar pet | 8 | 13 | 21 | Média |

| #121 HSP-PBI-019 Consultar usuários | 17 | 26 | 42 | Média |

| #122 HSP-PBI-020 Reativar conta | 14 | 23 | 44 | Baixa · precisa de ajuste |

| #123 HSP-PBI-021 Analisar cadastro pendente | 15 | 23 | 37 | Média |

| #124 HSP-PBI-022 Aprovar ou rejeitar cadastro | 22 | 36 | 68 | Baixa · precisa de ajuste |

| #125 HSP-PBI-023 Aplicar penalidade | — | precisa de ajuste | — | Baixa |

| #126 HSP-PBI-024 Spike telas Mobile (restante) | 16 | 24 | 38 | Média |

| #127 HSP-PBI-025 Spike telas Web (restante) | 9 | 14 | 22 | Média · precisa de ajuste |

| #245 HSP-PBI-079 Ajustar UC e US às decisões DF/DA (restante) | 26 | 40 | 64 | Média |

| #128 HSP-PBI-026 Validar cobertura UX | — | Done | — | — |

| #129 HSP-PBI-027 Hierarquia e plano de 7 sprints | — | Done | — | — |

| #131 HSP-PBI-029 Teste de carga com Faker (restante) | 17 | 28 | 53 | Baixa · precisa de ajuste |

| #130 HSP-PBI-028 PostgreSQL e Prisma | — | Done | — | — |

| #163 HSP-PBI-077 Ambiente do back-end | — | Done | — | — |

| **Total do Epic** | **441** | **694** | **1179** | |

  

### HSP-EP-002 Marketplace e contratação

  

| PBI | Otimista | Provável | Pessimista | Confiança |

|---|---:|---:|---:|---|

| #188 HSP-PBI-030 Buscar prestadores | 37 | 62 | 118 | Baixa |

| #189 HSP-PBI-031 Detalhes do prestador | 13 | 20 | 32 | Média |

| #190 HSP-PBI-032 Favoritar | 6 | 9 | 14 | Média |

| #191 HSP-PBI-033 Compartilhar perfil | 12 | 20 | 38 | Baixa |

| #192 HSP-PBI-034 Avaliar contratação | 12 | 20 | 38 | Baixa |

| #193 HSP-PBI-035 Consultar avaliações | 8 | 13 | 21 | Média |

| #194 HSP-PBI-036 Agenda de hospedagem | 24 | 40 | 76 | Baixa |

| #195 HSP-PBI-037 Área e horários do Sitter | 13 | 22 | 42 | Baixa |

| #196 HSP-PBI-038 Área e horários do Walker | 12 | 19 | 30 | Média |

| #203 HSP-PBI-039 Criar solicitação de hospedagem | 31 | 51 | 97 | Baixa |

| #201 HSP-PBI-040 Impedir reserva incompatível | 20 | 33 | 63 | Baixa |

| #198 HSP-PBI-041 Solicitações pendentes do Host | 8 | 13 | 21 | Média |

| #202 HSP-PBI-042 Aceitar ou recusar reserva | 17 | 29 | 55 | Baixa |

| #200 HSP-PBI-043 Solicitar Sitter ou Walker | 22 | 37 | 70 | Baixa |

| #197 HSP-PBI-044 Gerenciar visita (Sitter) | 14 | 22 | 35 | Média |

| #204 HSP-PBI-045 Gerenciar passeio (Walker) | 8 | 13 | 21 | Média |

| #199 HSP-PBI-046 Histórico de contratações | 14 | 21 | 34 | Média |

| #205 HSP-PBI-047 Cancelar contratação | 24 | 40 | 76 | Baixa |

| **Total do Epic** | **295** | **484** | **881** | |

  

### HSP-EP-003 Execução, comunicação e suporte

  

| PBI | Otimista | Provável | Pessimista | Confiança |

|---|---:|---:|---:|---|

| #213 HSP-PBI-048 Check-in | 12 | 19 | 30 | Média |

| #212 HSP-PBI-049 Check-out | 13 | 21 | 40 | Baixa |

| #207 HSP-PBI-050 Rotina diária | 14 | 23 | 44 | Baixa |

| #211 HSP-PBI-051 Mídia e incidente | 19 | 32 | 61 | Baixa |

| #206 HSP-PBI-052 Timeline | 10 | 16 | 26 | Média |

| #208 HSP-PBI-053 Execução do Sitter | 20 | 34 | 65 | Baixa |

| #214 HSP-PBI-054 Execução do passeio | 11 | 18 | 34 | Baixa |

| #209 HSP-PBI-055 Iniciar conversa | 16 | 25 | 40 | Média |

| #210 HSP-PBI-056 Mensagens em tempo real | 34 | 57 | 108 | Baixa |

| #215 HSP-PBI-057 Elegibilidade do atendimento humano | 8 | 12 | 19 | Média |

| #216 HSP-PBI-058 Reconexão e mensagens pendentes | 16 | 27 | 51 | Baixa |

| #217 HSP-PBI-059 FAQ do Host | 12 | 18 | 29 | Média |

| #218 HSP-PBI-060 Atendimento pré-reserva | 6 | 9 | 14 | Média |

| #219 HSP-PBI-061 Respostas da IA | 37 | 61 | 116 | Baixa |

| #220 HSP-PBI-062 Transferir para humano | 13 | 21 | 40 | Baixa |

| #221 HSP-PBI-063 Conversas escaladas | 14 | 22 | 35 | Média |

| #222 HSP-PBI-064 Produzir notificações | 28 | 47 | 89 | Baixa |

| #223 HSP-PBI-065 Consultar notificações | 9 | 14 | 22 | Média |

| **Total do Epic** | **292** | **476** | **863** | |

  

### HSP-EP-004 Pagamentos e operações financeiras

  

| PBI | Otimista | Provável | Pessimista | Confiança |

|---|---:|---:|---:|---|

| #230 HSP-PBI-066 Forma de pagamento | 15 | 25 | 48 | Baixa |

| #234 HSP-PBI-067 Pagar contratação | 28 | 46 | 87 | Baixa |

| #229 HSP-PBI-068 Recusa e outro meio | 14 | 23 | 44 | Baixa |

| #227 HSP-PBI-069 Aplicar cupom | 11 | 19 | 36 | Baixa |

| #225 HSP-PBI-070 Extrato do Tutor | 8 | 13 | 21 | Média |

| #224 HSP-PBI-071 Financeiro do Host | 13 | 20 | 32 | Média |

| #226 HSP-PBI-072 Transferência de saldo | 31 | 52 | 99 | Baixa · precisa de ajuste |

| #233 HSP-PBI-073 Pagamento idempotente no gateway | 28 | 47 | 89 | Baixa |

| #228 HSP-PBI-074 Saldo e repasse | 24 | 40 | 76 | Baixa |

| #231 HSP-PBI-075 Estorno | 22 | 37 | 70 | Baixa · precisa de ajuste |

| #232 HSP-PBI-076 Ledger imutável | 21 | 32 | 51 | Média |

| **Total do Epic** | **215** | **354** | **653** | |

  

### Itens antigos sem Feature

  

| PBI | Otimista | Provável | Pessimista | Confiança |

|---|---:|---:|---:|---|

| #39 Kick-off | — | Done | — | — |

| #40 Design | — | precisa de ajuste | — | Baixa |

| #41 Arquitetura | — | Done | — | — |

| #42 Gestão | — | Done | — | — |

| #69 Levantamento de Casos de Uso | — | precisa de ajuste | — | Baixa |

| #78 Levantamento de User Story | — | Done | — | — |

  

## Trabalho que ainda não tem PBI

  

Os PBIs acima supõem que estas bases já existem. Sem elas, o primeiro PBI que precisar de cada uma absorve essas horas.

  

| Item | Otimista | Provável | Pessimista | Confiança |

|---|---:|---:|---:|---|

| App Mobile base (React Native/Expo) | 26 | 40 | 64 | Média |

| App Web base do Administrador (React) | 18 | 28 | 45 | Média |

| Base do back-end (camadas, erro padrão, JWT, permissões) | 17 | 26 | 42 | Média |

| Armazenamento de arquivos (fotos e documentos) | 12 | 20 | 38 | Baixa |

| Deploy e CI dos dois serviços | 17 | 28 | 53 | Baixa |

| Spike de gateway de pagamento | 6 | 10 | 16 | Média |

| **Total** | **96** | **152** | **258** | |

  

## Resumo

  

| Conjunto | Otimista | Provável | Pessimista | Esperado (PERT) |

|---|---:|---:|---:|---:|

| PBIs com horas (74) | 1243 | 2008 | 3576 | 2142 |

| Trabalho sem PBI | 96 | 152 | 258 | 160 |

| **Total** | **1339** | **2160** | **3834** | **2302** |

  

- Esperado (PERT) = (otimista + 4 × provável + pessimista) ÷ 6, somado PBI a PBI.

- Somar todos os otimistas ou todos os pessimistas dá extremos improváveis. Para planejar, use o provável ou o esperado.

- Com a capacidade do time (horas por pessoa por sprint) dá para converter o total em número de sprints.

  

## Por que precisa de ajuste

  

- **#40 Design:** Sem critérios de aceite; segue aberto na Sprint 2, que já terminou.

- **#69 Levantamento de Casos de Uso:** Sem descrição e sem critérios; resta só a Task #75 (validar com o modelo do professor).

- **#125 HSP-PBI-023 Aplicar penalidade:** Não está pronto: HSP-FND-015 aberto e o caso de uso de denúncia ainda será criado. Depois da reescrita, a expectativa é de 25 a 50 horas.

- **#103 HSP-PBI-001 Criar conta:** Os critérios de aceite conflitam com o ADR-011 (sem nome no cadastro; PIN antes de criar a conta).

- **#109 HSP-PBI-007 Autenticar Administrador:** Será reescrito pelas decisões DF/DA (papel único de Administrador). Horas já usam o escopo novo.

- **#122 HSP-PBI-020 Reativar conta:** Será reescrito: fica só a reativação de conta, com motivo e auditoria. Horas já usam o escopo novo.

- **#124 HSP-PBI-022 Aprovar ou rejeitar cadastro:** Será reescrito: entra feedback e reenvio do cadastro rejeitado (DA-08). Horas já usam o escopo novo.

- **#127 HSP-PBI-025 Spike telas Web (restante):** Será reescrito: Web passa a ser só do Administrador e da área comercial.

- **#131 HSP-PBI-029 Teste de carga com Faker (restante):** Será reescrito, e o AC-01 cita dados (pets, reservas, financeiro) que ainda não existem no banco. Horas recortadas às entidades atuais.

- **#226 HSP-PBI-072 Transferência de saldo:** Será reescrito: falhas de transferência passam a ser tratadas manualmente pelo Administrador.

- **#231 HSP-PBI-075 Estorno:** Será reescrito: falhas de estorno passam a ser tratadas manualmente pelo Administrador.

  

Itens Done não recebem horas: #39, #41, #42, #78, #128, #129, #130 e #163.

  

## Composição do provável

  

- **#103 HSP-PBI-001 Criar conta** (53 h): contrato de provisionamento 2; migration auth_user_id e senha opcional 3; rota interna idempotente com token de serviço 8; estado do usuário e perfil (/me) 4; bloqueio por completude do perfil 4; testes de integração 6; telas Mobile de cadastro (e-mail, PIN, senha e aceites) 16; integração e erros no app 4; revisão de PR 4; validação da PO 2.

- **#104 HSP-PBI-002 Completar perfil do Tutor** (29 h): migration de endereço e estado do perfil 2; rotas de pendências e conclusão com validação 6; criação do TUTOR e elegibilidade 3; testes 4; pendências e formulário no Mobile 8; redirecionamento no login 2; revisão de PR 3; validação da PO 1.

- **#105 HSP-PBI-003 Redefinir senha** (30 h): token de redefinição e migration 3; rota de solicitar sem revelar a conta 4; rota de confirmar com Argon2id e revogação de sessões 5; testes 5; 2 telas Mobile 8; modelo de e-mail 1; revisão de PR 3; validação da PO 1.

- **#106 HSP-PBI-004 Atualizar perfil do Tutor** (16 h): rota de atualização com validação 4; testes 3; tela Mobile de edição 6; revisão de PR 2; validação da PO 1.

- **#107 HSP-PBI-005 Autenticar conta** (28 h): rota de login no Auth Server 5; proteção das rotas no back-end 1; tela de login Mobile 5; armazenamento seguro e renovação do token no app 6; redirecionamento de perfil incompleto 2; testes 5; revisão de PR 3; validação da PO 1.

- **#108 HSP-PBI-006 Credencial inválida e conta restrita** (23 h): mensagem genérica e consulta do estado da conta 6; bloqueio de conta restrita no back-end 3; mensagens Mobile por estado 5; testes 5; revisão de PR 3; validação da PO 1.

- **#109 HSP-PBI-007 Autenticar Administrador** (26 h): refresh em cookie httpOnly no Auth Server 5; login do Administrador 2; guarda de permissão no back-end 2; criação das contas administrativas 2; tela de login Web 5; guarda de rota na Web 2; testes 4; revisão de PR 3; validação da PO 1.

- **#244 HSP-PBI-078 Auth Server (restante)** (28 h): emissão e validação do JWT RS256 (#254, em andamento) 5; refresh token e renovação (#256) 6; testes de integração e segurança (#255) 8; provedor real de e-mail fora da transação 5; revisão de PR 3; validação da PO 1.

- **#110 HSP-PBI-008 Submeter perfil de Host** (50 h): migration de documentos, restrições e endereço 4; rotas de rascunho e envio 8; upload de documentos e fotos 4; regra de não publicar antes da aprovação 2; testes 6; formulário Mobile em etapas com upload 20; revisão de PR 4; validação da PO 2.

- **#111 HSP-PBI-009 Atualizar perfil de Host** (20 h): rota de atualização com regra de revalidação 6; testes 4; edição Mobile reaproveitando o formulário 6; revisão de PR 3; validação da PO 1.

- **#112 HSP-PBI-010 Inativar perfil de Host** (17 h): transição com checagem de compromissos 5; retirada da busca 1; testes 4; ação e mensagens no Mobile 4; revisão de PR 2; validação da PO 1.

- **#113 HSP-PBI-011 Submeter perfil de Sitter** (27 h): migration de documentos e experiência 2; rotas 5; upload (reuso) 2; testes 4; formulário Mobile (reuso) 10; revisão de PR 3; validação da PO 1.

- **#114 HSP-PBI-012 Atualizar perfil de Sitter** (12 h): rota de atualização 3; testes 2; edição Mobile 4; revisão de PR 2; validação da PO 1.

- **#115 HSP-PBI-013 Submeter perfil de Walker** (19 h): migration da região de atuação 2; rotas (reuso) 3; testes 3; formulário Mobile (reuso) com região 8; revisão de PR 2; validação da PO 1.

- **#116 HSP-PBI-014 Atualizar perfil de Walker** (10 h): rota de atualização 2; testes 2; edição Mobile 3; revisão de PR 2; validação da PO 1.

- **#117 HSP-PBI-015 Serviços e preços** (32 h): migration de vigência e histórico de preço 4; rotas de serviço e publicação com snapshot 8; testes 5; telas Mobile de serviços e preços 10; revisão de PR 3; validação da PO 2.

- **#118 HSP-PBI-016 Cadastrar pet** (28 h): migration de domínios e documentos 2; rotas de cadastro e lista 5; testes 4; formulário e lista no Mobile 10; upload opcional de documentos 3; revisão de PR 3; validação da PO 1.

- **#119 HSP-PBI-017 Atualizar pet** (19 h): rota de atualização com regra de campo crítico 6; testes 4; edição Mobile 5; revisão de PR 3; validação da PO 1.

- **#120 HSP-PBI-018 Inativar pet** (13 h): inativação lógica com checagem de reservas 4; testes 3; ação Mobile 3; revisão de PR 2; validação da PO 1.

- **#121 HSP-PBI-019 Consultar usuários** (26 h): rota de lista com filtros, paginação e junções 6; rota de detalhe 2; testes 4; telas Web de lista e detalhe 10; revisão de PR 3; validação da PO 1.

- **#122 HSP-PBI-020 Reativar conta** (23 h): migration de auditoria 3; rota de reativação com motivo e efeito no Auth Server 5; evento de notificação 1; testes 4; ação Web com motivo e trilha 6; revisão de PR 3; validação da PO 1.

- **#123 HSP-PBI-021 Analisar cadastro pendente** (23 h): rotas de fila e detalhe com documentos protegidos 6; testes 3; telas Web de fila e detalhe 10; revisão de PR 3; validação da PO 1.

- **#124 HSP-PBI-022 Aprovar ou rejeitar cadastro** (36 h): rota de decisão única com motivo e auditoria 6; reenvio após rejeição (DA-08) 4; migration dos campos de decisão 2; evento de notificação 1; testes 5; tela Web de aprovação 5; feedback e reenvio no Mobile 8; revisão de PR 4; validação da PO 1.

- **#126 HSP-PBI-024 Spike telas Mobile (restante)** (24 h): navegação, componentes, dados e estados por tela (#133) 8; fluxo de busca e reserva (#134) 6; telas HPET07 e HPET10 em andamento 6; revisão com a PO 4.

- **#127 HSP-PBI-025 Spike telas Web (restante)** (14 h): tela HPET20 Gerenciar Usuários (#258) 6; ajuste dos ACs às decisões DF/DA 5; revisão com a PO 3.

- **#245 HSP-PBI-079 Ajustar UC e US às decisões DF/DA (restante)** (40 h): ajuste e criação de casos de uso (#246) 14; reescrita dos 8 PBIs e novas User Stories (#247) 14; fluxo Mobile dos 4 perfis (#248) 8; revisão com a PO 4.

- **#131 HSP-PBI-029 Teste de carga com Faker (restante)** (28 h): gerador Faker para as entidades existentes (#156) 10; volume configurável, limpeza e trava contra produção 5; banco isolado para a carga 3; execução e métricas (#158) 5; documentação (#159) 2; revisão de PR 3.

- **#188 HSP-PBI-030 Buscar prestadores** (62 h): geolocalização: decisão e migration 6; geocodificação de endereços 6; consulta com filtros (tipo, pet, preço, disponibilidade) 14; paginação estável e ordenação 3; cache Redis 4; testes 8; telas Mobile de filtros e resultados 14; revisão de PR 5; validação da PO 2.

- **#189 HSP-PBI-031 Detalhes do prestador** (20 h): rota de detalhe público 4; testes 2; card de resultado e tela de detalhe 10; revisão de PR 3; validação da PO 1.

- **#190 HSP-PBI-032 Favoritar** (9 h): rotas de favoritar e desfavoritar 2; testes 2; botão no Mobile 3; revisão de PR 1; validação da PO 1.

- **#191 HSP-PBI-033 Compartilhar perfil** (20 h): slug ou token público e rota pública 4; página pública ou deep link 8; compartilhamento nativo 2; testes 3; revisão de PR 2; validação da PO 1.

- **#192 HSP-PBI-034 Avaliar contratação** (20 h): rota de avaliação com elegibilidade e recálculo da nota 6; testes 4; formulário Mobile 6; revisão de PR 3; validação da PO 1.

- **#193 HSP-PBI-035 Consultar avaliações** (13 h): rota de lista e média 3; testes 2; seção no perfil do Mobile 5; revisão de PR 2; validação da PO 1.

- **#194 HSP-PBI-036 Agenda de hospedagem** (40 h): migration de intervalos e capacidade 3; rotas de agenda e bloqueio com sobreposição 10; testes 6; calendário no Mobile 16; revisão de PR 4; validação da PO 1.

- **#195 HSP-PBI-037 Área e horários do Sitter** (22 h): migration de áreas e horários 2; rotas 5; testes 3; tela de configuração no Mobile 8; revisão de PR 3; validação da PO 1.

- **#196 HSP-PBI-038 Área e horários do Walker** (19 h): migration da área e do slot com dia 3; rotas (reuso) 4; testes 3; tela Mobile (reuso) 6; revisão de PR 2; validação da PO 1.

- **#203 HSP-PBI-039 Criar solicitação de hospedagem** (51 h): migration de chave de idempotência 2; rota de criação com validação e snapshot 10; reenvio sem duplicar 4; testes 8; fluxo Mobile em etapas 20; revisão de PR 5; validação da PO 2.

- **#201 HSP-PBI-040 Impedir reserva incompatível** (33 h): checagem de compatibilidade 4; revalidação atômica contra double booking 10; migration de espécies e portes aceitos 2; testes de concorrência 8; estados de erro no Mobile 4; revisão de PR 4; validação da PO 1.

- **#198 HSP-PBI-041 Solicitações pendentes do Host** (13 h): rota da fila 3; testes 2; fila no Mobile 5; revisão de PR 2; validação da PO 1.

- **#202 HSP-PBI-042 Aceitar ou recusar reserva** (29 h): transições sob concorrência com bloqueio da agenda 10; migration do estado aguardando pagamento 1; evento ao Tutor 1; testes 6; ações no Mobile 6; revisão de PR 4; validação da PO 1.

- **#200 HSP-PBI-043 Solicitar Sitter ou Walker** (37 h): migration de endereço, duração e tipo 3; rota de criação com disponibilidade e snapshot 8; evento ao prestador 1; testes 6; fluxo Mobile 14; revisão de PR 4; validação da PO 1.

- **#197 HSP-PBI-044 Gerenciar visita (Sitter)** (22 h): rotas de lista, aceite e recusa com bloqueio de horário 7; testes 4; fila e ações no Mobile 7; revisão de PR 3; validação da PO 1.

- **#204 HSP-PBI-045 Gerenciar passeio (Walker)** (13 h): rotas (reuso) com capacidade do slot 4; testes 3; Mobile (reuso) 3; revisão de PR 2; validação da PO 1.

- **#199 HSP-PBI-046 Histórico de contratações** (21 h): rota unificada com filtros e paginação 6; testes 3; lista e filtros no Mobile 8; revisão de PR 3; validação da PO 1.

- **#205 HSP-PBI-047 Cancelar contratação** (40 h): estados canceláveis e cálculo da política 8; liberação atômica da agenda 4; notificação ao prestador 1; evento de estorno 2; migration dos campos de cancelamento 2; testes 8; fluxo de cancelamento no Mobile 8; revisão de PR 5; validação da PO 2.

- **#213 HSP-PBI-048 Check-in** (19 h): transição com janela, fuso e timestamp 6; evento ao Tutor 1; testes 4; ação no Mobile 4; revisão de PR 3; validação da PO 1.

- **#212 HSP-PBI-049 Check-out** (21 h): transição e eventos sem duplicar 8; testes 5; ação no Mobile 4; revisão de PR 3; validação da PO 1.

- **#207 HSP-PBI-050 Rotina diária** (23 h): migration de autoria e campos 2; rota de registro com janela e limites 5; testes 4; formulário Mobile 8; revisão de PR 3; validação da PO 1.

- **#211 HSP-PBI-051 Mídia e incidente** (32 h): migration de anexos e incidente 3; upload de mídia 4; incidente, evento prioritário e ocorrência 5; testes 5; anexo e incidente no Mobile 10; revisão de PR 4; validação da PO 1.

- **#206 HSP-PBI-052 Timeline** (16 h): rota da timeline 3; testes 2; timeline com mídia no Mobile 8; revisão de PR 2; validação da PO 1.

- **#208 HSP-PBI-053 Execução do Sitter** (34 h): migration de início, fim, evidência e consentimento 3; rotas de início e fim com evidência 8; gatilhos de avaliação e repasse 1; testes 5; captura de foto e localização no Mobile 12; revisão de PR 4; validação da PO 1.

- **#214 HSP-PBI-054 Execução do passeio** (18 h): rotas (reuso) 5; testes 3; Mobile (reuso) 6; revisão de PR 3; validação da PO 1.

- **#209 HSP-PBI-055 Iniciar conversa** (25 h): migration da regra de unicidade 1; rota de conversa e histórico com autorização 6; testes 4; lista e tela de conversa no Mobile 10; revisão de PR 3; validação da PO 1.

- **#210 HSP-PBI-056 Mensagens em tempo real** (57 h): servidor WebSocket com autenticação 10; persistência e entrega 6; mídia com validação 5; migration de autor, mídia e entrega 2; ajuste de deploy para WebSocket 3; testes 8; cliente em tempo real no Mobile 16; revisão de PR 5; validação da PO 2.

- **#215 HSP-PBI-057 Elegibilidade do atendimento humano** (12 h): regra de elegibilidade 4; testes 3; mensagem de bloqueio no Mobile 2; revisão de PR 2; validação da PO 1.

- **#216 HSP-PBI-058 Reconexão e mensagens pendentes** (27 h): reconexão, fila e ordem no Mobile 12; deduplicação no servidor 4; testes de instabilidade 6; revisão de PR 4; validação da PO 1.

- **#217 HSP-PBI-059 FAQ do Host** (18 h): rotas de FAQ com limites 4; testes 3; gestão de FAQ no Mobile 8; revisão de PR 2; validação da PO 1.

- **#218 HSP-PBI-060 Atendimento pré-reserva** (9 h): migration e rota 3; testes 2; opção no Mobile 2; revisão de PR 1; validação da PO 1.

- **#219 HSP-PBI-061 Respostas da IA** (61 h): Spike de provedor e checagem do regulamento 8; recuperação sobre a FAQ 12; geração com limiar de confiança e fallback 12; integração ao chat 6; conjunto de avaliação de qualidade 6; testes 6; respostas da IA no Mobile 4; revisão de PR 5; validação da PO 2.

- **#220 HSP-PBI-062 Transferir para humano** (21 h): lógica de escalonamento e evento 8; testes 4; pedido e status no Mobile 5; revisão de PR 3; validação da PO 1.

- **#221 HSP-PBI-063 Conversas escaladas** (22 h): fila, atribuição e resposta pelo chat 7; testes 4; fila no Mobile 7; revisão de PR 3; validação da PO 1.

- **#222 HSP-PBI-064 Produzir notificações** (47 h): mecanismo de eventos (outbox) 12; serviço de notificação com deduplicação 8; migration de chave e referência 2; primeiros 3 eventos ligados 6; retry 4; testes 8; revisão de PR 5; validação da PO 2.

- **#223 HSP-PBI-065 Consultar notificações** (14 h): rotas de lista e leitura 3; testes 2; central no Mobile 6; revisão de PR 2; validação da PO 1.

- **#230 HSP-PBI-066 Forma de pagamento** (25 h): cliente no gateway, token e meio padrão 6; tokenização com SDK no Mobile 10; testes em sandbox 5; revisão de PR 3; validação da PO 1.

- **#234 HSP-PBI-067 Pagar contratação** (46 h): intenção de pagamento com idempotência 10; transição atômica de pagamento e reserva 6; comprovante 3; migration de idempotência, gateway e moeda 2; testes 8; checkout no Mobile 10; revisão de PR 5; validação da PO 2.

- **#229 HSP-PBI-068 Recusa e outro meio** (23 h): modelo de tentativas e nova tentativa 6; migration 2; testes 5; recusa e troca de meio no Mobile 6; revisão de PR 3; validação da PO 1.

- **#227 HSP-PBI-069 Aplicar cupom** (19 h): validação do cupom, recálculo e limite de usos 7; testes 4; campo de cupom no Mobile 4; revisão de PR 3; validação da PO 1.

- **#225 HSP-PBI-070 Extrato do Tutor** (13 h): rota do extrato 3; testes 2; tela Mobile 5; revisão de PR 2; validação da PO 1.

- **#224 HSP-PBI-071 Financeiro do Host** (20 h): cálculo de saldo pelo ledger 6; testes 4; tela Mobile 6; revisão de PR 3; validação da PO 1.

- **#226 HSP-PBI-072 Transferência de saldo** (52 h): solicitação, conta bancária e migration 4; reserva atômica de saldo 6; integração com provedor de transferência 8; reconciliação de falha 5; testes 8; tela de solicitação no Mobile 6; tratamento manual na Web 8; revisão de PR 5; validação da PO 2.

- **#233 HSP-PBI-073 Pagamento idempotente no gateway** (47 h): cliente do gateway 6; webhook com assinatura 6; migration de transação 2; idempotência, retry e timeout 8; conciliação 6; testes com sandbox e webhooks simulados 10; logs e observabilidade 3; revisão de PR 5; validação da PO 1.

- **#228 HSP-PBI-074 Saldo e repasse** (40 h): geração de saldo com taxa no ledger 8; processamento do repasse 10; correlação contra pagamento duplo 4; falha recuperável e auditável 4; testes 8; revisão de PR 5; validação da PO 1.

- **#231 HSP-PBI-075 Estorno** (37 h): cálculo de estorno integral ou parcial 6; chamada única ao gateway 5; migration de vínculo com a transação original 2; atualização e evento 2; tratamento manual na Web 8; testes 8; revisão de PR 5; validação da PO 1.

- **#232 HSP-PBI-076 Ledger imutável** (32 h): tabela append-only com proteção contra alteração 6; serviço de ledger com ajustes 6; outbox, retry e alerta 6; integração com transações 3; testes 6; revisão de PR 4; validação da PO 1.

- **App Mobile base (React Native/Expo)** (40 h): projeto e navegação 10; design system básico 10; cliente da API e erros 6; armazenamento seguro de tokens 4; build de teste 4; revisão 6.

- **App Web base do Administrador (React)** (28 h): projeto e roteamento 6; layout e componentes 8; cliente da API com cookie httpOnly 6; guarda de rota 3; revisão 5.

- **Base do back-end (camadas, erro padrão, JWT, permissões)** (26 h): camadas e envelope de erro (#56) 8; validação com Joi 3; middleware JWT RS256 5; controle de permissões 4; CORS, helmet e express.json 2; revisão 4.

- **Armazenamento de arquivos (fotos e documentos)** (20 h): escolha do serviço e upload 8; URLs protegidas e limites 6; testes 3; revisão 3.

- **Deploy e CI dos dois serviços** (28 h): pipeline de build e testes 8; deploy dos 2 serviços 10; variáveis, chaves e banco 5; revisão 5.

- **Spike de gateway de pagamento** (10 h): comparar provedores e sandbox 8; decisão registrada 2.