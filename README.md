# SOAT12-FSMT - FullStack Motors

**FIAP - Tech Challenge**  
**Pós-graduação Software Architecture**

---

## Visão Geral

Sistema para gestão de oficinas mecânicas, desenvolvido como projeto na pos graduacao em Software Architecture da FIAP. O sistema permite o controle completo de ordens de serviço, clientes, veículos, peças e notificações através de uma arquitetura distribuída em Azure.

---

## Estrutura do Projeto

Este repositório contém a documentação centralizada do projeto SOAT12-FSMT, organizada por fases:

### [Fase 3 - Arquitetura Cloud-Native](fase-3/README.md)

Documentação completa da arquitetura implementada, incluindo:
- **Diagrama de Componentes** - Visão completa da arquitetura em nuvem
- **Diagramas de Sequência** - Fluxos detalhados do sistema
- **Decisões Técnicas (RFCs)** - Justificativas para escolhas arquiteturais
- **ADRs** - Architecture Decision Records permanentes
- **Justificativa do Banco de Dados** - Modelo relacional e normalização

---

## Repositórios do Projeto

O sistema está dividido em 4 repositórios especializados:

| Repositório | Descrição | Tecnologias |
|-------------|-----------|-------------|
| [SOAT12-FSMT-API](https://github.com/ViniciusMarinheiro/SOAT12-FSMT-API) | API REST principal | NestJS, TypeORM, PostgreSQL |
| [SOAT12-FSMT-LAMBDA](https://github.com/ViniciusMarinheiro/SOAT12-FSMT-LAMBDA) | Autenticação serverless | Azure Functions, Node.js |
| [SOAT12-FSMT-INFRA](https://github.com/ViniciusMarinheiro/SOAT12-FSMT-INFRA) | Infraestrutura como Código | Terraform, Azure |
| [SOAT12-FSMT-DB](https://github.com/ViniciusMarinheiro/SOAT12-FSMT-DB) | Scripts de banco de dados | PostgreSQL, SQL |

---

## Tecnologias Principais

- **Backend**: NestJS 11, TypeScript
- **Database**: Azure PostgreSQL Flexible Server
- **Cache/Queue**: Redis + BullMQ
- **Orchestration**: Azure Kubernetes Service (AKS)
- **Serverless**: Azure Functions v4
- **Observability**: New Relic APM
- **CI/CD**: GitHub Actions
- **IaC**: Terraform

---

## Equipe

| Nome | RM | GitHub |
|------|-----|--------|
| Andrey Hurpia da Rocha | 366440 | [@ahrocha](https://github.com/ahrocha) |
| Rodrigo Tavares Franco Junior | 366199 | [@rodrigo_juniorj](https://github.com/rodrigo_juniorj) |
| Vinicius Marinheiro Rodrigues Silva | 365289 | [@marinheirovini](https://github.com/marinheirovini) |

---

**Última atualização:** Janeiro de 2026