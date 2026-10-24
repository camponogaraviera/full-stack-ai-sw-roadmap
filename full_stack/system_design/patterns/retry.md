<div align='center'>
    <h1> Reliability Patterns </h1>
    <h2> Retry Pattern </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
- [How it Works](#how-it-works)
  - [Key Elements](#key-elements)
  - [Pitfalls](#pitfalls)
- [References](#references)

# Introduction

The Retry Pattern lets an application automatically retry an operation that fails due to a transient fault: a temporary failure likely to resolve on its own, such as temporary network connectivity loss, service unavailability, rate limiting/throttling, or a timeout [1][2].

Applications often combine the Retry Pattern with the [Circuit Breaker Pattern](./circuit_breaker.md) [1] and [idempotent operations](../../backend/api/reliability_safety/idem_keys.md) [1][2].

The three address different concerns:

- Retry answers: What happens when a request fails and we try it again? Retries handle transient failures [1][2].

- Circuit Breaker answers: Should we stop calling this service after multiple repeated failures? Circuit breakers temporarily block requests to an unhealthy service after failures reach a configured threshold [1].

- Idempotency answers: Is it safe to retry this operation? Idempotency ensures that repeating an operation doesn't produce unintended duplicate side effects [1][2]. Idempotency matters most when retrying operations that can produce side effects, because a service may process the request successfully without sending a response back to the client [1].

---

# How It Works

The following flow is based on [1][2].

1. Detect a failure and classify it as retryable or non-retryable.

2. Wait according to a backoff strategy.

3. Retry up to a maximum number of attempts or until an overall deadline is reached.

4. If all attempts fail, surface the error or use a fallback.

## Key Elements

- **Retryable errors**: Common examples include timeouts, connection resets, HTTP 429, 502, 503, and 504. Don't retry 400, 401, 403, 404, or validation errors.

- **Exponential backoff**: Increase the delay between attempts (e.g., 0ms, 100ms, 300ms, 700ms).

- **Jitter**: Randomize delays so that multiple clients do not retry in lockstep.

- **Retry Limit**: Cap the number of attempts and total elapsed time.

- **Retry-After**: Honor the server's Retry-After header when present.

## Pitfalls

- **Retry amplification**: Retries at multiple layers can increase the number of requests and load. Retry at one appropriate layer where possible.

- **Retry storm**: Is a self-inflicted system overload that happens when multiple clients or services repeatedly send failed requests. Retrying against an overloaded service can prolong an outage. Exponential backoff, jitter, retry limit/budget, and circuit breakers can help mitigate this problem.

- **Added tail latency**: Retries increase request latency. Set per-attempt timeouts and an overall deadline.

- **Duplicate side effects**: Retrying an operation that produces side effects can cause unintended duplicates unless the operation is naturally idempotent or mechanisms such as idempotency keys are used.

---

# References

[1] https://learn.microsoft.com/en-us/azure/architecture/patterns/retry

[2] https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/retry-backoff.html
