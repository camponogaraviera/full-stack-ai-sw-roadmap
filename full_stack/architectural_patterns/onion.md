<div align='center'>
  <h1> Application/System Architectural Patterns </h1>
  <h2> Onion Architecture </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
- [References](#references)

# Introduction

Onion Architecture is an architectural style/pattern proposed by Jeffrey Palermo in 2008 [1-4]. 

It emphasizes separation of concerns and control of coupling in long-lived business applications and applications with complex behavior. Unlike the traditional [layered architecture](./layered.md), coupling direction is toward the center. Inner layers define interfaces, while outer layers implement interfaces.

- `User Interface`: The outer layer responsible for the interaction with users. UI components depend on interfaces defined within the application core rather than directly on infrastructure implementations.
  - `Application Services`: Application-specific behavior that coordinates operations using the application core. The application core can contain multiple layers of behavior, so the exact number and responsibilities of these layers may vary.
    - `Domain Services`: Supporting business logic surrounding the domain model. These layers contain behavior while maintaining the inward dependency direction.
      - `Domain Model`: The innermost layer containing the independent object model. The combination of state and behavior that represents the organization's domain. It is the center of the architecture and is coupled only to itself.
        - `Application Core`: The collection of layers surrounding the domain model that contain the application's core behavior and abstractions. It defines interfaces, such as repository interfaces, while implementations of those interfaces reside in outer layers.
- `Tests`: Tests reside at the outskirts of the architecture because the application core does not depend on them. Instead, tests depend on the application core. Additional tests can surround the entire application when testing UI and infrastructure code.
- `Infrastructure`: External implementations and technical concerns that change frequently, such as database access, file systems, web services, messaging, and I/O. Infrastructure resides outside the application core and implements interfaces defined by inner layers.
      
---

# References

[1] https://jeffreypalermo.com/2008/07/the-onion-architecture-part-1/

[2] https://jeffreypalermo.com/2008/07/the-onion-architecture-part-2/

[3] https://jeffreypalermo.com/2008/08/the-onion-architecture-part-3/

[4] https://jeffreypalermo.com/2013/08/onion-architecture-part-4-after-four-years/
