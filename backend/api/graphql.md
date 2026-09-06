<div align='center'>
  <h1> 3.3 Backend </h1>
  <h2> 3.3.3 API Development and Communication</h2>
  <h3> GraphQL APIs </h3>
</div>

# Table of Contents

- [About](#About)
- [Best suited for](#Best-suited-for)
- [Similarities with RESTful APIs](#Similarities-with-RESTful-APIs)
- [Advantages and Disadvantages over RESTful APIs](#Advantages-and-disadvantages-over-RESTful-APIs)
- [CRUD Operations in GraphQL](#CRUD-operations-in-GraphQL)
- [GraphQL Syntax](#GraphQL-Syntax)
- [Example of GraphQL Schema and Resolvers](#example-of-graphql-schema-and-resolvers)
- [Implementing a GraphQL Server](#implementing-a-graphql-server)
- [Deployment](#deployment)

---

# About

GraphQL is a schema-based API query language and runtime that enables clients to request exactly the data they need from a server in a single request.

It specifies how the frontend should request structured data from the backend and how the backend should respond to it. The backend then aggregates and returns the requested data, potentially from multiple sources.

GraphQL is transport-agnostic, programming language agnostic, and database agnostic. Although HTTP is the most common transport for GraphQL queries and mutations, it can also be used over Server-Sent Events (SSE) and WebSockets, particularly for [subscriptions](https://graphql.org/learn/subscriptions/).

```bash
GraphQL
├── Stateless Query Operation    -> read/request data
├── Stateless Mutation Operation -> write/modify data
└── Subscriptions                -> receive real-time updates
                                         │
                                         ├── WebSocket transport protocol
                                         └── Server-Sent Events (SSE) transport protocol
```

GraphQL can be used in both the frontend and the backend of an application:

- Frontend: In the frontend, a GraphQL client, such as Apollo Client, is used to send GraphQL [queries](https://www.apollographql.com/docs/react/data/queries/) and [mutations](https://www.apollographql.com/docs/react/data/mutations/) to the backend server.

- Backend: In the backend of the application, GraphQL is used to define a `schema` that specifies the structure of the data (`types`) and the available operations (`queries, mutations, subscriptions`). Alongside the schema, `resolvers` (functions) are implemented to handle the aforementioned operations such as fetching the data from the appropriate sources and shaping it according to the schema before returning it to the user.

For organization and maintainability as the application grows, large GraphQL schemas are commonly modularized across files/modules.

---

# Best suited for

A GraphQL-based API is best suited for:

1. Large, complex, and interrelated (nested) data that need to be fetched with a single API call (request) through a single URL endpoint. For example, fetching a user, their posts, and the comments on those posts.

2. Composite patterns where there is a need to retrieve data from multiple sources (e.g., logs, third-party analytics, etc.).

3. When there is limited bandwidth (e.g., mobile phones and IoT devices) and a need to limit the number of requests and responses.

4. When requests and responses are expected to vary.

---

# Similarities with RESTful APIs

1. Both are stateless: Future requests do not depend on data stored from previous requests, i.e., there is no response history between requests. There's no information about which server has served which request, i.e., there is no information about where the request was routed to. Any server can handle any request.

2. Both use a Client-Server model: Requests from a single client result in responses from a single server.

3. Both use CRUD operations to create, read, update, and delete data.

4. Both use JSON, XML, or HTML data formats to return data from the server to the client.

5. Both support caching.

6. Both are database-agnostic, i.e., they can work with `any database structure` and programming language.

---

# Advantages and Disadvantages over RESTful APIs

Problems in RESTful APIs:

1. Under-fetching: A single API request does not return enough data, requiring multiple requests to different endpoints to obtain related data. For example, retrieving a user's phone number and last purchase may require requests to /person and /purchase.
2. Over-fetching: The response returns more data than the client needs. For example, requesting a user's phone number is done via an endpoint that also returns their name, date of birth, address, etc.
3. Adding new query capabilities may require changes to the API, such as supporting new query parameters, filters, or sorting options. REST APIs can use versioning when introducing breaking changes, such as removing or renaming a field. For example, an older API might expose (`/api.domain.TLD/v1/users`) and introduce (`/api.domain.TLD/v2/users`) for a breaking change.

GraphQL solution:

1. A GraphQL client makes a single API request to the GraphQL endpoint to fetch/retrieve data from the exact fields needed and no more. However, the GraphQL server still has to resolve those fields (see the N+1 query problem below).

2. GraphQL direct client requests to a single URL/endpoint (usually `/graphql`) for queries and mutations. Adding a new query capability generally does not require a new endpoint. Instead, the GraphQL schema is extended, and the corresponding resolver/server-side logic is implemented. The endpoint remains the same as the schema evolves, including when fields are deprecated. Fields can be marked as deprecated using the `@deprecated` directive, which signals to clients that they should migrate away from the field.

GraphQL Disadvantages:

1. GraphQL requires server-side implementation to support GraphQL in the frontend.

2. Caching is complex with GraphQL.

3. [N+1 query problem](https://graphql.org/learn/performance/): Because GraphQL resolves requested fields independently, with a resolver associated with each field on each type, naive implementations can trigger multiple database requests. For example, a single API request to the `users` field could result in 101 database queries: one query to fetch the users and one query for `User.posts` for each of the 100 users. One common solution is a batching mechanism such as DataLoader.

---

# CRUD Operations in GraphQL

The communication between the client and the backend can be done through three possible operations:

- `Query:` used to read/fetch data. Is set by default.
- `Mutation:` used to write/modify data.
- `Subscription:` used to receive real-time updates. When a certain event occurs on the server (such as a data change), the server pushes the update to the client without the need for the client to repeatedly request it.

CRUD operations are those that can be used to access and manipulate a resource (data or object). There are four such operations:

- Create (Mutation): Typically implemented as a mutation field, such as `createUser`, which creates a new resource.
- Read (Query): Typically implemented as a query field, such as `user` or `users`, which retrieves one or more resources.
- Update (Mutation): Typically implemented as a mutation field, such as `updateUser`, which modifies an existing resource.
- Delete (Mutation): Typically implemented as a mutation field, such as `deleteUser`, which removes a resource.

The specific mutation and query field names are defined by the GraphQL schema. GraphQL itself does not require particular names such as `createUser`, `updateUser`, or `deleteUser`.

Unlike REST, these operations are not distinguished by HTTP methods such as `POST`, `PUT`, or `DELETE`. A GraphQL HTTP server uses `HTTP POST` method to support query and mutation operations, and every request is directed to a single URL/endpoint, usually `/graphql`.

In short: REST commonly maps CRUD operations to HTTP methods and endpoints, whereas GraphQL maps CRUD operations to `query/mutation` fields within its schema.

---

# GraphQL Syntax

- Schema: defines the types, queries, mutations, and subscriptions available in the GraphQL API.

- Type: defines the structure and shape of the data that can be queried or mutated in the GraphQL API. It is classified into:
  - Scalar types: String, Int, Float, Boolean, and ID.
  - Root types: such as `Query` (fetching) and `Mutation` (create, update, delete).
  - Object types: custom types, such as `User` that contain fields, each of which has its own types.
  - Enum type: is a special scalar type that allows a field to have one of a specific set of allowed values.

- Field (a.k.a attribute): represents the actual data that can be queried or mutated within a type defined in the GraphQL schema. Each type in a schema can have one or more fields.

- Resolver: is a function responsible for reading or writing the value of a field in a GraphQL schema type (e.g., Query, Mutation, User, etc.). This effectively shapes the response according to the schema's definitions. It receives four arguments:
  - `parent` (`obj` or `root`): an object containing the result returned from the resolver on the parent field, or the `rootValue` from the server configuration in the case of a top-level Query field.
  - `args`: an object with the arguments to be provided to the field in the query. For example, if the object is `author(name: "Bob")`, then `args = { "name": "Bob" }`.
  - `context`: an object shared by all resolvers in a query. It contains request-specific data (e.g., authentication and DataLoader instance) required to resolve a query.
  - `info`: metadata about the query execution (used in advanced cases).

- Type Modifier: modifies the behavior of a specific type.
  - Non-null type: represented by an exclamation mark `!`, means that the server/resolver should always return a non-null value. The resolver should handle a null value from the database and return a default non-null value. If the resolver returns a null value, GraphQL will throw an error.
  - List type: means that the server/resolver should return an array of a type defined within a square bracket `[type]`. The list itself can be null unless marked with `!`, and its elements can be null unless the type inside is marked with `!`.

---

# Example of GraphQL Schema and Resolvers

The following example creates a schema and a corresponding nested `resolvers object` that maps type and field names to resolver functions following the [resolver-map](https://the-guild.dev/graphql/tools/docs/resolvers?utm_source=chatgpt.com) convention used by tools such as [makeExecutableSchema](https://the-guild.dev/graphql/tools/docs/generate-schema#makeexecutableschemaoptions).

- Schema:

```javascript
const schema = buildSchema(`
  # Defining a User type that contains five fields with type modifier:
  type User {
    id: ID!         # Non-null ID. Must have a non-null value.
    name: String!   # Non-null string. Must have a non-null value.
    email: String!  # Non-null string. Must have a non-null value.
    age: Int!       # Non-null integer. Must have a non-null value.
    friends: [User] # Nullable list of Users. Can be null or contain null elements.
  }

  # Defining a Query type containing the User field:
  type Query {
    user(id: ID!): User
  }

  type Mutation {
    createUser(name: String!, email: String!, age: Int!): User
    updateUser(id: ID!, name: String, email: String, age: Int): User
    deleteUser(id: ID!): Boolean
  }
`);
```

- Mock database:

```javascript
// Mock data:
const usersData = [
  {
    id: "1",
    name: "Alice",
    email: "alice@example.com",
    age: 30,
    friends: ["2", "3"],
  },
  {
    id: "2",
    name: "Bob",
    email: "bob@example.com",
    age: 30,
    friends: ["1", "3"],
  },
  {
    id: "3",
    name: "Charles",
    email: "charles@example.com",
    age: 30,
    friends: ["1", "2"],
  },
];
```

- Nested resolvers for the above schema:

```javascript
const resolvers = {
  // Query Resolvers:
  Query: {
    user: (_, args) => {
      return usersData.find((user) => user.id === args.id) ?? null;
    },
  },

  // Mutation Resolvers:
  Mutation: {
    createUser: (_, args) => {
      const newUser = {
        id: String(usersData.length + 1),
        name: args.name,
        email: args.email,
        age: args.age,
        friends: [],
      };
      usersData.push(newUser);
      return newUser;
    },
    updateUser: (_, args) => {
      const user = usersData.find((u) => u.id === args.id);
      if (!user) throw new Error("User not found");
      if (args.name) user.name = args.name;
      if (args.email) user.email = args.email;
      if (args.age) user.age = args.age;
      return user;
    },
    deleteUser: (_, args) => {
      const index = usersData.findIndex((u) => u.id === args.id);
      if (index === -1) throw new Error("User not found");
      usersData.splice(index, 1);
      return true;
    },
  },

  // Field-level Resolver (for nested 'friends'):
  User: {
    friends(user) {
      return user.friends.map((id) => usersData.find((user) => user.id === id));
    },
  },
};
```

See [Create the Server](#create-the-server) for the resolver structure using `buildSchema()` + `rootValue`.

---

# Implementing a GraphQL Server

The following are available options for building a GraphQL server. Recall that GraphQL is transport-agnostic.

- [graphql-http](https://graphql.org/blog/2022-11-07-graphql-http/): To build GraphQL servers with minimal setup. Allows full control over the GraphQL schema and resolvers.

- `AWS Amplify + AWS AppSync GraphQL:` To build serverless GraphQL APIs. It's a fully managed solution that integrates seamlessly with other AWS services.
  - `AWS Amplify:` Used to abstract away the complexity of connecting the frontend with the backend.
  - `AWS AppSync GraphQL:` Used to simplify the development of serverless GraphQL APIs that can integrate with other AWS services (e.g., DynamoDB).

- `Apollo + GraphQL:` To build scalable, production-ready GraphQL servers with out-of-the-box features. In a microservice architecture, [Apollo Federation](https://www.apollographql.com/docs/graphos/schema-design/federated-schemas/federation) is a popular architecture to unify multiple APIs into a single federated GraphQL API. This allows clients to query data through a single entry point named router, which distributes the request across the respective microservice APIs (e.g., GraphQL APIs or REST APIs).

## Installing Packages

```bash
yarn add graphql express graphql-http
```

## Create the Server

```javascript
import express from "express";
import { createHandler } from "graphql-http/lib/use/express"; // Handler.
import { buildSchema } from "graphql";

// Schema:
const schema = buildSchema(`
  type User {
    id: ID!
    name: String!
    email: String!
    age: Int!
    friends: [User]
  }

  type Query {
    user(id: ID!): User
  }

  type Mutation {
    createUser(name: String!, email: String!, age: Int!): User
    updateUser(id: ID!, name: String, email: String, age: Int): User
    deleteUser(id: ID!): Boolean
  }
`);

// Mock data:
const usersData = [
  {
    id: "1",
    name: "Alice",
    email: "alice@example.com",
    age: 30,
    friends: ["2", "3"],
  },
  {
    id: "2",
    name: "Bob",
    email: "bob@example.com",
    age: 30,
    friends: ["1", "3"],
  },
  {
    id: "3",
    name: "Charles",
    email: "charles@example.com",
    age: 30,
    friends: ["1", "2"],
  },
];

// Root resolvers:
// buildSchema() resolves each Query/Mutation field by looking up a same-named function directly on rootValue,
// called as fn(args, context, info).
const root = {
  user: ({ id }) => usersData.find((user) => user.id === id),
  createUser: ({ name, email, age }) => {
    const newUser = {
      id: String(usersData.length + 1),
      name,
      email,
      age,
      friends: [],
    };
    usersData.push(newUser);
    return newUser;
  },
  updateUser: ({ id, ...updates }) => {
    const user = usersData.find((u) => u.id === id);
    if (!user) throw new Error("User not found");
    Object.assign(user, updates);
    return user;
  },
  deleteUser: ({ id }) => {
    const index = usersData.findIndex((u) => u.id === id);
    if (index === -1) throw new Error("User not found");
    usersData.splice(index, 1);
    return true;
  },
};

// Non-root fields are resolved against the parent object returned by their parent field, not against rootValue.
// Therefore, the User.friends resolver must be attached to the schema's field directly:
schema.getType("User").getFields().friends.resolve = (parent) =>
  parent.friends.map((id) => usersData.find((user) => user.id === id));

// Create an instance of the Express application:
const app = express();

// The Port is dynamically assigned by the host environment:
const PORT = process.env.PORT || 3000;

// Mount the GraphQL handler for all HTTP methods at /graphql.
app.all(
  "/graphql",
  createHandler({
    schema: schema,
    rootValue: root,
  }),
);

// Start the server and listen for incoming requests on the defined port:
app.listen(PORT, () => {
  // Log a message to the console:
  console.log(
    `Running server in ${
      process.env.NODE_ENV || "development"
    } mode and listening on http://localhost:${PORT}/graphql\n`,
  );
});
```

## Run the Server

```bash
node server.js
```

- Unlike `express-graphql`, `graphql-http` does not bundle a GraphQL IDE. Use [`ruru`](https://www.npmjs.com/package/ruru), a wrapper around GraphiQL:

```bash
npx ruru -Pe http://localhost:3000/graphql -p 4000
```

Then open `http://localhost:4000` in the browser. The `-P` flag proxies incoming requests to avoid CORS issues, and `-e` sets the target GraphQL endpoint for query and mutation operations.

## Operations

- Query to get only the name and friends fields. Run via `ruru` or any GraphQL client:

```graphql
query {
  user(id: "1") {
    name
    friends {
      name
    }
  }
}
```

- Create a User (Mutation):

```graphql
mutation {
  createUser(name: "Telamon", email: "telamon@example.com", age: 30) {
    id # Returns only the ID.
  }
}
```

- Update a User:

```graphql
mutation {
  updateUser(
    id: "1" # Required: ID of the user to update.
    name: "Updated Name" # Optional: New name (skip if unchanged).
    email: "new@email.com" # Optional: New email (skip if unchanged).
    age: 31 # Optional: New age (skip if unchanged).
  ) {
    id # Fields to return after update (choose what you need).
    name
    email
    age
  }
}
```

- Delete a User (Mutation):

```graphql
mutation {
  deleteUser(id: "2")
}
```

---

# Deployment

- **AppSync + Amplify** does not require deployment on Beanstalk or Fargate.

- [AWS Fargate](https://aws.amazon.com/fargate/):
  - Use Docker to containerize/package the application workload.
  - Push the Docker image to a container registry, such as Amazon ECR.
  - Deploy the containerized application on Fargate through ECS (Elastic Container Service) or EKS (Elastic Kubernetes Service). There is no need to manage EC2 instances. AWS takes care of the underlying infrastructure (VMs, scaling, etc).
