# Portfolio de Qualidade — NaSalinha

**Trainee:** Carlos Vinicius Teixeira de Souza

---

## Descricao

Este repositorio contém a auditoria completa de qualidade realizada no sistema **NaSalinha**, cobrindo as tres áreas funcionais core: **Autenticacao JWT**, **Check-in por Foto** e **Sistema de Pontos**.

O trabalho segue o ciclo completo de QA: planejamento de casos de teste, execucao via API (Insomnia), documentacao dos bugs encontrados e relatorio final de qualidade.

---

## Tecnologias e Ferramentas

| Ferramenta | Uso |
|---|---|
| Insomnia | Execucao e validacao dos testes de API |
| Docker | Execucao do ambiente local da API (Node.js) |
| GitHub Issues | Registro e rastreabilidade dos Bug Reports |
| GitHub Projects | Kanban de gestao do ciclo de testes |

---

## Tipos de Testes Realizados

- Testes Funcionais — validacao do comportamento esperado em fluxos positivos e negativos
- Testes de API — validacao de endpoints, status codes e corpo das respostas via Insomnia
- Testes de Autenticacao e Autorizacao — controle de acesso com JWT e verificacao de roles (RBAC)
- Testes de Seguranca — SQL Injection, IDOR, hash de senha
- Testes de Validacao de Entrada — campos obrigatorios, formatos invalidos, strings espaçadas, valores inexistentes
- Testes de Consistencia de Dados — integridade do saldo de pontos e contagem de check-ins
- Testes de Regressao — validacao de que correcoes nao causam efeitos colaterais

---

## Estrutura do Repositorio

```
/
├── README.md        → Este documento (documentacao completa)
└── /insomnia        → Colecao exportada com todos os testes de API
```

---

## Como Executar os Testes

1. Subir o ambiente local com Docker
2. Importar o arquivo `Insomnia_2026-04-21.yaml` localizado em `/insomnia`
3. Executar os requests organizados pelas pastas: `logins_cadastros`, `esqueceu_senha`, `admin`, `get_api`, `modulo-checkin`, `pontuacoes`
4. Validar as respostas conforme os resultados esperados descritos nos casos de teste abaixo

---

## Escopo dos Testes Executados

### Pasta: logins_cadastros > teste-logins

| Request | Metodo | Endpoint | Cenario |
|---|---|---|---|
| login-usuario | POST | /api/auth/login | Login valido com credenciais corretas |
| login-sem-criar-conta | POST | /api/auth/login | Login com e-mail inexistente no sistema |
| login-usuario-invalido | POST | /api/auth/login | Login com usuario que nao possui conta ativa |
| login-senha-invalida | POST | /api/auth/login | Login com senha incorreta para usuario existente |
| login-campos-vazios | POST | /api/auth/login | Login com e-mail e senha em branco |
| login-faltando-senha | POST | /api/auth/login | Login sem o campo senha no body |
| login-faltando-email | POST | /api/auth/login | Login sem o campo e-mail no body |
| login-formato-invalido-email | POST | /api/auth/login | Login com e-mail sem formato valido (ex: "pedro") |
| login-case-sensitive | POST | /api/auth/login | Login com e-mail e senha com variacao de capitalização |
| login-sql-injection | POST | /api/auth/login | Tentativa de SQL Injection no campo e-mail |
| login-varias-tentativas-erradas | POST | /api/auth/login | Multiplas tentativas de login com credenciais erradas |

### Pasta: logins_cadastros > teste cadastros

| Request | Metodo | Endpoint | Cenario |
|---|---|---|---|
| cadastrar-usuario | POST | /api/auth/register | Cadastro valido com role ADMIN |
| cadastro-sem-nome | POST | /api/auth/register | Cadastro com campo nome vazio |
| cadastro-sem-email | POST | /api/auth/register | Cadastro com campo e-mail vazio |
| cadastro-ja-cadastrado | POST | /api/auth/register | Cadastro com e-mail ja existente no sistema |
| cadastro-senha-fraca | POST | /api/auth/register | Cadastro com senha curta/fraca |
| cadastro-faltando-role-conta-ja-existe | POST | /api/auth/register | Cadastro sem campo role para conta ja existente |
| cadastro-faltando-role-conta-nao-existe | POST | /api/auth/register | Cadastro sem campo role para conta nova |
| cadastro-com-role-inexistente | POST | /api/auth/register | Cadastro com role invalida ("PRESIDENTE") |
| cadastro-com-string-espacada | POST | /api/auth/register | Cadastro com espacos nos campos nome, e-mail e senha |
| cadastro-com-email-e-role-espacado | POST | /api/auth/register | Cadastro com espacos no e-mail e na role |

