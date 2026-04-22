# Portfolio de Qualidade — NaSalinha

**Trainee:** Carlos Vinicius Teixeira de Souza

---

## Descrição

Este repositório contém a auditoria completa de qualidade realizada no sistema **NaSalinha**, cobrindo as três áreas funcionais core: **Autenticação JWT**, **Check-in por Foto** e **Sistema de Pontos**.

O trabalho segue o ciclo completo de QA: planejamento de casos de teste, execução via API (Insomnia), documentação dos bugs encontrados e relatório final de qualidade.

---

## Tecnologias e Ferramentas

| Ferramenta | Uso |
|---|---|
| Insomnia | Execução e validação dos testes de API |
| Docker | Execução do ambiente local da API (Node.js) |
| GitHub Issues | Registro e rastreabilidade dos Bug Reports |
| GitHub Projects | Kanban de gestão do ciclo de testes |

---

## Tipos de Testes Realizados

- Testes Funcionais — validação do comportamento esperado em fluxos positivos e negativos
- Testes de API — validação de endpoints, status codes e corpo das respostas via Insomnia
- Testes de Autenticação e Autorização — controle de acesso com JWT e verificação de roles (RBAC)
- Testes de Segurança — SQL Injection, IDOR, hash de senha
- Testes de Validação de Entrada — campos obrigatórios, formatos inválidos, strings espaçadas, valores inexistentes
- Testes de Consistência de Dados — integridade do saldo de pontos e contagem de check-ins
- Testes de Regressão — validação de que correções não causam efeitos colaterais

---

