<div align='center'>
    <h1> Concurrency & Performance </h1>
    <h2> Thundering Herd </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
- [Facebook's Thundering Herd Issue](#facebooks-thundering-herd-issue)
- [Mitigations](#mitigations)

---

# Introduction

The thundering herd is a concurrency problem that occurs ["when a large number of processes or threads waiting for an event are awoken when that event occurs, but only one process is able to handle the event"](https://en.m.wikipedia.org/wiki/Thundering_herd_problem), causing the others to be awakened unnecessarily. It can lead to system inefficiency, as all awakened processes consume CPU time and other resources, even though most will immediately go back to sleep.

- Example 1: Imagine 100 server processes all waiting to accept a new client connection request. When a connection arrives, all 100 "wake up" to handle it, but only one actually gets the connection.

- Example 2: Millions of users are trying to watch a live stream.
  - **Cache Stampede (Cache Breakdown):** A frequently accessed "hot" cache key (e.g., stream metadata, viewer count) expires. The cache can no longer serve the responses, and requests must fetch the data from the origin server.
  - **Thundering Herd:** Thousands of concurrent user requests detect the cache miss at roughly the same time and simultaneously attempt to fetch the same data from the backend.
  - **Database Contention:** The sudden spike of concurrent queries overwhelms the database, causing lock contention, increased query latency, and connection pool exhaustion.

---

# Facebook's Thundering Herd Issue

["When millions of people tune in to a Live broadcast simultaneously, potentially 100s of thousands of video requests will see a cache miss at the Edge Cache servers. This results in excessive queries to the Origin Cache and Live processing servers, which are not designed to handle high concurrent loads. We solved this "thundering herd" problem by creating request queues at the Edge cache servers, allowing one request to go through to the livestream server and return the content to the Edge cache, where it is distributed to the rest of the queue all at once."](https://m.facebook.com/watch/?v=10153675295382200&vanity=Engineering)

---

# Mitigations

To mitigate the thundering herd, the following strategies can be implemented:

1. `Load balancing`: Distribute the load of incoming traffic (requests) across different threads/processes.
2. `Request queuing`: Serialize access to shared resources so that only one thread/process handles a resource at a time, preventing simultaneous wake-ups.
3. `Rate limit`: Limiting how often threads/processes wake up can prevent too many of them from waking up all at once.

---

# References

[1] https://en.m.wikipedia.org/wiki/Thundering_herd_problem

[2] https://m.facebook.com/watch/?v=10153675295382200&vanity=Engineering
