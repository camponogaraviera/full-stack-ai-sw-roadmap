<div align='center'>
    <h1> Indexing & Query Optimization </h1>
    <h2> Query Optimization with Indexing </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
- [References](#references)

# Introduction

Query optimization with indexing improves database query performance by minimizing disk accesses. 

A [database index](https://en.wikipedia.org/wiki/Database_index) is a data structure that stores copies of selected columns from a database row to facilitate efficient data retrieval. The index contains a key or direct link to the original table row, allowing the row to be retrieved without scanning the entire table. Common implementations include B-trees and hash indexes.

- Benefits:
  - Reduces query execution time by avoiding full table scans during `read-heavy operations`.
  - Enables efficient random lookups and access of ordered records.
  - Faster queries reduce transaction duration, indirectly lowering lock contention (the "I" in [ACID](./acid.md)).

- Drawbacks:
  - Adds overhead to write operations (INSERT, UPDATE, DELETE) due to index maintenance.
  - May introduce [database contention](../../../system_design/concurrency/contention.md) or fragmentation in high-write scenarios.
  - Increases storage space.

---

# References

[1] https://docs.oracle.com/en/database/other-databases/nosql-database/23.3/nsdev/using-indexes-query-optimization.html

[2] https://en.wikipedia.org/wiki/Database_index
