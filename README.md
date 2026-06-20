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
- Deep DDD
- Vertical Slice mastery
- Every MediatR pipeline behavior

#### Those are nice-to-have. Nothing else.

---

# Domain 1 — C# Fundamentals

## Goal

Never fail a basic C# question.

### Topics

* Value Type vs Reference Type
* class vs struct
* ref vs out vs in
* abstract class vs interface
* string vs StringBuilder
* var vs dynamic
* const vs readonly
* boxing vs unboxing
* IEnumerable vs IQueryable
* IEnumerable vs List
* LINQ
* Generics
* Delegates
* Action / Func / Predicate
* async/await
* Task vs Thread
* Task.Run
* CancellationToken
* Exception Handling
* Garbage Collection
* Stack vs Heap

---

# Domain 2 — ASP.NET Core

## Goal

Defend backend experience confidently.

### Topics

* Middleware Pipeline
* Dependency Injection
* Singleton
* Scoped
* Transient
* IActionResult vs ActionResult<T>
* Model Binding
* Filters
* Global Exception Handling
* Configuration
* Options Pattern
* Health Checks
* BackgroundService
* Hosted Service
* Caching
* Rate Limiting
* CORS
* REST APIs
* API Versioning
* Swagger
* SignalR
* Authentication
* Authorization

---

# Domain 3 — Entity Framework Core

### Topics

* DbContext Lifetime
* Migrations
* Tracking vs NoTracking
* First vs FirstOrDefault
* Single vs SingleOrDefault
* Eager Loading
* Lazy Loading
* Explicit Loading
* AsNoTracking
* Transactions
* Optimistic Concurrency
* Indexes
* Query Performance
* Raw SQL
* ToList vs ToListAsync
* N+1 Problem

---

# Domain 4 — SQL

### Topics

* Primary Key
* Foreign Key
* Unique Key
* Index
* Clustered Index
* Non-Clustered Index
* INNER JOIN
* LEFT JOIN
* RIGHT JOIN
* GROUP BY
* HAVING
* UNION
* UNION ALL
* DELETE
* TRUNCATE
* VIEW
* Stored Procedure
* Normalization
* Transactions
* ACID
* Deadlock

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

### Topics

* What is Microservice
* Monolith vs Microservice
* Synchronous Communication
* Asynchronous Communication
* REST
* gRPC
* RabbitMQ
* Kafka
* Message Broker
* Producer
* Consumer
* Queue
* Exchange
* Topic
* Pub/Sub
* Dead Letter Queue
* Retry
* Idempotency
* SAGA Pattern
* Compensating Transaction
* Eventual Consistency
* Outbox Pattern
* API Gateway
* YARP
* Circuit Breaker
* Distributed Tracing

---

# Domain 8 — Docker

### Topics

* Container
* Image
* Dockerfile
* Docker Registry
* Volumes
* Networks
* docker build
* docker run
* docker ps
* docker logs
* docker exec
* docker stop
* docker compose
* Multi-stage Builds

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

# Priority Order

1. C# Fundamentals
2. ASP.NET Core
3. React
4. JavaScript
5. EF Core
6. SQL
7. Microservices
8. Docker
9. Kubernetes
10. Azure
11. Leadership

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
