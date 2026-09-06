<div align='center'>
    <h1> 3.3 Backend </h1>
    <h2> 3.3.3 Database Fundamentals </h2>
    <h3> Transactions & Consistency Models </h3>
    <h4> Base Properties </h4>
</div>

# Table of Contents

- [About](#about)
- [Basically Available](#basically-available)
- [Soft-state](#soft-state)
- [Eventually Consistent](#eventually-consistent)

---

# About

BASE is a database transaction model mostly used by NoSQL databases. The BASE model is not as strict as the ACID model when it comes to consistency. While ACID properties prioritizes consistency, BASE properties aim for higher availability and partition tolerance in distributed systems.

---

# Basically Available

Focuses on ensuring the system remains operational and can serve requests, even during partial failures or network partitions.

---

# Soft-state

Acknowledges that data stored in the system may not always be immediately consistent across all nodes due to replication delays or partitions.

---

# Eventually Consistent

Means that while immediate consistency isn't guaranteed, given enough time without new updates, the data will eventually reach a consistent state across all nodes.
