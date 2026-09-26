<div align='center'>
  <h1> Application/System Architectural Patterns </h1>
  <h2> Hexagonal Architecture </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
- [References](#references)

# Introduction

Hexagonal (a.k.a. Ports and Adapters) Architecture is an architectural style/pattern proposed by Alistair Cockburn in 2005 [1].

"The primary purpose of this pattern is to focus on the inside-outside asymmetry" [1]. 

This architecture isolates the application's business logic (domain logic) from infrastructure code that interacts with external technologies (e.g., databases, APIs, etc.). The application communicates with the outside world (external agencies) through interfaces called Ports, while Adapters (implementations) translate the exchange. The system's interfaces are therefore designed according to purpose, while external technologies are represented by interchangeable adapters [1]. This facilitates replacing external technologies with limited impact to business logic.

- `The Application Core`: The core of the application containing use cases (application logic), [business logic (domain logic)](https://en.wikipedia.org/wiki/Business_logic), and ports (interfaces).
  
- `Ports (interfaces)`: A port identifies a purposeful conversation/interface between the application and an external agency [1]. In other words, is an abstraction or contract defined by the application core that specifies what operations the application expose or require from external systems (e.g., an incoming use-case or driving API). There can be multiple adapters to one port [1].
  - **Primary Ports:** Define the operations through which external actors drive the application. Examples include: creating a user or placing an order.
  - **Secondary Ports:** Define the interactions/capabilities through which the application communicates with external systems. Examples include: retrieving or persisting data, sending an email notification, or accessing a file system.
    
- `Adapters (implementations)`: Connect external systems to the application's ports. They are the concrete, technology-specific implementation that sits outside the core and translates external signals (e.g., user button clicks or web form submission) into calls that the application's port understands.
  - **Primary/Driving Adapters (Input):** Translate external requests into calls to application use cases. Examples include: The user interface (UI), an HTTP adapter, a test harness adapter, and a program-to-program adapter.
  - **Secondary/Driven Adapters (Output):** Translate application operations into infrastructure-specific operations. Examples include: A SQL database access adapter, an email adapter, or a flat file adapter.

---

# References

[1] https://alistair.cockburn.us/hexagonal-architecture

[2] https://en.wikipedia.org/wiki/Hexagonal_architecture_(software)

[3] https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/hexagonal-architecture.html
