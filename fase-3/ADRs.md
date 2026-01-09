# ADRs - Architecture Decision Records

**Projeto:** Tech Challenge - Fase 3 - SOAT12  
**Data:** Janeiro de 2026  
**Versão:** 1.0

---

## Índice

1. [Introdução](#introdução)
2. [ADR-001: Horizontal Pod Autoscaler (HPA)](#adr-001-horizontal-pod-autoscaler-hpa)
3. [ADR-002: Azure Functions para Autenticação de Clientes](#adr-002-azure-functions-para-autenticação-de-clientes)
4. [ADR-003: BullMQ para Processamento Assíncrono](#adr-003-bullmq-para-processamento-assíncrono)
5. [ADR-004: TypeORM para Persistência](#adr-004-typeorm-para-persistência)
6. [ADR-005: New Relic para Observabilidade](#adr-005-new-relic-para-observabilidade)
7. [ADR-006: PostgreSQL como Banco de Dados](#adr-006-postgresql-como-banco-de-dados)
8. [ADR-007: Redis para Cache e Filas](#adr-007-redis-para-cache-e-filas)
9. [ADR-008: Recursos de Pods Definidos](#adr-008-recursos-de-pods-definidos)
10. [Resumo de Decisões](#resumo-de-decisões)

---

## Introdução

Os ADRs (Architecture Decision Records) documentam as principais decisões arquiteturais permanentes extraídas da implementação real do sistema FullStack Motors (FSMT). Cada ADR segue o formato:

- **Status:** Estado atual da decisão (Implementado, Proposto, Depreciado)
- **Arquivo(s):** Referência aos arquivos de código relacionados
- **Decisão:** Descrição sucinta da escolha técnica
- **Implementação/Configuração:** Detalhes técnicos extraídos do código
- **Justificativa:** Razões técnicas e de negócio para a decisão

---

## ADR-001: Horizontal Pod Autoscaler (HPA)

**Status:** Implementado  
**Arquivo:** [k8s/hpa.yml](SOAT12-FSMT-API/k8s/hpa.yml)

**Decisão:** Auto-scaling de pods baseado em métricas de CPU.

**Configuração Implementada:**
```yaml
minReplicas: 2
maxReplicas: 5
targetCPUUtilizationPercentage: 70
```

**Justificativa:**
- Otimização de custos (reduz pods fora do horário de pico)
- Mantém performance em momentos de alta demanda
- Configuração declarativa via Kubernetes nativo

---

## ADR-002: Azure Functions para Autenticação de Clientes

**Status:** Implementado  
**Arquivo:** [SOAT12-FSMT-LAMBDA/src/functions/authClient](SOAT12-FSMT-LAMBDA/src/functions/authClient/index.ts)

**Decisão:** Autenticação de clientes via CPF em Azure Function separada da API principal.

**Implementação:**
- Function HTTP com `authLevel: anonymous`
- Validação de CPF/CNPJ
- Consulta ao banco de dados PostgreSQL
- Geração de JWT com role `client`

**Justificativa:**
- Separação de responsabilidades (auth ≠ core business)
- Escalabilidade independente (serverless)
- Custo zero (free tier Azure Functions)

---

## ADR-003: BullMQ para Processamento Assíncrono

**Status:** Implementado  
**Arquivos:** 
- [src/providers/email/job/send-email-queue](SOAT12-FSMT-API/src/providers/email/job/send-email-queue)
- [package.json](SOAT12-FSMT-API/package.json) (`@nestjs/bullmq`)

**Decisão:** Uso de BullMQ com Redis para processamento assíncrono de e-mails.

**Implementação:**
```typescript
@InjectQueue('SEND_EMAIL_QUEUE')
private sendEmailQueue: Queue

await this.sendEmailQueue.add('SEND_EMAIL_QUEUE', emailData)
```

**Justificativa:**
- Desacopla envio de e-mail da resposta HTTP (API responde < 200ms)
- Retry automático em falhas de SMTP
- Persistência no Redis (não perde jobs)

---

## ADR-004: TypeORM para Persistência

**Status:** Implementado  
**Arquivos:** 
- [src/config/database](SOAT12-FSMT-API/src/config/database)
- [src/migrations](SOAT12-FSMT-API/src/migrations)

**Decisão:** TypeORM como ORM principal com migrations versionadas.

**Implementação:**
- Migrations automáticas (`migration:generate`, `migration:run`)
- Entities declarativas com decorators
- Connection pooling configurado

**Justificativa:**
- Integração nativa com NestJS
- Migrations versionadas (controle de schema)
- Suporte completo a PostgreSQL (JSONB, triggers, views)

---

## ADR-005: New Relic para Observabilidade

**Status:** Implementado  
**Arquivo:** [k8s/deployment.yml](SOAT12-FSMT-API/k8s/deployment.yml) + [k8s/instrumentation.yaml](SOAT12-FSMT-API/k8s/instrumentation.yaml)

**Decisão:** New Relic APM como ferramenta principal de observabilidade.

**Configuração:**
```yaml
env:
  - name: NEW_RELIC_APP_NAME
    value: "12_soat_main"
  - name: NEW_RELIC_LICENSE_KEY
    valueFrom:
      secretKeyRef:
        name: oficina-secrets
        key: NEW_RELIC_LICENSE_KEY
  - name: NEW_RELIC_NO_CONFIG_FILE
    value: "true"
```

**Justificativa:**
- Distributed tracing automático (API → DB → Redis)
- Free tier generoso (100GB/mês)
- Auto-instrumentation via Kubernetes operator

---

## ADR-006: PostgreSQL como Banco de Dados

**Status:** Implementado  
**Configuração:** [k8s/deployment.yml](SOAT12-FSMT-API/k8s/deployment.yml)

**Decisão:** Azure Database for PostgreSQL Flexible Server.

**Conexão:**
```yaml
DB_HOST: psql-oficina-fsmt-soat-12.postgres.database.azure.com
DB_PORT: 5432
PGSSLMODE: require
```

**Justificativa:**
- Modelo relacional adequado (Cliente → Veículo → Work Order)
- Transações ACID críticas (estoque, status)
- Backups automáticos (Azure gerenciado)

---

## ADR-007: Redis para Cache e Filas

**Status:** Implementado  
**Arquivos:** 
- [k8s/redis.yml](SOAT12-FSMT-API/k8s/redis.yml)
- [k8s/deployment.yml](SOAT12-FSMT-API/k8s/deployment.yml)

**Decisão:** Redis como cache e message broker (BullMQ).

**Configuração:**
```yaml
REDIS_HOST: redis-service
REDIS_PORT: 6379
```

**Justificativa:**
- BullMQ requer Redis
- Cache de queries (futuro)
- In-memory performance

---

## ADR-008: Recursos de Pods Definidos

**Status:** Implementado  
**Arquivo:** [k8s/deployment.yml](SOAT12-FSMT-API/k8s/deployment.yml)

**Decisão:** Definição explícita de requests e limits para cada pod.

**Configuração:**
```yaml
resources:
  requests:
    cpu: "100m"
    memory: "256Mi"
  limits:
    cpu: "500m"
    memory: "512Mi"
```

**Justificativa:**
- HPA requer requests definidos para funcionar
- Evita pods consumindo recursos descontroladamente
- Garante QoS (Quality of Service) no Kubernetes

---

## Resumo de Decisões

| ADR | Decisão | Status | Impacto |
|-----|---------|--------|---------|
| 001 | HPA (2-5 pods, 70% CPU) | Implementado | Otimização de custos |
| 002 | Azure Functions (authClient) | Implementado | Separação auth/core |
| 003 | BullMQ (email queue) | Implementado | Performance API |
| 004 | TypeORM + Migrations | Implementado | Versionamento schema |
| 005 | New Relic APM | Implementado | Observabilidade |
| 006 | PostgreSQL (Azure) | Implementado | Dados transacionais |
| 007 | Redis (cache + queue) | Implementado | Performance + filas |
| 008 | Pod resources | Implementado | HPA + QoS |

---

**Documento elaborado por:** Equipe SOAT12  
**Última atualização:** 08 de Janeiro de 2026  
**Versão:** 1.0
