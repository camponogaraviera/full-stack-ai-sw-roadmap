<div align='center'>
    <h1> 3.3 Backend </h1>
    <h2> 3.3.3 Database Fundamentals </h2>
    <h3> Concurrency & Performance Bottlenecks </h3>
    <h4> Deadlock </h4>
</div>

# Table of Contents

- [Introduction](#introduction)
- [Mitigations](#mitigations)

---

# Introduction

Deadlock is a problem in concurrent programming where two or more processes are waiting for each other to release resources, creating a cycle of dependencies that prevents any of them from proceeding.

---

# Mitigations

The following techniques can be used to minimize/prevent deadlocks:

- **Consistent lock ordering:** Concurrent transactions should access objects in the same order. For example, one transaction is blocked on table A until the transaction on table B is completed.

- **Keep transactions short, and in one batch:** The longer the transaction, the longer locks are held. Keep transactions short to minimize network round trips.

- **Avoid unnecessarily high isolation levels:** Avoid `REPEATABLE READ` and `SERIALIZABLE` when not required.

- **Use row versioning-based isolation:** Can minimize deadlocks between read and write operations.

- **Use bound connections:** Two or more connections can cooperate without blocking.

- **Timeout-based detection:** Abort or roll back a transaction if it waits for a resource longer than a configured threshold.

- **Wait-for graph:** Track which processes or transactions are waiting for resources held by others using an internal directed graph representation, and detect cycles that indicate a deadlock.

---

# References

[1] https://learn.microsoft.com/en-us/sql/relational-databases/sql-server-deadlocks-guide?view=sql-server-ver16

[2] https://en.wikipedia.org/wiki/Wait-for_graph