## Estrutura do Repositório
```
/
├── README.md → Este documento (documentação completa)
└── /insomnia → Coleção exportada com todos os testes de API
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

| Request | Método | Endpoint | Cenário |
|---|---|---|---|
| login-usuario | POST | /api/auth/login | Login válido com credenciais corretas |
| login-sem-criar-conta | POST | /api/auth/login | Login com e-mail inexistente no sistema |
| login-usuario-invalido | POST | /api/auth/login | Login com usuário que não possui conta ativa |
| login-senha-invalida | POST | /api/auth/login | Login com senha incorreta para usuário existente |
| login-campos-vazios | POST | /api/auth/login | Login com e-mail e senha em branco |
| login-faltando-senha | POST | /api/auth/login | Login sem o campo senha no body |
| login-faltando-email | POST | /api/auth/login | Login sem o campo e-mail no body |
| login-formato-invalido-email | POST | /api/auth/login | Login com e-mail sem formato válido (ex: "pedro") |
| login-case-sensitive | POST | /api/auth/login | Login com e-mail e senha com variação de capitalização |
| login-sql-injection | POST | /api/auth/login | Tentativa de SQL Injection no campo e-mail |
| login-varias-tentativas-erradas | POST | /api/auth/login | Múltiplas tentativas de login com credenciais erradas |

### Pasta: logins_cadastros > teste cadastros

| Request | Método | Endpoint | Cenário |
|---|---|---|---|
| cadastrar-usuario | POST | /api/auth/register | Cadastro válido com role ADMIN |
| cadastro-sem-nome | POST | /api/auth/register | Cadastro com campo nome vazio |
| cadastro-sem-email | POST | /api/auth/register | Cadastro com campo e-mail vazio |
| cadastro-ja-cadastrado | POST | /api/auth/register | Cadastro com e-mail já existente no sistema |
| cadastro-senha-fraca | POST | /api/auth/register | Cadastro com senha curta/fraca |
| cadastro-faltando-role-conta-ja-existe | POST | /api/auth/register | Cadastro sem campo role para conta já existente |
| cadastro-faltando-role-conta-nao-existe | POST | /api/auth/register | Cadastro sem campo role para conta nova |
| cadastro-com-role-inexistente | POST | /api/auth/register | Cadastro com role inválida ("PRESIDENTE") |
| cadastro-com-string-espacada | POST | /api/auth/register | Cadastro com espaços nos campos nome, e-mail e senha |
| cadastro-com-email-e-role-espacado | POST | /api/auth/register | Cadastro com espaços no e-mail e na role |

### Pasta: esqueceu_senha

| Request | Método | Endpoint | Cenário |
|---|---|---|---|
| email_existe | POST | /api/auth/forgot-password | Recuperação de senha com e-mail cadastrado |
| email_nao_existe | POST | /api/auth/forgot-password | Recuperação de senha com e-mail inexistente |
| email_vazio | POST | /api/auth/forgot-password | Recuperação de senha com campo e-mail vazio |

### Pasta: admin

| Request | Método | Endpoint | Cenário |
|---|---|---|---|
| logando_sem_ser_admin | GET | /api/seasons | Acesso a rota restrita com token de usuário não-admin |
| atualizar_nota | PATCH | /checkins/:id | Tentativa de alteração de status de check-in |

### Pasta: get_api

| Request | Método | Endpoint | Cenário |
|---|---|---|---|
| acessar_api_sendo_member | GET | /api/users | Acesso ao endpoint de usuários com role MEMBER |
| check_senha_hash | GET | /api/users | Verificação se senha aparece hasheada na resposta da API |
| get_checkins | GET | /api/checkins | Listagem de todos os check-ins com token de ADMIN |
| get_seasons_sendo_member | GET | /api/seasons | Acesso ao endpoint de seasons com role MEMBER |

### Pasta: modulo-checkin

| Request | Método | Endpoint | Cenário |
|---|---|---|---|
| enviar_sem_foto | POST | /api/checkins | Envio de check-in sem arquivo anexado |
| enviar_formato_incorreto | POST | /api/checkins | Envio de check-in com arquivo .cpp (formato inválido) |
| enviar_foto | POST | /api/checkins | Envio de check-in com imagem válida (.png) |
| checar_checkin | GET | /api/checkins/ | Consulta dos check-ins do usuário autenticado |
| checar_checkin_outra_pessoa_sem_admin | GET | /api/checkins/{id} | Acesso ao check-in de outro usuário sem permissão de admin |
| deletar_foto | DELETE | /api/checkins/{id} | Remoção de check-in próprio |
| deletar_foto_admin | DELETE | /api/checkins/{id} | Remoção de check-in por usuário ADMIN |

### Pasta: pontuacoes

| Request | Método | Endpoint | Cenário |
|---|---|---|---|
| checar_pontos | GET | /api/rankings | Consulta do ranking de pontuação com usuário autenticado |

---

## Casos de Teste (CT)

> Estrutura: 2 casos por requisito. Para cada área core há ao menos 1 caso funcional, 1 caso de API e 1 caso de regressão.

---

### Área 1 — Autenticação JWT

---

#### CT-AUTH-01 — Login válido retorna tokens JWT (API — Positivo)

| Campo | Detalhe |
|---|---|
| Request | login-usuario |
| Pré-condição | Usuário `carlosviniciusadmin@gmail.com` cadastrado e ativo |
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
| Pré-condição | Usuário `pedro@example.com` cadastrado |
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
| Pré-condição | E-mail naoexiste@gmail.com não está cadastrado |
| Tipo | API |
| Passos | 1. POST /api/auth/login com e-mail e senha de conta inexistente |
| Resultado Esperado | 401 Unauthorized ou 404 Not Found |
| Resultado Obtido | 401 Unauthorized |
| Status | PASS |

---

#### CT-AUTH-04 — Login com campos vazios deve retornar erro de validação (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | login-campos-vazios |
| Pré-condição | Nenhuma |
| Tipo | API |
| Passos | 1. POST /api/auth/login com e-mail "" e password "" |
| Resultado Esperado | 400 Bad Request com mensagem de validação |
| Resultado Obtido | 400 Bad Request |
| Status | PASS |

---

#### CT-AUTH-05 — Login sem campo senha deve retornar erro de validação (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | login-faltando-senha |
| Pré-condição | Nenhuma |
| Tipo | API |
| Passos | 1. POST /api/auth/login com apenas o campo e-mail no body |
| Resultado Esperado | 400 Bad Request |
| Resultado Obtido | 400 Bad Request |
| Status | PASS |

---

#### CT-AUTH-06 — Login com formato de e-mail inválido deve retornar erro (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | login-formato-invalido-email |
| Pré-condição | Nenhuma |
| Tipo | API |
| Passos | 1. POST /api/auth/login com email "pedro" (sem @) |
| Resultado Esperado | 400 Bad Request com mensagem de formato inválido |
| Resultado Obtido | 400 Bad Request |
| Status | PASS |

---

#### CT-AUTH-07 — Tentativa de SQL Injection no login deve ser bloqueada (Segurança — Negativo)

| Campo | Detalhe |
|---|---|
| Request | login-sql-injection |
| Pré-condição | Nenhuma |
| Tipo | Segurança |
| Passos | 1. POST /api/auth/login com email "'OR 1=1 --" |
| Resultado Esperado | 400 ou 401 — payload malicioso não deve ser executado |
| Resultado Obtido | 400/401 sem execução da injeção |
| Status | PASS |

---

#### CT-AUTH-08 — Cadastro válido deve criar usuário (API — Positivo)

| Campo | Detalhe |
|---|---|
| Request | cadastrar-usuario |
| Pré-condição | E-mail não cadastrado |
| Tipo | API |
| Passos | 1. POST /api/auth/register com nome, e-mail, senha e role válidos |
| Resultado Esperado | 201 Created — usuário criado com status PENDENTE, sem tokens retornados |
| Resultado Obtido | 201 Created — API retorna accessToken e refreshToken imediatamente, sem verificação de e-mail |
| Status | FAIL |
| Bug Report | #1 |

---

#### CT-AUTH-09 — Cadastro com e-mail já existente deve retornar erro (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | cadastro-ja-cadastrado |
| Pré-condição | E-mail bruno@gmail.com já cadastrado |
| Tipo | API |
| Passos | 1. POST /api/auth/register com e-mail já existente |
| Resultado Esperado | 409 Conflict ou 400 Bad Request |
| Resultado Obtido | 409 Conflict |
| Status | PASS |

---

#### CT-AUTH-10 — Cadastro sem nome deve ser rejeitado (Funcional — Negativo)

| Campo | Detalhe |
|---|---|
| Request | cadastro-sem-nome |
| Pré-condição | Nenhuma |
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
| Pré-condição | Nenhuma |
| Tipo | Funcional |
| Passos | 1. POST /api/auth/register com password "123" |
| Resultado Esperado | 400 Bad Request com mensagem sobre requisitos mínimos de senha |
| Resultado Obtido | 400 Bad Request |
| Status | PASS |

---

#### CT-AUTH-12 — Cadastro sem campo role deve atribuir role padrão ou retornar erro (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | cadastro-faltando-role-conta-nao-existe |
| Pré-condição | E-mail semrole@gmail.com não cadastrado |
| Tipo | API |
| Passos | 1. POST /api/auth/register sem o campo role |
| Resultado Esperado | 201 com role padrão atribuída, ou 400 informando campo obrigatório |
| Resultado Obtido | Conta criada com padrão MEMBER |
| Status | PASS |

---

#### CT-AUTH-13 — Cadastro com role inexistente deve ser rejeitado (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | cadastro-com-role-inexistente |
| Pré-condição | Nenhuma |
| Tipo | API |
| Passos | 1. POST /api/auth/register com role "PRESIDENTE" |
| Resultado Esperado | 400 Bad Request — role inválida não deve ser aceita |
| Resultado Obtido | 400 Bad Request |
| Status | PASS |

---

#### CT-AUTH-14 — Cadastro com strings espaçadas deve ser sanitizado (Funcional — Negativo)

| Campo | Detalhe |
|---|---|
| Request | cadastro-com-string-espacada / cadastro-com-email-e-role-espacado |
| Pré-condição | Nenhuma |
| Tipo | Funcional |
| Passos | 1. POST /api/auth/register com espaços extras em nome, e-mail e role |
| Resultado Esperado | Sistema deve sanitizar (trim) os campos ou retornar erro de validação |
| Resultado Obtido | String é sanitizada |
| Status | PASS |

---

#### CT-AUTH-15 — Recuperação de senha com e-mail inexistente não deve revelar existência de conta (Segurança — Negativo)

| Campo | Detalhe |
|---|---|
| Request | email_nao_existe |
| Pré-condição | E-mail carlosviniciusNAOEXISTE@gmail.com não cadastrado |
| Tipo | Segurança |
| Passos | 1. POST /api/auth/forgot-password com e-mail inexistente |
| Resultado Esperado | 200 OK com mensagem genérica (não deve confirmar nem negar existência do e-mail) |
| Resultado Obtido | 200 OK com mensagem genérica |
| Status | PASS |

---

#### CT-AUTH-16 — Usuário não-admin não deve acessar endpoint restrito de seasons (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | logando_sem_ser_admin / get_seasons_sendo_member |
| Pré-condição | Usuários com roles MEMBER e TRAINEE autenticados |
| Tipo | API |
| Passos | 1. GET /api/seasons com token de usuário não-admin |
| Resultado Esperado | 403 Forbidden |
| Resultado Obtido | 200 OK com dados retornados |
| Status | FAIL |
| Bug Report | #2 |

---

#### CT-AUTH-17 — Usuário MEMBER não deve acessar endpoint de listagem de usuários (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | acessar_api_sendo_member |
| Pré-condição | Usuário autenticado com role MEMBER |
| Tipo | API |
| Passos | 1. GET /api/users com token de MEMBER |
| Resultado Esperado | 403 Forbidden |
| Resultado Obtido | "Sem permissao para acessar esse recurso" |
| Status | PASS |

---

#### CT-AUTH-18 — Senha não deve ser retornada em texto plano na listagem de usuários (Segurança — Negativo)

| Campo | Detalhe |
|---|---|
| Request | check_senha_hash |
| Pré-condição | Usuário ADMIN autenticado |
| Tipo | Segurança |
| Passos | 1. GET /api/users com token de ADMIN; 2. Verificar campo password na resposta |
| Resultado Esperado | Campo password deve estar hasheado (bcrypt) ou ausente na resposta |
| Resultado Obtido | Campo password não aparece na resposta da API |
| Status | Indefinido |

#### CT-AUTH-19 — Regressão: após correção dos bugs de autenticação e autorização, fluxos de acesso devem funcionar corretamente (Regressão)

| Campo | Detalhe |
|---|---|
| Pré-condição | Correções dos bugs #1 e #2 implementadas |
| Tipo | Regressão |
| Cenário simulado | Cadastro exige verificação de e-mail; endpoints restritos bloqueiam roles não autorizadas |
| Testes a repetir | 1. Cadastrar novo usuário — tokens não devem ser retornados antes da verificação de e-mail; 2. Login com e-mail não verificado — deve ser bloqueado; 3. Login válido após verificação — deve retornar tokens; 4. GET /api/seasons com token MEMBER — deve retornar 403 Forbidden; 5. GET /api/seasons com token ADMIN — deve retornar 200 OK |
| Objetivo | Garantir que a correção do fluxo de cadastro e do RBAC não afetou o login válido nem o acesso de administradores |
| Status | Aguardando correção |

---

### Área 2 — Check-in por Foto

---

#### CT-CHECKIN-01 — Check-in com imagem válida deve ser criado com status PENDENTE (Funcional — Positivo)

| Campo | Detalhe |
|---|---|
| Request | enviar_foto |
| Pré-condição | Usuário autenticado; nenhum check-in realizado nas últimas 24h |
| Tipo | Funcional |
| Passos | 1. POST /api/checkins com imagem .png válida e token JWT |
| Resultado Esperado | 201 Created — check-in criado com status PENDENTE, sem pontos atribuídos |
| Resultado Obtido | 201 Created — check-in criado com status APPROVED e pontos atribuídos imediatamente |
| Status | FAIL |
| Bug Report | #4 |

---

#### CT-CHECKIN-02 — Check-in sem foto deve ser rejeitado (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | enviar_sem_foto |
| Pré-condição | Usuário autenticado |
| Tipo | API |
| Passos | 1. POST /api/checkins sem arquivo no campo foto |
| Resultado Esperado | 400 Bad Request — campo foto obrigatório |
| Resultado Obtido | 400 Bad Request — campo foto obrigatório |
| Status | PASS |

---

#### CT-CHECKIN-03 — Check-in com formato de arquivo inválido deve ser rejeitado (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | enviar_formato_incorreto |
| Pré-condição | Usuário autenticado |
| Tipo | API |
| Passos | 1. POST /api/checkins com arquivo .cpp no campo foto |
| Resultado Esperado | 400 Bad Request — apenas imagens devem ser aceitas |
| Resultado Obtido | 400 Bad Request — apenas imagens devem ser aceitas |
| Status | PASS |

---

#### CT-CHECKIN-04 — Consulta de check-ins deve retornar apenas os do usuário autenticado (API — Positivo)

| Campo | Detalhe |
|---|---|
| Request | checar_checkin |
| Pré-condição | Usuário autenticado com ao menos um check-in realizado |
| Tipo | API |
| Passos | 1. GET /api/checkins/ com token JWT |
| Resultado Esperado | 200 OK com lista de check-ins pertencentes ao usuário autenticado |
| Resultado Obtido | Retorna todos os check-ins (de todos os usuários) |
| Status | FAIL |

---

#### CT-CHECKIN-05 — Usuário não deve acessar check-in de outro usuário (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | checar_checkin_outra_pessoa_sem_admin |
| Pré-condição | Usuário autenticado; ID de check-in pertencente a outro usuário conhecido |
| Tipo | API |
| Passos | 1. GET /api/checkins/{id} com ID de check-in alheio e token de usuário não-admin |
| Resultado Esperado | 403 Forbidden ou 404 Not Found |
| Resultado Obtido | 200 OK com dados completos do check-in alheio |
| Status | FAIL |
| Bug Report | #6 |

---

#### CT-CHECKIN-06 — Remoção de check-in próprio deve funcionar (API — Positivo)

| Campo | Detalhe |
|---|---|
| Request | deletar_foto |
| Pré-condição | Usuário autenticado com check-in próprio existente |
| Tipo | API |
| Passos | 1. DELETE /api/checkins/{id} com ID de check-in próprio |
| Resultado Esperado | 200 ou 204 — check-in removido com sucesso; pontos subtraídos |
| Resultado Obtido | Check-in removido com sucesso; pontos e contagem permanecem inalterados |
| Status | FAIL |
| Bug Report | #8 |

---

#### CT-CHECKIN-07 — Admin deve poder remover check-in de qualquer usuário (API — Positivo)

| Campo | Detalhe |
|---|---|
| Request | deletar_foto_admin |
| Pré-condição | Usuário ADMIN autenticado; check-in de outro usuário existente |
| Tipo | API |
| Passos | 1. DELETE /api/checkins/{id} com token de ADMIN e ID de check-in de outro usuário |
| Resultado Esperado | 200 ou 204 — check-in removido com sucesso |
| Resultado Obtido | Checkin é deletado, mas pontos não são recomputados (permanece inalterado) |
| Status | FAIL |
| Bug Report | #8 | 

---

#### CT-CHECKIN-08 — Endpoint de alteração de status de check-in deve existir (Funcional — Positivo)

| Campo | Detalhe |
|---|---|
| Request | PATCH /checkins/:id (New Request) |
| Pré-condição | Check-in existente; usuário ADMIN autenticado |
| Tipo | Funcional |
| Passos | 1. PATCH /checkins/:id com body de atualização de status |
| Resultado Esperado | 200 OK — status atualizado para APPROVED ou REJECTED |
| Resultado Obtido | Rota não encontrada — endpoint não implementado |
| Status | FAIL |
| Bug Report | #5 |

---

#### CT-CHECKIN-09 — Sistema não deve permitir múltiplos check-ins no mesmo dia (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | enviar_foto (executado múltiplas vezes) |
| Pré-condição | Usuário autenticado com check-in já realizado no dia |
| Tipo | API |
| Passos | 1. POST /api/checkins repetido no mesmo dia |
| Resultado Esperado | 400 ou 409 a partir do segundo check-in |
| Resultado Obtido | 201 Created — múltiplos check-ins aceitos no mesmo dia |
| Status | FAIL |
| Bug Report | #3 |

---

#### CT-CHECKIN-10 — Regressão: após correção dos bugs de check-in, fluxo completo deve respeitar regras de negócio (Regressão)

| Campo | Detalhe |
|---|---|
| Pré-condição | Correções dos bugs #3, #4, #5 e #6 implementadas |
| Tipo | Regressão |
| Cenário simulado | Check-in criado com status PENDENTE; endpoint de moderação disponível; restrição de 24h ativa; acesso IDOR bloqueado |
| Testes a repetir | 1. POST /api/checkins com imagem válida — status deve ser PENDENTE; 2. POST /api/checkins no mesmo dia — deve ser bloqueado com 400 ou 409; 3. PATCH /checkins/:id com ADMIN — deve atualizar status para APPROVED ou REJECTED; 4. GET /api/checkins/{id} com token de outro usuário sem ser ADMIN — deve retornar 403 ou 404; 5. DELETE /api/checkins/{id} com dono do check-in — deve remover com sucesso |
| Objetivo | Garantir que as correções do módulo de check-in não causaram regressão nos fluxos de envio válido, listagem e remoção |
| Status | Aguardando correção |

---

#### CT-CHECKIN-XX (Ad-hoc) — Código de upload em disco não é utilizado em produção

| Campo | Detalhe |
|---|---|
| Tipo | Funcional / Análise de Código |
| Pré-condição | Acesso ao código-fonte do projeto; ambiente rodando via Docker |
| Passos | 1. Inspecionar o código de configuração de upload; 2. Verificar se há distinção de ambientes (dev/prod) no Dockerfile e docker-compose.yml; 3. Verificar se o código de upload em disco é referenciado em algum fluxo ativo |
| Resultado Esperado | Dockerfile e docker-compose devem definir builds distintas para desenvolvimento e produção; código de upload em disco deve estar ativo apenas no ambiente correto |
| Resultado Obtido | Nenhuma distinção de ambiente encontrada nos arquivos de configuração Docker; código de upload em disco presente mas inutilizado em qualquer ambiente |
| Status | FAIL |
| Bug Report | #7 |

---

### Área 3 — Sistema de Pontos

---

#### CT-PONTOS-01 — Ranking deve ser acessível a usuários autenticados (API — Positivo)

| Campo | Detalhe |
|---|---|
| Request | checar_pontos |
| Pré-condição | Usuário autenticado com token JWT válido |
| Tipo | API |
| Passos | 1. GET /api/rankings com token JWT |
| Resultado Esperado | 200 OK com lista de usuários ordenada por pontuação (crescente)|
| Resultado Obtido | 200 OK com lista de usuários ordenada por pontuação (crescente) |
| Status | PASS |

---

#### CT-PONTOS-02 — Pontos devem ser atribuídos apenas após aprovação do check-in (Funcional — Negativo)

| Campo | Detalhe |
|---|---|
| Request | enviar_foto + checar_pontos |
| Pré-condição | Usuário autenticado sem check-ins anteriores |
| Tipo | Funcional |
| Passos | 1. POST /api/checkins com imagem; 2. GET /api/rankings imediatamente após |
| Resultado Esperado | Pontos não devem ser atribuídos antes da aprovação por ADMINISTRADOR |
| Resultado Obtido | Pontos atribuídos imediatamente na criação do check-in |
| Status | FAIL |
| Bug Report | #4 |

---

#### CT-PONTOS-03 — Remoção de check-in deve subtrair os pontos correspondentes (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | deletar_foto + checar_pontos |
| Pré-condição | Usuário com check-in criado e pontos acumulados |
| Tipo | API |
| Passos | 1. Verificar pontos antes; 2. DELETE /api/checkins/{id}; 3. Verificar pontos após |
| Resultado Esperado | Saldo reduzido; contagem decrementada; ranking atualizado |
| Resultado Obtido | Check-in removido, pontos e contagem permanecem inalterados |
| Status | FAIL |
| Bug Report | #8 |

---

#### CT-PONTOS-04 — Múltiplos check-ins no mesmo dia não devem acumular pontos (API — Negativo)

| Campo | Detalhe |
|---|---|
| Request | enviar_foto (múltiplas execuções) + checar_pontos |
| Pré-condição | Usuário autenticado |
| Tipo | API |
| Passos | 1. Realizar 3 check-ins consecutivos; 2. GET /api/rankings |
| Resultado Esperado | Apenas o primeiro check-in do dia gera pontos |
| Resultado Obtido | Cada check-in adicional incrementa os pontos, corrompendo o ranking |
| Status | FAIL |
| Bug Report | #3 |

---

#### CT-PONTOS-05 — Regressão: após correção da consistência de pontos, saldo deve ser preciso em todo o ciclo (Regressão)

| Campo | Detalhe |
|---|---|
| Pré-condição | Correções dos bugs #4, #8 e #3 implementadas |
| Tipo | Regressão |
| Cenário simulado | Pontos atribuídos apenas após aprovação; subtraídos ao deletar check-in |
| Testes a repetir | 1. Criar check-in — pontos não devem ser atribuídos; 2. ADMIN aprovar check-in — pontos devem ser atribuídos; 3. Deletar check-in aprovado — pontos devem ser subtraídos; 4. Consultar GET /api/rankings — posição do usuário deve estar atualizada; 5. Segundo check-in no mesmo dia — bloqueado, pontos inalterados |
| Objetivo | Garantir que a correção da atribuição de pontos não afetou os fluxos de criação, aprovação, remoção e ranking |
| Status | Aguardando correção |

### Área 4 - Extras

#### CT-FRONTEND-01 (Ad-hoc) — Ranking de pontuação deve ser visível na interface para todos os usuários autenticados

| Campo | Detalhe |
|---|---|
| Tipo | Funcional / Interface |
| Pré-condição | Usuário autenticado com qualquer role |
| Passos | 1. Fazer login com qualquer usuário; 2. Navegar pela interface do sistema em busca da tela de ranking |
| Resultado Esperado | Tela de ranking deve estar acessível na interface para qualquer usuário autenticado |
| Resultado Obtido | Nenhuma tela de ranking existe no frontend; a pontuação só é consultável via requisição direta à API |
| Status | FAIL |
| Bug Report | #10 |

---

#### CT-FRONTEND-02 (Ad-hoc) — Sistema de pontos deve refletir decrementos na interface

| Campo | Detalhe |
|---|---|
| Tipo | Funcional / Interface |
| Pré-condição | Usuário autenticado com pontos acumulados; ao menos um check-in aprovado |
| Passos | 1. Verificar saldo de pontos na interface; 2. Deletar um check-in aprovado; 3. Verificar saldo de pontos na interface após a remoção |
| Resultado Esperado | Saldo de pontos deve ser decrementado e o ranking atualizado na tela |
| Resultado Obtido | Saldo de pontos permanece inalterado na interface após remoção do check-in. O que condiz com o erro apresentado em CT-PONTOS-03 |
| Status | FAIL |
| Bug Report | #11 |

---

# Bug Reports


## Resumo

| #  | Título                                                                 | Área                 | Severidade | Status |
|----|------------------------------------------------------------------------|----------------------|------------|--------|
| #1  | Cadastro permite autenticação sem verificação de e-mail              | Auth JWT             | Maior      | Aberto |
| #2  | Usuário não administrador acessa endpoint restrito                    | Auth JWT             | Maior      | Aberto |
| #3  | Sistema permite múltiplos check-ins por dia                           | Check-in / Pontos    | Maior      | Aberto |
| #4  | Check-ins criados com status APPROVED em vez de PENDENTE             | Check-in / Pontos    | Maior      | Aberto |
| #5  | Endpoint de moderação de status inexistente                           | Check-in             | Maior      | Aberto |
| #6  | IDOR: usuário acessa check-ins de outros usuários                     | Check-in             | Maior      | Aberto |
| #7  | Código de configuração de upload em disco não utilizado               | Check-in             | Menor      | Aberto |
| #8  | Remoção de check-in não atualiza pontos nem contagem                  | Pontos               | Maior      | Aberto |
| #9  | Sistema não respeita intervalo mínimo de 24h entre check-ins          | Check-in             | Maior      | Aberto |
| #10 | Ranking de usuários visível apenas para administradores               | Ranking / Auth / Visual     | Maior      | Aberto |
| #11 | Sistema de pontos não decrementa                                       | Pontos  / Visual              | Maior      | Aberto |

---

## Relatório de Testes Final

---

### Métricas de Execução

| Métrica | Valor |
|---|---|
| Total de Casos de Teste Planejados | 37 |
| Total de Casos de Teste Executados | 34 |
| Pass | 18  |
| Fail | 15  |
| Indefinido | 1 |
| Aguardando correção | 3 |
| Taxa de aprovação (sobre executados) | 53% |

> O  caso de regressão (CT-PONTOS-05) esta planejado mas aguarda correção dos bugs correspondentes para nova execução.

---

### Distribuição de Severidade dos Bugs

| Severidade | Quantidade | Issues |
|------------|------------|--------|
| Crítica    | 0          | 0      |
| Maior      | 10         | #1, #2, #3, #4, #5, #6, #8, #9, #10, #11 |
| Menor      | 1          | #7     |
| Total      | 11         | 11     |

### Análise por Área

| Área | Casos Planejados | Pass | Fail | Indefinido | Aguardando correção | Bugs Críticos |
|---|---|---|---|---|---|---|
| Autenticação JWT | 19 | 15 | 2 | 1 | 1 | 2 |
| Check-in por Foto | 10 | 2 | 7 | 0 | 1 | 5 |
| Sistema de Pontos | 5 | 1 | 3 | 0 | 1 | 3 |
> Alguns bugs impactam mais de uma área (ex: #3 e #4 afetam tanto Check-in quanto Pontos).

---

### Conclusão

O sistema NaSalinha apresenta falhas críticas em todas as três áreas core, sendo inapropriado para uso em produção no estado atual. Os principais riscos identificados são:

- **Segurança:** Ausência de verificação de e-mail no cadastro e vulnerabilidade IDOR permitem acesso indevido a dados e recursos de outros usuários
- **Autorização:** Falha no RBAC expõe endpoints administrativos a qualquer usuário autenticado, independente de role
- **Integridade de dados:** Pontos são atribuídos e mantidos incorretamente, comprometendo a confiabilidade do ranking
- **Regras de negócio:** Restrições de frequência de check-in e fluxo de moderação não estão implementados no backend

---

## Próximos Passos

- Acompanhar correção das issues críticas abertas (#1, #2, #3, #4, #5, #6, #8, #9)
- Registrar resultados dos 6 casos marcados "A verificar"
- Executar os 3 casos de regressão após as correções
- Expandir cobertura para a área de Temporadas (CRUD completo)
- Adicionar evidências 
- Configurar GitHub Projects