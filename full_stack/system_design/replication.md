<div align='center'>
    <h1> System Design </h1>
    <h2> Database Replication </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
- [Multi-master Replication](#multi-master-replication)
- [Bidirectional Replication](#bidirectional-replication)
- [Circular/Ring Replication](#circularring-replication)
- [References](#references)

# Introduction

In this approach, a master/slave relationship is used to ensure data Availability (see CAP theorem).

- Master: is the original database used to handle write operations.

- Slave: is a copy of the master database used to handle read operations.

When a master goes down, a slave database is ready to take its place.

---

# Multi-master Replication

---

# Bidirectional Replication

---

# Circular/Ring Replication

---

# References

[1] https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ReadRepl.html

[2] https://en.wikipedia.org/wiki/Multi-master_replication

[3] https://www.ibm.com/docs/en/idr/10.2.1?topic=multidirectional-bidirectional-replication

[4] https://mariadb.com/docs/server/ha-and-performance/standard-replication/replication-overview#ring-replication
