# RFC-001: Migração para Event-Driven Architecture

## Contexto
Precisamos escalar nosso sistema...

## Arquitetura Proposta
```mermaid
graph LR
    A[API] --> B[Message Broker]
    B --> C[Service 1]
    B --> D[Service 2]
    B --> E[Service 3]
```

## Decisões
- Usar Kafka como message broker
- [[Event Sourcing]] pattern