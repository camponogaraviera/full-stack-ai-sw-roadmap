<div align='center'>
    <h1> Data Management Patterns </h1>
    <h2> Database-Per-Service Pattern </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
- [References](#references)

# Introduction

Using the database-per-service pattern in a microservices architecture ensures that each service can manage its own database independently while still participating in a coordinated workflow. This allows for better isolation and scalability.

In [this example](https://docs.aws.amazon.com/prescriptive-guidance/latest/modernization-data-persistence/database-per-service.html), a system with three microservices (sales, customer, compliance) uses one dedicated database for each service to align with the database-per-service pattern. There are no restrictions on the choice of the database. Different microservices can use different databases (SQL or NoSQL) depending on the service's requirements.

---

# References

[1] https://docs.aws.amazon.com/prescriptive-guidance/latest/modernization-data-persistence/database-per-service.html
