<div align='center'>
    <h1> 3.3 Backend </h1>
    <h2> 3.3.1 Database Fundamentals </h2>
    <h3> Data Modeling & Schema Design </h3>
    <h4> Data Normalization vs Denormalization </h4>
</div>

# Table of Contents

- [Data Normalization](#data-denormalization)
- [Data Denormalization](#data-denormalization)

---

# Data Normalization

Normalization is the traditional way of splitting entities across different tables in a relational database. For example, storing customers and restaurants into different tables in a relational database.

- Benefits:
  - Data in a single table is not redundant (requires less storage).
  - Enables faster writes, and updates happen in just one table (e.g., updating a customer's phone number).

- Drawbacks:
  - Slower reads (more lookups or roundtrips between different tables), and indexing is less efficient due to JOINs.

---

# Data Denormalization

By design, horizontally distributed NoSQL databases, such as DynamoDB and MongoDB, do not support traditional SQL-based JOIN operations across shards. As a circumvention, a JOIN clause can be faked by doing a second key-value lookup (not recommended as it goes against the design philosophy of NoSQL). Another workaround is data denormalization.

Denormalization is an optimization technique that stores related data in a `single table or document`, avoiding join operations at the cost of redundancy. It can be applied to both SQL and NoSQL databases. In horizontally distributed NoSQL databases, its design philosophy prioritizes a single table design enabling queries to be performed in a single table avoiding JOIN operations.

- Benefits:
  - Reduces the number of read operations required for a single query, since the information for a given request is in a single row.
  - Helps mitigate read contention in read-heavy workloads, such as OLAP queries or analytics workloads where reads are more frequent than writes.

- Drawbacks:
  - Introduces data redundancy (requires more storage, although cheaper then CPU) causing duplicated updates that involve iterating through the table's rows and, consequently, slower writes.
  - May increase write database contention if multiple transactions update the same duplicated data.
