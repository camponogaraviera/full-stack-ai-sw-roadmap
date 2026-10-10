<div align='center'>
    <h1> Reliability Patterns </h1>
    <h2> Circuit Breaker Pattern </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
- [How it Works](#how-it-works)
- [References](#references)

# Introduction

A circuit breaker is a design pattern that stops an application from repeatedly attempting an operation that is likely to fail, for example when a remote service is overloaded or connectivity is partly lost. 

It also detects when the fault has been resolved. If it is, the application is allowed to call the operation again.

---

# How it Works

The following is based on [1][2].

In practice, a circuit breaker acts as a proxy for potentially failing operations, and can be implemented as a state machine with the following three states:

- Closed: Requests are routed to the operation while the proxy counts recent failures. If the number of failures exceed a threshold within a time period, the proxy opens and a time-out timer starts.

- Open: Requests fail immediately without attempting the operation and the proxy returns an exception to the application. A time-out timer starts, giving the system a time to recover from the failure.

- Half-open: When the timeout expires, a limited number of trial requests are allowed through. If enough succeed, the proxy closes and resets its failure count. If any fails, it reopens and restarts the timeout.

---

# References

[1] https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/circuit-breaker.html

[2] https://learn.microsoft.com/en-us/azure/architecture/patterns/circuit-breaker
