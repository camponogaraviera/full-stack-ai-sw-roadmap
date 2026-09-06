<div align='center'>
    <h1> 3.3 Backend </h1>
    <h2> 3.3.1 System Architecture Styles & Patterns </h2>
    <h3> Data Patterns for Distributed Systems </h3>
</div>

# Table of Contents

- [Saga Pattern](#saga-pattern)
  - [Serverless Implementation](#serverless-implementation)
- [Database-Per-Service Pattern](#database-per-service-pattern)
- [Shared-Database-Per-Service Anti-Pattern](#shared-database-per-service-anti-pattern)
- [CQRS Pattern](#cqrs-pattern)
- [Event Sourcing Pattern](#event-sourcing-pattern)

---

# Saga Pattern

The [saga pattern](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/saga.html) is a failure management pattern used to manage long-lived distributed transactions across multiple microservices, ensuring consistency through a series of `compensating actions if a failure occurs`. It avoids the complexity and overhead of traditional distributed transactions that rely on protocols such as [Two-Phase Commit (2PC)]() and [Three-Phase Commit (3PC)](). Instead of using a single atomic transaction across multiple services, the Saga pattern `breaks down a distributed transaction into a series of smaller, coordinated local transactions`. In this way, each step can either be completed successfully or be compensated (reverted to a previous step) if a transaction fails at some step in the workflow, maintaining data consistency. Each step then becomes ACID-compliant at the local level.

In a [microservices architecture](https://aws.amazon.com/microservices/#:~:text=Microservices), where each service is decoupled (isolated) and self-contained with its own data persistence layer (a `database-per-service pattern`), a single/local ACID transaction cannot span multiple services because each service manages its own database. Attempting to use a local ACID transaction across multiple services can result in partial distributed transactions, where only some operations succeed while others fail, potentially leading to data inconsistencies. The saga pattern is used to prevent these issues from happening.

Transactions can be coordinated in two ways:

1. `Choreography-based Saga:` A local transaction triggers another local transaction. Microservices communicate via events (rather than direct calls) using services such as AWS EventBridge, SQS, and SNS. Other services subscribe (listen) to those events to trigger actions. [AWS AppSync](https://github.com/camponogaraviera/aws/blob/main/services/aws_api.md) supports [GraphQL](https://github.com/camponogaraviera/full-stack-roadmap/blob/main/backend/api/grahql.md) resolvers to connect GraphQL operations (queries, mutations, and subscriptions) to services. For example, GraphQL subscriptions can push real-time updates to clients (usually via WebSocket or another transport protocol) when mutations (e.g., data changes and transactions) occur in the backend. While AppSync handles client-facing real-time updates, event-driven services like EventBridge, SNS, or SQS are better suited for backend microservice communication. Use case: a restaurant "Reservation Confirmed" event triggers a payment, then a "Payment Processed" event triggers a notification.
   - [EventBridge](https://github.com/camponogaraviera/aws/blob/main/services/aws_integration.md) can trigger compensating actions by routing events to appropriate handlers (functions). It is also useful for decoupling microservices, ensuring that failures in one service do not directly impact another.
   - [SQS](https://github.com/camponogaraviera/aws/blob/main/services/aws_messaging.md) supports retrying failed steps using [dead-letter queues (DLQs)](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html).
   - [SNS](https://github.com/camponogaraviera/aws/blob/main/services/aws_messaging.md) is typically used for broadcasting events from one service to multiple subscribers or services.

2. `Orchestration-based Saga:` a central orchestrator (e.g., [AWS Step Functions](https://github.com/camponogaraviera/aws/blob/dev/services/aws_integration.md#aws-step-functions)) coordinates the entire workflow, i.e., the sequence of steps in the saga, by calling each microservice and ensuring each step is completed before moving to the next. Step Functions can trigger [AWS Lambda functions](https://github.com/camponogaraviera/aws/blob/main/services/aws_compute_deployment.md#aws-lambda) directly or interact with [AWS AppSync](https://github.com/camponogaraviera/aws/blob/main/services/aws_api.md#aws-appsync) APIs indirectly (via API Gateway) to run code in response to events (e.g., user registration, payment transactions, DynamoDB table updates, etc.).

- Notes:
  - A local transaction is a sequence of database operations performed as a single unit within a single database. This ensures that all operations are successfully completed (committed) or none of them are (rolled back) if an error occurs, maintaining ACID compliance.
  - A distributed transaction is a sequence of coordinated database operations that span (are performed on) multiple databases, often located across different servers. These transactions can be coordinated with protocols such as Two-Phase Commit (2PC) and Three-Phase Commit (3PC).
  - A step is a single unit of work within a distributed transaction. Each step in the saga is typically a call to a service (a Lambda function or another AWS service) that performs a specific action, optionally with a compensating transaction if a failure occurs.

## Examples

Use a SAGA pattern:

1. If you have multiple distributed microservices that must maintain consistency:
   - Reservation service.
   - Payment service (e.g., Stripe).
   - Calendar service (e.g., Google Calendar integration).
   - Notification service.

2. If there are long-lived transactions, and you don't want microservices to be blocked when a microservice runs for a long time.
3. If you need complex rollback operations in case they fail:
   - Cancel a reservation: if an error occurred or the user decides.
   - Refund payments: if a reservation was canceled, the payment should be reversed.
   - Release calendar slots: if payment or another step fails, the reserved time slot should be freed.
   - Notify multiple parties: ensure users and service providers are informed about any changes (success or failure).

## Serverless Implementation

The saga workflow can be implemented using serverless components on AWS. `AWS API Gateway`, `AWS Step Functions`, `AWS Lambda`, and `Amazon DynamoDB` can be orchestrated to implement the saga pattern.

Use case: [Implement the serverless saga pattern by using AWS Step Functions](https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/implement-the-serverless-saga-pattern-by-using-aws-step-functions.html).

---

# Database-Per-Service Pattern

Using the database-per-service pattern in a microservices architecture ensures that each service can manage its own database independently while still participating in a coordinated workflow. This allows for better isolation and scalability.

In [this example](https://docs.aws.amazon.com/prescriptive-guidance/latest/modernization-data-persistence/database-per-service.html), a system with three microservices (sales, customer, compliance) uses one dedicated database for each service to align with the database-per-service pattern. There are no restrictions on the choice of the database. Different microservices can use different databases (SQL or NoSQL) depending on the service's requirements.

---

# Shared-Database-Per-Service Anti-Pattern

In this pattern, multiple services share the same database instance or schema, even if each service primarily uses its own tables or collections. The problem is shared ownership of the data layer across service boundaries, which creates tight coupling and hidden dependencies between services.

In [this example](https://docs.aws.amazon.com/prescriptive-guidance/latest/modernization-data-persistence/shared-database.html), three services each have their own tables (sales, customer, and compliance) within the same RDS database. Although the services may conceptually own different data, they still depend on the same underlying database infrastructure and can potentially access or modify each other's data. Changes to the database schema, access patterns, or configuration can therefore affect multiple services.

The anti-pattern is not specific to relational databases. Sharing a database instance or schema can create coupling with both relational and NoSQL databases. Features such as JOINs and foreign keys can make cross-service dependencies more explicit, but their presence is not what defines the anti-pattern.

---

# CQRS Pattern

The Command Query Responsibility Segregation (CQRS) pattern separates write models from read models, often using different representations or data stores. It is frequently combined with Event Sourcing, where state changes are persisted as an append-only sequence of events rather than only storing the current state. CQRS and Event Sourcing are distinct distributed-systems patterns, and CQRS can be implemented using mechanisms such as DynamoDB Streams to asynchronously update read models.

---

# Event Sourcing Pattern
