# Senior .NET + React Interview Preparation Roadmap

# Rules

1. No new certifications until employed.
2. No second side project.
3. No changing roadmap every week.
4. No learning because of FOMO.
5. A live interview always takes priority.
6. Do not move to the next domain until comfortable with the current one.

---

# Daily Schedule

## Morning (6:00 AM - 8:00 AM)

### Job Search

Daily Tasks:

* Apply to 5 targeted jobs
* Send 5 recruiter messages
* Reply to recruiters
* Track applications in spreadsheet

**Never skip this.**

---

## Afternoon (3:00 PM - 5:00 PM)

### Interview Preparation

#### Monday

* C# Fundamentals
* ASP.NET Core
* EF Core

#### Tuesday

* React
* JavaScript

#### Wednesday

* Microservices
* RabbitMQ
* Kafka
* SAGA Pattern

#### Thursday

* Docker
* Kubernetes
* Azure

#### Friday

* Leadership
* Azure DevOps
* Behavioral Questions
* Mock Interviews

#### Saturday

* Weak Areas Only

#### Sunday

* Revision Only

---

## Evening (9:00 PM - 10:00 PM)

### Learning Goal

* Microservices Course (Do not aim to finish the course).

#### Aim to learn:
- Docker
- Docker Compose
- RabbitMQ
- MassTransit basics
- Microservice communication
- SAGA
- Outbox pattern
- Eventual consistency
- Kubernetes
- YARP
- gRPC

#### You don't need:
- Marten
- Carter
- Deep DDD mastery
- Vertical Slice mastery
- Every MediatR pipeline behavior

#### Those are nice-to-have. Nothing else.

---

# Topic Priority Legend

- **[High]** — Must know for interviews. Learn and practice first.
- **[Medium]** — Learn after the High-priority topics are comfortable.

---

# Domain Revision Files

