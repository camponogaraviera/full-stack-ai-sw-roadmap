<div align='center'>
    <h1> 5. Mock System Design Interviews with AWS </h1>
    <h2> Design a Search Engine like Google </h2>
</div>

# Table of Contents

- [Functional Requirements](#functional-requirements)
- [Non-Functional Requirements](#non-functional-requirements)
- [Infrastructure](#infrastructure)
- [Services](#services)
- [Architecture](#architecture)
- [Estimations](#estimations)
  - [Traffic](#traffic)
  - [Storage](#storage)
  - [Bandwidth](#bandwidth)

---

# Functional Requirements

The user should be able to:

- Search for webpage keywords, phrases, or natural language queries.

The system should:

- Display ranked search results with titles, URLs, and snippets.

---

# Non-Functional Requirements

The system should:

- Scale with increasing crawled data.
- Use a database that prioritizes consistency and partition tolerance (CP system) over availability.
- Be available and fault-tolerant.
- Handle high throughput (~10k to 100k RPS) and millions of concurrent (simultaneous) connections.
- Have low-latency (p99 < 100ms) requests.

Note: p99 < 100ms means that 99% of all requests should complete within 100 milliseconds. Only the slowest 1% of requests are allowed to exceed 100ms.

---

# Infrastructure

1. `Scalability`:

- Scalability requires a horizontally partitioned distributed database. According to the CAP theorem, prioritizing [consistency and partition tolerance](https://github.com/camponogaraviera/full-stack-ai-sw-roadmap/blob/main/backend/database/core/cap_theorem.md) implies that `Apache HBase`, `Bigtable`, `Colossus`, or `MongoDB` can be used. However, SSTables + LSM-tree are better for fast range scan-heavy workloads. Google currently uses [Colossus](https://cloud.google.com/blog/products/storage-data-transfer/a-peek-behind-colossus-googles-file-system), while early versions used [Bigtable](https://cloud.google.com/bigtable), which is built on SSTables (for storage) + LSM-tree (for write efficiency).
- NoSQL databases are designed for `horizontal scalability at the database level (sharding)` while having the advantage of avoiding JOIN operations at the cost of redundancy.
- Implement database sharding to split table rows across multiple shards (nodes, servers), improving query performance, scalability, and fault tolerance. [Sharding](https://github.com/camponogaraviera/full-stack-ai-sw-roadmap/blob/main/system_design/horizontal_scaling.md) can be achieved using range-based sharding (e.g., `term_hash`).

2. `Availability`:

- Configure a dedicated compute instance to handle peak traffic.

3. `Fault Tolerance`:

- Use cold/warm/hot standbys to ensure fast failover.
- Use **multi-AZ deployments** for backend services.
- Use **Route 53 health checks** and **failover routing** to reroute users during regional failures.
- Set up database replicas for read-heavy workloads, reducing hits to a single database server, avoiding single points of failure, and enhancing availability and fault tolerance. In AWS, [global tables](https://aws.amazon.com/dynamodb/global-tables/) can be used to replicate DynamoDB tables automatically.

4. `High Throughput with Millions of Concurrent Connections`:

- Replace Amazon API Gateway with an Application Load Balancer (ALB) to distribute incoming traffic (requests) across a fleet of horizontally distributed `stateless servers/nodes` (e.g., Amazon EC2 instances).
- Implement auto-scaling policies to adjust the number of instances based on traffic patterns and resource utilization.
- Set up a rate-limiting mechanism using AWS WAF rate-based rules in front of the ALB to throttle requests when they exceed the burst limit (429 Too Many Requests error response).
- Use a load testing tool such as [AWS Distributed Load Testing](https://aws.amazon.com/solutions/implementations/distributed-load-testing-on-aws/) (an alternative to [Apache JMeter](https://jmeter.apache.org/) and [Gatling](https://gatling.io/)) to simulate high traffic (heavy load) on servers and identify potential bottlenecks in the system before going live.

5. `Low Latency`:

- Implement a distributed caching mechanism with TTL and pre-warming (pre-fetching) to `reduce latency` by caching the most frequent queries.

---

# Services

1. `Frontend`:

- The Web UI can be implemented with React.js or Lynx.
- The frontend (static part) can be hosted with built-in SSL/TLS certificate support via [AWS Amplify](https://github.com/camponogaraviera/aws/blob/main/services/hosting/hosting.md#aws-amplify).

2. `Web Crawler:` used to fetch web pages and store compressed crawled data in a distributed file system (e.g., [Hadoop HDFS](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html) or [Amazon S3](https://aws.amazon.com/s3/)). Distributed crawlers (e.g., [Apache Nutch](https://nutch.apache.org/) or [Googlebot](https://developers.google.com/search/docs/crawling-indexing/googlebot)) can run on [Kubernetes](https://kubernetes.io/) or [Amazon EC2](https://aws.amazon.com/pm/ec2). Google might crawl a news site every few minutes to catch breaking news before anyone searches for it.
3. `Parser:` used to parse the raw HTML data fetched by the crawler to extract meaningful content such as text, links, metadata, and other signals.
4. `Storage:` used to store parsed data (e.g., [Hadoop HDFS](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html), [Amazon S3](https://aws.amazon.com/s3/), or Colossus).
5. `Indexer:` used to map documents to a vocabulary of tokens/keywords or other signals (e.g., formatting, position). This stage can use [EKS to schedule Apache Spark applications on Kubernetes](https://aws.amazon.com/blogs/containers/best-practices-for-running-spark-on-amazon-eks/). Data can be stored in a NoSQL key-value database (e.g., [DynamoDB](https://github.com/camponogaraviera/full-stack-ai-sw-roadmap/blob/main/backend/database/technologies/dynamodb.md) or [Bigtable](https://cloud.google.com/bigtable)).
6. `Inverted Index (Posting List) Service:` used to map each keyword/token in the vocabulary to a list containing references/pointers/indexes to the respective documents/webpages containing that keyword. A scalable database (e.g., [Bigtable](https://cloud.google.com/bigtable)) can be used to store the tokens and their corresponding **posting lists of indexes**, and [MapReduce](https://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html) can be used for indexing.
7. `Page Ranking Service:` to rank documents retrieved by the query service based on their relevance to the user's search keyword.
8. `Query Service:` to handle user search queries, retrieving and returning relevant documents from the `inverted indexer`.
9. `Caching Service`: to `reduce latency` by caching the most frequently queried search terms and web pages from the `inverted indexer`.

- **In-memory Caching**: use Redis, Memcached, or [Amazon Elasticache](https://aws.amazon.com/pm/elasticache/) (general-purpose) to cache hot queries.
- **Edge Caching**: use CloudFront (CDN) to cache static results.
- **Index Caching**: use Elasticsearch to keep **inverted index** segments in memory.

---

# Architecture

A monolithic architecture can be a starting point for prototyping and product validation.

As the system scales and matures, [microservices architecture with Saga pattern](https://github.com/camponogaraviera/full-stack-ai-sw-roadmap/blob/main/system_design/patterns.md) is the way to go for isolation and data consistency in a distributed system/application where business transactions span multiple microservices. The saga workflow can be implemented serverless with `Amazon API Gateway`, `AWS Step Functions`, `AWS Lambda`, and [Amazon Keyspaces](https://aws.amazon.com/pt/keyspaces/), which is `compatible with Apache Cassandra`.

Consider Sagas for: **Crawling → Indexing → Ranking** pipelines.

---

# Estimations

## Traffic

Suppose the system has 1B DAUs, and each user makes 8 searches per day on average.

- Search requests per day:

$$
1B \space (users) \times 8 \space (searches) = 8B \space searches/day
$$

- Number of Requests Per Second (RPS):

$$
\frac{8B}{(24 \times 3600 \space seconds)} \sim 92,593 \space RPS
$$

## Storage

Suppose 100M website pages of 1MB are crawled per day, on average.

- Daily storage usage = 100M websites \* 0.001GB/website = 100,000 GB/day or 100 TB/day.
- Monthly storage usage = 100 TB/day \* 30 days = 3000 TB/month = 3PB/month.

## Bandwidth

The Bandwidth (data transfer), considering an ingress of 100 TB of data stored per day:

$$
\frac{100 \space TB}{(24 \times 3600 \space seconds)} \sim 1.16 \space GB/second
$$
