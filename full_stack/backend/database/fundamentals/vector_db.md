<div align='center'>
    <h1> Database Models </h1>
    <h2> Vector Databases </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
- [Motivation](#motivation)
- [Use Cases](#use-cases)
- [Technologies](#technologies)
- [References](#references)

# Introduction

A vector database or vector store is a database that can store data embeddings or vectors (fixed-length arrays of numbers). It often provides built-in data management features to insert/update, index, and search vectors. Searching is typically done using a vector similarity search algorithm such as Approximate Nearest Neighbor (ANN) search.

---

# Motivation

Unstructured data, such as image files, can be stored in either a SQL or NoSQL database as per their:

1. Binary data: Raw image data (pixels) is actually stored in a data lake (e.g., AWS S3), and only the S3 Object Key is stored in the database.

2. Metadata: Image metadata (e.g., title, object key, etc.) is stored in database tables.

3. Tags: Also stored in database tables, tags are used to separate semantic elements in the image (e.g., color, texture, pattern, object) and for filtering.

However, querying images with similar characteristics poses a problem. A SQL query such as `select * where color = green` does not capture the multidimensional features of the data.

Therefore, a `vector database` can be used to store and index data embeddings that represent each image sample so that the closest matching database record can be retrieved by using Approximate Nearest Neighbor (ANN) search.

---

# Use Cases

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

---

# References

[1] https://en.wikipedia.org/wiki/Vector_database
