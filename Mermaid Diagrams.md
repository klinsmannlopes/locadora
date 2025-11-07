# Mermaid Diagrams - Guia de Referência Rápida

Este documento contém exemplos de todos os tipos de diagramas Mermaid úteis para documentação técnica(utils).

---

## 1. Flowchart (Fluxograma)

**Uso:** Fluxos de decisão, lógica de negócio, processos

```mermaid
flowchart TD
    Start([Início]) --> Input[Receber Requisição]
    Input --> Auth{Usuário Autenticado?}
    Auth -->|Sim| Process[Processar Request]
    Auth -->|Não| Error401[Retornar 401]
    Process --> Valid{Dados Válidos?}
    Valid -->|Sim| Save[Salvar no Banco]
    Valid -->|Não| Error400[Retornar 400]
    Save --> Success[Retornar 200]
    Success --> End([Fim])
    Error401 --> End
    Error400 --> End
```

**Direções disponíveis:**

- `TD` (Top Down - de cima para baixo)
- `LR` (Left to Right - esquerda para direita)
- `RL` (Right to Left)
- `BT` (Bottom to Top)

---

## 2. Sequence Diagram (Diagrama de Sequência)

**Uso:** Interações entre sistemas, APIs, comunicação entre serviços

```mermaid
sequenceDiagram
    participant C as Cliente
    participant API as API Gateway
    participant Auth as Auth Service
    participant DB as Database
    participant Queue as Message Queue
    
    C->>API: POST /checkout
    API->>Auth: Validar Token
    Auth-->>API: Token Válido
    
    API->>DB: Buscar Produtos
    DB-->>API: Lista de Produtos
    
    API->>DB: Criar Pedido
    DB-->>API: Pedido Criado
    
    API->>Queue: Publicar Evento
    Queue-->>API: ACK
    
    API-->>C: 201 Created
    
    Note over Queue: Processamento assíncrono
    Queue->>EmailService: Enviar Email
```

**Tipos de setas:**

- `->` Linha sólida sem seta
- `-->` Linha tracejada sem seta
- `->>` Linha sólida com seta
- `-->>` Linha tracejada com seta (resposta)
- `-x` Linha com X no final
- `-)` Linha assíncrona

---

## 3. Class Diagram (Diagrama de Classes)

**Uso:** Modelagem orientada a objetos, estrutura de classes

```mermaid
classDiagram
    class User {
        -int id
        -string email
        -string password
        -datetime createdAt
        +login() bool
        +logout() void
        +updateProfile() void
    }
    
    class Admin {
        -array permissions
        +deleteUser(userId) void
        +banUser(userId) void
        +viewLogs() array
    }
    
    class Product {
        -int id
        -string name
        -decimal price
        -int stock
        +updateStock(quantity) void
        +calculateDiscount() decimal
    }
    
    class Order {
        -int id
        -datetime createdAt
        -string status
        -decimal total
        +addItem(product) void
        +calculateTotal() decimal
        +checkout() bool
    }
    
    User <|-- Admin : herança
    User "1" --> "*" Order : faz
    Order "1" --> "*" Product : contém
```

**Relacionamentos:**

- `<|--` Herança
- `*--` Composição
- `o--` Agregação
- `-->` Associação
- `--` Link
- `..>` Dependência
- `..|>` Realização

**Visibilidade:**

- `+` Public
- `-` Private
- `#` Protected
- `~` Package

---

## 4. Entity Relationship Diagram (Diagrama ER)

**Uso:** Modelagem de banco de dados, relacionamentos entre tabelas

```mermaid
erDiagram
    USER ||--o{ ORDER : places
    USER {
        uuid id PK
        string email UK
        string password
        string name
        datetime created_at
        datetime updated_at
    }
    
    ORDER ||--|{ ORDER_ITEM : contains
    ORDER {
        uuid id PK
        uuid user_id FK
        decimal total
        string status
        datetime created_at
    }
    
    PRODUCT ||--o{ ORDER_ITEM : includes
    PRODUCT {
        uuid id PK
        string name
        text description
        decimal price
        int stock
        uuid category_id FK
    }
    
    ORDER_ITEM {
        uuid id PK
        uuid order_id FK
        uuid product_id FK
        int quantity
        decimal unit_price
    }
    
    CATEGORY ||--o{ PRODUCT : categorizes
    CATEGORY {
        uuid id PK
        string name
        string slug UK
    }
    
    USER ||--o{ REVIEW : writes
    PRODUCT ||--o{ REVIEW : receives
    REVIEW {
        uuid id PK
        uuid user_id FK
        uuid product_id FK
        int rating
        text comment
        datetime created_at
    }
```

