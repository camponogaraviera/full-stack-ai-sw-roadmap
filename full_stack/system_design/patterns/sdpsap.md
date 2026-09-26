<div align='center'>
    <h1> Data Management Patterns </h1>
    <h2> Shared-Database-Per-Service Anti-Pattern </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
- [References](#references)

# Introduction

In this pattern, multiple services share the same database instance or schema, even if each service primarily uses its own tables or collections. The problem is shared ownership of the data layer across service boundaries, which creates tight coupling and hidden dependencies between services.

In [this example](https://docs.aws.amazon.com/prescriptive-guidance/latest/modernization-data-persistence/shared-database.html), three services each have their own tables (sales, customer, and compliance) within the same RDS database. Although the services may conceptually own different data, they still depend on the same underlying database infrastructure and can potentially access or modify each other's data. Changes to the database schema, access patterns, or configuration can therefore affect multiple services.

The anti-pattern is not specific to relational databases. Sharing a database instance or schema can create coupling with both relational and NoSQL databases. Features such as JOINs and foreign keys can make cross-service dependencies more explicit, but their presence is not what defines the anti-pattern.

---

# References

[1] https://docs.aws.amazon.com/prescriptive-guidance/latest/modernization-data-persistence/shared-database.html