- [Domain 1 — C# Fundamentals](domains/01-csharp-fundamentals.md)
- [Domain 2 — ASP.NET Core](domains/02-aspnet-core.md)
- [Domain 4 — SQL](domains/04-sql.md)
- [Domain 5 — JavaScript](domains/05-javascript.md)
- [Domain 6 — React](domains/06-react.md)
- [Domain 7 — Microservices](domains/07-microservices.md)
- [Domain 12 — Project & Behavioral Stories (STAR)](domains/12-project-stories.md)

---

# Domain 1 — C# Fundamentals

## Goal

Never fail a basic C# question.

### Topics

* [High] Value Type vs Reference Type
* [High] class vs struct
* [High] ref vs out vs in
* [High] abstract class vs interface
* [High] string vs StringBuilder
* [High] var vs dynamic
* [High] const vs readonly
* [High] boxing vs unboxing
* [High] IEnumerable vs IQueryable
* [High] IEnumerable vs List
* [High] LINQ
* [High] Generics
* [High] Delegates
* [High] Action / Func / Predicate
* [High] async/await
* [High] Task vs Thread
* [High] Task.Run
* [High] CancellationToken
* [High] Exception Handling
* [High] Garbage Collection
* [High] Stack vs Heap
* [Medium] .NET 8 and C# 12 interview-relevant features
* [Medium] Records
* [Medium] Primary constructors
* [High] Pattern matching
* [High] Nullable reference types
* [Medium] Required members

---

# Domain 2 — ASP.NET Core

## Goal

Defend backend experience confidently.

### Topics

* [High] Middleware Pipeline
* [High] Dependency Injection
* [High] Singleton
* [High] Scoped
* [High] Transient
* [High] IActionResult vs ActionResult<T>
* [High] Model Binding
* [High] Filters
* [High] Global Exception Handling
* [High] Configuration
* [High] Options Pattern
* [High] Health Checks
* [High] BackgroundService
* [High] Hosted Service
* [High] Caching
* [High] Rate Limiting
* [High] CORS
* [High] REST APIs
* [High] API Versioning
* [Medium] Swagger / OpenAPI
* [High] SignalR
* [High] Authentication
* [High] Authorization
* [High] ASP.NET Core Minimal APIs
* [High] Minimal APIs vs Controllers
* [High] FluentValidation fundamentals
* [Medium] Centralized validation pipeline
* [Medium] Validation pipeline behaviors
* [High] IHttpClientFactory
* [Medium] Refit basics
* [High] Fixed-window rate limiting
* [Medium] Sliding-window rate limiting
* [Medium] Token-bucket rate limiting
* [High] Database health checks
* [High] Redis health checks
* [High] RabbitMQ health checks
* [High] Structured logging
* [High] Correlation IDs
* [High] Global exception handling with middleware or IExceptionHandler

---

# Domain 3 — Entity Framework Core

### Topics

* [High] DbContext Lifetime
* [High] Migrations
* [High] Tracking vs NoTracking
* [High] First vs FirstOrDefault
* [High] Single vs SingleOrDefault
* [High] Eager Loading
* [Medium] Lazy Loading
* [Medium] Explicit Loading
* [High] AsNoTracking
* [High] Transactions
* [High] Optimistic Concurrency
* [High] Indexes
* [High] Query Performance
* [Medium] Raw SQL
* [High] ToList vs ToListAsync
* [High] N+1 Problem
* [High] Code-First approach
* [High] IEntityTypeConfiguration<T>
* [High] Applying migrations during deployment
* [High] Risks of automatically migrating during application startup
* [Medium] SQL Server provider behavior
* [Medium] PostgreSQL provider behavior
* [Medium] SQLite provider behavior
* [Medium] Repository pattern with EF Core
* [Medium] EF Core inside Clean Architecture
* [Medium] Domain entities vs database entities

---

# Domain 4 — SQL & Data Storage

### Topics

* [High] Primary Key
* [High] Foreign Key
* [High] Unique Key
* [High] Index
* [High] Clustered Index
* [High] Non-Clustered Index
* [High] INNER JOIN
* [High] LEFT JOIN
* [Medium] RIGHT JOIN
* [High] GROUP BY
* [High] HAVING
* [High] UNION
* [High] UNION ALL
* [High] DELETE
* [High] TRUNCATE
* [Medium] VIEW
* [Medium] Stored Procedure
* [High] Normalization
* [High] Transactions
* [High] ACID
* [High] Deadlock
* [High] Relational vs document databases
* [Medium] PostgreSQL vs SQL Server vs SQLite
* [Medium] When SQLite is appropriate
* [High] Redis is not a primary relational database
* [High] Database-per-service pattern
* [High] Data ownership in microservices

---

# Domain 5 — JavaScript

## Goal

Become comfortable with JavaScript fundamentals.

### Topics

* var
* let
* const
* Hoisting
* Closures
* == vs ===
* null vs undefined
* Truthy/Falsy
* map
* filter
* find
* reduce
* Spread Operator
* Shallow Copy
* Deep Copy
* Promise
* async/await
* Event Loop
* setTimeout
* Microtask Queue
* Macrotask Queue
* this keyword
* Arrow Functions

---

# Domain 6 — React

### Topics

* JSX
* Props
* State
* Controlled Components
* useState
* useEffect
* useMemo
* useCallback
* useRef
* React.memo
* Context API
* Redux
* Custom Hooks
* Component Lifecycle
* Virtual DOM
* Keys
* List Rendering
* Forms
* Authentication
* Route Protection
* Performance Optimization

---

# Domain 7 — Microservices

## Goal

Understand distributed systems and communication.

### Core Architecture and Communication

* [High] What is a Microservice
* [High] Monolith vs Microservice
* [High] Synchronous Communication
* [High] Asynchronous Communication
* [High] REST
* [High] Message Broker
* [High] Producer
* [High] Consumer
* [High] Queue
* [High] Topic
* [High] Pub/Sub
* [High] Retry
* [High] Idempotency
* [High] SAGA Pattern
* [High] Compensating Transaction
* [High] Eventual Consistency
* [High] Outbox Pattern
* [High] Circuit Breaker
* [Medium] Distributed Tracing

### gRPC

* [High] gRPC fundamentals
* [High] REST vs gRPC
* [High] Protocol Buffers
* [High] Internal service-to-service communication
* [Medium] Unary calls vs streaming
* [Medium] gRPC service and client generation
* [High] When not to use gRPC

### RabbitMQ and MassTransit

* [High] RabbitMQ fundamentals
* [High] Exchange
* [High] RabbitMQ topic exchanges
* [High] Routing keys
* [High] Dead Letter Queue
* [High] MassTransit purpose
* [High] Why use MassTransit instead of the raw RabbitMQ client
* [High] Publishing vs sending
* [High] Consumer configuration
* [High] Endpoint and queue configuration
* [High] Retry policies
* [Medium] Message redelivery
* [Medium] Fault messages
* [High] Dead-letter and error queues
* [High] Shared message-contract library
* [High] Versioning integration-event contracts

### Events and Delivery Guarantees

* [High] Domain Events vs Integration Events
* [High] Event contracts
* [High] Event versioning
* [High] Consumer idempotency
* [High] Duplicate-message handling
* [High] At-least-once delivery
* [High] Transactional Outbox

### Kafka

* [High] Kafka fundamentals
* [High] Kafka vs RabbitMQ
* [Medium] Topics, partitions, offsets, and consumer groups
* [Medium] Event retention and replay

### API Gateway and YARP

* [High] API Gateway
* [High] Gateway routing pattern
* [High] YARP fundamentals
* [High] Routes
* [High] Clusters
* [High] Destinations
* [Medium] Path transforms
* [Medium] Header transforms
* [High] Gateway rate limiting
* [High] Authentication at the gateway vs service
* [High] API Gateway vs load balancer
* [High] API Gateway failure risks

---

# Domain 8 — Docker

### Topics

* [High] Container
* [High] Image
* [High] Dockerfile
* [Medium] Docker Registry
* [High] Volumes
* [High] Networks
* [High] docker build
* [High] docker run
* [High] docker ps
* [High] docker logs
* [High] docker exec
* [High] docker stop
* [High] docker compose
* [High] Multi-stage Builds
* [High] Docker Compose for multiple services
* [High] Containerizing databases, Redis, and RabbitMQ
* [High] Docker Compose service dependencies
* [High] Container networking and DNS by service name
* [High] Environment-variable overrides
* [Medium] .env files
* [High] Health checks in Docker Compose
* [High] Persistent volumes for databases
* [High] Startup ordering vs actual readiness
* [Medium] Development Compose vs production orchestration

---

# Domain 9 — Kubernetes

### Topics

* Pod
* Deployment
* ReplicaSet
* Service
* Ingress
* Namespace
* ConfigMap
* Secret
* Persistent Volume
* Horizontal Pod Autoscaler
* Rolling Update
* Self Healing

### Commands

```bash
kubectl get pods

kubectl get deployments

kubectl get svc

kubectl logs POD

kubectl describe pod POD

kubectl exec -it POD -- sh

kubectl delete pod POD

kubectl apply -f deployment.yaml
```

---

# Domain 10 — Azure / Cloud

### Topics

* Azure App Service
* Azure Key Vault
* Secret Rotation
* Azure Storage
* Blob Storage
* Azure SQL
* Managed Identity
* Azure Kubernetes Service (AKS)
* Azure Entra ID
* Azure Redis Cache
* Application Insights
* Monitoring
* Logging
* CI/CD

---

# Domain 11 — Leadership / Behavioral

## Goal

Defend seniority and ownership.

### Topics

* Sprint Planning
* Epics
* Features
* User Stories
* Tasks
* Bugs
* Azure DevOps Boards
* Velocity
* Burndown Chart
* Risk Management
* Stakeholder Communication
* Code Reviews
* Mentoring
* Conflict Resolution
* Production Incident Handling
* Project Ownership
* On-time Delivery
* Team Leadership

---

# Domain 12 — Project & Behavioral Stories (STAR)

[Open the project and behavioral STAR stories](domains/12-project-stories.md)

---

# Domain 13 — Architecture, DDD, CQRS & Design Patterns

## Goal

Understand the architecture concepts used in modern .NET microservice systems without overengineering.

### Architecture Styles

* [High] Layered / N-Layer Architecture
* [High] Clean Architecture
* [High] Hexagonal Architecture fundamentals
* [High] Vertical Slice Architecture
* [Medium] Feature-folder organization
* [High] Dependency Rule
* [High] Core, Application, Infrastructure, and Presentation layers
* [High] Clean Architecture vs Vertical Slice Architecture
* [High] When architecture becomes overengineering

### CQRS and MediatR

* [High] Command vs Query
* [High] Command handlers
* [High] Query handlers
* [High] MediatR request and handler flow
* [High] Pipeline behaviors
* [High] Validation behavior
* [Medium] Logging behavior
* [Medium] Transaction behavior
* [High] CQRS without separate databases
* [High] When CQRS is unnecessary

### Domain-Driven Design

* [High] Entity
* [High] Value Object
* [High] Aggregate
* [High] Aggregate Root
* [Medium] Domain Service
* [High] Application Service
* [High] Repository
* [High] Bounded Context
* [Medium] Ubiquitous Language
* [High] Domain Event
* [High] Integration Event
* [High] Anemic domain model vs rich domain model

### Design Patterns

* [Medium] Proxy Pattern
* [High] Decorator Pattern
* [High] Cache-Aside Pattern
* [High] Repository Pattern
* [High] Strategy Pattern
* [High] Factory / Resolver Pattern
* [High] Dependency Inversion
* [High] SOLID in real applications

---

# Priority Order

1. C# Fundamentals
2. ASP.NET Core
3. React
4. JavaScript
5. EF Core
6. SQL & Data Storage
7. Microservices
8. Architecture, DDD, CQRS & Design Patterns
9. Docker
10. Kubernetes
11. Azure
12. Leadership
13. Project & Behavioral Stories

---

# Side Project Rule

Allowed:

**AI Task Manager**

Stack:

* .NET
* React
* OpenAI
* Redis
* Docker

Purpose:

Demonstrate AI integration publicly.

After completion:

**Stop and focus on interviews.**

---

# Final Reminder

You are not trying to become:

* Architect
* Staff Engineer
* Kubernetes Expert
* Kafka Expert
* AI Researcher

right now.

You are trying to become:

> Senior Full Stack Engineer (.NET + React)
>
> who can confidently clear interviews.

That is already within reach.