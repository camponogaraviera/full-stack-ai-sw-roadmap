<div align='center'>
    <h1> 5. Mock System Design Interviews with AWS </h1>
    <h2> Design a YouTube-like On-Demand Video Streaming Service </h2>
</div>

# Table of Contents

- [Functional Requirements](#functional-requirements)
- [Non-Functional Requirements](#non-functional-requirements)
- [Infrastructure](#infrastructure)
- [Services](#services)
- [Estimations](#estimations)
  - [Traffic](#traffic)
  - [Storage](#storage)
  - [Bandwidth](#bandwidth)
- [Architecture](#architecture)
- [Entity Chart](#entity-chart)
- [Normalized Database Design](#normalized-database-design)
  - [Entity-Relationship Diagram (ERD)](#entity-relationship-diagram-erd)
  - [Single Relational Database Design](#single-relational-database-design)
- [Denormalized Database Design](#denormalized-database-design)
  - [DynamoDB Access (Query) Patterns](#dynamodb-access-query-patterns)
  - [DynamoDB Single Table Design](#dynamodb-single-table-design)
- [AWS Pricing](#aws-pricing)

---

# Functional Requirements

The user should be able to:

- Sign up and sign in with email and password.
- Upload, encode, and transcode a video.
- Search for a video.
- Stream and watch a video on demand or live.
- Submit reviews/feedback.

---

# Non-Functional Requirements

The system should:

- Scale with increasing user base.
- Use a database that prioritizes consistency and partition tolerance (CP system) over availability.
- Be available and fault-tolerant (without losing uploads).
- Handle high throughput (~10k to 100k RPS) and millions of concurrent (simultaneous) connections.
- Have fast loading times and minimal latency.

---

# Infrastructure

1. `Scalability`:

- A normalized database design with `PostgreSQL` can be a starting point for an average-sized app. This avoids complex update operations in a denormalized database.
- As Access Patterns require more complex join operations and performance bottlenecks emerge, a NoSQL database can be a viable alternative for scalability.
- Scalability requires a horizontally partitioned distributed database to store video metadata (title, description, URL location, image thumbnail, upload date, view count, etc.). According to the CAP theorem, prioritizing [consistency and partition tolerance](https://github.com/camponogaraviera/full-stack-ai-sw-roadmap/blob/main/backend/database/core/relational_db.md) implies that `MySQL` with [Vitess](https://vitess.io/) can be used.
- NoSQL databases are designed for `horizontal scalability at the database level (sharding)` while having the advantage of avoiding JOIN operations at the cost of redundancy.
- Implement database sharding to split table rows across multiple shards (nodes, servers), improving query performance, scalability, and fault tolerance. [Sharding](https://github.com/camponogaraviera/full-stack-ai-sw-roadmap/blob/main/system_design/horizontal_scaling.md) can be achieved using a shard key.

2. `Availability`:

- Configured AWS S3 for storing static assets (images/binary blobs, JS, CSS), leveraging automatic scaling to handle peak traffic and multi-AZ replication to ensure availability.

- Use **DynamoDB in on-demand mode** (auto-scaling), instead of provisioned mode, to automatically adjust read and write capacity units based on traffic, preventing [Throttling](https://github.com/camponogaraviera/full-stack-ai-sw-roadmap/blob/main/system_design/celebrity.md).

- Configure a dedicated compute instance to handle peak traffic.

3. `Fault Tolerance`:

- Use cold/warm/hot standbys to ensure fast failover.
- Use **multi-AZ deployments** for backend services (e.g., via AWS Fargate).
- Use **Route 53 health checks** and **failover routing** to reroute users during regional failures.
- Set up database replicas for read-heavy workloads, reducing hits to a single database server, avoiding single points of failure, and enhancing availability and fault tolerance. In AWS, [global tables](https://aws.amazon.com/dynamodb/global-tables/) can be used to replicate DynamoDB tables automatically.

4. `High-throughput and Millions of Concurrent Connections`:

- Replace API Gateway with an Application Load Balancer (ALB) to distribute incoming traffic (requests) across a fleet of horizontally distributed `stateless servers/nodes`.
- Implement auto-scaling policies to adjust the number of instances based on traffic patterns and resource utilization.
- Set up a rate-limiting mechanism using AWS WAF rate-based rules in front of the ALB to throttle requests when they exceed the burst limit (429 Too Many Requests error response).
- Use a load testing tool such as [AWS Distributed Load Testing](https://aws.amazon.com/solutions/implementations/distributed-load-testing-on-aws/) (an alternative to [Apache JMeter](https://jmeter.apache.org/) and [Gatling](https://gatling.io/)) to simulate high traffic (heavy load) on servers and identify potential bottlenecks in the system before going live.
- Session Management: store sessions in a caching mechanism (e.g., Redis or ElastiCache) instead of on servers for statelessness.
- To avoid hotspots when a video goes viral and to enhance availability, temporarily store the video in a CDN.

5. `Fast Loading Times and Minimal Latency`:

- Edge Layer (CDN): Use a CDN to cache and serve static assets (e.g., images, JS, CSS) from geographically distributed edge locations close to users. This reduces round-trip latency for users worldwide, decreases load on origin servers, lowers request counts to object storage (e.g., S3 bucket), and speeds up content delivery.
- Application Layer (Distributed Caching): Implement distributed application-level caching to store frequently accessed or computationally expensive data (e.g., user profile data, feed queries, metadata, API responses), reducing query latency and database load.
  - Use TTL to balance freshness and performance.
  - Implement cache pre-warming for high-traffic or predictable workloads to prevent cold-start latency spikes.

---

# Services

1. `Frontend`:

- The UI can be implemented with React.js (web), React Native (mobile), or Lynx.
- The frontend (static part) can be hosted with built-in SSL/TLS certificate support via [AWS Amplify](https://aws.amazon.com/amplify/).

2. `Backend API for Communication`:

2.1 `Low-throughput or short-lived processes (use serverless)`:

- The API for client-server communication can be a [RESTful API](https://github.com/camponogaraviera/full-stack-roadmap/blob/main/backend/api/restfull_api.md) implemented serverless with [Amazon API Gateway HTTP](https://aws.amazon.com/api-gateway/) + [AWS Lambda](https://aws.amazon.com/lambda/).
  - API Gateway (29-sec timeout) provides a single entry point for clients to interact with various backend services, handling HTTP geo-routing, authentication (Cognito/IAM), throttling, caching, and WebSockets.
  - Lambda executes API logic. It is suitable for short-lived, event-driven applications within Lambda's constraints (15-minute timeout, max [10GB RAM, and 6 vCPU cores](https://aws.amazon.com/about-aws/whats-new/2021/07/aws-lambda-supports-10-gb-memory-6-vcpu-cores-bahrain-osaka-hong-kong-regions/)). **CRUD operations should be implemented directly on Lambda**.
- As fetching becomes complex and performance bottlenecks emerge, a [GraphQL API](https://github.com/camponogaraviera/full-stack-roadmap/blob/main/backend/api/grahql.md), implemented with [AppSync](https://aws.amazon.com/appsync/)+[Amplify](https://aws.amazon.com/amplify/) or [graphql-http](https://graphql.org/blog/2022-11-07-graphql-http/), can be a viable alternative.

  2.2 `High-throughput or long-running processes (e.g., custom video encoding outside AWS MediaConvert)`:

- For high-throughput and long-running processes, the workload can be executed on EC2 instances, regardless of the communication API. AWS Lambda can still be used to initiate or coordinate these jobs, but it is not suitable for executing them directly if they are long-running. API Gateway should be replaced with an ALB, as mentioned in the infrastructure section.

3. `Security`:

- User authentication (sign-up, sign-in) and authorization can be implemented with [Amazon Cognito](https://aws.amazon.com/pm/cognito/) and [AWS IAM](https://aws.amazon.com/iam/), respectively. Cognito automatically handles the storage of user credentials (e.g., passwords, tokens) and metadata (e.g., email, phone number, username) inside **Cognito User Pools**.

4. `Storage`:

- [Amazon S3](https://aws.amazon.com/s3/) can be used to store raw videos, post-processed videos, image thumbnails, and other binary data with auto replication.

5. `Database`: PostgreSQL or DynamoDB can be used to store user preferences/settings, video metadata (title, description, URL location, upload date, view count, etc.), comments, and likes.

6. `Media`: video upload and pre-processing (encoding/transcoding) can be decoupled using a message queue (e.g., Amazon SQS), allowing video processing tasks to be asynchronously offloaded to processing workers (possibly a fleet of servers/EC2 instances). Post-processed videos are then stored in Amazon S3. **The following are fully managed, standalone AWS services that do not require Fargate or Beanstalk to function (API Gateway triggering Lambda functions would suffice):**
   - [AWS Elemental MediaLive](https://aws.amazon.com/medialive/): real-time video encoder for **live streaming** used to convert raw video streams into compressed formats for broadcast. It does not deliver content to end users.
   - [AWS Elemental MediaConvert](https://aws.amazon.com/mediaconvert/): professional-grade, file-based video transcoding service used to convert **on-demand** video content (e.g., MP4 uploads) into adaptive bitrate streaming formats (e.g., HLS, DASH, CMAF) and other codecs, including AVC (H.264 or MPEG-4 Part 10), HEVC (H.265), and more.
   - [AWS Elemental MediaPackage](https://aws.amazon.com/pt/mediapackage/): package and serve the video content into adaptive bitrate streaming formats (e.g., HLS, DASH, CMAF) and enable DVR features (pause, resume, start-over) using offset.
   - [Amazon CloudFront](https://aws.amazon.com/cloudfront): to cache and distribute streaming content to end users at scale with low latency.

- `On-demand Streaming AWS Pipeline:`

```bash
Amazon S3 (raw video source) → MediaConvert (transcoding) → MediaPackage (optional: encryption, DVR, playback) → CloudFront (content delivery at scale) → End Users
```

- `Live Streaming AWS Pipeline:`

```bash
MediaLive (real-time encoding) → MediaPackage (optional: encryption, DVR, playback) → CloudFront (content delivery at scale) → End Users
```

Note: [Amazon IVS](https://aws.amazon.com/ivs/) can handle the entire live video streaming pipeline from ingestion to delivery.

7. `Search Engine:` the search engine can be implemented with [Elasticsearch](https://www.elastic.co/elasticsearch), [OpenSearch](https://aws.amazon.com/what-is/opensearch/), or [Amazon CloudSearch](https://aws.amazon.com/cloudsearch/).

8. `URL Sharing:` users can share their favorite videos using a `URL shortener service`.

9. `CDN`: used to speed up `content delivery` and mitigate the `celebrity problem`. The CDN takes static data from [Amazon S3 Storage](https://aws.amazon.com/s3/). Technologies: [Amazon CloudFront](https://aws.amazon.com/cloudfront/).

10. `Caching`: [Amazon ElastiCache](https://aws.amazon.com/pm/elasticache/) (general-purpose), or [Amazon DAX](https://aws.amazon.com/dynamodb/dax/) (purpose-built for DynamoDB). This caching solution stays in the AWS cloud.

# Architecture

1. A monolithic architecture with a single database can be a starting point for prototyping and product validation.

2. As systems evolve into distributed microservices with database-per-service patterns, [microservices architecture with Saga pattern](https://github.com/camponogaraviera/full-stack-ai-sw-roadmap/blob/main/system_design/patterns.md) becomes a useful approach for isolation and data consistency. The SAGA workflow can be implemented serverless using `AWS API Gateway`, `AWS Step Functions`, `AWS Lambda`, and `Amazon DynamoDB`. Instead of deleting records, systems typically rely on state transitions and compensating actions to maintain consistency.

---

# Estimations

## Traffic

Suppose the system has 122M DAUs, and each user searches/watches 10 videos per day on average.

- Search requests per day:

$$
122M \space (users) \times 10 \space (searches) = 1.22B \space searches/day
$$

- Number of Requests Per Second (RPS):

$$
\frac{1.22B}{(24 \times 3600 \space seconds)} \sim 14,120 \space RPS
$$

## Storage

Suppose each user uploads 5 videos of 100MB per day on average.

- Storage per user per day: 5 \* 100 MB = 500 MB = 0.5 GB
- Daily storage usage for 122M DAU = 122M users \* 0.5GB/user = 61M GB = 61,000 TB = 61PB/Day.
- Monthly storage usage = 61PB/Day \* 30 = 1,830 PB/month.

## Bandwidth

The Bandwidth (data transfer), assuming all 61 PB/day of uploaded video data enters the system:

$$
\frac{61 \space PB}{(24 \times 3600 \space seconds)} \sim 706 \space GB/second
$$

---

# Normalized Database Design

Unlike a microservice architecture, where services are decoupled and independently deployed, a monolithic architecture consolidates all business logic within a single application/code base deployed as a unit.

Within a monolithic architecture, it is common to have a single relational database for the entire application, which might include multiple tables that interact through JOIN operations.

## Entity-Relationship Diagram (ERD)

...

## Entity Chart

- In a relational database, each entity is represented by a table.
- In a NoSQL database, entities are the nouns of the application.

A minimal version of the system has the following entity chart:

| Entity  | Primary Key: PK | Sort Key: SK  |
| ------- | --------------- | ------------- |
| User    | USER#username   | USER#username |
| Videos  | VIDEO#VideoID   | VIDEO#VideoID |
| Reviews | VIDEO#VideoID   | REV#ReviewID  |

## Single Relational Database Design

1. Users Table:
   - Primary Key: userID (uuid)
   - name (varchar)
   - email (varchar)
   - role (varchar) - Member or Admin
   - createdAt (timestamp)

2. Videos Table:
   - Primary Key: videoID (uuid)
   - userID (uuid) - Foreign Key referencing Users table
   - videoTitle (varchar)
   - description (varchar)
   - thumbURL (varchar or JSON) - JSON array for storing thumbnail URL
   - videoURL (varchar or JSON) - JSON array for storing video URL
   - uploadedAt (timestamp)

3. Reviews Table:
   - Primary Key: reviewID (uuid)
   - userID (uuid) - Foreign Key referencing the Users table
   - videoID (uuid) - Foreign Key referencing the Videos table
   - rating (int)
   - comment (varchar)
   - createdAt (timestamp)

---

# Denormalized Database Design

In a microservice architecture, the [database-per-service-pattern](https://docs.aws.amazon.com/prescriptive-guidance/latest/modernization-data-persistence/database-per-service.html) is a best practice to ensure loose coupling and independent scalability. This pattern uses a single database instance (SQL or NoSQL) per microservice.

When using DynamoDB for a microservice, a denormalized single-table design per service is the recommended approach.

While it is technically possible to use a single DynamoDB table for all microservices, this is strongly discouraged due to coupling, access control, and scalability risks, even with careful PK/SK design.

## DynamoDB [Access (Query) Patterns](https://docs.aws.amazon.com/prescriptive-guidance/latest/dynamodb-data-modeling/step3.html)

Access patterns are required to be known before modeling DynamoDB Tables. Recall that when there is more than one entity (item) and a `fetch many` access pattern, a composite primary key should be used to satisfy the required access pattern.

1. Register a User/Sign up unique on both username and email address (**not required when using Amazon Cognito**):
   - Partition Key (PK): `USER#username`
   - Sort key (SK): `USER#username`
   - Attributes: `Username`, `Email`, `Name`, `Role`, `DateJoined`, etc.
   - Post: use `PutItem` with `PK` and `SK` both as `USER#username`.

2. Upload a Video:
   - Partition Key (PK): `VIDEO#VideoID`
   - Sort key (SK): `VIDEO#VideoID`
   - Attributes: `VideoID`, `VideoTitle`, `Description`, `Timestamp`, `URL`, etc.
   - Post: use `PutItem` with `PK` and `SK` both as `VIDEO#VideoID`.
3. Fetch all Videos from a particular User:
   - Partition Key (PK): `USER#username`
   - Sort key (SK): `VIDEO#VideoID`
   - Attributes: `VideoID`, `VideoTitle`, `Description`, `Timestamp`, `URL`, etc.
   - Get: use `Query` with `PK` as `USER#username` and a sort key condition starting with `VIDEO#`.

4. Fetch all Reviews from a particular Video:
   - Partition Key (PK): `VIDEO#VideoID`
   - Sort key (SK): `REV#ReviewID`
   - Attributes: `Username`, `Rating`, `Feedback`, etc.
   - Get: use `Query` with `PK` as `VIDEO#VideoID` and a sort key condition starting with `REV#`.

5. Search for a Video:
   - DynamoDB:
     - Partition Key (PK): `VIDEO#<VideoID>`
     - Sort key (SK): `METADATA`
     - Store video metadata such as title, description, tags, etc.
   - OpenSearch:
     - Index searchable fields such as title, description, and tags.
     - [Perform a search query using OpenSearch](https://aws.amazon.com/blogs/database/implementing-search-on-amazon-dynamodb-data-using-zero-etl-integration-with-amazon-opensearch-service/).

## DynamoDB Single Table Design

- Recall that access patterns such as `fetch a user and videos from user` require a `pre-join`, i.e., the Video items should belong to the same partition key (same item collection) as the parent item (e.g., USER#username). This allows for fetching Users and Videos in a single request.

- In the following table, PK and SK are used as generic names to represent the partition key and sort key, respectively. This is called `primary key overloading`. Also, a prefix (e.g., USER#, VIDEO#, REV#) is added to each value of a partition key and sort key to avoid overlap between items with the same name (e.g., a user and a video with the same name), since a primary key must be unique across items.

<table>
  <tr> 
    <td colspan="2"> Primary Key </td>
    <td colspan="5">Attributes</td>
  </tr>
  <tr>
    <th>Partition Key: PK</th>
    <th>Sort Key: SK</th>
    <th>Attribute 1</th>
    <th>Attribute 2</th>
    <th>Attribute 3</th>
    <th>Attribute 4</th>
    <td>Attribute 5</td>
  </tr>
  <tr> 
  </tr> 
  <tr>
    <td rowspan="2">USER#jamesbond</td>
    <td colspan="1">  </td>
    <td> Username </td>
    <td> Email </td>
    <td> Name </td>
    <td> Role </td>
    <td> DateJoined </td>
  </tr>
  <tr>
    <td>USER#jamesbond</td>
    <td>"jamesbond"</td>
    <td>"jamesbond@example.com"</td>
    <td>"James Bond"</td>
    <td>"admin"</td>
    <td>2024-06-01T00:00:00Z</td>
  </tr>
  <tr>
    <td rowspan="4">Item Collection<br>USER#sherlock</td>
    <td colspan="1">  </td>
    <td>VideoID</td>
    <td>VideoTitle</td>
    <td>Description</td>
    <td>Timestamp</td>
    <td>URL</td>
  </tr>
  <tr>
    <td>VIDEO#001</td>
    <td>001</td>
    <td>"Sherlock Holmes"</td>
    <td>"Granada Television"</td>
    <td>2024-06-01T10:00:00Z</td>
    <td>{url: '...'}</td>
  </tr>
  <tr>
    <td colspan="1">  </td>
    <td> Username </td>
    <td> Email </td>
    <td> Name </td>
    <td> Role </td>
    <td> DateJoined </td>
  </tr>
  <tr>
    <td>USER#sherlock</td>
    <td>"sherlock"</td>
    <td>"sherlock@example.com"</td>
    <td>"Sherlock Holmes"</td>
    <td>"user"</td>
    <td>2024-06-01T00:00:00Z</td>
  </tr>
  <tr>
    <td rowspan="2">VIDEO#001</td>
    <td colspan="1">  </td>
    <td> Username </td>
    <td> Rating </td>
    <td> Feedback </td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>REV#001</td>
    <td>"jamesbond"</td>
    <td>5</td>
    <td>"Superb!"</td>
    <td></td>
    <td></td>
  </tr>
</table>

---

# AWS Pricing

Try the [AWS Calculator](https://calculator.aws/#/addService) to get a quotation for each AWS service used.
