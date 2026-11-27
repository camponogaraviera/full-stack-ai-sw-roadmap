<div align='center'>
    <h1> Distributed System Architecture </h1>
    <h2> Microservices Architecture </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
- [FAQ](#faq)
- [References](#references)

# Introduction

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

---

# FAQ

Question: The frontend was deployed on AWS Amplify while the Express.js backend was containerized and deployed on Elastic Beanstalk. Is this a microservice or a modular monolithic architecture?

Answer: This is a client-server modular monolithic architecture. The fact that the frontend and backend are deployed separately does not automatically make it microservices. Microservices are not defined by having multiple deployments, containers, or by separating frontend and backend. They are defined by independently deployable backend (e.g., Express) services (e.g., authentication, bookings, payments), each responsible for a specific business capability and communicating through APIs.

---

# References

[1] https://aws.amazon.com/compare/the-difference-between-monolithic-and-microservices-architecture/

[2] https://aws.amazon.com/microservices