**Cardinalidade:**

- `||--||` Um para um
- `||--o{` Um para zero ou muitos
- `||--|{` Um para um ou muitos
- `}o--o{` Zero ou muitos para zero ou muitos

---

## 5. State Diagram (Diagrama de Estados)

**Uso:** Ciclo de vida de entidades, máquina de estados

```mermaid
stateDiagram-v2
    [*] --> Rascunho
    
    Rascunho --> Pendente : submit()
    Rascunho --> Cancelado : cancel()
    
    Pendente --> EmAnalise : approve()
    Pendente --> Rejeitado : reject()
    Pendente --> Cancelado : cancel()
    
    EmAnalise --> Aprovado : finalApprove()
    EmAnalise --> Pendente : requestChanges()
    EmAnalise --> Rejeitado : reject()
    
    Aprovado --> EmProcessamento : process()
    
    EmProcessamento --> Concluido : complete()
    EmProcessamento --> Falhou : error()
    
    Falhou --> EmProcessamento : retry()
    Falhou --> Cancelado : cancel()
    
    Concluido --> [*]
    Cancelado --> [*]
    Rejeitado --> [*]
    
    note right of EmAnalise
        Aguardando revisão
        do time de compliance
    end note
```

---

## 6. Gantt Chart (Cronograma)

**Uso:** Planejamento de sprints, roadmaps, timelines de projeto

```mermaid
gantt
    title Roadmap Q1 2025
    dateFormat YYYY-MM-DD
    
    section Infraestrutura
    Setup Kubernetes           :done, infra1, 2025-01-01, 15d
    CI/CD Pipeline            :done, infra2, 2025-01-10, 10d
    Monitoring (Grafana)      :active, infra3, 2025-01-20, 12d
    
    section Backend
    API REST v1               :done, back1, 2025-01-05, 20d
    Autenticação JWT          :done, back2, 2025-01-15, 10d
    Integração Payment        :active, back3, 2025-01-25, 15d
    WebSocket Real-time       :back4, 2025-02-05, 10d
    
    section Frontend
    Setup React + Vite        :done, front1, 2025-01-10, 5d
    Design System             :active, front2, 2025-01-15, 20d
    Dashboard Admin           :front3, 2025-02-01, 15d
    App Mobile (React Native) :front4, 2025-02-10, 30d
    
    section Testes & QA
    Testes Unitários          :crit, test1, 2025-02-01, 10d
    Testes E2E                :crit, test2, 2025-02-08, 12d
    Load Testing              :test3, 2025-02-15, 5d
```

---

## 7. Git Graph (Fluxo Git)

**Uso:** Documentar estratégia de branching, fluxo de trabalho Git

```mermaid
gitGraph
    commit id: "Initial commit"
    commit id: "Setup project structure"
    
    branch develop
    checkout develop
    commit id: "Add base configs"
    
    branch feature/auth
    checkout feature/auth
    commit id: "Add JWT service"
    commit id: "Add login endpoint"
    commit id: "Add tests"
    
    checkout develop
    merge feature/auth
    
    branch feature/products
    checkout feature/products
    commit id: "Add product model"
    commit id: "Add CRUD endpoints"
    
    checkout develop
    branch feature/orders
    checkout feature/orders
    commit id: "Add order service"
    
    checkout develop
    merge feature/products
    merge feature/orders
    
    checkout main
    merge develop tag: "v1.0.0"
    
    checkout develop
    commit id: "Hotfix: security patch"
```

---

## 8. Pie Chart (Gráfico de Pizza)

**Uso:** Distribuição percentual, métricas

```mermaid
pie title Distribuição de Tecnologias no Projeto
    "TypeScript" : 45
    "Python" : 25
    "Go" : 15
    "SQL" : 10
    "Shell Scripts" : 5
```

---

## 9. Journey Diagram (Jornada do Usuário)

**Uso:** User experience, fluxo de usuário

```mermaid
journey
    title Jornada do Usuário - Compra Online
    section Descoberta
      Buscar produto: 5: Cliente
      Ver detalhes: 4: Cliente
      Ler reviews: 3: Cliente
    section Decisão
      Adicionar ao carrinho: 5: Cliente
      Aplicar cupom: 4: Cliente
      Revisar pedido: 3: Cliente
    section Pagamento
      Preencher dados: 2: Cliente
      Escolher método pagamento: 3: Cliente
      Confirmar compra: 5: Cliente
    section Pós-compra
      Receber confirmação: 5: Cliente, Sistema
      Rastrear pedido: 4: Cliente, Sistema
      Receber produto: 5: Cliente
      Avaliar compra: 3: Cliente
```

