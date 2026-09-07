<div align='center'>
    <h1> 3.3 Backend </h1>
    <h2> 3.3.1 Database Fundamentals </h2>
    <h3> Concurrency & Performance </h3>
    <h4> Database Contention </h4>
</div>

# Table of Contents

- [Introduction](#introduction)
- [Mitigations](#mitigations)

---

# Introduction

Contention refers to a situation in which multiple processes or threads simultaneously compete for the same resource.

In the context of databases, contention typically involves multiple transactions (parallel requests) attempting to access the same data or database object, such as tables, rows, or indexes, simultaneously. It can lead to decreased performance, deadlocks, and, in severe cases, system crashes.

Examples:

1. Two customers competing for the same flight seat, hotel room, or restaurant reservation slot.

2. A video goes viral, and millions of concurrent transactions are sent to the "videos" table to update the `videos.views` counter.

3. Multiple transactions are trying to update the same row in a table, leading to deadlocks or lock waits (when one transaction holds a lock on a resource).

Obs: In locking and serialization, locks themselves can cause contention, particularly in write-heavy workloads.

---

# Mitigations

To mitigate the consequences of database contention, the following strategies can be implemented:

1. `Multiversion Concurrency Control (MVCC)`: Modern databases often use MVCC, allowing multiple reads and writes to occur simultaneously without blocking each other.

- Benefits:
  - MVCC reduces read-write contention by keeping historical versions of rows for read operations.

- Drawbacks:
  - `MVCC` reduces read contention but does not fully eliminate write-write conflicts.

2. `Database Denormalization`:

- Benefits:
  - Helps mitigate contention in read-heavy workloads, such as OLAP queries or analytics workloads.

- Drawbacks:
  - May increase write contention if multiple transactions update the same duplicated data.

---

# References
