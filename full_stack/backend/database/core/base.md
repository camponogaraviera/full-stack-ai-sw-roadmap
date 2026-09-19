<div align='center'>
    <h1> Base Properties </h1>
</div>

# Table of Contents

- [About](#about)
- [Basically Available](#basically-available)
- [Soft State](#soft-state)
- [Eventually Consistent](#eventually-consistent)
- [References](#references)

---

# About

BASE is a database transaction model mostly used by NoSQL databases. The BASE model is not as strict as the ACID model when it comes to consistency. While ACID properties prioritize consistency, BASE properties aim for higher availability and partition tolerance in distributed systems.

---

# Basically Available

Basically Available means the system remains operational and responds to requests, even during partial failures or network partitions.

---

# Soft State

In distributed systems, Soft State means that the state of the data can change over time without user interaction, and it may temporarily be out of sync across different nodes due to replication delays, network partitions, or background updates.

---

# Eventually Consistent

Eventually Consistent means that while immediate consistency isn't guaranteed, given enough time without new updates, the data will eventually reach a consistent state across all nodes.

Eventual Consistency means that if no new updates are made, all replicas or nodes in a distributed system will eventually converge and show the exact same data.

---

# References

[1] https://aws.amazon.com/compare/the-difference-between-acid-and-base-database/
