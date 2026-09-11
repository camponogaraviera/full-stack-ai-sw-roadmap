<div align='center'>
  <h1> Application/System Architectural Patterns </h1>
  <h2> Hexagonal Architecture </h2>
</div>

# About

Hexagonal Architecture, also known as Ports and Adapters, is an architectural pattern that uses the Dependency Inversion Principle (DIP) to isolate the domain and application's core business logic from external systems such as APIs, databases, storage, user interfaces, and third-party services. 

- The Core (Domain and Application): This is the center of the application. It contains business rules and application use cases that should remain independent of infrastructure concerns.
  
- Ports: These define the boundaries between the core and the outside world.
  - Inbound ports describe operations that the application exposes, such as creating a user or placing an order.
  - Outbound ports describe capabilities that the application requires from external systems, such as persisting a user, sending an email, or charging a payment method.
    
- Adapters (Implementations): These connect external systems to the application's ports. 
  - Inbound adapters, such as REST controllers, GraphQL resolvers, CLI commands, or message consumers, translate external requests into calls to application use cases.
  - Outbound adapters, such as database repositories, email clients, API clients, and message publishers, implement the outbound ports and translate application operations into infrastructure-specific operations.
