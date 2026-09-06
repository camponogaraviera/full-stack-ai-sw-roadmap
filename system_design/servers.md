<div align='center'>
    <h1> 3.5 System Design </h1>
    <h2> 3.5.2 Servers </h2>
</div>

# Table of Contents

- [Stateful Servers](#stateful-servers)
- [Stateless Servers](#stateless-servers)
- [Hybrid Systems](#hybrid-systems)
- [Cold standby](#cold-standby)
- [Warm standby](#warm-standby)
- [Hot standby](#hot-standby)

---

# Stateful Servers

In a stateful system, requests from the same client are routed to the same stateful server.
This stateful server maintains session state across multiple requests from the same client.
This approach is ideal for vertical scaling.

A stateful system can have more than one stateful server, where each server can handle multiple sessions from different users.

## When to Use Stateful Servers?

Choose a stateful approach when:

- You intentionally keep session or request state in server memory rather than in an external store (e.g., redis or database), requiring mechanisms such as sticky sessions (session affinity).
  - Sticky session is a mechanism designed to ensure that all requests from a single client during a session are consistently routed to the same backend server.
  - Examples: Preserving authentication (login) state and caching temporary user preferences or data. The session data (state) is always stored in server memory.

- You need long-lived in-memory connections.
  - Examples: WebSockets for real-time collaboration, multiplayer gaming, or streaming systems.

- Your workload and growth expectations can be handled primarily through vertical scaling, and the added operational complexity of horizontal scaling is unnecessary.

Caveats:

- Harder to scale horizontally: Requests are no longer interchangeable since session state ties/binds a user to a specific server instance.
- Failure recovery is more complex: In-memory state and active connections are lost on instance failure. Session replication can mitigate but not eliminate.
- Load balancing constraints: Sticky session routing limits the load balancer's ability to freely distribute traffic and can lead to uneven load.

---

# Stateless Servers

Statelessness means that the server does not maintain client-specific session state between requests. In other words, there is no history of responses between requests or information about where the request was routed to. Any server can handle any request. Each request from a client to a server must contain all the information necessary for the server to understand and process that request. Each request is therefore independent of previous requests.

This approach is ideal for horizontal scaling.

## When to Use Stateless Servers?

Choose a stateless approach when:

- Session data can be offloaded to a shared store (e.g., Redis, database, or JWT).

- You are building a RESTful API or distributed architecture (e.g., microservice or cloud-based system), where stateless designs are the norm.

- You expect high traffic and need to scale horizontally across multiple servers.

---

# Hybrid Systems

Hybrid systems combine techniques from both stateless and stateful architectures.

Example use case:

- Uber.
  - Stateful microservices: To keep track of real-time, session-specific data, such as passenger and driver location, ride status (waiting, in-progress, completed), route (starting point, destination, path), and chat messages.
  - Stateless microservices: For tasks that do not require session persistence, such as matching riders with drivers, pricing, and route planning.

- Facebook.
  - Stateful services: To keep track of user session data for real-time conversations, notifications, and user presence.
  - Stateless services: For public requests, such as fetching public posts.

---

# Cold standby

Data from the primary database is backed up/copied to a standby database that is offline or not continuously synchronized. The standby must be started/prepared before it can take over.

Its primary purpose is disaster recovery.

---

# Warm standby

Data from the primary database is replicated periodically to a standby system, but the standby may not be fully up-to-date or immediately ready to serve traffic. If the primary fails, the standby is brought online, and traffic is redirected to it.

Its primary purpose is disaster recovery and high availability.

---

# Hot standby

Data from the primary database is continuously replicated to a standby system that is running and kept ready to take over immediately if the primary fails.

Its primary purpose is fast failover and high availability.
