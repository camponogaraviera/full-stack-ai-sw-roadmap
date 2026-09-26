<div align='center'>
    <h1> Partitioning </h1>
</div>

# Table of Contents

- [Vertical Partitioning](#vertical-partitioning)
- [Horizontal Partitioning](#horizontal-partitioning)
- [References](#references)

# Vertical Partitioning

Partitioning splits a large table into smaller, physically or logically separate pieces (partitions) within the same database and database server. To applications and users, it still appears and behaves as a single unified table.

- Pros: Partitioning improves query performance without distributing the system across different servers.

- Cons: Limited to the machine's capacity.

---

# Horizontal Partitioning

In horizontal partitioning (a.k.a. sharding), the data is split across multiple servers and independent database instances while maintaining the same database schema.

- Pros: Reduces [contention](../../../system_design/concurrency/contention.md) and enables the database to handle larger datasets and increased traffic.

- Cons: Queries require JOIN-like operations that are expensive across shards.

---

# References

[1] https://learn.microsoft.com/en-us/azure/well-architected/design-guides/partition-data
