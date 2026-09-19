<div align='center'>
    <h1> Partitioning </h1>
</div>

# Table of Contents

- [About](#about)
- [References](#references)

---

# About

Partitioning splits a large table into smaller, physically or logically separate pieces (partitions) within the same database and database server. To applications and users, it still appears and behaves as a single unified table.

- Pros: Partitioning improves query performance without distributing the system across different servers.
- Cons: Limited to the machine's capacity.

Partitioning is sometimes classified into:

- Vertical Partitioning: The definition provided above.
- Horizontal Partitioning a.k.a. [sharding](../../../system_design/horizontal_scaling.md#sharding) [1].

In this chapter, partitioning is always referred to as vertical partitioning. 

---

# References

[1] https://learn.microsoft.com/en-us/azure/well-architected/design-guides/partition-data
