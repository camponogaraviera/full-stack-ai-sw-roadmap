<div align='center'>
    <h1> Database Models </h1>
    <h2> Non-Relational (NoSQL) Databases </h2>
</div>

# Table of Contents

- [About](#about)
- [Pros and Cons](#pros-and-cons)
- [Data formats](#data-formats)
- [Paradigms](#paradigms)
  - [Key-Value Pairs]()
  - [Wide Columns]()
  - [Document Stores]()
  - [Graph Stores]()
  - [In-memory]()
- [Best practices](#best-practices)
- [Queries](#queries)
- [ACID or BASE-compliant](#acid-or-base-compliant)
- [Key Takeaways](#key-takeaways)

---

# About

Non-relational databases are useful for storing non-relational, unstructured data, i.e., with a flexible schema (that can be changed on the fly). Non-relational databases can either prioritize `Availability` and `Partition-Tolerance` (e.g., Cassandra, DynamoDB), or `Consistency` and `Partition-Tolerance` (e.g., MongoDB).

By design, most NoSQL databases **do not support SQL-style JOIN operations** as a primary query mechanism, since JOINs can become performance bottlenecks at scale when related data reside on different shards, requiring additional network communication during query execution. NoSQL databases favor denormalization, data locality, and access patterns over dynamically joining normalized datasets. An exception worth naming is MongoDB's $lookup aggregation stage, which provides join-like capability.

NoSQL can be used alongside relational databases via Apache Hive or Amazon Redshift.

---

# Pros and Cons

- Pros: suitable for OLTP (transactional) workloads and dynamic data. Writing is fast. It is easier and cheaper to scale horizontally since it **avoids table JOINs**. Since NoSQL databases use simple data formats (JSON, XML), they are well suited for applications with low-latency (high-speed) requirements that have:
  - High volume: a large amount of data.
  - High velocity: a large number of I/O operations (concurrent connections) per second.
  - High variety: semi-structured or unstructured data.
- Cons: data might look different without a schema.

---

# Data formats

There are two types of data formats commonly stored in NoSQL databases:

- Semi-structured data:
  - Description: data does not conform to a fixed schema like traditional relational databases. Instead, it contains tags or markers to separate semantic elements and enforce hierarchies. Examples include JSON, XML, and HTML formats.
  - Use cases: data that can have a variable schema, such as spreadsheet data, weather data, and surveys.
  - Technologies for storage: `Apache Casandra`, `MongoDB`, `DynamoDB`, `Apache HBase`, and data lakes such as `AWS S3`.
- Unstructured data:
  - Description: data does not have a fixed data structure. Used to store images, sounds, videos, and text documents.
  - Use cases: storage of photos, movies, music, books, emails, chat messages, social media posts, and web pages.
  - Technologies for storage: data lakes such as `AWS S3`.

---

# Paradigms

Non-relational data can be stored in 5 different ways:

- [Key-Value Pairs](https://aws.amazon.com/nosql/key-value/): data is stored in key-value pairs. Ideal for scenarios where fast access (read-heavy operations) to data is crucial.
  - **Technologies:** `DynamoDB`, `CouchBase`, `Redis`, and `Memcached`.
  - **Use cases:** for session management, e-commerce main database, social media storage (user, reactions, comments, photos, etc), caching layer to reduce data latency, etc.
  - **Data Structures used:** `Log-Structured Merge (LSM) Trees`, `Hash Tables`, `QuickList`, and `Skip Lists`.

- [Wide-columns](https://www.scylladb.com/glossary/wide-column-database/): optimized for write-heavy workloads of columns, but low-read. Easy to scale and replicate data across nodes.
  - **Technologies:** `Apache Cassandra`, `ScyllaDB`, `Apache HBase`, `Google Bigtable`, and `Microsoft Azure Cosmos DB`.
  - **Use cases:** to store time-series data such as sensor readings, records, history, etc.
  - **Data Structures used:** `Log-Structured Merge (LSM) Trees`, `Bloom Filters`, and `B-Trees`.

- [Document Stores](https://aws.amazon.com/nosql/document/): data is stored in JSON-like documents, where each document is a container of key-value pairs, often organized as nested objects (Hash Tables).
  - **Technologies:** `DynamoDB`, `CouchBase`, `documentDB`, and `MongoDB`.
  - **Use cases:** suitable for IOT, content management, catalogs, etc.
  - **Data Structures used:** `Hash Tables`.

- [Graph Stores](https://aws.amazon.com/nosql/graph/): data entities are represented as nodes and the relationships between them as edges.
  - **Technologies:** `Neo4j`, `Amazon Neptune`, and `GraphDB`.
  - **Use cases:** Facebook/Meta uses `undirected Graphs` to store friend connections (followers, followed). Google Maps uses `weighted Graphs` to find the shortest route. Search Engines use a Graph database to store `web pages` for a Web Crawler.
  - **Data Structures used:** `Graphs` (directed, undirected, weighted, unweighted).

- [In-memory key-value stores](https://aws.amazon.com/nosql/in-memory/): offers fast read and write operations by keeping data in RAM.
  - **Technologies:** `Redis`, `Memcached`, and `Amazon ElastiCache`.
  - **Use cases:** for caching frequently accessed data such as gaming leaderboards, session stores, activity feeds, and real-time data analytics.
  - **Data Structures used:** `Hash Tables`, `QuickList` (a Doubly Linked List where each node contains a small Ziplist), and `Skip Lists` (used to implement sorted sets a.k.a zsets).

---

# Best practices

- Apply [data denormalization](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/database/05_norm_denorm.md) to store more than one entity in a single table, avoiding JOIN operations. For example, `users`, `reactions`, and `photos` can all be stored in a single DynamoDB table.
- Reuse secondary indexes across data types (index overloading).

---

# Queries

Data from a NoSQL database is often queried using an object-based API. For example, DynamoDB uses the [DynamoDB API](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.API.html).

---

# ACID or BASE-compliant

Most NoSQL databases are BASE-compliant by default, but not ACID-compliant. MongoDB is ACID-compliant but not BASE-compliant [because transactions are consistent and not eventually consistent, as per the rules of BASE](https://www.mongodb.com/databases/acid-compliance#:~:text=MongoDB%2C%20for%20example%2C%20cannot%20be,per%20the%20rules%20of%20BASE).

DynamoDB is ACID-compliant, enabled by transactions.

---

# Key Takeaways

1. NoSQL databases can still represent relationships between data, just not via the relational/foreign-key/JOIN model.
2. NoSQL databases do not deprecate RDBMS technologies.
3. Choose NoSQL for OLTP or DSS when scale is a priority. Choose RDBMS technologies for OLAP or OLTP database management systems when scale is not a priority.
