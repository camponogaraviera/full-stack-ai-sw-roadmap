<div align='center'>
    <h1> 3.3 Backend </h1>
    <h2> 3.3.3 Database Fundamentals </h2>
    <h3> Data Architecture & Workload Patterns </h3>
    <h4> Data Lakes vs Data Warehouses </h4>
</div>

# Table of Contents

- [Data Lakes](#data-lakes)
- [Data Warehouses](#data-warehouses)

---

# Data Lakes

Data lakes are used to store raw data from a wide range of sources.

- Data Types: Structured, unstructured, semi-structured, relational, and non-relational data.

- Data Lakes use a `schema-on-read` approach, meaning that the data schema is defined only when the data is read or queried.

---

# Data Warehouses

Data warehouses generally require data to be pre-processed, cleaned, and structured before being stored. This means data must often be transformed into a structured format suitable for analytics, making data warehouses less flexible in the types of data they store.

- Use Cases: They are optimized for analytics, operational reporting, and business intelligence (BI) use cases, where structured and reliable data is essential for generating insights.

- Data Warehouses use a `schema-on-write` approach, meaning that data must conform to a defined schema before it is written into the warehouse.
