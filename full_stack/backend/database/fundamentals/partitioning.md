<div align='center'>
    <h1> Partitioning & Sharding </h1>
</div>

# Table of Contents

- [Vertical Partitioning](#vertical-partitioning)
- [Horizontal Partitioning](#horizontal-partitioning)
  - [Sharding](#sharding)
- [References](#references)

# Vertical Partitioning

Vertical partitioning splits a table by columns or fields, storing different subsets of columns or fields into separate tables based on access patterns (e.g., how frequently columns are accessed).

The partitions may reside on the same database server or, depending on the architecture, on different servers. The application can combine the partitions when it needs data from multiple column groups.

- Pros: Faster scans of hot columns (reduced I/O when queries touch only frequently accessed columns), better cache use, and the option to apply different storage or security to sensitive columns. 

- Cons: Queries that need columns from multiple partitions require JOINs on the primary key to reassemble rows, and writes are more complex because they may touch several tables.

---

# Horizontal Partitioning

Horizontal partitioning splits a table by rows, with each partition containing a subset of the table's rows based on partition key values. 

Every partition has the same schema. Partitions typically reside within the same database server (when distributed across servers, it's sharding).

- Pros: Can improve query performance through partition pruning (queries scan only the relevant partitions based on the partition key) and simplifies maintenance, e.g., dropping an old partition instead of deleting rows.

- Cons: Limited to the capacity of the database server.

## Sharding

When horizontal partitions containing a subset of data rows are distributed across multiple database servers or independent database instances, the process is commonly called sharding and partitions are named shards.

Each shard has the same schema, but different subsets of data.

- Pros: Sharding can reduce [contention](../../../system_design/concurrency/contention.md) when the workload is effectively distributed across shards. It also enables the database to handle larger datasets and increased traffic (horizontal scalability beyond a single machine).

- Cons: Queries that need data from multiple shards may involve distributed operations (e.g., cross-shard JOINs), which are more expensive than operations within a single shard. Resharding/rebalancing and cross-shard transactions are also disadvantages.

---

# References

[1] https://learn.microsoft.com/en-us/azure/well-architected/design-guides/partition-data
