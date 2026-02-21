# Tech Challenge - Fase 4 - SOAT12

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

1. [Contexto da Fase](#contexto-da-fase)
2. [O que implementamos](#o-que-implementamos)
3. [Arquitetura de microsserviços](#arquitetura-de-microsserviços)
4. [Documentação técnica complementar](#documentação-técnica-complementar)
5. [Saga Pattern](#saga-pattern)
6. [Atendimento dos requisitos do enunciado](#atendimento-dos-requisitos-do-enunciado)
7. [Entregáveis e evidências](#entregáveis-e-evidências)
8. [Próximos passos](#próximos-passos)
9. [Compensação de pagamento na saga](#compensação-de-pagamento-na-saga)

---

## Contexto da Fase

Nesta fase, o objetivo foi refatorar a solução da oficina mecânica para um modelo distribuído com microsserviços independentes, comunicação assíncrona e tratamento transacional entre domínios com Saga Pattern, conforme o enunciado do **"12SOAT - Fase 4 - Tech challenge"**.

O desafio exigiu principalmente:
- separação em múltiplos serviços com responsabilidades claras;
- integração desacoplada via mensageria;
- controle de falhas com compensação;
- automação de build, testes e deploy em Kubernetes.

---

## O que implementamos

A solução foi dividida em três microsserviços principais:

| Microsserviço | Responsabilidade principal | Repositório |
|---|---|---|
| **MS-ORDER** | Gestão de ordens de serviço (abertura, status, histórico) e coordenação do fluxo de negócio | `SOAT12-FSMT-MS-ORDER` |
| **MS-PAYMENT** | Integração de pagamento (Mercado Pago), processamento de pagamentos e publicação de eventos de aprovação | `SOAT12-FSMT-MS-PAYMENT` |
| **MS-PRODUCTION** | Execução/produção da ordem (fila de execução e atualização de progresso) | `SOAT12-FSMT-MS-PRODUCTION` |

Além da separação por domínio, foi aplicada comunicação orientada a eventos com RabbitMQ para reduzir acoplamento entre os serviços.

---

## Arquitetura de microsserviços

### Comunicação

- **Assíncrona (principal):** RabbitMQ com publicação/consumo de eventos entre os serviços.
- **Síncrona (quando necessário):** APIs HTTP documentadas via Swagger em cada serviço.

Eventos observados na implementação atual:
- `work_order.created`
- `work_order.budget_generated`
- `work_order.awaiting_approval`
- `payment.v1.requested`
- `payment.processed`
- `payment.approved`
- `send-to-production`
- `status-update`
- `compensate`

### Banco de dados por serviço

- Cada microsserviço possui configuração e camada de persistência próprias.
- O ecossistema contempla uso de banco relacional (PostgreSQL).
- Existe configuração de infraestrutura para MongoDB no escopo de produção, mantendo a estratégia de evolução para NoSQL no contexto distribuído.

## Documentação técnica complementar

Para visão arquitetural detalhada da fase 4 (serviços, comunicação, saga, compensação e fluxo ponta a ponta), consultar:

- [ARCHITECTURE.md](ARCHITECTURE.md)
- [SAGA-COMPENSACAO-PAGAMENTO.md](SAGA-COMPENSACAO-PAGAMENTO.md)

---

## Saga Pattern

### Estratégia escolhida

Foi adotado **Saga Pattern coreografado (event-driven)**.

### Justificativa

- Melhor aderência ao modelo desacoplado entre domínios (ordem, pagamento e produção).
- Menor dependência de um orquestrador único.
- Facilidade para escalar consumidores independentemente.
- Boa integração com o backbone de mensageria já existente (RabbitMQ).

### Fluxo resumido

1. Ordem de serviço é criada no `MS-ORDER`.
2. Evento de avanço de etapa é publicado para continuidade do fluxo.
3. `MS-PAYMENT` processa solicitação e publica status de pagamento.
4. Em caso de aprovação, ordem segue para produção (`MS-PRODUCTION`).
5. `MS-PRODUCTION` publica atualizações de execução.
6. Em caso de falha em etapa crítica, evento de **compensação** (`compensate`) é disparado para rollback lógico do processo.

---

## Atendimento dos requisitos do enunciado

| Requisito da Fase 4 | Status | Evidência no projeto |
|---|---|---|
| Separação em pelo menos 3 microsserviços | **Atendido** | Repositórios `MS-ORDER`, `MS-PAYMENT`, `MS-PRODUCTION` |
| Comunicação entre serviços sem acesso direto ao banco de outro serviço | **Atendido** | Integração via RabbitMQ e APIs |
| Implementação de Saga Pattern com compensação | **Atendido** | Eventos de saga e estratégia de compensação no fluxo de OS |
| Integração de pagamentos (Mercado Pago) | **Atendido** | Integração implementada no `MS-PAYMENT` |
| Deploy em Kubernetes | **Atendido** | Manifestos `k8s/` por microsserviço |
| Testes unitários por serviço | **Atendido** | Estrutura de testes nos serviços e execução via Jest |
| BDD com ao menos um fluxo completo | **Parcial / Evolução** | Recomendado consolidar cenário BDD ponta a ponta entre serviços |
| Cobertura mínima de 80% por serviço | **Atendido** | Cobertura na data de entrega estava acima de 90% |
| Qualidade em CI | **Parcial / Evolução** | Estrutura de CI presente; recomendada formalização do gate de qualidade |

---

## Entregáveis e evidências

Conforme o enunciado da fase, os entregáveis foram organizados em torno dos microsserviços e da documentação:

- código-fonte dos serviços;
- `Dockerfile` por serviço;
- manifestos Kubernetes por serviço (`k8s/`);
- documentação de fluxo de pagamento e mensageria (ex.: diagrama no `MS-PAYMENT`);
- endpoints e documentação técnica via Swagger em cada aplicação NestJS.

## Compensação de pagamento na saga

Foi adicionada uma documentação específica da compensação de saga para falhas de pagamento no `MS-PAYMENT`:

- [SAGA-COMPENSACAO-PAGAMENTO.md](SAGA-COMPENSACAO-PAGAMENTO.md)

---

**Última atualização:** Fevereiro de 2026
