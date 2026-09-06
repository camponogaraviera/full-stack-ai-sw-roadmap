<div align='center'>
    <h1> 3.3 Backend </h1>
    <h2> 3.3.3 API Development and Communication</h2>
    <h3> gRPC APIs </h3>
</div>

# Table of Contents

- [About](#about)
- [Best suited for](#best-suited-for)
- [Advantages over RESTful APIs](#advantages-over-restful-apis)

---

# About

gRPC is a language-agnostic, cross-platform Remote Procedure Call (RPC) framework developed by Google for building high-performance (low-latency and high-throughput) APIs.

# Best suited for

1. Systems that require low-latency, real-time, bidirectional communication and streaming between backend services, particularly for service-to-service communication. For browser-based real-time communication, technologies such as WebSockets are often more appropriate.
2. Distributed systems with services implemented in different programming languages that need a strongly typed, language-neutral communication contract.

> **Browser limitation:** Standard gRPC cannot be called directly from browser JavaScript. Although browsers use HTTP/2 transport layer protocol internally, browser networking APIs (JavaScript code running inside a web browser) do not have direct access to HTTP/2's low-level features required by gRPC. For browser clients, use **gRPC-Web** or expose a REST/HTTP API instead.

# Advantages over RESTful APIs

1. gRPC uses Protocol Buffers (Protobuf) for binary serialization of structured data in a forward- and backward-compatible way | REST uses plain-text formats (e.g., JSON and XML) for data exchange.
2. gRPC requires HTTP/2 | REST is transport-version agnostic, i.e., it can use HTTP/1.1 or HTTP/2.
3. gRPC supports four communication patterns/methods (unary, server streaming, client streaming, bidirectional streaming) | REST supports only unary.

Supported communication patterns:

1. Unary RPC (single request, single response).
2. Server streaming (single request, multiple responses).
3. Client streaming (multiple requests, single response).
4. Bidirectional streaming (multiple requests, multiple responses).
