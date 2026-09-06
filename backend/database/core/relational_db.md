<div align='center'>
    <h1> 3.3 Backend </h1>
    <h2> 3.3.1 Database Fundamentals </h2>
    <h3> Database Models </h3>
    <h4> Relational Databases </h4>
</div>

# Table of Contents

- [About](#about)
- [Components of an RDB](#components-of-an-rdb)
- [RDBMS Technologies](#rdbms-technologies)

---

# About

A Relational Database (RDB) is a type of database structure that stores structured data in tables with a strict schema using rows and columns. Relational databases prioritize data `Consistency` and `Availability` (see [CAP theorem](./cap_theorem.md)).

- Pros: suitable for OLAP (**analytical operations**) workloads and to perform **table joins**. Good for static data. Data always maintains the same format. Has support for ACID transactions and is optimized for read-heavy workloads.
- Cons: More expensive to scale horizontally since table joins require multiple round trips to query data. It makes it more difficult to handle too many concurrent connections.

- Keywords:
  - `Table joins (JOIN clause):` are query operations that combine rows from two or more tables based on related columns (fields) between them. This is typically done through foreign keys. For example, a social media application with a MySQL database design would have the following tables: `users table`, `reactions table`, `comments table`, and `photos table`. These tables are then related by foreign keys (userID, reactionID, commentID, photoID). A foreign key in table A is a primary key from table B. It is used to ensure referential integrity, i.e., to ensure that an item or row in table A also exists/matches an item in table B.
  - `Analytical operations:` data aggregation from multiple sources, complex filtering, and statistical analysis.

Note: `Vitess` is a database clustering system for horizontal scaling of MySQL created by YouTube.

---

# Components of an RDB

- A Schema defines the structure of the database, including the tables and the relationships between them.
- A Table is a collection of rows (tuples) and columns (a.k.a attributes).

- Each table has a primary key, which is a unique `identifier` and is indexed by default. Secondary indexes can be added to non-key fields (columns) to improve query performance.

- The number of items or rows in a table is known as the `cardinality`.

- The number of columns in a table is known as the `Degree`.

- Each record has the same number of columns.

- Each column has a domain, i.e., a constraint for input data.

- A foreign key identifies the relationship between two pieces of data.

---

# RDBMS Technologies

A `Relational Database Management System (RDBMS)` is the software used to manage and interact with a relational database.

The following are examples of RDBMS software programs and libraries used to implement/create a relational database model. These technologies use Structured Query Language (SQL) as their primary language for managing and querying a database:

- Standalone database software:
  - MySQL.
  - Microsoft SQL Server (MSSQL).
  - PostgreSQL.
  - Oracle Database.
  - MariaDB.
  - CockroachDB.
- Libraries:
  - SQLite.

- Managed database/cloud services:
  - Amazon RDS.
  - Aurora (OLTP).
  - Redshift (OLAP data warehouse).
