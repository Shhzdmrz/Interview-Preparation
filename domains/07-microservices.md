# Microservices Revision Notes

## SAGA Pattern

# Question

In a microservice workflow, Order is created, Payment succeeds, but Inventory reservation fails. What should happen, and what pattern is commonly used?

# Senior-Level Answer

Payment should be refunded through a compensating transaction.

In microservices, each service usually owns its own database, so we cannot rely on one ACID transaction across Order, Payment, and Inventory services. Instead, each service performs a local transaction and publishes events for the next step.

If a later step fails, previous successful steps are undone through business-level compensating actions. This approach is called the SAGA pattern.

SAGA provides eventual consistency, not immediate ACID consistency.

# Example

```text
1. Order Service creates order
2. Payment Service charges payment
3. Inventory Service fails to reserve stock
4. Payment Service refunds payment
5. Order Service marks order as cancelled
```

# 15-Second Version

Use the SAGA pattern. Each service performs a local transaction. If a later step fails, compensating transactions run, such as refunding payment. SAGA is eventually consistent, not ACID.

---

## ACID vs Eventual Consistency

# Question

Is SAGA an ACID transaction?

# Senior-Level Answer

No. SAGA is not an ACID transaction across services.

ACID transactions usually apply within a single database transaction where all operations succeed or fail together.

In a microservices architecture, each service has its own database and owns its own transaction. Because there is no single shared transaction across all services, the system uses eventual consistency.

SAGA coordinates multiple local transactions and uses compensating transactions to handle failures.

# Example

ACID example:

```text
One database transaction updates two tables.
If one update fails, the whole transaction rolls back.
```

SAGA example:

```text
Order Service creates order.
Payment Service charges payment.
Inventory Service fails.
Payment Service refunds payment.
Order Service cancels order.
```

# 15-Second Version

SAGA is not ACID. ACID is usually one database transaction. SAGA uses local transactions, events, compensating actions, and eventual consistency across services.

---

## Choreography vs Orchestration

# Question

What is the difference between SAGA choreography and orchestration?

# Senior-Level Answer

In choreography, there is no central coordinator. Services react to events from other services and publish their own events. It is decentralized and loosely coupled, but it can become harder to understand as workflows grow.

In orchestration, a central orchestrator controls the workflow. It tells each service what to do and decides what compensating action should run when a step fails. It is easier to monitor and reason about for complex workflows, but it introduces a central coordination component.

# Example

Choreography:

```text
OrderCreated event
Payment Service reacts
PaymentCompleted event
Inventory Service reacts
InventoryFailed event
Payment Service reacts and refunds
```

Orchestration:

```text
Saga Orchestrator
-> Create Order
-> Charge Payment
-> Reserve Inventory
-> If inventory fails, refund payment and cancel order
```

# 15-Second Version

Choreography means services react to events without a central controller. Orchestration means a central orchestrator controls the workflow and compensation steps.

---

## RabbitMQ Message Handling

# Question

If Order Service publishes an `OrderCreated` event but Email Service is down, will RabbitMQ lose the message?

# Senior-Level Answer

RabbitMQ will not lose the message if the queue and message are configured as durable and persistent.

A durable queue survives broker restart. A persistent message is stored to disk instead of only memory.

When the Email Service comes back online, it can consume the message from the queue.

The consumer should send an acknowledgement after successfully processing the message. If the consumer fails before acknowledging, RabbitMQ can redeliver the message.

If a message repeatedly fails, it can be moved to a Dead Letter Queue for later investigation or manual retry.

# Example

```text
Order Service
-> publishes OrderCreated
-> RabbitMQ stores message in durable queue
-> Email Service is down
-> Email Service comes back
-> consumes message
-> sends ACK after email is sent
```

# 15-Second Version

RabbitMQ will not lose the message if the queue is durable and the message is persistent. The consumer ACKs after success. Failed messages can be retried or sent to a Dead Letter Queue.

---

## RabbitMQ vs Kafka

# Question

What is the difference between RabbitMQ and Kafka?

# Senior-Level Answer

RabbitMQ is a traditional message broker. It is commonly used for task queues, reliable message delivery, routing, and background processing.

Kafka is a distributed event streaming platform. It is designed for high throughput, durable event logs, multiple consumers, replayability, analytics, and event-driven pipelines.

In RabbitMQ, messages are usually removed from the queue after successful consumption. In Kafka, events are retained for a configured period, and consumers track their own offsets.

# Example

RabbitMQ is a good fit for:

```text
Send email
Generate invoice
Process background job
Notify customer
```

Kafka is a good fit for:

```text
Audit logs
Analytics pipeline
Event sourcing
High-throughput event streaming
```

# 15-Second Version

RabbitMQ is a message broker for queues and task processing. Kafka is an event streaming platform for high-throughput, retained events, replay, and multiple consumers.
