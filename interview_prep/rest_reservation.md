<div align='center'>
    <h1> 5. Mock System Design Interviews with AWS </h1>
    <h2> Design a Restaurant Reservation System </h2>
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
  - [AWS Step Functions](#aws-step-functions)
- [Double Booking and Database Contention](#double-booking-and-database-contention)
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

- Log in with email and password.
- Search for a restaurant.
- Make a reservation.
- Choose the reservation date and time.
- Specify the reservation length.
- Specify the party size.
- Specify dietary restrictions.
- Cancel a reservation.
- Choose a payment method.
- Receive an email confirmation.
- Rate a restaurant and submit reviews/feedback.
- Save favorite restaurants.

The restaurant owner should be able to:

- Register a restaurant.
- Upload `images` of the restaurant.
- Receive booking confirmations, with details of schedule, party size, and reservation length.
- Receive online payments.
- Get analytics and feedback from customers.

---

# Non-Functional Requirements

The system should:

- Scale with increasing user base.
- Use a database that prioritizes availability and partition tolerance (AP system) over strong consistency.
- Be available and fault-tolerant (without losing uploads).
- Handle light throughput (~10k RPS).
- Have fast loading times and minimal latency.

---

# Infrastructure

1. `Scalability`:

- A normalized database design with `PostgreSQL` can be a starting point for an average-sized app. This avoids complex update operations in a denormalized database.
- As Access Patterns require more complex join operations and performance bottlenecks emerge, a NoSQL database can be a viable alternative for scalability.
- Scalability requires a horizontally partitioned distributed database to store restaurant metadata. According to the CAP theorem, prioritizing [availability and partition tolerance](https://github.com/camponogaraviera/full-stack-ai-sw-roadmap/blob/main/backend/database/core/cap_theorem.md) implies that `Cassandra`, `CouchDB`, or [DynamoDB](https://github.com/camponogaraviera/full-stack-roadmap/blob/main/backend/database/technologies/dynamodb.md) can be used.
- NoSQL databases are designed for `horizontal scalability at the database level (sharding)` while having the advantage of avoiding JOIN operations at the cost of redundancy.
- Implement database sharding to split table rows across multiple shards (nodes, servers), improving query performance, scalability, and fault tolerance. [Sharding](https://github.com/camponogaraviera/full-stack-ai-sw-roadmap/blob/main/system_design/horizontal_scaling.md) can be achieved using a shard key.

2. `Availability`:

- Configured AWS S3 for storing static assets (images/binary blobs, JS, CSS), leveraging automatic scaling to handle peak traffic and multi-AZ replication to ensure availability.

- Use **DynamoDB in on-demand mode** (auto-scaling), instead of provisioned mode, to automatically adjust read and write capacity units based on traffic, preventing [Throttling](https://github.com/camponogaraviera/full-stack-ai-sw-roadmap/blob/main/system_design/celebrity.md).

- Configure a dedicated compute instance to handle peak traffic.

3. `Fault Tolerance`:

- Use cold/warm/hot standbys to ensure fast failover.
- Use **multi-AZ deployments** for backend services (e.g., Lambda).
- Use **Route 53 health checks** and **failover routing** to reroute users during regional failures.
- Set up database replicas for read-heavy workloads, reducing hits to a single database server, avoiding single points of failure, and enhancing availability and fault tolerance. In AWS, [global tables](https://aws.amazon.com/dynamodb/global-tables/) can be used to replicate DynamoDB tables automatically.

4. `Low-throughput`:

- Use **API Gateway**, which provides built-in support for routing HTTP requests to backend services such as **AWS Lambda**.
- Use **AWS Lambda** as the compute layer. It automatically scales based on incoming requests, eliminating the need to manually scale compute resources such as EC2 instances.

5. `Fast Loading Times and Minimal Latency`:

- Edge Layer (CDN): Use a CDN to cache and serve static assets (e.g., images, JS, CSS) from geographically distributed edge locations close to users. This reduces round-trip latency for users worldwide, decreases load on origin servers, lowers request counts to object storage (e.g., S3 bucket), and speeds up content delivery.

- Application Layer (Distributed Caching): Implement distributed application-level caching to store frequently accessed or computationally expensive data (e.g., user profile data, feed queries, metadata, API responses), reducing query latency and database load.
  - Use TTL to balance freshness and performance.
  - Implement cache pre-warming for high-traffic or predictable workloads to prevent cold-start latency spikes.

---

# Services

1. `Frontend`:

- The UI can be implemented with React.js (web), React Native (mobile), or Lynx.
- The frontend (static part) can be hosted with built-in SSL/TLS certificate support via [AWS Amplify](https://github.com/camponogaraviera/aws/blob/main/services/hosting/hosting.md#aws-amplify).

2. `Backend API for Communication`:

- The API for client-server communication can be a [RESTful API](https://github.com/camponogaraviera/full-stack-roadmap/blob/main/backend/api/restfull_api.md) implemented serverless with [Amazon API Gateway HTTP](https://aws.amazon.com/api-gateway/) + [AWS Lambda](https://aws.amazon.com/lambda/).
  - API Gateway (29-sec timeout) provides a single entry point for clients to interact with various backend services, handling HTTP geo-routing, authentication (Cognito/IAM), throttling, caching, and WebSockets.
  - Lambda executes API logic. It is suitable for short-lived, event-driven applications within Lambda's constraints (15-minute timeout, max [10GB RAM, and 6 vCPU cores](https://aws.amazon.com/about-aws/whats-new/2021/07/aws-lambda-supports-10-gb-memory-6-vcpu-cores-bahrain-osaka-hong-kong-regions/)). **CRUD operations should be implemented directly on Lambda**.
- As fetching becomes complex and performance bottlenecks emerge, a [GraphQL API](https://github.com/camponogaraviera/full-stack-roadmap/blob/main/backend/api/grahql.md), implemented with [AppSync](https://aws.amazon.com/appsync/)+[Amplify](https://aws.amazon.com/amplify/) or [graphql-http](https://graphql.org/blog/2022-11-07-graphql-http/), can be a viable alternative.

3. `Security`: User authentication (sign-up, sign-in) and authorization can be implemented with [Amazon Cognito](https://aws.amazon.com/pm/cognito/) and [AWS IAM](https://aws.amazon.com/iam/), respectively. Cognito automatically handles the storage of user credentials (e.g., passwords, tokens) and metadata (e.g., email, phone number, username) inside **Cognito User Pools**.

4. `Storage`: [Amazon S3](https://aws.amazon.com/s3/) can be used to store compressed images and other binary data with auto-replication.

5. `Database`: PostgreSQL or DynamoDB can be used to store user preferences/settings, restaurant metadata (name, description, etc.), bookings, reviews, and favourites.

6. `Search Engine:` The search engine can be implemented with [Elasticsearch](https://www.elastic.co/elasticsearch), [OpenSearch](https://aws.amazon.com/what-is/opensearch/), or [Amazon CloudSearch](https://aws.amazon.com/cloudsearch/).

7. `URL Sharing:` Users can share their favorite restaurants using a `URL shortener service`.

8. `CDN`: Used to speed up `content delivery`. The CDN takes static data from [Amazon S3 Storage](https://aws.amazon.com/s3/). Technologies: [Amazon CloudFront](https://aws.amazon.com/cloudfront/).

9. `Caching`: [Amazon Elasticache](https://aws.amazon.com/pm/elasticache/) (general-purpose), or [Amazon DAX](https://aws.amazon.com/dynamodb/dax/) (purpose-built for DynamoDB). This caching solution stays in the AWS cloud.

10. `Payments:` Payment transactions can be processed with `Stripe` or `PayPal`.

11. `Message Queuing:` [Amazon SQS](https://aws.amazon.com/sqs/) can be used to allow asynchronous communication between microservices, such as reservation requests, payment processing, and notification dispatching.

12. `Email Confirmation:` After a reservation is successfully created, an email confirmation template with relevant reservation details can be triggered via an [AWS Lambda function](https://aws.amazon.com/lambda/). Customers and restaurant owners can then receive the email from an SMTP server (e.g., using `Nodemailer`) or through an email service provider's API (e.g., [Amazon SES API](https://docs.aws.amazon.com/ses/latest/dg/send-email-api.html) or [SendGrid](https://sendgrid.com/en-us).

13. `Push Notification:` [Amazon SNS](https://aws.amazon.com/sns/) can be used to notify users about booking confirmations via SMS or notify services about state changes (e.g., a reservation update).

14. `Metrics and Analytics`: DynamoDB does not support analytical operations out of the box. One solution is to combine services such as [DynamoDB Streams](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html) and [Amazon Kinesis Data Analytics](https://docs.aws.amazon.com/kinesisanalytics/latest/dev/what-is.html) to process and aggregate data in real-time. A custom Lambda function can be set up to be triggered whenever data changes in a specific DynamoDB table to be analyzed. [Amazon QuickSight](https://docs.aws.amazon.com/managedservices/latest/userguide/quicksight.html) can be used to create dashboards for analytics, data visualization, and reporting.

15. `Serverless Orchestration Workflow`: [AWS Step Functions](https://aws.amazon.com/step-functions/) can be used to manage bookings, notify the restaurant, send customer reminders, or handle cancellations.

---

# Estimations

## Traffic

Suppose the system has 500k DAUs, and each user makes 10 requests for a particular location per day, on average.

- Search requests per day:

$$
500k \space (users) \times 10 \space (searches) = 5M \space searches/day
$$

- Number of Requests Per Second (RPS):

$$
\frac{5M}{(24 \times 3600 \space seconds)} \sim 58 \space RPS
$$

## Storage

Suppose that 10% of DAU are restaurant owners sharing 5 photos of 1MB per day (0.005GB/user), on average.

- Daily storage usage for 50k DAU = 50k users \* 0.005GB/user = 250 GB/day.
- Monthly storage usage = 250 GB/day \* 30 days = 7,5 TB/month.

## Bandwidth

The Bandwidth (data transfer), considering an ingress of 250 GB of data stored per day:

$$
\frac{250 \space GB}{(24 \times 3600 \space seconds)} \sim 2.9 \space MB/second
$$

---

# Architecture

1. A monolithic architecture with a single database can be a starting point for prototyping and product validation.

2. As systems evolve into distributed microservices with database-per-service patterns, [microservices architecture with Saga pattern](https://github.com/camponogaraviera/full-stack-ai-sw-roadmap/blob/main/system_design/patterns.md) becomes a useful approach for isolation and data consistency. The SAGA workflow can be implemented serverless using `AWS API Gateway`, `AWS Step Functions`, `AWS Lambda`, and `Amazon DynamoDB`. Instead of deleting records, systems typically rely on state transitions and compensating actions to maintain consistency.

## AWS Step Functions

In the workflow of a [database-per-service pattern](https://docs.aws.amazon.com/prescriptive-guidance/latest/modernization-data-persistence/database-per-service.html) (a single database for each microservice), using a DynamoDB table for Restaurants and another for Payments, the following [AWS Step Functions](https://aws.amazon.com/step-functions/) can be used for serverless Saga orchestration:

1. Book/reserve a restaurant: inserts a record into the DynamoDB Restaurants table with a `reservationStatus = pending`.

2. Process a payment:

3. Call an external payment API and insert a record into the DynamoDB Payments table with a `paymentStatus = succeeded | failed`.

4. If payment fails, update the reservation record status in the DynamoDB Restaurants table to `reservationStatus = cancelled`.

5. If payment is successful, `continue`.

6. Confirm a reservation: update the reservation record status to `reservationStatus = confirmed`.

7. If confirmation fails, refund the payment and update the reservation record status to `reservationStatus = cancelled`.

---

# Double Booking and Database Contention

Two customers competing for the same restaurant reservation slot is an example of [database contention](https://github.com/camponogaraviera/full-stack-ai-sw-roadmap/blob/main/backend/database/core/contention.md).

To prevent database contention, the following strategies can be implemented: `query optimization with indexing (for relational databases)`, `database denormalization`, and concurrency control via `Locking and serialization`.

---

# Entity Chart

- In a relational database, each entity is represented by a table.
- In a NoSQL database, entities are the nouns of the application.

A minimal version of the system has the following entity chart:

| Entity      | Primary Key: PK   | Sort Key: SK      |
| ----------- | ----------------- | ----------------- |
| User        | USER#username     | USER#username     |
| Restaurants | REST#RestaurantID | REST#RestaurantID |
| Bookings    | USER#username     | BOOK#BookingID    |
| Payments    | USER#username     | PAY#PaymentID     |
| Reviews     | REST#RestaurantID | REV#ReviewID      |

---

# Normalized Database Design

Unlike a microservice architecture, where services are decoupled and independently deployed, a monolithic architecture consolidates all business logic within a single application/code base deployed as a unit.

Within a monolithic architecture, it is common to have a single relational database for the entire application, which might include multiple tables that interact through JOIN operations.

## Entity-Relationship Diagram (ERD)

...

## Single Relational Database Design

1. Users Table:
   - Primary Key: userID (uuid)
   - name (varchar)
   - email (varchar)
   - phoneNumber (varchar)
   - favouriteRestaurants (JSON)
   - role (varchar) - customer or restaurant owner
   - createdAt (timestamp)

2. Restaurants Table:
   - Primary Key: restaurantID (uuid)
   - userID (uuid) - Foreign Key for owner's ID referencing Users table
   - name (varchar)
   - description (varchar)
   - address (varchar or point)
   - ContactInfo (JSON)
   - CuisineType (varchar)
   - OpeningHours (varchar)
   - availableTables (JSON) - available tables and seats per table
   - availableCreditCards (JSON)
   - imageURL (varchar or JSON) - JSON array for storing image URLs

3. Bookings Table:
   - Primary Key: bookingID (uuid)
   - userID (uuid) - Foreign Key referencing Users table
   - restaurantID (uuid) - Foreign Key referencing the Restaurants table
   - reservationDate (timestamp)
   - partySize (int)
   - reservationLength (int)
   - dietaryRestrictions (varchar)
   - paymentStatus (enum) - Pending/Confirmed/Cancelled

4. Payments Table:
   - Primary Key: paymentID (uuid)
   - bookingID (uuid) - Foreign Key referencing Bookings table
   - transactionID (uuid)
   - paymentMethod (enum) - Visa, Mastercard, PayPal, etc.
   - amount (float)
   - paymentStatus (enum) - Completed/Denied
   - paymentDate (timestamp)

5. Reviews Table:
   - Primary Key: reviewID (uuid)
   - userID (uuid) - Foreign Key referencing the Users table
   - restaurantID (uuid) - Foreign Key referencing the Restaurants table
   - rating (int)
   - feedback (varchar)
   - reviewDate (timestamp)

# Denormalized Database Design

In a microservice architecture, the [database-per-service-pattern](https://docs.aws.amazon.com/prescriptive-guidance/latest/modernization-data-persistence/database-per-service.html) is a best practice to ensure loose coupling and independent scalability. This pattern uses a single database instance (SQL or NoSQL) per microservice.

When using DynamoDB for microservices, a denormalized single-table design per service is the recommended approach.

While it is technically possible to use a single DynamoDB table for all microservices, this is strongly discouraged due to coupling, access control, and scalability risks, even with careful PK/SK design.

## DynamoDB [Access (Query) Patterns](https://docs.aws.amazon.com/prescriptive-guidance/latest/dynamodb-data-modeling/step3.html)

Access patterns are required to be known before modeling DynamoDB Tables. Recall that when there is more than one entity (item) and a `fetch many` access pattern, a composite primary key should be used to satisfy the required access pattern.

1. Register a User unique on both username and email address (**not required when using Amazon Cognito**):
   - Partition Key (PK): `USER#username`
   - Sort key (SK): `USER#username`
   - Attributes: `username`, `email`, `name`, `role`, etc.
   - Post: use `TransactWriteItems` to create an item with `PK` and `SK` both as `USER#username`.
2. Register a Restaurant (unique on ID):
   - Partition Key (PK): `REST#RestaurantID`
   - Sort key (SK): `REST#RestaurantID`
   - Attributes: `restaurantName`, `address`, `cuisineType`, etc.
   - Post: use `PutItem` with `PK` and `SK` both as `REST#RestaurantID`.
3. Book a Restaurant:
   - Partition Key (PK): `USER#username`
   - Sort key (SK): `#BOOK#BookingID` (KSUID)
   - Attributes: `restaurantID`, `reservationDate`, `reservationLength`, `partySize`, `dietaryRestrictions`, `paymentStatus`.
   - Post: use `TransactWriteItems` with `PK` as `USER#username` and `SK` as `BOOK#BookingID`.
4. Update a Booking:
   - Partition Key (PK): `USER#username`
   - Sort key (SK): `BOOK#BookingID`
   - Put: use `UpdateItem` with `PK` as `USER#username` and `SK` as `BOOK#BookingID`.
5. Fetch the most recent Bookings for a particular User:
   - Partition Key (PK): `USER#username`
   - Sort key (SK): `BOOK#BookingID`
   - Attributes: `restaurantID`, `reservationDate`, `partySize`, `dietaryRestrictions`, `paymentStatus`.
   - Get: use `Query` with `ScanIndexForward=False`, `PK` as `USER#username` and a sort key condition starting with `BOOK#`.
6. Fetch all Restaurants from a particular User:
   - Partition Key (PK): `USER#username`
   - Sort key (SK): `REST#RestaurantID`
   - Attributes: `restaurantName`, `address`, `cuisineType`, `ownerEmail`, etc.
   - Get: use `Query` with `PK` as `USER#username` and a sort key condition starting with `REST#`.
7. Fetch all Restaurants from a particular Location:
   - Partition Key (PK): `LOC#LocationID`
   - Sort key (SK): `REST#RestaurantID`
   - Attributes: `restaurantID`, `restaurantName`, `address`, `cuisineType`, `ownerEmail`, etc.
   - Get: use `Query` with `PK` as `LOC#LocationID` and a sort key condition starting with `REST#`.

8. Fetch all Payments from a particular User:
   - Partition Key (PK): `USER#username`
   - Sort key (SK): `PAY#PaymentID`
   - Attributes: `date`, `amount`, `paymentStatus`, `bookingID`, etc.
   - Get: use `Query` with `PK` as `USER#username` and a sort key condition starting with `PAY#`.
9. Fetch all Reviews from a particular Restaurant:
   - Partition Key (PK): `REST#RestaurantID`
   - Sort key (SK): `REV#ReviewID`
   - Attributes: `Username`, `Rating`, `Feedback`, etc.
   - Get: use `Query` with `PK` as `REST#RestaurantID` and a sort key condition starting with `REV#`.

## DynamoDB Single Table Design

- Recall that access patterns such as `fetch user and bookings from user` (one-to-many relationship) require a `pre-join`, i.e., the Booking items should belong to the same partition key (same item collection) as the parent item (e.g., USER#username). This allows for fetching Users and Bookings in a single request. The same applies to fetching restaurants from locations, etc.

- In the following table, PK and SK are used as generic names to represent the partition key and sort key, respectively. This is called `primary key overloading`. Also, a prefix (e.g., USER#, REST#, REV#, PAY#) is added to each value of a partition key and sort key to avoid overlapping between items with the same name (e.g., a user and a restaurant with the same name), since a primary key must be unique across items.

<table>
  <tr> 
    <td colspan="2"> Primary Key </td>
    <td colspan="6">Attributes</td>
  </tr>
    <th>Partition Key: PK</th>
    <th>Sort Key: SK</th>
    <th>Attribute 1</th>
    <th>Attribute 2</th>
    <th>Attribute 3</th>
    <th>Attribute 4</th>
    <th>Attribute 5</th>
    <th>Attribute 6</th>
  </tr>
  <tr>  
  </tr>	
  <tr>
    <td rowspan="4">Item Collection<br>USER#sherlock</td>
    <td colspan="1">  </td>
    <td>restaurantName</td>
    <td>address</td>
    <td>cuisineType</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>REST#221</td>
    <td>The Mazarin Stone</td>
    <td>{Street: 221B Baker}</td>
    <td>"Vegan" </td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td colspan="1">  </td>
    <td> Username </td>
    <td> Email </td>
    <td> Name </td>
    <td> Role </td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>USER#sherlock</td>
    <td>sherlock</td>
    <td>sherlock@example.com</td>
    <td>Sherlock Holmes</td>
    <td>Restaurant Owner</td>
    <td></td>
    <td></td>
  </tr>

  <tr>
    <td rowspan="6">Item Collection<br>USER#jamesbond</td>
    <td colspan="1">  </td>
    <td>restaurantName</td>
    <td>address</td>
    <td>cuisineType</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>REST#007</td>
    <td>Goldfinger</td>
    <td>{Street: 007B Square}</td>
    <td>"English" </td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td colspan="1">  </td>
    <td> restaurantID </td>
    <td> reservationDate </td>
    <td> reservationLength </td>
    <td> partySize </td>
    <td> dietaryRestrictions </td>
    <td> paymentStatus </td>
  </tr>
  <tr>
    <td>#BOOK#001</td>
    <td>001</td>
    <td>2024-06-01T10:00:00Z</td>
    <td>"1 hour"</td>
    <td>"Table for 2"</td>
    <td> "Peanut Allergy" </td>
    <td>"Pending"</td>
  </tr>
  <tr>
    <td colspan="1">  </td>
    <td> Username </td>
    <td> Email </td>
    <td> Name </td>
    <td> Role </td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>USER#jamesbond</td>
    <td>jamesbond</td>
    <td>jamesbond@example.com</td>
    <td>James Bond</td>
    <td>Restaurant Owner</td>
    <td></td>
    <td></td>
  </tr>
  
  <tr>
    <td rowspan="4">Item Collection<br>LOC#LDN</td>
    <td colspan="1">  </td>
    <td>restaurantID</td>
    <td>restaurantName</td>
    <td>address</td>
    <td>cuisineType</td>
    <td>ownerEmail</td>
    <td></td>
  </tr>
  <tr>
    <td>REST#221</td>
    <td>221</td>
    <td>The Mazarin Stone</td>
    <td>{Street: 221B Baker}</td>
    <td>"Vegan" </td>
    <td>sherlock@example.com</td>
    <td></td>
  </tr>
  <tr>
    <td colspan="1">  </td>
    <td>restaurantID</td>
    <td>restaurantName</td>
    <td>address</td>
    <td>cuisineType</td>
    <td>ownerEmail</td>
    <td></td>
  </tr>
  <tr>
    <td>REST#007</td>
    <td>007</td>
    <td>Goldfinger</td>
    <td>{Street: 007B Square}</td>
    <td>"English" </td>
    <td>jamesbond@example.com</td>
    <td></td>
  </tr>
 
  <tr>
    <td rowspan="2">USER#jamesbond</td>
    <td colspan="1">  </td>
    <td>date</td>
    <td>amount</td>
    <td>paymentStatus</td>
    <td>bookingID</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>PAY#001</td>
    <td>2024-06-01T10:00:00Z</td>
    <td>50</td>
    <td>Completed</td>
    <td>001</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td rowspan="2">REST#221</td>
    <td colspan="1">  </td>
    <td> Username </td>
    <td> Rating </td>
    <td> Feedback </td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>REV#001</td>
    <td>jamesbond</td>
    <td>5</td>
    <td>"Superb!"</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</table>

---

# AWS Pricing

Try the [AWS Calculator](https://calculator.aws/#/addService) to get a quotation for each AWS service used.
