<div align='center'>
  <h1> Application/System Architectural Patterns </h1>
  <h2> Hexagonal Architecture </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
- [Examples](#examples)
- [References](#references)

# Introduction

Hexagonal Architecture (a.k.a. Ports and Adapters) is an architectural style/pattern proposed by Alistair Cockburn in 2005 [1].

"The primary purpose of this pattern is to focus on the inside-outside asymmetry" [1].

This architecture isolates the application's business logic (domain logic) from infrastructure code that interacts with external technologies (e.g., databases, APIs, etc.). The application communicates with the outside world (external agencies) through interfaces called Ports, while Adapters (implementations) translate the exchange. The system's interfaces are therefore designed according to purpose, while external technologies are represented by interchangeable adapters [1]. This facilitates replacing external technologies with limited impact to business logic.

- `The Application Core`: The central part of the application containing [business logic (domain logic)](https://en.wikipedia.org/wiki/Business_logic), application/use-case logic, and ports (interfaces).

- `Ports (interfaces)`: A port identifies a purposeful conversation/interface between the application and an external agency [1]. In other words, it is an abstraction or contract defined by the application core that specifies what operations the application exposes or requires from external systems. There can be multiple adapters connecting to a single port [1], and a single adapter can invoke multiple ports.
  - **Primary Ports (Outside -> Core):** Define the operations triggered by external actors that drive the application core. Examples include: Creating/querying a user and placing an order.
  - **Secondary Ports (Core -> Outside):** Define the capabilities required by the application core to communicate with external systems. Examples include: Persisting/retrieving data, sending an email notification, and accessing a file system.

- `Adapters (implementations)`: Concrete components that connect the application core to external actors and technologies. They translate between external protocols/technologies and the application's ports.
  - **Primary/Driving Adapters (Input):** Translate external requests into calls to application use cases. Examples include: A user interface (UI) adapter, an HTTP/REST adapter, a test harness adapter, and a program-to-program adapter.
  - **Secondary/Driven Adapters (Output):** Translate application operations into infrastructure-specific operations. Examples include: A database access adapter (e.g., PostgreSQL, DynamoDB), an email adapter (e.g., SMTP, SES), and a local filesystem adapter.

---

# Examples

The following example applies the Hexagonal Architecture to a backend application.

- `Application Core`:

```typescript
// Domain model:
class User {
  constructor(
    public readonly id: string,
    public name: string,
    public email: string,
  ) {}

  // Business rule (domain logic):
  changeEmail(newEmail: string) {
    if (!newEmail.includes("@")) {
      throw new Error("Invalid email");
    }

    this.email = newEmail;
  }
}

// Primary Port (Outside -> Core):
interface UserUseCases {
  getUser(id: string): Promise<User>;
}

// Secondary Port (Core -> Outside):
interface UserRepository {
  findById(id: string): Promise<User>;
  save(user: User): Promise<void>;
}

// Application service implementation:
class UserService implements UserUseCases {
  constructor(private readonly repository: UserRepository) {}

  // Use case:
  async getUser(id: string): Promise<User> {
    return this.repository.findById(id);
  }
}
```

- **Secondary/Driven Adapter**: `DynamoDBUserRepository`.
  - Secondary Flow: Application Core -> invokes `UserRepository` (secondary port) -> implemented by `DynamoDBUserRepository` (secondary adapter) -> that triggers DynamoDB.

```typescript
class DynamoDBUserRepository implements UserRepository {
  async findById(id: string): Promise<User> {
    // DynamoDB operations...
    return user;
  }

  async save(user: User): Promise<void> {
    // DynamoDB operations...
  }
}
```

- **Primary/Driving Adapter**: `app.get()` (HTTP Controller).
  - Primary Flow: `app.get()` (primary adapter) -> invokes `UserUseCases` (primary port) -> implemented by `UserService` (application core) -> invokes `UserRepository` (secondary port) -> implemented by `DynamoDBUserRepository` (secondary adapter) -> that triggers DynamoDB.

```typescript
const repository: UserRepository = new DynamoDBUserRepository();
const userService: UserUseCases = new UserService(repository);

// HTTP Controller:
app.get("/users/:id", async (req, res) => {
  const user = await userService.getUser(req.params.id);
  const { id, name, email } = user; // Destructuring the response.
  res.json({ id, name, email });
});
```

- The following violates the intended separation because the primary adapter has become coupled directly to a secondary adapter/infrastructure technology, bypassing the application core.

```typescript
app.get("/users/:id", async (req, res) => {
  const repository = new DynamoDBUserRepository();
  const user = await repository.findById(req.params.id);
  res.json(user);
});
```

---

# References

[1] https://alistair.cockburn.us/hexagonal-architecture

[2] https://en.wikipedia.org/wiki/Hexagonal_architecture_(software)

[3] https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/hexagonal-architecture.html
