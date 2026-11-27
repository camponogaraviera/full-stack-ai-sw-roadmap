<div align='center'>
    <h1> System Design </h1>
    <h2> Prefetching and Caching </h2>
</div>

# Table of Contents

- [Prefetching](#prefetching)
- [Caching](#caching)
  - [Cache Miss](#cache-miss)
  - [Expiration Policy](#expiration-policy)
  - [Cache Invalidation and Consistency](#cache-invalidation-and-consistency)
  - [Eviction Cache Policies](#eviction-cache-policies)
  - [The Cold-Start Problem](#the-cold-start-problem)
  - [Caching Technologies](#caching-technologies)
- [References](#references)

# Prefetching

- Prefetching involves proactively loading data into the cache before it is needed, keeping it ready for quick access to minimize latency and improve performance.

- Caching, on the other hand, typically involves adding or removing data according to a cache policy.

---

# Caching

Caching strategies are used to avoid disk seeks as much as possible to enhance performance, i.e., to reduce request-response latency by storing frequently accessed data in RAM, enabling the system to retrieve a response in milliseconds.

The internal caching in a database might not be enough, and one needs a caching layer whose job is to keep in-memory copies of the most popular requests. This caching layer can be built into the application server (generally built-in, off-the-shelf) or be a fleet of servers that are independent of the application.

Make sure to have a cache closer to the application hosts to avoid a hop to the data center.

## Cache Miss

A cache miss is any single request for data not currently in the cache. 

Example:
  - L1 cache entry for product:123 is invalidated.
  - 100 requests come in within 50ms for that product.
  - All 100 requests miss the L1 cache and go to the L2 shared cache tier.
  - If L2 also misses, all 100 fall through to the source database.

## Expiration Policy

The Time to Live (TTL) is a setting that tells the cache how long to keep a specific piece of data before it expires. Too long, and your data goes stale. Too short, and the cache won't work.

After the TTL expires, the next request will trigger a cache miss, and the data will be fetched again from the source and re-cached.

## Cache Invalidation and Consistency

In a single cache node and with TTL alone, caching is simple: when an entry expires, the next read misses and pulls fresh data from the source, and stale data is bounded by the TTL. 

- Adding active invalidation (deleting keys when the source changes) improves freshness but introduces a race condition. 
    - The Race Condition: A reader gets a cache miss and reads the old value (v1) from the database. Before it can store that value in the cache, a writer updates the database to v2 and sends a delete to the cache. The key is still empty, so the delete does nothing. The reader then caches v1, and the stale value stays until its TTL expires.
      - Mitigations include:
        - Leases or tokens: a miss hands out a token, and the cache rejects a refill if the key was invalidated since.

- Separately, any cache, with or without invalidation, can suffer a stampede when a hot key expires.
    - The Stampeding Herd: When a hot key expires, many concurrent requests can miss at once and all hit the database for the same data.
      - Mitigations include: 
        - Request coalescing or locking, so only one request refills the key while the others wait.
        - Serving stale data while one request refreshes in the background (stale-while-revalidate).
        - Probabilistic early refresh, so the entry is renewed before it expires.
        - Jittered TTLs, which help when many keys would otherwise expire together (they don't solve the single hot key case).

- Finally, distributing the cache across many nodes adds a lag window. 
    - The "Lag" Window: Invalidations take time to reach every cache replica. During that window, users hitting different replicas may briefly see different versions of the data. This is eventual consistency, typically lasting milliseconds to seconds.

## Eviction Cache Policies

Eviction cache policies are used to determine how data is added or removed from a cache layer.

- Least Recently Used (LRU): most recently used keys are moved to the head of a doubly-linked list, while least recently used are moved to the tail and evicted when space is needed. It evicts data that hasn't been accessed for the longest amount of time. Works at scale, assuming a large enough cache.

- Least Frequently Used (LFU): evicts keys that are less frequently used.

- First In First Out (FIFO): the first added item in the cache is the first to be out.

- Last In First Out (LIFO): the last added item in the cache is the first to be out.

- Most Recently Used (MRU): the opposite of LRU, i.e., the most recently used item is discarded first.

- Random Replacement (RR): randomly discards an item.

## The Cold-Start Problem

The cold-start problem arises when the caching layer goes down and one needs to restart it. When this happens, all the traffic is going to hit the database until the caching layer is repopulated.

This can cause the database server to crash. One way of solving this is to generate artificial traffic to the caching layer by playing back logs from previous data.

## Caching Technologies

- Memcached: an open-source in-memory key-value store.

- Redis: a feature-rich in-memory key-value store that supports advanced data structures.

- NCache: an in-memory distributed caching solution for .NET and Java applications.

- Ehcache: an open-source, Java-based cache that supports distributed caching.

- AWS ElastiCache: a managed service that runs Redis, Memcached, or Valkey.

---

# References

[1] https://en.wikipedia.org/wiki/Prefetching

[2] https://aws.amazon.com/caching