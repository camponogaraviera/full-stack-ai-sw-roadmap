<div align='center'>
    <h1> 3.5 System Design </h1>
    <h2> 3.5.5 Sharding and Horizontal Scaling </h2>
</div>

# Table of Contents

- [Sharding](#sharding-database-level)
- [Horizontal Scaling](#horizontal-scaling-application-level)
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

---

# Sharding (database level)

Sharding is a form of horizontal scaling at the database level, where data can be split across multiple shards. It is used to balance the database workload, enabling the database to handle larger datasets and increased traffic.

Sharding involves dividing a table's rows (for RDB) or items (for NoSQL) across multiple shards (a.k.a partitions), where each shard is stored on a separate server or node (e.g., an Amazon EC2 instance for DynamoDB). A shard key (e.g., UserID) is used to determine which shard a particular row (or item) of data belongs to.

Each shard has the same schema but contains a subset of the data. The data in each shard is unique and does not overlap with data in other shards, which helps distribute the load across multiple servers.

By distributing the data in this way, sharding improves query performance, scalability, and fault tolerance, which are key benefits of horizontal scaling.

While sharding is difficult to implement with SQL databases, it is possible in modern MySQL using Vitess, a database clustering system originally developed at YouTube.

---

# Horizontal Scaling (application level)

Horizontal scaling at the application level (scaling out) is the practice of increasing the number of servers, i.e., computer instances to distribute the load of incoming requests across many servers and handle more traffic or data. This practice increases the number of machines (server nodes), rather than the machine's read and write capacity units.

At the application level, this means deploying the application to multiple servers (instances) and using a load balancer to distribute incoming requests (traffic) across these servers evenly.

Having stateless servers greatly simplifies the process of horizontal scaling because any server can handle any request without having to store session-specific data or state. In contrast, stateful servers require more complex scaling solutions to manage session consistency across instances.

- Pros: A distributed system, which relies on multiple machines, increases fault tolerance and reduces latency by allowing multi-region deployments in regions closer to the user's location.

- Cons: More expensive than vertical scaling in the short run.

## Load Balancer

A load balancer redirects/offloads/distributes incoming traffic (requests) across a fleet of servers, preventing overload on a single server.

The backend servers get private IPs, and the load balancer is assigned the public-facing IP that clients connect to.

## Routing Algorithms

### Round Robin Load Balancing Algorithm

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

### Weighted Round Robin Load Balancing Algorithm

Allows traffic to be distributed across servers according to their capacity (e.g., CPU, max connections, etc.). Servers with more read and write capacity can handle more requests.

### Least Connections

### Weighted Least Connections

### IP Hash

### Least Response Time

### Random

### Least Bandwidth

## Consistent Hashing

In distributed systems, data needs to be distributed among shards. Typically, this is achieved using a hashing function that hashes the sharding keys, a.k.a partition keys, such as UserId or IPs, used to retrieve and modify data. However, shards can become overloaded over time (shard exhaustion), and in addition, adding (scaling up) and removing (scaling down) shards causes uneven data distribution. To circumvent this issue, consistent hashing is employed for data resharding (redistribution).

## Database Replication

In this approach, a master/slave relationship is used to ensure data Availability (see CAP theorem).

- Master: is the original database used to handle write operations.

- Slave: is a copy of the master database used to handle read operations.

When a master goes down, a slave database is ready to take its place.

### Multi-master Replication

### Bidirectional and Circular Replication