### Pasta: esqueceu_senha

| Request | Metodo | Endpoint | Cenario |
|---|---|---|---|
| email_existe | POST | /api/auth/forgot-password | Recuperacao de senha com e-mail cadastrado |
| email_nao_existe | POST | /api/auth/forgot-password | Recuperacao de senha com e-mail inexistente |
| email_vazio | POST | /api/auth/forgot-password | Recuperacao de senha com campo e-mail vazio |

### Pasta: admin

| Request | Metodo | Endpoint | Cenario |
|---|---|---|---|
| logando_sem_ser_admin | GET | /api/seasons | Acesso a rota restrita com token de usuario nao-admin |
| atualizar_nota | PATCH | /checkins/:id | Tentativa de alteracao de status de check-in |

### Pasta: get_api

| Request | Metodo | Endpoint | Cenario |
|---|---|---|---|
| acessar_api_sendo_member | GET | /api/users | Acesso ao endpoint de usuarios com role MEMBER |
| check_senha_hash | GET | /api/users | Verificacao se senha aparece hasheada na resposta da API |
| get_checkins | GET | /api/checkins | Listagem de todos os check-ins com token de ADMIN |
| get_seasons_sendo_member | GET | /api/seasons | Acesso ao endpoint de seasons com role MEMBER |

### Pasta: modulo-checkin

| Request | Metodo | Endpoint | Cenario |
|---|---|---|---|
| enviar_sem_foto | POST | /api/checkins | Envio de check-in sem arquivo anexado |
| enviar_formato_incorreto | POST | /api/checkins | Envio de check-in com arquivo .cpp (formato invalido) |
| enviar_foto | POST | /api/checkins | Envio de check-in com imagem valida (.png) |
| checar_checkin | GET | /api/checkins/ | Consulta dos check-ins do usuario autenticado |
| checar_checkin_outra_pessoa_sem_admin | GET | /api/checkins/{id} | Acesso ao check-in de outro usuario sem permissao de admin |
| deletar_foto | DELETE | /api/checkins/{id} | Remocao de check-in proprio |
| deletar_foto_admin | DELETE | /api/checkins/{id} | Remocao de check-in por usuario ADMIN |

### Pasta: pontuacoes

| Request | Metodo | Endpoint | Cenario |
|---|---|---|---|
| checar_pontos | GET | /api/rankings | Consulta do ranking de pontuacao com usuario autenticado |

---

## Casos de Teste (CT)

> Estrutura: 2 casos por requisito. Para cada area core há ao menos 1 caso funcional, 1 caso de API e 1 caso de regressao.

---

### Area 1 — Autenticação JWT

---

#### CT-AUTH-01 — Login valido retorna tokens JWT (API — Positivo)

| Campo | Detalhe |
|---|---|
| Request | login-usuario |
| Pre-condicao | Usuario `carlosviniciusadmin@gmail.com` cadastrado e ativo |
| Tipo | API |
| Passos | 1. POST /api/auth/login com e-mail e senha corretos |
| Resultado Esperado | 200 OK com accessToken e refreshToken |
| Resultado Obtido | 200 OK com tokens retornados corretamente |
| Status | PASS |

---

#### CT-AUTH-02 — Login com senha incorreta deve retornar erro (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | login-senha-invalida |
| Pre-condicao | Usuario `pedro@example.com` cadastrado |
| Tipo | API |
| Passos | 1. POST /api/auth/login com senha errada "senha123Errada" |
| Resultado Esperado | 401 Unauthorized |
| Resultado Obtido | 401 Unauthorized |
| Status | PASS |

---

#### CT-AUTH-03 — Login com e-mail inexistente deve retornar erro (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | login-sem-criar-conta |
| Pre-condicao | E-mail naoexiste@gmail.com nao esta cadastrado |
| Tipo | API |
| Passos | 1. POST /api/auth/login com e-mail e senha de conta inexistente |
| Resultado Esperado | 401 Unauthorized ou 404 Not Found |
| Resultado Obtido | 401 Unauthorized |
| Status | PASS |

