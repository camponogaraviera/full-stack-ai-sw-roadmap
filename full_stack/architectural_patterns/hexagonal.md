<div align='center'>
  <h1> Application/System Architectural Patterns </h1>
  <h2> Hexagonal Architecture </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
- [Examples](#examples)
- [References](#references)

# Introduction

Hexagonal (a.k.a. Ports and Adapters) Architecture is an architectural style/pattern proposed by Alistair Cockburn in 2005 [1].

"The primary purpose of this pattern is to focus on the inside-outside asymmetry" [1].

This architecture isolates the application's business logic (domain logic) from infrastructure code that interacts with external technologies (e.g., databases, APIs, etc.). The application communicates with the outside world (external agencies) through interfaces called Ports, while Adapters (implementations) translate the exchange. The system's interfaces are therefore designed according to purpose, while external technologies are represented by interchangeable adapters [1]. This facilitates replacing external technologies with limited impact to business logic.

- `The Application Core`: The core of the application containing [business logic (domain logic)](https://en.wikipedia.org/wiki/Business_logic), use cases (application logic), and ports (interfaces).
  
- `Ports (interfaces)`: A port identifies a purposeful conversation/interface between the application and an external agency [1]. In other words, it is an abstraction or contract defined by the application core that specifies what operations the application exposes or requires from external systems. There can be multiple adapters connecting to a single port [1], and a single adapter can invoke multiple ports.
  - **Primary Ports:** Define the operations triggered by external actors that drive the application. Examples include: Creating/querying a user and placing an order.
  - **Secondary Ports:** Define the capabilities required by the application to communicate with external systems. Examples include: Persisting/retrieving data, sending an email notification, and accessing a file system.
    
- `Adapters (implementations)`: Connect external systems to the application's ports. They are the concrete, technology-specific implementation of ports, decoupled from the application core.
  - **Primary/Driving Adapters (Input):** Translate external requests into calls to application use cases. Examples include: A user interface (UI) adapter, an HTTP/REST adapter, a test harness adapter, and a program-to-program adapter.
  - **Secondary/Driven Adapters (Output):** Translate application operations into infrastructure-specific operations. Examples include: A database access adapter (e.g., PostgreSQL, DynamoDB), an email adapter (e.g., SMTP, SES), and a local filesystem adapter.

---

# Examples

- Application core:

```typescript
// Domain model:
class User {
  constructor(
    public readonly id: string,
    public name: string,
    private email: string,
  ) {}

  // Business rule (domain logic):
  changeEmail(newEmail: string) {
    if (!newEmail.includes("@")) {
      throw new Error("Invalid email");
    }

    this.email = newEmail;
  }
}

// Port (uses domain terms such as User, findById, and save):
interface UserRepository {
  findById(id: string): Promise<User>;
  save(user: User): Promise<void>;
}

// Application service:
class UserService {
  constructor(private repository: UserRepository) {}

  // Use case:
  async getUser(id: string): Promise<User> {
    return this.repository.findById(id);
  }
}
```

- Adapter (implements the above port with a specific technology):

```typescript
class ApiUserRepository implements UserRepository {
  async findById(id: string): Promise<User> {
    // HTTP request...
  }

  async save(user: User): Promise<void> {
    // HTTP request...
  }
}
```

---

# References

[1] https://alistair.cockburn.us/hexagonal-architecture

[2] https://en.wikipedia.org/wiki/Hexagonal_architecture_(software)

[3] https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/hexagonal-architecture.html
