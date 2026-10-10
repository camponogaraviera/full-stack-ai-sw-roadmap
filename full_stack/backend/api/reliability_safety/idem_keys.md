<div align='center'>
    <h1> API Reliability & Safety </h1>
    <h2> Idempotency Keys </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
- [How It Works](#how-it-works)
- [References](#references)

# Introduction

An idempotency key is a common mechanism for making non-idempotent operations safely retryable. It allows a server to recognize multiple requests attempting to perform the same logical operation.

For example, during a payment process, the client may time out after the server has successfully processed the request. If the client retries the request using the same idempotency key, the server will recognize the retry and avoid processing a second payment.

According to the HTTP specification [1], `PUT`, `DELETE`, and all safe methods are idempotent, while `POST` and `PATCH` are not guaranteed [2]. 

`POST` and `PATCH` requests can be made idempotent by using an HTTP `Idempotency-Key` request header [3]. However, this feature is an IETF draft (latest revision 07, now expired) rather than a standard [4].

---

# How it Works

The following flow is based on [3].

1. The client generates a unique idempotency key (e.g., UUID for payment or order) for each new request and sends it in the `Idempotency-Key` HTTP header.

2. If the client receives no response, it resends the same request with the same key.

3. When the server receives a `POST` or `PATCH` request with a key, it checks whether it has already received a request with that key.

4. If it hasn't, the server performs the operation, responds, and then stores the key in a table.

5. If it has, the server does not perform the operation again, but responds as though it had.

The server must ensure that requests using the same idempotency key cannot concurrently produce duplicate side effects. If a request with the same key is still being processed, the server should reject the retry, typically with `409 Conflict` [3].

---

# References

[1] https://httpwg.org/specs/rfc9110.html#idempotent.methods

[2] https://developer.mozilla.org/en-US/docs/Glossary/Idempotent

[3] https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Idempotency-Key

[4] https://datatracker.ietf.org/doc/draft-ietf-httpapi-idempotency-key-header/