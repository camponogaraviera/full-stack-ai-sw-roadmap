<div align='center'>
    <h1> 3.3 Backend </h1>
    <h2> 3.3.1 Database Fundamentals </h2>
    <h3> Transactions, Consistency & Scaling </h3>
    <h4> ACID Properties </h4>
</div>

# Table of Contents

- [About](#about)
- [Atomicity](#atomicity)
- [Consistency](#consistency)
- [Isolation](#isolation)
- [Durability](#durability)
- [Can NoSQL Databases be ACID-compliant?](#can-nosql-databases-be-acid-compliant)

---

# ACID

ACID is a database transaction model mostly used by Relational databases. ACID stands for Atomicity, Consistency, Isolation, and Durability.
These form a set of properties that ensure database transactions are processed reliably.

Note: A transaction is a sequence of database operations performed as a single unit of work. ACID transactions are typically used within a single database to ensure data integrity.

---

# Atomicity

Ensures the transaction is either executed completely (succeeds) or not at all (fails).

---

# Consistency

The transaction undergoes a rollback (is reverted) unless all database rules are satisfied. In other words, data inserted into the table must conform to all defined database rules and constraints.

---

# Isolation

Concurrent transactions are isolated from one another according to the guarantees of the selected isolation level. A database may use mechanisms such as locking, MVCC, or a combination of techniques to provide these guarantees.

---

# Durability

After a transaction is committed, it persists even if the system experiences a crash immediately afterward. Unlike a memory cache, in the event of a server failure, the data remains (persistent storage).

---

# Can NoSQL Databases be ACID-compliant?

Some NoSQL databases have implemented features to ensure partial ACID compliance. One example is [mongoDB](https://www.mongodb.com/basics/acid-transactions), which has support for [multi-document ACID transactions](https://www.mongodb.com/blog/post/mongodb-multi-document-acid-transactions-general-availability). Another example is [DynamoDB](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/system_design_and_infrastructure/database/06_technologies/DynamoDB.md), with the newly added support for [transactions](https://aws.amazon.com/blogs/aws/new-amazon-dynamodb-transactions/) that has enabled ACID properties to be enforced by the application.

Without Transactions, DynamoDB is BASE-compliant.
