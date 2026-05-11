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
├── /docs - Documentos auxiliares de report
└── /insomnia → Coleção exportada com todos os testes de API
```

---

## Como Executar os Testes

1. Subir o ambiente local com Docker
2. Importar o arquivo `qa.yaml` localizado em `/insomnia`
3. Executar os requests organizados pelas pastas: `logins_cadastros`, `esqueceu_senha`, `admin`, `get_api`, `modulo-checkin`, `pontuacoes`
4. Validar as respostas conforme os resultados esperados descritos nos casos de teste abaixo

---

## Escopo dos Testes Executados

O escopo dos testes executados está detalhado no documento `NaSalinha_Escopos_dos_Testes.pdf`, dentro da pasta `docs/`

---

## Casos de Teste (CT)

> Estrutura: 2 casos por requisito. Para cada área core há ao menos 1 caso funcional, 1 caso de API e 1 caso de regressão.

A documentação dos casos de teste detalhada consta no documento `NaSalinha_Casos_de_Teste.pdf`, dentro da pasta `docs/`

--- 

# Bug Reports — Resumo


| #   | Título                                                              | Área                        | Severidade | Status |
|-----|---------------------------------------------------------------------|-----------------------------|------------|--------|
| #1  | Cadastro permite autenticação sem verificação de e-mail             | Auth JWT                    | Maior      | Aberto |
| #2  | Usuário não administrador acessa endpoint restrito                  | Auth JWT                    | Maior      | Aberto |
| #3  | Sistema permite múltiplos check-ins por dia                         | Check-in / Pontos           | Maior      | Aberto |
| #4  | Check-ins criados com status APPROVED em vez de PENDENTE            | Check-in / Pontos           | Maior      | Aberto |
| #5  | Endpoint de moderação de status inexistente                         | Check-in                    | Maior      | Aberto |
| #6  | IDOR: usuário acessa check-ins de outros usuários                   | Check-in                    | Maior      | Aberto |
| #7  | Código de configuração de upload em disco não utilizado             | Check-in                    | Menor      | Aberto |
| #8  | Remoção de check-in não atualiza pontos nem contagem                | Pontos                      | Maior      | Aberto |
| #9  | Sistema não respeita intervalo mínimo de 24h entre check-ins        | Check-in                    | Maior      | Aberto |
| #10 | Ranking de usuários visível apenas para administradores             | Ranking / Visual            | Maior      | Aberto |
| #11 | Sistema de pontos não decrementa                                    | Pontos / Visual             | Maior      | Aberto |
| #15 | Frontend exibe múltiplas temporadas ativas simultaneamente          | Temporadas / Frontend       | Menor      | Aberto |
| #16 | API aceita datas inválidas e converte para epoch (1970)             | Temporadas / Validação      | Maior      | Aberto |

As issues #12, #13, #14 apresentam um resumo breve de cada área principal, não incluso o sistema de temporadas (que está disponível através do PDF).

---

# Relatório de Testes Final

---

## Métricas de Execução
 
| Métrica                                    | Valor |
|--------------------------------------------|-------|
| Total de Casos de Teste Planejados         | 45    |
| Total de Casos de Teste Executados         | 42    |
| Pass                                       | 24    |
| Fail                                       | 17    |
| Indefinido                                 | 1     |
| Aguardando correção                        | 3     |
| Taxa de aprovação (sobre executados)       | 57%   |


> Os casos de regressão (CT-PONTOS-05, CT-CHECKIN-10, CT-AUTH-19) estão planejados mas aguardam correção dos bugs correspondentes para nova execução.

---

## Distribuição de Severidade dos Bugs

| Severidade | Quantidade | Issues                                                         |
|------------|------------|----------------------------------------------------------------|
| Crítica    | 0          | —                                                              |
| Maior      | 12         | #1, #2, #3, #4, #5, #6, #8, #9, #10, #11, #12, #16            |
| Menor      | 2          | #7, #15                                                             |
| **Total**  | **14**     | **14**                                                         |

---

## Análise por Área

| Área               | Casos Planejados | Pass | Fail | Indefinido | Aguardando correção | Bugs Encontrados |
|--------------------|------------------|------|------|------------|---------------------|------------------|
| Autenticação JWT   | 19               | 15   | 2    | 1          | 1                   | 2                |
| Check-in por Foto  | 10               | 2    | 8    | 0          | 1                   | 5                |
| Sistema de Pontos  | 5                | 1    | 3    | 0          | 1                   | 3                |
| Frontend           | 2                | 0    | 2    | 0          | 0                   | 2                 |
| Temporadas         | 8                | 6    | 2    | 0          | 0                   | 2                |
| **Total**          | **42**           | **24** | **17** | **1**  | **3**               | **14**           |

> Alguns bugs impactam mais de uma área (ex: #3 e #4 afetam tanto Check-in quanto Pontos). Testes ad-hoc (CT-FRONTEND-01, CT-FRONTEND-02, CT-CHECKIN-XX) não constam na tabela de planejados por não terem sido previstos originalmente.

---

## Conclusão

O sistema NaSalinha apresenta falhas críticas em todas as áreas testadas, sendo inapropriado para uso em produção no estado atual. Os principais riscos identificados são:

- **Segurança:** Ausência de verificação de e-mail no cadastro e vulnerabilidade IDOR permitem acesso indevido a dados e recursos de outros usuários
- **Autorização:** Falha no RBAC expõe endpoints administrativos a qualquer usuário autenticado, independente de role
- **Integridade de dados:** Pontos são atribuídos e mantidos incorretamente, comprometendo a confiabilidade do ranking
- **Regras de negócio:** Restrições de frequência de check-in e fluxo de moderação não estão implementados no backend
- **Temporadas:** Ausência de validação de datas e inconsistência entre frontend e backend comprometem a confiabilidade do gerenciamento de temporadas
