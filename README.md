# Processo trainee: Carlos Vinicius Teixeira de Souza

## API QA Testing - NaSalinha

## Descrição

Este repositório contém a execução de testes de API para o sistema NaSalinha, com foco na validação dos fluxos de autenticação e cadastro de usuários.

Os testes foram realizados utilizando o Insomnia, simulando diferentes cenários de uso para garantir o comportamento correto da API.

---

## Escopo Atual

Até o momento, foram testados os seguintes cenários:

### Login

* Login com credenciais válidas
* Login com senha incorreta
* Login com usuário inexistente
* Campos obrigatórios ausentes
* Validação de formato de entrada (email inválido)
* Acesso a rotas protegidas com e sem token

### Cadastro de Usuário

* Cadastro com dados válidos
* Cadastro com email já existente
* Validação de senha fraca
* Campos obrigatórios ausentes (email, nome)
* Validação de formato de email
* Validação de role (valores permitidos e inválidos)
* Comportamento de role padrão (default)

---

## Tecnologias e Ferramentas

* Insomnia → Execução dos testes de API
* API local (Node.js) → Ambiente testado
* Github Issues (WIP)
* Github Projects (WIP)

---

## Tipos de Testes Realizados

* Testes funcionais
* Testes de validação de entrada
* Testes de autenticação
* Testes de casos negativos

---

## Estrutura do Projeto

* `/insomnia` → Coleção com os testes de login e cadastro

---

## Como Executar os Testes

1. Importar a coleção do Insomnia
2. Executar os requests organizados por pastas (login e cadastro)
3. Validar as respostas conforme esperado

---

## Próximos Passos

* Testes de upload de imagens
* Testes de sistema de pontuação
* Estruturação de bug reports (GitHub Issues)
