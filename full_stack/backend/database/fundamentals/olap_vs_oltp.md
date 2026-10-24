<div align='center'>
    <h1> Data Architecture & Workload Patterns </h1>
    <h2> OLAP vs OLTP </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
- [OLAP](#olap)
- [OLTP](#oltp)
- [References](#references)

# Introduction

"Both online analytical processing (OLAP) and online transaction processing (OLTP) are database management systems for storing and processing data in large volumes. They require efficient and reliable IT infrastructure to run smoothly. You can use them both to query existing data or store new data. Both support data-driven decision-making in an organization." —[AWS](https://aws.amazon.com/compare/the-difference-between-olap-and-oltp/#:~:text=OLAP%20and%20OLTP.-,Key%20differences%3A%20OLAP%20vs.%20OLTP,data%20analysis%2C%20and%20identify%20trends.).

---

# OLAP

Online Analytical Processing (OLAP) is a database management system used to perform analytical operations on aggregated data, possibly from multiple sources.

- Database: Uses multidimensional (cubes) or relational databases.
- Data model: Uses star schema (denormalized), snowflake schema (normalized), or other analytical models.
- Queries: Complex, involving many database records.
- Response time: Slower than OLTP.
- Use cases: In a retail system, OLAP can be used to analyze trends, predict customer behavior, and other key metrics that drive business decisions.
- Technologies: [Amazon Redshift](https://aws.amazon.com/redshift/).

---

# OLTP

Online Transaction Processing (OLTP) is a database management system used to process database transactions in real time. OLTP ensures [data integrity](https://aws.amazon.com/what-is/data-integrity/) through [ACID](./acid.md) compliance.

- Database: Primarily uses relational databases, though modern systems use OLTP with non-relational (NoSQL) databases.
- Data model: Uses normalized or denormalized models.
- Queries: Simple, involving one or a few database records.
- Response time: Faster than OLAP.
- Use cases: ATMs, credit card payments, online booking, customer orders, and customer data.
- Technologies: AWS Aurora (RDBMS), AWS DynamoDB (NoSQL), MongoDB (NoSQL), and Cassandra (NoSQL).

---

# References

[1] https://aws.amazon.com/compare/the-difference-between-olap-and-oltp

[2] https://aws.amazon.com/what-is/data-integrity

[3] https://www.ibm.com/think/topics/oltp
