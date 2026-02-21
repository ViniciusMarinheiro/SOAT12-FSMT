# SOAT12-FSMT - FullStack Motors

**FIAP - Tech Challenge**  
**Pós-graduação Software Architecture**

---

## Equipe

| Nome | RM | GitHub |
|------|-----|--------|
| Andrey Hurpia da Rocha | 366440 | [@ahrocha](https://github.com/ahrocha) |
| Rodrigo Tavares Franco Junior | 366199 | [@rodrigo_juniorj](https://github.com/rodrigo_juniorj) |
| Vinicius Marinheiro Rodrigues Silva | 365289 | [@marinheirovini](https://github.com/marinheirovini) |

---

## Visão Geral

Sistema para gestão de oficinas mecânicas, desenvolvido como projeto da pós-graduação em Software Architecture da FIAP.

O projeto evolui por fases e mantém como objetivo comum:
- gestão ponta a ponta de ordens de serviço;
- integração entre atendimento, orçamento, pagamento e produção;
- arquitetura escalável, resiliente e observável;
- automação de testes e deploy.

---

## Estrutura do Projeto

Este repositório contém a documentação centralizada do projeto SOAT12-FSMT, organizada por fases:

### [Fase 4 - Microsserviços e Saga Pattern](fase-4/README.md)

Documentação da evolução para arquitetura distribuída, com foco em microsserviços e Saga Pattern.

Arquivos da fase 4:
- [README.md](fase-4/README.md)
- [ARCHITECTURE.md](fase-4/ARCHITECTURE.md)

### [Fase 3 - Arquitetura Cloud-Native](fase-3/README.md)

Base arquitetural da solução, com decisões técnicas, diagramas e modelagem de dados.

Arquivos da fase 3:
- [README.md](fase-3/README.md)
- [ARCHITECTURE.md](fase-3/ARCHITECTURE.md)
- [ADRs.md](fase-3/ADRs.md)
- [DATABASE.md](fase-3/DATABASE.md)

---

## Informações Comuns do Projeto

### Domínios de negócio

- Ordem de Serviço
- Pagamento
- Produção/Execução

### Princípios arquiteturais comuns

- Separação por responsabilidade de domínio
- Comunicação desacoplada entre serviços
- Persistência isolada por contexto
- Observabilidade e rastreabilidade de ponta a ponta
- Infraestrutura preparada para execução em Kubernetes

---

## Entregáveis Comuns (todas as fases/repositórios)

- Código-fonte versionado por contexto
- Documentação técnica atualizada
- Conteinerização com Docker
- Manifests/artefatos de deploy para Kubernetes
- Testes automatizados e evidências de execução

---

**Última atualização:** Fevereiro de 2026