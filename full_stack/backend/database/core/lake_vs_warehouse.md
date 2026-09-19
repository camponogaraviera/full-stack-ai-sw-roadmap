<div align='center'>
    <h1> Data Architecture & Workload Patterns </h1>
    <h2> Data Lakes vs Data Warehouses </h2>
</div>

# Table of Contents

- [Data Lakes](#data-lakes)
- [Data Warehouses](#data-warehouses)
- [References](#references)

---

# Data Lakes

Data lakes [1] are used to store raw data from a wide range of sources.

- Data Types: Structured, unstructured, semi-structured, relational, and non-relational data.
- Data Lakes use a `schema-on-read` approach, meaning that the data schema is defined only when the data is read or queried.

---

# Data Warehouses

Data warehouses [2] are optimized for data analysis that can drive business decisions, where structured and reliable data is essential for generating insights.

- Data Types: Structured data, typically organized into tables and columns. A data warehouse may contain multiple databases.
- Data Warehouses use a `schema-on-write` approach, meaning that incoming data must conform to a pre-defined structure or schema before it can be stored.

---

# References

[1] https://aws.amazon.com/what-is/data-lake/

[2] https://aws.amazon.com/what-is/data-warehouse/
