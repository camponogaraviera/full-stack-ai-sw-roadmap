<div align='center'>
    <h1> Communication Patterns </h1>
    <h2> CQRS Pattern </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
- [References](#references)

# Introduction

The Command Query Responsibility Segregation (CQRS) pattern separates write models from read models, often using different representations or data stores. It is frequently combined with Event Sourcing, where state changes are persisted as an append-only sequence of events rather than only storing the current state. CQRS and Event Sourcing are distinct distributed-systems patterns, and CQRS can be implemented using mechanisms such as DynamoDB Streams to asynchronously update read models.

---

# References

[1] https://docs.aws.amazon.com/prescriptive-guidance/latest/modernization-data-persistence/cqrs-pattern.html