---

#### CT-AUTH-04 — Login com campos vazios deve retornar erro de validacao (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | login-campos-vazios |
| Pre-condicao | Nenhuma |
| Tipo | API |
| Passos | 1. POST /api/auth/login com e-mail "" e password "" |
| Resultado Esperado | 400 Bad Request com mensagem de validacao |
| Resultado Obtido | 400 Bad Request |
| Status | PASS |

---

#### CT-AUTH-05 — Login sem campo senha deve retornar erro de validacao (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | login-faltando-senha |
| Pre-condicao | Nenhuma |
| Tipo | API |
| Passos | 1. POST /api/auth/login com apenas o campo e-mail no body |
| Resultado Esperado | 400 Bad Request |
| Resultado Obtido | 400 Bad Request |
| Status | PASS |

---

#### CT-AUTH-06 — Login com formato de e-mail invalido deve retornar erro (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | login-formato-invalido-email |
| Pre-condicao | Nenhuma |
| Tipo | API |
| Passos | 1. POST /api/auth/login com email "pedro" (sem @) |
| Resultado Esperado | 400 Bad Request com mensagem de formato invalido |
| Resultado Obtido | 400 Bad Request |
| Status | PASS |

---

#### CT-AUTH-07 — Tentativa de SQL Injection no login deve ser bloqueada (Segurança — Negativo)

| Campo | Detalhe |
|---|---|
| Request | login-sql-injection |
| Pre-condicao | Nenhuma |
| Tipo | Seguranca |
| Passos | 1. POST /api/auth/login com email "'OR 1=1 --" |
| Resultado Esperado | 400 ou 401 — payload malicioso nao deve ser executado |
| Resultado Obtido | 400/401 sem execucao da injecao |
| Status | PASS |

---

#### CT-AUTH-08 — Cadastro valido deve criar usuario (API — Positivo)

| Campo | Detalhe |
|---|---|
| Request | cadastrar-usuario |
| Pre-condicao | E-mail nao cadastrado |
| Tipo | API |
| Passos | 1. POST /api/auth/register com nome, e-mail, senha e role validos |
| Resultado Esperado | 201 Created — usuario criado com status PENDENTE, sem tokens retornados |
| Resultado Obtido | 201 Created — API retorna accessToken e refreshToken imediatamente, sem verificacao de e-mail |
| Status | FAIL |
| Bug Report | #1 |

---

#### CT-AUTH-09 — Cadastro com e-mail ja existente deve retornar erro (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | cadastro-ja-cadastrado |
| Pre-condicao | E-mail bruno@gmail.com ja cadastrado |
| Tipo | API |
| Passos | 1. POST /api/auth/register com e-mail ja existente |
| Resultado Esperado | 409 Conflict ou 400 Bad Request |
| Resultado Obtido | 409 Conflict |
| Status | PASS |

---

#### CT-AUTH-10 — Cadastro sem nome deve ser rejeitado (Funcional — Negativo)

| Campo | Detalhe |
|---|---|
| Request | cadastro-sem-nome |
| Pre-condicao | Nenhuma |
| Tipo | Funcional |
| Passos | 1. POST /api/auth/register com campo name vazio |
| Resultado Esperado | 400 Bad Request |
| Resultado Obtido | 400 Bad Request |
| Status | PASS |

---

#### CT-AUTH-11 — Cadastro com senha fraca deve ser rejeitado (Funcional — Negativo)

| Campo | Detalhe |
|---|---|
| Request | cadastro-senha-fraca |
| Pre-condicao | Nenhuma |
| Tipo | Funcional |
| Passos | 1. POST /api/auth/register com password "123" |
| Resultado Esperado | 400 Bad Request com mensagem sobre requisitos minimos de senha |
| Resultado Obtido | 400 Bad Request |
| Status | PASS |

---

#### CT-AUTH-12 — Cadastro sem campo role deve atribuir role padrao ou retornar erro (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | cadastro-faltando-role-conta-nao-existe |
| Pre-condicao | E-mail semrole@gmail.com nao cadastrado |
| Tipo | API |
| Passos | 1. POST /api/auth/register sem o campo role |
| Resultado Esperado | 201 com role padrao atribuida, ou 400 informando campo obrigatorio |
| Resultado Obtido | A verificar conforme comportamento documentado |
| Status | A verificar |

---

