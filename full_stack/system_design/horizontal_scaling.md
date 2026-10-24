<div align='center'>
    <h1> System Design </h1>
    <h2> Horizontal Scaling </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
- [Load Balancer](#load-balancer)
- [Routing Algorithms](#routing-algorithms)
  - [Random](#random)
  - [Round Robin](#round-robin)
  - [Weighted Round Robin](#weighted-round-robin)
  - [Least Connections](#least-connections)
  - [Weighted Least Connections](#weighted-least-connections) 
  - [Least Response Time](#least-response-time)
  - [Least Bandwidth](#least-bandwidth)
  - [IP Hash](#ip-hash)
- [References](#references)

# Introduction

Horizontal scaling (a.k.a. scaling out) increases the system's capacity by adding more machines or server instances, allowing workloads to be distributed across multiple nodes via [horizontal partitioning](../backend/database/fundamentals/partitioning.md).

Stateless servers greatly simplify horizontal scaling because any server can handle any request without storing session-specific data or state.

---

# Load Balancer

A load balancer distributes incoming traffic (requests) across a fleet of backend servers.

In a typical internet architecture, clients connect to the load balancer through a public endpoint, while backend servers use private IPs. An internal load balancer, however, can also be used for private traffic only.

Load balancing is commonly performed at two layers:

1. Transport layer 4 (TCP/UDP): L4 load balancing distributes connections based primarily on transport-layer information such as source/destination IP addresses and TCP/UDP ports. It does not need to understand the application protocol or HTTP contents.
  - Suitable for: high-throughput, low-latency TCP/UDP traffic, long-lived connections, and message-broker traffic.
  - Examples: [Network Load Balancer (NLB)](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/introduction.html) and [HAProxy (TCP mode)](https://www.haproxy.com/documentation/haproxy-configuration-tutorials/proxying-essentials/configuration-basics/backends/).

2. Application Layer 7 (HTTP): L7 load balancing understands application-layer protocols such as HTTP/HTTPS and can make path/host-based routing and HTTP-oriented routing decisions based on information such as the hostname, URL path, HTTP headers, cookies, and HTTP method.
  - Suitable for: web applications, REST APIs, microservices, HTTP/HTTPS traffic, WebSockets, and content-based routing.
  - Examples: Application Load Balancer (ALB), [NGINX](https://nginx.org/en/docs/http/load_balancing.html), [Envoy](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/upstream/load_balancing/load_balancers), and [HAProxy (HTTP mode)](https://www.haproxy.com/documentation/haproxy-configuration-tutorials/proxying-essentials/configuration-basics/backends/).

---

# Routing Algorithms

## Random

Selects a server uniformly at random from the available servers.

Power of Two Choices: Selects two servers at random and routes the request to the less-loaded server.

## Round Robin

A load-balancing algorithm that distributes requests sequentially across a pool of servers.

It works best when backend servers have approximately equal capacity (e.g., CPU, max connections, etc.) and requests have relatively similar costs (1ms, 30s).

Requests are then routed to servers in order, cycling back to the first server after reaching the last.

Suppose there are 3 servers in total: A, B, and C.

```bash
User Requests
  |
  v
Load Balancer
  |
  v
Request 1 -> Server A
Request 2 -> Server B
Request 3 -> Server C
Request 4 -> Server A
Request 5 -> Server B
...
```

Note: Round-robin load balancing does not require [round-robin DNS](./url_dns.md). A load balancer can perform round-robin routing locally behind a single entry point, without DNS rotating or returning multiple server IP addresses.

## Weighted Round Robin

Distributes traffic across servers according to their capacity (e.g., CPU, max connections, etc.). Servers with greater capacity can be assigned higher weights.

Suppose there are 3 servers in total (A, B, and C) and that the weight ratio is: 1:2:4.

```bash
User Requests
  |
  v
Load Balancer
  |
  v
Server A (2 cores): Handles N traffic.
Server B (4 cores): Handles 2N traffic.
Server C (8 cores): Handles 4N traffic.
...
```

Since traffic can vary, weights could be fine-tuned to balance the relationship between request volume and resource consumption.

## Least Connections

Routes a new connection/request to the backend with the fewest active connections.

It does not directly account for how much work each connection or request is generating.

## Weighted Least Connections

Routes new connections to the server with the lowest weighted connection count. Backend servers with greater capacity can be assigned higher weights.

## Least Response Time

Routes requests to the server with the lowest observed response time, often combined with the number of active connections.

## Least Bandwidth

Routes new traffic toward the backend currently consuming the least network bandwidth, typically based on recently observed traffic.

## IP Hash

The client's IP address is hashed to determine which server handles the request. 

Aassuming the backend pool and hash configuration remain stable, requests from the same IP are therefore typically routed to the same server, providing session affinity without requiring a centralized session store.

The downside is uneven distribution. For example, one corporate NAT with thousands of employees behind a single shared public IP creates a hotspot on a single server.

Note: A Corporate Network Address Translation (NAT) is a system that allows many devices inside a business network to share a small number of public IP addresses when connecting to a server.

---

# References

[1] https://wa.aws.amazon.com/wellarchitected/2020-07-02T19-33-23/wat.concept.horizontal-scaling.en.html
