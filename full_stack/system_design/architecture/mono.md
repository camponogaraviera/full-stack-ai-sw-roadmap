<div align='center'>
    <h1> Distributed System Architecture </h1>
    <h2> Monolithic Architecture </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
- [References](#references)

# Introduction

In a monolithic system, while the internal codebase may be organized into separate modules, all application components (UI, business logic, and data access layers) are tightly coupled, i.e., deployed as a single logical unit (service). This single unit is usually packaged in a single container image, regardless of how many databases it connects to.

Pros:

- Easier to manage and deploy initially.

- The entire application and dependencies are installed in a single environment.

- All components are packaged, deployed, and versioned together.

- Performance can be better due to fewer network hops, since internal function calls are faster than remote service calls in distributed systems.

Cons:

- A monolith can (and typically does in production) run as multiple cloned instances across multiple servers/AZs behind a load balancer. What it can't do is scale individual internal components independently. If one part fails (e.g., a bug or a memory leak), the whole application is affected.

- Difficult to scale. The entire application must be scaled (e.g., more CPU) even if only one part is experiencing high load.

- Harder to maintain and update over time as the codebase grows.

---

# References

[1] https://aws.amazon.com/compare/the-difference-between-monolithic-and-microservices-architecture/

[2] https://www.ibm.com/think/topics/monolithic-architecture