#### CT-AUTH-13 — Cadastro com role inexistente deve ser rejeitado (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | cadastro-com-role-inexistente |
| Pre-condicao | Nenhuma |
| Tipo | API |
| Passos | 1. POST /api/auth/register com role "PRESIDENTE" |
| Resultado Esperado | 400 Bad Request — role invalida nao deve ser aceita |
| Resultado Obtido | 400 Bad Request |
| Status | PASS |

---

#### CT-AUTH-14 — Cadastro com strings espacadas deve ser sanitizado (Funcional — Negativo)

| Campo | Detalhe |
|---|---|
| Request | cadastro-com-string-espacada / cadastro-com-email-e-role-espacado |
| Pre-condicao | Nenhuma |
| Tipo | Funcional |
| Passos | 1. POST /api/auth/register com espacos extras em nome, e-mail e role |
| Resultado Esperado | Sistema deve sanitizar (trim) os campos ou retornar erro de validacao |
| Resultado Obtido | A verificar conforme comportamento documentado |
| Status | A verificar |

---

#### CT-AUTH-15 — Recuperacao de senha com e-mail inexistente nao deve revelar existencia de conta (Segurança — Negativo)

| Campo | Detalhe |
|---|---|
| Request | email_nao_existe |
| Pre-condicao | E-mail carlosviniciusNAOEXISTE@gmail.com nao cadastrado |
| Tipo | Seguranca |
| Passos | 1. POST /api/auth/forgot-password com e-mail inexistente |
| Resultado Esperado | 200 OK com mensagem generica (nao deve confirmar nem negar existencia do e-mail) |
| Resultado Obtido | A verificar conforme comportamento documentado |
| Status | A verificar |

---

#### CT-AUTH-16 — Usuario nao-admin nao deve acessar endpoint restrito de seasons (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | logando_sem_ser_admin / get_seasons_sendo_member |
| Pre-condicao | Usuarios com roles MEMBER e TRAINEE autenticados |
| Tipo | API |
| Passos | 1. GET /api/seasons com token de usuario nao-admin |
| Resultado Esperado | 403 Forbidden |
| Resultado Obtido | 200 OK com dados retornados |
| Status | FAIL |
| Bug Report | #2 |

---

#### CT-AUTH-17 — Usuario MEMBER nao deve acessar endpoint de listagem de usuarios (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | acessar_api_sendo_member |
| Pre-condicao | Usuario autenticado com role MEMBER |
| Tipo | API |
| Passos | 1. GET /api/users com token de MEMBER |
| Resultado Esperado | 403 Forbidden |
| Resultado Obtido | A verificar conforme comportamento documentado |
| Status | A verificar |

---

#### CT-AUTH-18 — Senha nao deve ser retornada em texto plano na listagem de usuarios (Segurança — Negativo)

| Campo | Detalhe |
|---|---|
| Request | check_senha_hash |
| Pre-condicao | Usuario ADMIN autenticado |
| Tipo | Seguranca |
| Passos | 1. GET /api/users com token de ADMIN; 2. Verificar campo password na resposta |
| Resultado Esperado | Campo password deve estar hasheado (bcrypt) ou ausente na resposta |
| Resultado Obtido | A verificar conforme comportamento documentado |
| Status | A verificar |

---

#### CT-AUTH-19 — Regressãoo: apos correcao do RBAC, rotas protegidas devem bloquear nao-administradores (Regressão)

| Campo | Detalhe |
|---|---|
| Pre-condicao | Correcao do bug #2 implementada |
| Tipo | Regressao |
| Cenario simulado | Middleware de RBAC adicionado nas rotas de gestao apos correcao do bug #2 |
| Testes a repetir | 1. TRAINEE tenta GET /api/seasons — deve receber 403; 2. MEMBER tenta GET /api/seasons — deve receber 403; 3. ADMIN tenta GET /api/seasons — deve receber 200; 4. Login de TRAINEE continua funcionando — deve receber 200; 5. TRAINEE ainda acessa GET /api/checkins normalmente — deve receber 200 |
| Objetivo | Garantir que a correcao do RBAC nao bloqueou rotas permitidas a outros perfis |
| Status | Aguardando correcao |

---

### Area 2 — Check-in por Foto

---

#### CT-CHECKIN-01 — Check-in com imagem valida deve ser criado com status PENDENTE (Funcional — Positivo)

