<div align='center'>
  <h1> 3.3 Backend </h1>
  <h2> 3.3.2 API Development and Communication</h2>
  <h3> Apollo GraphQL </h3>
</div>
  
# Table of Contents

- [Introduction](#introduction)
- [Apollo Client](#apollo-client)
- [Apollo Server](#apollo-server)
- [Apollo Studio](#apollo-studio)
- [Apollo Federation](#apollo-federation)

---

# Introduction

[Apollo GraphQL](https://master--apollo-docs-index.netlify.app/docs/intro/platform/) is an ecosystem for building and deploying GraphQL APIs. It offers out-of-the-box features, a client-side library ([Apollo Client](https://www.apollographql.com/docs/react/why-apollo)), a server-side library ([Apollo Server](https://www.apollographql.com/docs/apollo-server)), a cloud-based suite of tools ([Apollo Studio](https://master--apollo-docs-index.netlify.app/docs/intro/platform/#:~:text=In%20addition%20to%20its%20open,together%20known%20as%20Apollo%20Studio)), and support for creating a distributed GraphQL schema ([Apollo Federation](https://master--apollo-docs-index.netlify.app/docs/intro/platform/#4-federate-your-graph-with-apollo-federation)).

---

## [Apollo Client](https://www.apollographql.com/docs/react/why-apollo)

Apollo Client is a JavaScript state management library that acts as a GraphQL client. It is used to facilitate the interaction between the client-side (frontend) and the server-side (backend) of applications based on GraphQL. Its capabilities include `managing both local and remote states` of the application. It can reduce or eliminate the need for a separate state library for server-derived state specifically. It is not a drop-in, always-sufficient replacement for general client state management. For remote data, it handles and caches the results of GraphQL queries (data fetching). For local state, it stores and manages client-side data. All within the same unified caching system.

- Reactive Updates: when data in the cache changes (whether it is local or remote), Apollo Client automatically updates the React components from the UI.

- Local state: data is stored and managed directly within the client application or component memory (local storage). It does not require network requests to fetch and update data by the client. Data is not persistent beyond the user's session (data is lost after logout). Access is immediately available and fast to retrieve.

- Remote state: data is stored and managed on the server, typically in a database. It requires network requests (e.g., using GraphQL or REST APIs) to fetch and update data by the client. Data is persistent and shared among multiple clients. Data may not be immediately available. Requires a mechanism to keep local and server data in sync.

---

## [Apollo Server](https://www.apollographql.com/docs/apollo-server)

Apollo Server is a JavaScript/TypeScript GraphQL server-side library for building scalable and production-ready GraphQL servers in Node.js. It has support for schema stitching, real-time subscriptions, custom directives, and Node.js middlewares.

---

## [Apollo Studio](https://www.apollographql.com/tutorials/fullstack-quickstart/06-connecting-graphs-to-apollo-studio#:~:text=Apollo%20Studio%20is%20a%20cloud,this%20tutorial%20are%20free%20features)

Apollo Studio is a cloud-based suite of tools with support for prototyping, performance monitoring, and deployment.

---

## [Apollo Federation](https://master--apollo-docs-index.netlify.app/docs/intro/platform/#4-federate-your-graph-with-apollo-federation)

Apollo Federation is an architecture used for building a distributed GraphQL schema, i.e., to separate the GraphQL API into individual/dedicated services according to their functionality. This federated architecture is akin to a microservice architecture. Useful for systems that scale, allowing teams to work independently on different parts of the API.
