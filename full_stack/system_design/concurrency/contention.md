<div align='center'>
    <h1> Concurrency </h1>
    <h2> Contention </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
- [Mitigations](#mitigations)
- [References](#references)

---

# Introduction

Contention refers to a situation in which multiple processes or threads simultaneously compete for the same resource.

In the context of databases, contention typically involves multiple transactions (parallel requests) attempting to access the same data or database object, such as tables, rows, or indexes, simultaneously. It can lead to decreased performance, deadlocks, and, in severe cases, system crashes.

Examples:

1. Two customers competing for the same flight seat, hotel room, or restaurant reservation slot.

2. A video goes viral, and millions of concurrent transactions are sent to the "videos" table to update the `videos.views` counter.

3. Multiple transactions are trying to update the same row in a table, leading to deadlocks or lock waits (when one transaction holds a lock on a resource).

Note: Locks themselves can cause contention, particularly in write-heavy workloads.

---

# Mitigations

To mitigate the consequences of database contention, the following strategies can be implemented:

1. [Multiversion Concurrency Control (MVCC)](https://cloudberry.apache.org/docs/tutorials/product-principles/about-mvcc/): Modern databases often use MVCC, allowing multiple reads and writes to occur simultaneously without blocking each other. MVCC reduces read-write contention by operating on independent versions of the same record, instead of overwriting the existing record in place.

2. [Database Denormalization](../../backend/database/core/norm_denorm.md): Helps mitigate contention in read-heavy workloads. May increase write contention when multiple concurrent operations attempt to update duplicated items that reside within the same database partition.

---

# References

[1] https://cloudberry.apache.org/docs/tutorials/product-principles/about-mvcc/