| Campo | Detalhe |
|---|---|
| Request | enviar_foto |
| Pre-condicao | Usuario autenticado; nenhum check-in realizado nas ultimas 24h |
| Tipo | Funcional |
| Passos | 1. POST /api/checkins com imagem .png valida e token JWT |
| Resultado Esperado | 201 Created — check-in criado com status PENDENTE, sem pontos atribuidos |
| Resultado Obtido | 201 Created — check-in criado com status APPROVED e pontos atribuidos imediatamente |
| Status | FAIL |
| Bug Report | #4 |

---

#### CT-CHECKIN-02 — Check-in sem foto deve ser rejeitado (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | enviar_sem_foto |
| Pre-condicao | Usuario autenticado |
| Tipo | API |
| Passos | 1. POST /api/checkins sem arquivo no campo photo |
| Resultado Esperado | 400 Bad Request — campo photo obrigatorio |
| Resultado Obtido | A verificar conforme comportamento documentado |
| Status | A verificar |

---

#### CT-CHECKIN-03 — Check-in com formato de arquivo invalido deve ser rejeitado (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | enviar_formato_incorreto |
| Pre-condicao | Usuario autenticado |
| Tipo | API |
| Passos | 1. POST /api/checkins com arquivo .cpp no campo photo |
| Resultado Esperado | 400 Bad Request — apenas imagens devem ser aceitas |
| Resultado Obtido | A verificar conforme comportamento documentado |
| Status | A verificar |

---

#### CT-CHECKIN-04 — Consulta de check-ins deve retornar apenas os do usuario autenticado (API — Positivo)

| Campo | Detalhe |
|---|---|
| Request | checar_checkin |
| Pre-condicao | Usuario autenticado com ao menos um check-in realizado |
| Tipo | API |
| Passos | 1. GET /api/checkins/ com token JWT |
| Resultado Esperado | 200 OK com lista de check-ins pertencentes ao usuario autenticado |
| Resultado Obtido | A verificar conforme comportamento documentado |
| Status | A verificar |

---

#### CT-CHECKIN-05 — Usuario nao deve acessar check-in de outro usuario (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | checar_checkin_outra_pessoa_sem_admin |
| Pre-condicao | Usuario autenticado; ID de check-in pertencente a outro usuario conhecido |
| Tipo | API |
| Passos | 1. GET /api/checkins/{id} com ID de check-in alheio e token de usuario nao-admin |
| Resultado Esperado | 403 Forbidden ou 404 Not Found |
| Resultado Obtido | 200 OK com dados completos do check-in alheio |
| Status | FAIL |
| Bug Report | #6 |

---

#### CT-CHECKIN-06 — Remocao de check-in proprio deve funcionar (API — Positivo)

| Campo | Detalhe |
|---|---|
| Request | deletar_foto |
| Pre-condicao | Usuario autenticado com check-in proprio existente |
| Tipo | API |
| Passos | 1. DELETE /api/checkins/{id} com ID de check-in proprio |
| Resultado Esperado | 200 ou 204 — check-in removido com sucesso; pontos subtraidos |
| Resultado Obtido | Check-in removido com sucesso; pontos e contagem permanecem inalterados |
| Status | FAIL |
| Bug Report | #8 |

---

#### CT-CHECKIN-07 — Admin deve poder remover check-in de qualquer usuario (API — Positivo)

| Campo | Detalhe |
|---|---|
| Request | deletar_foto_admin |
| Pre-condicao | Usuario ADMIN autenticado; check-in de outro usuario existente |
| Tipo | API |
| Passos | 1. DELETE /api/checkins/{id} com token de ADMIN e ID de check-in de outro usuario |
| Resultado Esperado | 200 ou 204 — check-in removido com sucesso |
| Resultado Obtido | A verificar conforme comportamento documentado |
| Status | A verificar |

---

#### CT-CHECKIN-08 — Endpoint de alteracao de status de check-in deve existir (Funcional — Positivo)

| Campo | Detalhe |
|---|---|
| Request | PATCH /checkins/:id (New Request) |
| Pre-condicao | Check-in existente; usuario ADMIN autenticado |
| Tipo | Funcional |
| Passos | 1. PATCH /checkins/:id com body de atualizacao de status |
| Resultado Esperado | 200 OK — status atualizado para APPROVED ou REJECTED |
| Resultado Obtido | Rota nao encontrada — endpoint nao implementado |
| Status | FAIL |
| Bug Report | #5 |

