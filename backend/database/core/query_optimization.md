<div align='center'>
    <h1> 3.3 Backend </h1>
    <h2> 3.3.3 Database Fundamentals </h2>
    <h3> Indexing & Query Optimization </h3>
    <h4> Query optimization with Indexing </h4>
</div>

# Table of Contents

- [Introduction](#introduction)

---

# Introduction

Query optimization with indexing means improving the performance of database queries by using indexes so the database can find data faster and do less work. A [database index](https://en.wikipedia.org/wiki/Database_index) stores values from one or more columns that are frequently queried in a data structure (commonly a B-Tree) along with pointers to the underlying rows, enabling efficient lookups (search and retrieval) without scanning the entire table.

- Benefits:
  - Reduces query execution time by avoiding full table scans during `read-heavy operations`.
  - Enables efficient lookups, range queries, joins, and sorting.
  - Faster queries reduce transaction duration, indirectly lowering lock contention (the "I" in [ACID](./acid.md)).

- Drawbacks:
  - Adds overhead to write operations (INSERT, UPDATE, DELETE) due to index maintenance.
  - May introduce [database contention](./contention.md) or fragmentation in high-write scenarios.
  - Increases storage usage.
