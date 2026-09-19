<div align='center'>
    <h1> System Design </h1>
    <h2> Horizontal Scaling </h2>
</div>

# Table of Contents

- [About](#about)
- [Sharding](#sharding)
- [Load Balancer](#load-balancer)
- [Routing Algorithms](#routing-algorithms)
  - [Round Robin Load Balancing Algorithm](#round-robin-load-balancing-algorithm)
  - [Weighted Round Robin Load Balancing Algorithm](#weighted-round-robin-load-balancing-algorithm)
  - [Least Connections](#least-connections)
  - [Weighted Least Connections](#weighted-least-connections)
  - [IP Hash](#ip-hash)
  - [Least Response Time](#least-response-time)
  - [Random](#random)
  - [Least Bandwidth](#least-bandwidth)
- [Consistent Hashing](#consistent-hashing)
- [Database Replication](#database-replication)
  - [Multi-master Replication](#multi-master-replication)
  - [Bidirectional and Circular Replication](#bidirectional-and-circular-replication)
- [References](#references)

---

# About

Horizontal scaling is the practice of increasing the system's capacity by adding more machines or server instances, allowing workloads to be distributed across multiple nodes.

While vertical scaling increases the resources of an existing machine (e.g., CPU, RAM), horizontal scaling increases the number of machines.

Having stateless servers greatly simplifies the process of horizontal scaling because any server can handle any request without having to store session-specific data or state.

---

# Sharding

Sharding is a form of horizontal partitioning where data is split across multiple servers and independent database instances while maintaining the same database schema.

- Pros: Reduces [contention](./concurrency/contention.md) and enables the database to handle larger datasets and increased traffic.
- Cons: Queries require JOIN-like operations that are expensive across shards.

---

# Load Balancer

A load balancer redirects/offloads/distributes incoming traffic (requests) across a fleet of servers, preventing overload on a single server.

The backend servers get private IPs, and the load balancer is assigned the public-facing IP that clients connect to.

---

# Routing Algorithms

## Round Robin Load Balancing Algorithm

A general load-balancing method where all servers have the same capacity (e.g., CPU, max connections, etc.). Requests are then routed to servers in order:

```bash
Users
  |
  v
Load Balancer
  |
  v
Request 1 -> Server A
Request 2 -> Server B
Request 3 -> Server C
```

## Weighted Round Robin Load Balancing Algorithm

Allows traffic to be distributed across servers according to their capacity (e.g., CPU, max connections, etc.). Servers with more read and write capacity can handle more requests.

## Least Connections

## Weighted Least Connections

## IP Hash

## Least Response Time

## Random

## Least Bandwidth

---

# Consistent Hashing

In distributed systems, data needs to be distributed among shards. Typically, this is achieved using a hashing function that hashes the sharding keys, a.k.a partition keys, such as UserId or IPs, used to retrieve and modify data. However, shards can become overloaded over time (shard exhaustion), and in addition, adding (scaling up) and removing (scaling down) shards causes uneven data distribution. To circumvent this issue, consistent hashing is employed for data resharding (redistribution).

---

# Database Replication

In this approach, a master/slave relationship is used to ensure data Availability (see CAP theorem).

- Master: is the original database used to handle write operations.

- Slave: is a copy of the master database used to handle read operations.

When a master goes down, a slave database is ready to take its place.

## Multi-master Replication

## Bidirectional and Circular Replication

---

# References

[1] https://learn.microsoft.com/en-us/azure/well-architected/design-guides/partition-data