---

#### CT-CHECKIN-09 — Sistema nao deve permitir multiplos check-ins no mesmo dia (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | enviar_foto (executado multiplas vezes) |
| Pre-condicao | Usuario autenticado com check-in ja realizado no dia |
| Tipo | API |
| Passos | 1. POST /api/checkins repetido no mesmo dia |
| Resultado Esperado | 400 ou 409 a partir do segundo check-in |
| Resultado Obtido | 201 Created — multiplos check-ins aceitos no mesmo dia |
| Status | FAIL |
| Bug Report | #3 |

---

#### CT-CHECKIN-10 — Regressao: apos correcao da restricao diaria, fluxo principal deve continuar funcionando (Regressao)

| Campo | Detalhe |
|---|---|
| Pre-condicao | Correcoes dos bugs #3 e #9 implementadas |
| Tipo | Regressao |
| Cenario simulado | Validacao de 1 check-in por dia e intervalo de 24h adicionadas ao backend |
| Testes a repetir | 1. Primeiro check-in do dia — deve ser aceito (201); 2. Segundo check-in no mesmo dia — deve ser bloqueado (409); 3. GET /api/checkins — deve retornar apenas check-ins do usuario; 4. DELETE /api/checkins/{id} — deve continuar funcionando; 5. Ranking deve ser atualizado corretamente apos o check-in |
| Objetivo | Garantir que a restricao de frequencia nao quebrou os fluxos de criacao, consulta e remocao |
| Status | Aguardando correcao |

---

### Area 3 — Sistema de Pontos

---

#### CT-PONTOS-01 — Ranking deve ser acessivel a usuarios autenticados (API — Positivo)

| Campo | Detalhe |
|---|---|
| Request | checar_pontos |
| Pre-condicao | Usuario autenticado com token JWT valido |
| Tipo | API |
| Passos | 1. GET /api/rankings com token JWT |
| Resultado Esperado | 200 OK com lista de usuarios ordenada por pontuacao |
| Resultado Obtido | A verificar conforme comportamento documentado |
| Status | A verificar |

---

#### CT-PONTOS-02 — Pontos devem ser atribuidos apenas apos aprovacao do check-in (Funcional — Negativo)

| Campo | Detalhe |
|---|---|
| Request | enviar_foto + checar_pontos |
| Pre-condicao | Usuario autenticado sem check-ins anteriores |
| Tipo | Funcional |
| Passos | 1. POST /api/checkins com imagem; 2. GET /api/rankings imediatamente apos |
| Resultado Esperado | Pontos nao devem ser atribuidos antes da aprovacao por ADMINISTRADOR |
| Resultado Obtido | Pontos atribuidos imediatamente na criacao do check-in |
| Status | FAIL |
| Bug Report | #4 |

---

#### CT-PONTOS-03 — Remocao de check-in deve subtrair os pontos correspondentes (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | deletar_foto + checar_pontos |
| Pre-condicao | Usuario com check-in criado e pontos acumulados |
| Tipo | API |
| Passos | 1. Verificar pontos antes; 2. DELETE /api/checkins/{id}; 3. Verificar pontos apos |
| Resultado Esperado | Saldo reduzido; contagem decrementada; ranking atualizado |
| Resultado Obtido | Check-in removido, pontos e contagem permanecem inalterados |
| Status | FAIL |
| Bug Report | #8 |

---

#### CT-PONTOS-04 — Multiplos check-ins no mesmo dia nao devem acumular pontos (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | enviar_foto (multiplas execucoes) + checar_pontos |
| Pre-condicao | Usuario autenticado |
| Tipo | API |
| Passos | 1. Realizar 3 check-ins consecutivos; 2. GET /api/rankings |
| Resultado Esperado | Apenas o primeiro check-in do dia gera pontos |
| Resultado Obtido | Cada check-in adicional incrementa os pontos, corrompendo o ranking |
| Status | FAIL |
| Bug Report | #3 |

---

#### CT-PONTOS-05 — Regressão: apos correcao da consistencia de pontos, saldo deve ser preciso em todo o ciclo (Regressão)

