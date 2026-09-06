<div align='center'>
    <h1> 3.3 Backend </h1>
    <h2> 3.3.1 Database Fundamentals </h2>
    <h3> Database Models </h3>
    <h4> Vector Databases </h4>
</div>

# Table of Contents

- [About](#About)
- [Motivation](#motivation)
- [Use cases](#use-cases)
- [Technologies](#technologies)

---

# About

A vector database or vector store is a database that can store, manage, and index vectors (fixed-length lists of numbers) that represent data.

A vector database management system (VDBMS) is a software system that provides the functionality required to store, manage, index, and efficiently search vector data, typically using vector similarity search algorithms such as Approximate Nearest Neighbor (ANN) search.

---

# Motivation

Unstructured data, such as image files, can be stored in either a SQL or NoSQL database as per their:

1. Binary data: Raw image data (pixels) is actually stored in a data lake (e.g., AWS S3), and only the S3 Object Key is stored in the database.

2. Metadata: Image metadata (e.g., title, object key, etc.) is stored in database tables.

3. Tags: Also stored in database tables, tags are used to separate semantic elements in the image (e.g., color, texture, pattern, object) and for filtering.

However, querying images with similar characteristics poses a problem. A SQL query such as `select * where color = green` does not capture the multidimensional features of the data.

Therefore, a `vector database` can be used to store and index vector embeddings that represent each image sample so that the closest matching database record can be retrieved by using Approximate Nearest Neighbor (ANN) search.

---

# Use cases

1. Similarity Search.
2. Semantic Search.
3. Multi-Modal Search.
4. Retrieval-Augmented Generation (RAG).
5. Retrieval-Augmented Recommendation (RAR).

---

# Technologies

- [Milvus](https://milvus.io/): A vector database for storage of embeddings, vector indexing, ANN search, filtering, and top-k retrieval.
- [Pinecone](https://www.pinecone.io/): A managed vector database designed for similarity search and AI applications.
- [pgvector](https://github.com/pgvector/pgvector): A PostgreSQL extension that adds vector storage and similarity search capabilities.
