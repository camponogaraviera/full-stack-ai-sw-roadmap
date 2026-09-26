<div align='center'>
    <h1> System Design </h1>
    <h2> Horizontal Scaling </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
- [Load Balancer](#load-balancer)
- [Routing Algorithms](#routing-algorithms)
  - [Round Robin](#round-robin)
  - [Weighted Round Robin](#weighted-round-robin)
  - [Least Connections](#least-connections)
  - [Weighted Least Connections](#weighted-least-connections)
  - [IP Hash](#ip-hash)
  - [Least Response Time](#least-response-time)
  - [Random](#random)
  - [Least Bandwidth](#least-bandwidth)
- [Consistent Hashing](#consistent-hashing)
- [References](#references)

# Introduction

Horizontal scaling (a.k.a. scaling out) increases the system's capacity by adding more machines or server instances, allowing workloads to be distributed across multiple nodes via [horizontal partitioning](../backend/database/fundamentals/partitioning.md).

Stateless servers greatly simplify horizontal scaling because any server can handle any request without storing session-specific data or state.

---

# Load Balancer

A load balancer redirects/offloads/distributes incoming traffic (requests) across a fleet of servers, preventing overload on a single server.

The backend servers get private IPs, and the load balancer is assigned the public-facing IP that clients connect to.

---

# Routing Algorithms

## Round Robin

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

## Weighted Round Robin

Distributes traffic across servers according to their capacity (e.g., CPU, max connections, etc.). Servers with more read and write capacity can handle more requests.

## Least Connections

...

## Weighted Least Connections

...

## IP Hash

...

## Least Response Time

...

## Random

...

## Least Bandwidth

---

# Consistent Hashing

In distributed systems, data needs to be distributed among shards. Typically, this is achieved using a hashing function that hashes the sharding keys, a.k.a. partition keys, such as `UserId` or `IPs`, used to retrieve and modify data. However, shards can become overloaded over time (shard exhaustion), and adding (scaling up) and removing (scaling down) shards can cause uneven data distribution. To solve this issue, consistent hashing is employed for data resharding (redistribution).

---

# References

[1] https://wa.aws.amazon.com/wellarchitected/2020-07-02T19-33-23/wat.concept.horizontal-scaling.en.html
