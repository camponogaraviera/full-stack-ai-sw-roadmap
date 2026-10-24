<div align='center'>
    <h1> Data Modeling & Schema Design </h1>
    <h2> Data Normalization vs Denormalization </h2>
</div>

# Table of Contents

- [Data Normalization](#data-normalization)
- [Data Denormalization](#data-denormalization)
- [References](#references)

# Data Normalization

Normalization is the traditional way of splitting entities across different tables in a relational database. For example, storing customers and restaurants in different tables in a relational database.

- Benefits:
  - Data in a single table is not redundant (requires less storage).
  - Enables faster writes, and updates happen in just one table (e.g., updating a customer's phone number).

- Drawbacks:
  - Slower reads (more lookups or round trips between different tables), and indexing is less efficient due to JOIN operations.

---

# Data Denormalization

Denormalization is an optimization technique that intentionally introduces data redundancy or restructures data to optimize read performance and access patterns, reducing the need for JOIN-like operations. Denormalization can be applied to both relational (SQL) and non-relational (NoSQL) databases.

By design, some horizontally distributed NoSQL databases, such as DynamoDB, do not support traditional SQL-like JOIN operations across tables. As a workaround, a JOIN-like clause can be simulated with a second key-value lookup. However, since this goes against the philosophy of DynamoDB, another approach is data denormalization. In DynamoDB, the design philosophy prioritizes a `single-table design`, where multiple `entity types` (e.g., users, reactions, and photos) are stored in a single table using partition and sort keys to support efficient queries without the need for JOIN-like operations.

Note: Single-table design is not the same thing as denormalization, although they are often used together in NoSQL databases such as DynamoDB.

- Benefits:
  - Can optimize database access for known query patterns.
  - Can reduce network round trips and read operations required for a particular access pattern by storing related data together.
  - Can reduce the need for application-level JOIN-like operations.

- Drawbacks:
  - Introduces data redundancy, increasing storage and write costs.
  - May increase write contention when multiple concurrent operations attempt to update duplicated items that reside within the same database partition.
  - May contribute to hot partitions when duplicated items within the same partition are frequently accessed or updated.

---

# References

[1] https://aws.amazon.com/blogs/database/should-your-dynamodb-table-be-normalized-or-denormalized/

[2] https://aws.amazon.com/blogs/database/single-table-vs-multi-table-design-in-amazon-dynamodb/
