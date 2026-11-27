<div align='center'>
    <h1> Data Access </h1>
    <h2> ORM and ODM </h2>
</div>

# Table of Contents

- [ORM](#orm)
- [ODM](#odm)
- [References](#references)

# ORM

An Object-Relational Mapping (ORM) is a programming technique used to convert data between a relational database and the [memory heap](https://en.wikipedia.org/wiki/Memory_management#HEAP) of an [object-oriented programming (OOP) language](https://github.com/camponogaraviera/javascript/blob/main/js-course/notebooks/oop/intro.js).

It allows developers to query and perform CRUD operations on a [relational database](../fundamentals/relational_db.md) using an object-oriented paradigm. Instead of writing raw SQL queries, developers interact with the database using the same programming language used to implement the backend logic.

Examples of ORM for SQL:

- Drizzle (TypeScript).
- Sequelize (Node.js).
- TypeORM.
- Hibernate (Java).
- SQLAlchemy (Python).

---

# ODM

An Object Document Mapping (ODM) is the ORM version for NoSQL databases.

Examples of ODM for NoSQL:

- Mongoose for MongoDB uses JavaScript + Node.js.
- Morphia for MongoDB uses Java.
- Dynamoose for DynamoDB.

---

# References

[1] https://en.m.wikipedia.org/wiki/Object%E2%80%93relational_mapping