| Campo | Detalhe |
|---|---|
| Pre-condicao | Correcoes dos bugs #4, #8 e #3 implementadas |
| Tipo | Regressao |
| Cenario simulado | Pontos atribuidos apenas apos aprovacao; subtraidos ao deletar check-in |
| Testes a repetir | 1. Criar check-in — pontos nao devem ser atribuidos; 2. ADMIN aprovar check-in — pontos devem ser atribuidos; 3. Deletar check-in aprovado — pontos devem ser subtraidos; 4. Consultar GET /api/rankings — posicao do usuario deve estar atualizada; 5. Segundo check-in no mesmo dia — bloqueado, pontos inalterados |
| Objetivo | Garantir que a correcao da atribuicao de pontos nao afetou os fluxos de criacao, aprovacao, remocao e ranking |
| Status | Aguardando correcao |

---

## Bug Reports

### Resumo

| # | Titulo | Area | Severidade | Status |
|---|--------|------|------------|--------|
| #1 | Cadastro permite autenticacao sem verificacao de e-mail | Auth JWT | Maior | Aberto |
| #2 | Usuario nao administrador acessa endpoint restrito | Auth JWT | Maior | Aberto |
| #3 | Sistema permite multiplos check-ins por dia | Check-in / Pontos | Maior | Aberto |
| #4 | Check-ins criados com status APPROVED em vez de PENDENTE | Check-in / Pontos | Maior | Aberto |
| #5 | Endpoint de moderacao de status inexistente | Check-in | Maior | Aberto |
| #6 | IDOR: usuario acessa check-ins de outros usuarios | Check-in | Maior | Aberto |
| #7 | Codigo de configuracao de upload em disco nao utilizado | Check-in | Baixa | Aberto |
| #8 | Remocao de check-in nao atualiza pontos nem contagem | Pontos | Maior | Aberto |
| #9 | Sistema nao respeita intervalo minimo de 24h entre check-ins | Check-in | Maior | Aberto |

---

## Relatorio de Testes Final

### Metricas de Execucao

| Metrica | Valor |
|---|---|
| Total de Casos de Teste Planejados | 24 |
| Total de Casos de Teste Executados | 18 |
| Pass | 9 |
| Fail | 9 |
| A verificar (dependem de observacao de resposta) | 6 |
| Aguardando correcao (Regressao) | 3 |
| Taxa de aprovacao (sobre executados) | 50% |

> Os 3 casos de regressao (CT-AUTH-19, CT-CHECKIN-10, CT-PONTOS-05) estao planejados mas aguardam a correcao dos bugs correspondentes para execucao. Os 6 casos marcados "A verificar" exigem observacao do comportamento real da API para registro do resultado final.

---

### Distribuicao de Severidade dos Bugs

| Severidade | Quantidade | Issues |
|---|---|---|
| Critica | 0 |  |
| Maiores | 10 | #1, #2, #3, #4, #5, #6, #8, #9, #10, 11 |
| Menores | 1 | #7 |
| Total | 11 | |

---

### Analise por Area

| Area | Casos Executados | Pass | Fail | A verificar | Bugs Criticos |
|---|---|---|---|---|---|
| Autenticacao JWT | 11 | 7 | 2 | 4 | 2 |
| Check-in por Foto | 9 | 0 | 5 | 4 | 5 |
| Sistema de Pontos | 4 | 0 | 3 | 1 | 3 |

> Alguns bugs impactam mais de uma area (ex: #3 e #4 afetam tanto Check-in quanto Pontos).

---

### Conclusao

O sistema NaSalinha apresenta falhas criticas em todas as tres areas core, sendo inapropriado para uso em producao no estado atual. Os principais riscos identificados sao:

- **Seguranca:** Ausencia de verificacao de e-mail no cadastro e vulnerabilidade IDOR permitem acesso indevido a dados e recursos de outros usuarios
- **Autorizacao:** Falha no RBAC expoe endpoints administrativos a qualquer usuario autenticado, independente de role
- **Integridade de dados:** Pontos sao atribuidos e mantidos incorretamente, comprometendo a confiabilidade do ranking
- **Regras de negocio:** Restricoes de frequencia de check-in e fluxo de moderacao nao estao implementados no backend

---

## Proximos Passos

- Acompanhar correcao das issues criticas abertas (#1, #2, #3, #4, #5, #6, #8, #9)
- Registrar resultados dos 6 casos marcados "A verificar"
- Executar os 3 casos de regressao apos as correcoes
- Expandir cobertura para a area de Temporadas (CRUD completo)
- Adicionar evidencias 
- Configurar Github Projects