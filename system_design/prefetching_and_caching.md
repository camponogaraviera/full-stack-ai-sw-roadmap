<div align='center'>
    <h1> 3.5 System Design </h1>
    <h2> 3.5.6 Prefetching and Caching </h2>
</div>

# Table of Contents

- [Prefetching](#prefetching)
- [Caching](#caching)
  - [Expiration Policy](#expiration-policy)
  - [Eviction Cache Policies](#eviction-cache-policies)
  - [The Cold-Start Problem](#the-cold-start-problem-in-massive-scale-systems)
  - [Cache Misses](#cache-misses)
  - [Caching Technologies](#caching-technologies)

---

# Prefetching

- Prefetching involves proactively loading data into the cache before it is needed, keeping it ready for quick access to minimize latency and improve performance.
- Caching, on the other hand, typically involves adding or removing data according to a cache policy.

---

# Caching

Caching strategies are used to avoid disk seeks as much as possible to enhance performance, i.e., to reduce request-response latency by storing frequently accessed data in RAM, enabling the system to retrieve a response in milliseconds.

The internal caching in a database might not be enough, and one needs a caching layer whose job is to keep in-memory copies of the most popular requests. This caching layer can be built into the application server (generally built-in, off-the-shelf) or be a fleet of servers that are independent of the application.

Every cache server is responsible for a segment of the database that is distributed across the application server. Make sure to have a cache closer to the application hosts to avoid a hop to the data center.

## Expiration Policy

The Time to Live (TTL) is a setting that tells the cache how long to keep a specific piece of data before it expires. Too long, and your data goes stale. Too short, and the cache won't work.

After the TTL expires, the next request will trigger a cache miss, and the data will be fetched again from the source and re-cached.

## Eviction Cache Policies

Eviction cache policies are used to determine how data is added or removed from a cache layer.

- Least Recently Used (LRU): most used keys are moved to the head of a doubly-linked list, while least recently used are moved to the tail and evicted when space is needed. It evicts data that hasn't been accessed for the longest amount of time. Works at scale, assuming a large enough cache.

- Least Frequently Used (LFU): evicts keys that are less frequently used. Used for small caches.

- First In First Out (FIFO): the first added item in the cache is the first to be out.

- Last In First Out (LIFO): the last added item in the cache is the first to be out.

- Most Recently Used (MRU): the opposite of LRU, i.e., the most recent item is discarded first.

- Random Replacement (RR): randomly discards an item.

## The Cold-Start Problem

Is a problem that arises when the caching layer goes down and one needs to restart it. When this happens, all the traffic is going to hit the database until the caching layer gets primed. This can cause the database server to crash. One way of solving this is to generate artificial traffic to the caching layer by playing back logs from previous data.

## Cache Misses

A cache miss is any single request for data not currently in the cache.

- Example:
  - L1 cache is invalidated for product:123.
  - 100 requests come in within 50 ms for that product.
  - 100 hits happen to the L2 layer (fallback database).
- Request Coalescing Solution:
  - Combines identical concurrent read requests for the same key into one shared backend fetch while other callers wait for the result.
  - The first request fetches, others wait, then all get the cached value.
  - Allows L1 to repopulate before serving additional requests.
  - Does not work if misses are due to `cold starts`.

## Caching Technologies

- Memcache: is an open-source in-memory key-value database.

- Redis: is a complex in-memory key-value database that supports advanced data structures.

- AWS ElastiCache: is a built-in-memory key-value store from AWS.

- Ncache: is an in-memory distributed caching solution for .NET and Java applications.

- Ehcache: is an open-source, Java-based cache that supports distributed caching.
