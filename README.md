# Bank Account - Event Sourcing & CQRS

Sistema de gerenciamento de contas bancárias implementado utilizando os padrões **Event Sourcing** e **CQRS** (Command Query Responsibility Segregation).

## Sobre o Projeto

Este projeto demonstra a implementação prática de Event Sourcing e CQRS em um contexto bancário, onde cada operação (depósito, saque, abertura/fechamento de conta) é armazenada como um evento imutável, permitindo reconstruir o estado atual e histórico completo de qualquer conta.

### Principais Características

- **Event Sourcing**: Todas as mudanças de estado são armazenadas como sequência de eventos
- **CQRS**: Separação clara entre comandos (escrita) e consultas (leitura)
- **Mensageria Assíncrona**: Comunicação entre serviços via Apache Kafka
- **Microservices**: Arquitetura distribuída com serviços independentes

## Stack Tecnológica

- **Java 17**
- **Spring Boot 2.4.5**
- **Apache Kafka** - Event streaming
- **MongoDB** - Event store e read database
- **Maven** - Gerenciamento de dependências
- **Lombok** - Redução de boilerplate
- **Docker Compose** - Orquestração de containers

## Arquitetura

```
├── cqrs.core/          # Framework CQRS/ES reutilizável
├── account.common/     # Eventos e DTOs compartilhados
├── account.cmd/        # Command API (write side)
└── account.query/      # Query API (read side)
```

### Fluxo de Dados

1. **Command Side**: Recebe comandos, valida, gera eventos e publica no Kafka
2. **Event Store**: Persiste eventos no MongoDB
3. **Event Bus**: Kafka distribui eventos para consumidores
4. **Query Side**: Consome eventos e atualiza modelos de leitura otimizados

## Pré-requisitos

- Java 17+
- Maven 3.6+
- Docker e Docker Compose

## Como Executar

### 1. Iniciar Infraestrutura (Kafka + Zookeeper)

```bash
docker-compose up -d
```

### 2. Compilar o Projeto

```bash
cd bank-account/bank-account
mvn clean install
```

### 3. Executar Command API

```bash
cd account.cmd
mvn spring-boot:run
```

### 4. Executar Query API

```bash
cd account.query
mvn spring-boot:run
```

## Comandos Disponíveis

- **OpenAccountCommand**: Abrir nova conta bancária
- **DepositFundsCommand**: Depositar valores
- **WithdrawFundsCommand**: Sacar valores
- **CloseAccountCommand**: Encerrar conta

## Eventos do Sistema

- **AccountOpenedEvent**: Conta criada com sucesso
- **FundsDepositedEvent**: Depósito realizado
- **FundsWithdrawnEvent**: Saque realizado
- **AccountClosedEvent**: Conta encerrada

## Estrutura do Projeto

```
java-event-sourcing/
│
├── docker-compose.yml              # Kafka + Zookeeper setup
│
└── bank-account/
    ├── cqrs-es/
    │   └── cqrs.core/             # Framework base CQRS/ES
    │
    └── bank-account/
        ├── account.common/         # Eventos e tipos compartilhados
        ├── account.cmd/           # API de comandos (write)
        └── account.query/         # API de consultas (read)
```


