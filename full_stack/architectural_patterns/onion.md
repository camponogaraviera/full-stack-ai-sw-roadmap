<div align='center'>
  <h1> Application/System Architectural Patterns </h1>
  <h2> Onion Architecture </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
- [References](#references)

# Introduction

Onion Architecture is an architectural style/pattern proposed by Jeffrey Palermo in 2008 [1-4]. 

It relies on the [Dependency Inversion Principle (DIP)](https://en.wikipedia.org/wiki/Dependency_inversion_principle) and shares the same premise as [hexagonal architecture](./hexagonal.md), in which infrastructure is externalized/decoupled from the application core. It emphasizes separation of concerns and control of coupling in long-lived business applications and applications with complex behavior. Unlike the traditional [layered architecture](./layered.md), coupling direction is toward the center. Inner layers define interfaces, while outer layers implement interfaces. 

- `User Interface`: The outer layer responsible for the interaction with users. UI components depend on interfaces defined within the application core rather than directly on infrastructure implementations.

- `Application Core`:
  - `Application Services`: Application-specific behavior that coordinates operations using the application core. The application core can contain multiple layers of behavior, so the exact number and responsibilities of these layers may vary.
    - `Domain Services`: Supporting business logic surrounding the domain model. These layers contain behavior while maintaining the inward dependency direction.
      - `Domain Model`: The innermost layer containing the independent object model. It combines state and behavior to represent the organization's domain. It is the center of the architecture and is coupled only to itself.

- `Infrastructure`: External implementations and technical concerns that may change in the future, such as database access, web services, messaging, and I/O. Infrastructure resides outside the application core and implements interfaces defined by inner layers.

- `Tests`: Tests reside outside of the architecture because the application core does not depend on them. Instead, tests depend on the application core. Additional tests can surround the entire application when testing UI and infrastructure code.
  
---

# References

[1] https://jeffreypalermo.com/2008/07/the-onion-architecture-part-1/

[2] https://jeffreypalermo.com/2008/07/the-onion-architecture-part-2/

[3] https://jeffreypalermo.com/2008/08/the-onion-architecture-part-3/

[4] https://jeffreypalermo.com/2013/08/onion-architecture-part-4-after-four-years/
