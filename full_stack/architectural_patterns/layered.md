<div align='center'>
  <h1> Application/System Architectural Patterns </h1>
  <h2> Layered Architecture </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
- [References](#references)

# Introduction

Layered (a.k.a. Multitier) Architecture is a traditional client-server architectural style/pattern in which a software system is organized into multiple layers, with each layer responsible for a specific set of concerns and coupled to the layer below it. 

Common layers include:

1. Presentation layer (a.k.a. User Interface layer): Responsible for interacting with the user or external clients. It handles rendering, input, request handling, and presentation-specific concerns while delegating application behavior to lower layers.
   
2. Application layer (a.k.a. service layer): Coordinates application use cases and workflows. It orchestrates operations performed by the business layer and manages application-specific concerns without containing core [business rules](https://en.wikipedia.org/wiki/Business_rule).
   
3. Business layer (a.k.a. business logic layer): Contains the [business logic or domain logic](https://en.wikipedia.org/wiki/Business_logic). It represents what the system does from a business perspective and should remain independent of presentation and infrastructure concerns.
   
4. Data access layer (a.k.a. persistence layer): Provides an abstraction for data storage and retrieval. It handles communication with databases, the file system, external data stores, and other persistence mechanisms, shielding higher layers from storage-specific details.

5. Infrastructure: Includes databases, files, external APIs, etc.

A drawback of this architecture is the sequential coupling. For example, the UI layer cannot work without the business logic layer, which cannot work without the data access layer. This makes it difficult to migrate/update the technology stack.

---

# References

[1] https://en.wikipedia.org/wiki/Multitier_architecture
