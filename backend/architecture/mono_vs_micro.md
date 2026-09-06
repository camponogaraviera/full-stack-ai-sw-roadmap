<div align='center'>
    <h1> 3.3 Backend </h1>
    <h2> 3.3.1 System Architecture Styles & Patterns </h2>
    <h3> Code base/Deployment Architecture Styles </h3>
    <h4> Monolithic vs Microservices </h4>
</div>

# Table of Contents

- [Monolithic](#monolithic)
- [Microservices](#microservices)
- [FAQ](#faq)

---

# Monolithic

In a monolithic system, while the internal code base may be organized into separate modules, all application components (UI, business logic, and data access layers) are tightly coupled, i.e., deployed as a single logical unit (service). This single unit is usually packaged in a single container image, regardless of how many databases it connects to.

Pros:

- Easier to manage and deploy initially.

- The entire application and dependencies are installed in a single environment.

- All components are packaged, deployed, and versioned together.

- Performance can be better due to fewer network hops since internal function calls are faster than remote service calls in distributed systems.

Cons:

- A monolith can (and typically does in production) run as multiple cloned instances across multiple servers/AZs behind a load balancer. What it can't do is scale individual internal components independently. If one part fails (e.g., a bug or a memory leak), the whole application is affected.

- Difficult to scale. The entire application must be scaled (e.g., more CPU) even if only one part is experiencing high load.

- Harder to maintain and update over time as the codebase grows.

---

# Microservices

In a distributed architecture, each deployable service in an application is decoupled (isolated) and self-contained with its own data persistence layer/schema, which often translates to a [database-per-service pattern](https://docs.aws.amazon.com/prescriptive-guidance/latest/modernization-data-persistence/database-per-service.html).

Microservices are defined by service boundaries, independence, and data ownership. Microservices communicate over network boundaries, typically using APIs, rather than sharing data within the same codebase. While one can still package multiple microservices in a single container and still have separate databases for each service, this is usually not recommended and contradicts the core philosophy of microservices. The recommended best practice is to deploy each microservice in its own container image.

Pros:

- Easy to scale: Decoupling allows for horizontal scalability, as individual services can be scaled independently based on demand.

- Resilience: If one service fails, others can continue to function.

- Flexibility: Different technologies and languages can be used for different services.

- Easier to maintain and update specific parts without affecting the whole system.

Cons:

- More complex to manage and deploy due to the need for inter-service communication and coordination.

- Risk of increased latency due to network calls between services.

- Increased operational overhead: Requires robust monitoring, logging, and automated deployment strategies.

- Data consistency can be more challenging to maintain. Can be overcome with the Saga Pattern.

```bash
                  Next.js Application
                (Frontend + BFF layer)
                           |
              API Gateway / Internal APIs
                           |
        ---------------------------------------
                 |                   |
           Course Service     Payment Service
           (Node/Express)     (Node/Express)
                 |                   |
             Course DB          Stripe API
```

# FAQ

Question: The frontend was deployed on AWS Amplify while the Express.js backend was containerized and deployed on Elastic Beanstalk. Is this a microservice or a modular monolithic architecture?

Answer: This is a client-server modular monolithic architecture. The fact that the frontend and backend are deployed separately does not automatically make it microservices. Microservices are not defined by having multiple deployments, containers, or by separating frontend and backend. They are defined by independently deployable backend (e.g., Express) services (e.g., authentication, bookings, payments), each responsible for a specific business capability and communicating through APIs.