---

## 10. Quadrant Chart

**Uso:** Priorização, análise de features

```mermaid
quadrantChart
    title Priorização de Features - Esforço vs Impacto
    x-axis Baixo Esforço --> Alto Esforço
    y-axis Baixo Impacto --> Alto Impacto
    quadrant-1 Fazer Depois
    quadrant-2 Prioridade Alta
    quadrant-3 Não Fazer
    quadrant-4 Ganhos Rápidos
    
    Autenticação OAuth: [0.8, 0.9]
    Dashboard Analytics: [0.7, 0.8]
    API Rate Limiting: [0.3, 0.9]
    Export CSV: [0.2, 0.4]
    Dark Mode: [0.3, 0.5]
    Notificações Push: [0.6, 0.7]
    Chat em Tempo Real: [0.9, 0.6]
    Busca Avançada: [0.5, 0.7]
```

---

## 11. Architecture Diagram (C4 Model Style)

**Uso:** Arquitetura de sistemas, visão macro

```mermaid
graph TB
    subgraph "Frontend Layer"
        Web[Web App - React]
        Mobile[Mobile App - React Native]
    end
    
    subgraph "API Gateway Layer"
        Gateway[Kong API Gateway]
    end
    
    subgraph "Microservices Layer"
        Auth[Auth Service]
        User[User Service]
        Product[Product Service]
        Order[Order Service]
        Payment[Payment Service]
    end
    
    subgraph "Data Layer"
        PG[(PostgreSQL)]
        Mongo[(MongoDB)]
        Redis[(Redis Cache)]
    end
    
    subgraph "Message Queue"
        Kafka[Apache Kafka]
    end
    
    subgraph "External Services"
        Stripe[Stripe API]
        Email[SendGrid]
        Storage[AWS S3]
    end
    
    Web --> Gateway
    Mobile --> Gateway
    
    Gateway --> Auth
    Gateway --> User
    Gateway --> Product
    Gateway --> Order
    
    Auth --> PG
    User --> PG
    Product --> Mongo
    Order --> PG
    
    Auth --> Redis
    Product --> Redis
    
    Order --> Kafka
    Payment --> Kafka
    
    Payment --> Stripe
    Order --> Email
    Product --> Storage
```

---

## 12. Mindmap

**Uso:** Brainstorming, estruturação de ideias

```mermaid
mindmap
  root((Sistema de E-commerce))
    Frontend
      Web App
        React
        Next.js
        Tailwind
      Mobile App
        React Native
        Expo
    Backend
      API Gateway
      Microservices
        Auth
        Products
        Orders
        Payments
      Database
        PostgreSQL
        MongoDB
        Redis
    DevOps
      CI/CD
        GitHub Actions
        ArgoCD
      Monitoring
        Grafana
        Prometheus
      Infrastructure
        Kubernetes
        AWS
    Security
      Authentication
        JWT
        OAuth2
      Authorization
        RBAC
      Encryption
        TLS
        At Rest
```

---

## Dicas Importantes

### Performance

- Evite diagramas muito grandes (máximo 50-60 nós)
- Para sistemas complexos, divida em múltiplos diagramas

### Estilo e Formatação

```mermaid
%%{init: {'theme':'dark'}}%%
graph LR
    A[Com tema dark]
```

### Links Clicáveis

```mermaid
graph LR
    A[Documentação]
    click A "https://mermaid.js.org" "Abrir docs"
```

### Comentários

```mermaid
graph TD
    %% Este é um comentário
    A[Início] --> B[Fim]
```

---

## Template para RFCs

Use este template ao criar RFCs:

```markdown
# RFC-XXX: [Título]

## Status
- [ ] Em Discussão
- [ ] Aprovado
- [ ] Implementado
- [ ] Rejeitado

## Contexto
[Descreva o problema ou necessidade]

## Proposta

### Arquitetura Atual
[Diagrama Mermaid da arquitetura atual]

### Arquitetura Proposta
[Diagrama Mermaid da nova arquitetura]

### Fluxo de Dados
[Sequence diagram do fluxo]

## Alternativas Consideradas
1. [Alternativa 1]
2. [Alternativa 2]

## Decisões
- [Decisão 1]
- [Decisão 2]

## Impacto
- **Performance**: 
- **Segurança**: 
- **Custo**: 

## Riscos
- [Risco 1]

## Timeline
[Gantt chart]

## Referências
- [[Link para ADR relacionado]]
- [[Link para documentação]]
```

---

**Última atualização:** 2025-01-06