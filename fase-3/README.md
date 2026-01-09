# Tech Challenge - Fase 3 - SOAT12

**Faculdade de Informática e Administração Paulista**  
**Pós-graduação Software Architecture**  
**FullStack Motors**

**Equipe:**
- Andrey Hurpia da Rocha - RM366440
- Rodrigo Tavares Franco Junior - RM366199
- Vinicius Marinheiro Rodrigues Silva - RM365289

**São Paulo, 2026**

---

## Índice

1. [Entregáveis](#entregáveis)
2. [Links dos Repositórios](#links-dos-repositórios)
3. [Documentação da Arquitetura](#documentação-da-arquitetura)
4. [Integrantes](#integrantes)

---

## Links

### Repositórios do Projeto

| Repositório | Descrição | Link |
|-------------|-----------|------|
| **SOAT12-FSMT-INFRA** | Infraestrutura como Código (Terraform) | https://github.com/ViniciusMarinheiro/SOAT12-FSMT-INFRA/ |
| **SOAT12-FSMT-DB** | Scripts de inicialização do banco de dados | https://github.com/ViniciusMarinheiro/SOAT12-FSMT-DB |
| **SOAT12-FSMT-LAMBDA** | Azure Functions para autenticação | https://github.com/ViniciusMarinheiro/SOAT12-FSMT-LAMBDA |
| **SOAT12-FSMT-API** | API REST principal (NestJS) | https://github.com/ViniciusMarinheiro/SOAT12-FSMT-API |
| **SOAT12-FSMT** | Documentação centralizada | https://github.com/ViniciusMarinheiro/SOAT12-FSMT |

---

## Documentação da Arquitetura

A documentação completa da arquitetura está organizada nos seguintes documentos:

### [Diagrama de Componentes](ARCHITECTURE.md#diagrama-de-componentes)
Visão completa da arquitetura em nuvem, incluindo:
- Azure Kubernetes Service (AKS)
- Azure Load Balancer
- PostgreSQL Flexible Server
- Azure Functions
- Redis + BullMQ
- New Relic APM
- Azure Container Registry

### [Diagramas de Sequência](ARCHITECTURE.md#diagramas-de-sequência)
Fluxos detalhados do sistema:
- Autenticação de cliente via CPF
- Criação de ordem de serviço
- Aprovação de orçamento
- Consulta de ordens de serviço
- Atualização de status

### [Decisões Técnicas (RFCs)](ARCHITECTURE.md#decisões-arquiteturais)
Justificativas para decisões técnicas relevantes:
- Escolha do Microsoft Azure como cloud provider
- Azure Database for PostgreSQL como banco de dados
- Arquitetura serverless para autenticação
- Azure Kubernetes Service (AKS) para orquestração
- Estratégia REST + Message Queue (BullMQ/Redis)
- New Relic + Azure Monitor para observabilidade

### [ADRs - Architecture Decision Records](ADRs.md)
Decisões arquiteturais permanentes implementadas:
- ADR-001: Horizontal Pod Autoscaler (HPA)
- ADR-002: Azure Functions para Autenticação
- ADR-003: BullMQ para Processamento Assíncrono
- ADR-004: TypeORM para Persistência
- ADR-005: New Relic para Observabilidade
- ADR-006: PostgreSQL como Banco de Dados
- ADR-007: Redis para Cache e Filas
- ADR-008: Recursos de Pods Definidos

### [Justificativa do Banco de Dados](DATABASE.md)
Documentação completa do modelo relacional:
- Escolha do PostgreSQL (comparação com alternativas)
- Diagrama Entidade-Relacionamento (ERD)
- Descrição das entidades e relacionamentos
- Normalização (3NF) e integridade referencial
- Justificativas de design (preços em centavos, ENUMs, etc.)
- Evolução do schema (migrations versionadas)
- Estratégia dual para testes (SQLite local, PostgreSQL CI/CD)

---

## Integrantes

| Nome | RM | GitHub Username |
|------|-----|-----------------|
| Andrey Hurpia da Rocha | 366440 | [@ahrocha](https://github.com/ahrocha) |
| Rodrigo Tavares Franco Junior | 366199 | [@rodrigo_juniorj](https://github.com/rodrigo_juniorj) |
| Vinicius Marinheiro Rodrigues Silva | 365289 | [@marinheirovini](https://github.com/marinheirovini) |
