<!-- Shields -->

[![Contributions](https://img.shields.io/badge/contributions-welcome-orange?style=flat-square)](https://github.com/camponogaraviera/full-stack-roadmap/pulls)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://github.com/camponogaraviera/full-stack-roadmap/graphs/commit-activity)

<!-- Dependencies -->

<a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript" target="_blank" rel="noopener noreferrer"><img src="https://github.com/camponogaraviera/logos/blob/main/assets/javascript.png" width="35"></a>
&nbsp;
&nbsp;
<a href="https://www.python.org/" target="_blank" rel="noopener noreferrer"><img src="https://github.com/camponogaraviera/logos/blob/main/assets/python.png" width="120"></a>
<a href="https://react.dev/" target="_blank" rel="noopener noreferrer"><img src="https://github.com/camponogaraviera/logos/blob/main/assets/react.svg" width="35"></a>
&nbsp;
&nbsp;
<a href="https://nodejs.org/en" target="_blank" rel="noopener noreferrer"><img src="https://github.com/camponogaraviera/logos/blob/main/assets/node.png" width="65"></a>
&nbsp;
&nbsp;
<a href="https://graphql.org/" target="_blank" rel="noopener noreferrer"><img src="https://github.com/camponogaraviera/logos/blob/main/assets/graphql.svg" width="40"></a>
&nbsp;
&nbsp;
<a href="https://pytorch.org/" target="_blank" rel="noopener noreferrer"><img src="https://github.com/camponogaraviera/logos/blob/main/assets/pytorch.png" width="110"></a>
&nbsp;
&nbsp;
<a href="https://aws.amazon.com/" target="_blank" rel="noopener noreferrer"><img src="https://github.com/camponogaraviera/logos/blob/main/assets/aws.png" width="50"></a>
<br>
<br>

<!-- Title -->

<div align='center'>
  <h1> Full-Stack AI Software Engineer Roadmap </h1>
</div>

# Introduction

This course provides a strong foundation for designing and developing production-grade full-stack AI web and mobile applications that scale.

Implementations follow official documentation, and references are provided throughout the course.

# Table of Contents

- [1. Tooling](#tooling)
  - 1.1 Version Control
  - 1.2 Environment Management
  - 1.3 Package Management

- [2. Programming Fundamentals](#programming)
  - 2.1 Modern JavaScript (ES6+): Fundamentals to Advanced Concepts
  - 2.2 TypeScript
  - 2.3 Data Structures and Algorithms in Python, Modern JavaScript (ES6+), and Modern C++

- [3. Full-Stack Development](#fullstack)
  - 3.1 Software Design & Architecture
  - 3.2 Frontend
  - 3.3 Backend
  - 3.4 System Design
  - 3.5 Software Development Life Cycle (SDLC)
  - 3.6 AWS Roadmap + Technical Interview

- [4. AI Roadmap](#ai) (Theory and Implementation)
  - 4.1 Deep Learning Roadmap + Technical Interview
  - 4.2 TensorFlow & PyTorch: API Tutorial
  - 4.3 Reinforcement Learning Roadmap: Theory and Implementations of Deep Reinforcement Learning Algorithms in TensorFlow and PyTorch
  - 4.4 Large Language Model Roadmap: End-to-end Large Language Models in PyTorch + Technical Interview

- [5. Mock System Design Interviews with AWS](#mock)
  - Design a Restaurant Reservation System
  - Design a YouTube-like On-Demand Video Streaming Service
  - Design a Search Engine like Google

- [6. Production-Grade Applications](#apps)
  - Chat Horizon
  - Social Eats

- [References](#references)

<!-- #region 1. Tooling -->
<details>
  <summary><h1 id="tooling">1. Tooling</h1></summary>
  
  - Version Control:
    - [SemVer](https://github.com/camponogaraviera/nvm-npm-yarn?tab=readme-ov-file#semantic-versioning-semver)
    - [Git](https://github.com/camponogaraviera/linux-git-conda/blob/main/github_essentials/README.md)
  
  - Environment Management:
    - [Conda](https://github.com/camponogaraviera/linux-git-conda/blob/main/conda_essentials/README.md)
    - [NVM](https://github.com/camponogaraviera/nvm-npm-yarn?tab=readme-ov-file#node-version-manager-nvm)
  
  - Package Management:
    - [NPM](https://github.com/camponogaraviera/nvm-npm-yarn?tab=readme-ov-file#node-package-manager-npm)
    - [Yarn](https://github.com/camponogaraviera/nvm-npm-yarn?tab=readme-ov-file#yarn-package-manager)

</details>
<!-- #endregion -->

---

<!-- #region 2. Programming Fundamentals -->
<details>
  <summary><h1 id="programming">2. Programming Fundamentals</h1></summary>

  <!-- #region JavaScript -->
  <details>
    <summary><h2 id="javascript">2.1 Modern JavaScript (ES6+)</h2></summary>
    
  - [Modern JavaScript (ES6+): Fundamentals to Advanced Concepts](https://github.com/camponogaraviera/javascript)

    </details>
    <!-- #endregion -->

---

  <!-- #region TypeScript -->
  <details>
    <summary><h2 id="TypeScript">2.2 TypeScript</h2></summary>
    
  - [TypeScript](programming/typescript.md)

    </details>
    <!-- #endregion -->

---

  <!-- #region DSA -->
  <details>
    <summary><h2 id="dsa">2.3 Data Structures and Algorithms in Python, Modern JavaScript (ES6+), and Modern C++ </h2></summary>
  
  - [Theoretical Fundamentals](https://github.com/camponogaraviera/ds-and-algo/blob/main/ds_and_algo/theory/README.md) 
  - Implementations:
    - [Python](https://github.com/camponogaraviera/ds-and-algo/blob/main/ds_and_algo/implementations/python/README.md)
    - [Modern JavaScript (ES6+)](https://github.com/camponogaraviera/ds-and-algo/blob/main/ds_and_algo/implementations/javascript/README.md)
    - [Modern C++](https://github.com/camponogaraviera/ds-and-algo-cpp/blob/main/ds_and_algo_cpp/implementations/README.md)
  - [Interview Questions](https://github.com/camponogaraviera/ds-and-algo/blob/main/ds_and_algo/interview_prep/questions/README.md) 
  - LeetCode Problems
    - [Python](https://github.com/camponogaraviera/ds-and-algo/blob/main/ds_and_algo/interview_prep/leetcode/python/README.md)
    - [Modern JavaScript (ES6+)](https://github.com/camponogaraviera/ds-and-algo/blob/main/ds_and_algo/interview_prep/leetcode/javascript/README.md)
    - [Modern C++](https://github.com/camponogaraviera/ds-and-algo-cpp/blob/main/ds_and_algo_cpp/interview_prep/leetcode/README.md)
  
  </details>
  <!-- #endregion -->

</details>
<!-- #endregion -->

---

<!-- #region 3. Full-Stack -->
<details>
  <summary><h1 id="fullstack"> 3. Full-Stack  Development </h1></summary>

  <!-- #region Software Design & Architecture -->
  <details>
    <summary><h2 id="patterns"> 3.1 Software Design Principles & Patterns </h2></summary>

### 3.1.1 Software Design Principles

- [KISS](full_stack/design/kiss.md)
- [YAGNI](full_stack/design/yagni.md)
- [DRY](full_stack/design/dry.md)
- [SOLID](full_stack/design/solid.md)

### 3.1.2 Presentation-Layer Architectural Patterns

- [Model-View-Controller (MVC)](full_stack/architectural_patterns/mvc.md)
- [Model-View-Presenter (MVP)](full_stack/architectural_patterns/mvp.md)
- [Model-View-ViewModel (MVVM)](full_stack/architectural_patterns/mvvm.md)

### 3.1.3 Application/System Architectural Patterns

- [Layered Architecture](full_stack/architectural_patterns/layered.md)
- [Hexagonal Architecture](full_stack/architectural_patterns/hexagonal.md)
- [Onion Architecture](full_stack/architectural_patterns/onion.md)
- [Clean Architecture](full_stack/architectural_patterns/clean.md)

  </details>
  <!-- #endregion -->

---

  <!-- #region Frontend -->
  <details>
    <summary><h2 id="frontend">3.2 Frontend</h2></summary>

### 3.2.1 Web Application Architecture Styles

- [Multi-Page Application (MPA)](full_stack/frontend/architectures/mpa.md)
- [Single-Page Application (SPA)](full_stack/frontend/architectures/spa.md)

### 3.2.2 Rendering Strategies

- [Client-Side Rendering (CSR)](full_stack/frontend/rendering/csr.md)
- [Server-Side Rendering (SSR)](full_stack/frontend/rendering/ssr.md)
- [Static Site Generation (SSG)](full_stack/frontend/rendering/ssg.md)

### 3.2.3. Web Application Libraries, Frameworks, and Tools

- Libraries: [React.js](full_stack/frontend/web/reactjs.md)
- Frameworks: [Next.js](full_stack/frontend/web/nextjs.md)
- Tools: [Vite](full_stack/frontend/web/vite.md)

### 3.2.4 Mobile Application Frameworks

- [Fundamentals & Industry Best Practices of React Native with Hooks](https://github.com/camponogaraviera/react-native)
- [Flutter](full_stack/frontend/mobile/flutter.md)

### 3.2.5 State Management Patterns and Libraries

- State Management Patterns:
  - [Flux](full_stack/frontend/state/flux.md)

- State Management Libraries:
  - [Redux](full_stack/frontend/state/redux.md)

  </details>
  <!-- #endregion -->

---

  <!-- #region Backend -->
  <details>
    <summary><h2 id="backend">3.3 Backend</h2></summary>

### 3.3.1 Database Fundamentals

- Database Models
  - [Relational Databases](full_stack/backend/database/fundamentals/relational_db.md)
  - [Non-Relational (NoSQL) Databases](full_stack/backend/database/fundamentals/non_relational_db.md)
  - [Vector Databases](full_stack/backend/database/fundamentals/vector_db.md)

- Data Modeling & Schema Design
  - [Data Normalization vs Denormalization](full_stack/backend/database/fundamentals/norm_denorm.md)

- Data Architecture & Workload Patterns
  - [OLAP vs OLTP](full_stack/backend/database/fundamentals/olap_vs_oltp.md)
  - [Data Lake vs Data Warehouse](full_stack/backend/database/fundamentals/lake_vs_warehouse.md)

- Transactions, Consistency & Distributed Data
  - [ACID Properties](full_stack/backend/database/fundamentals/acid.md)
  - [BASE Properties](full_stack/backend/database/fundamentals/base.md)
  - [CAP Theorem a.k.a Brewer's Theorem](full_stack/backend/database/fundamentals/cap_theorem.md)
  - [Partitioning](full_stack/backend/database/fundamentals/partitioning.md)
    - Vertical Partitioning
    - Horizontal Partitioning a.k.a. Sharding

- Indexing & Query Optimization
  - [Query Optimization with Indexing](full_stack/backend/database/fundamentals/query_optimization.md)
  - [Geospatial Indexes](full_stack/backend/database/fundamentals/geo_spatial_indexes.md)
    - Introduction
    - Geohashes
    - Quadtrees
    - Applications of Geohashes and Quadtrees

### 3.3.2 Database Technologies

- [DynamoDB](full_stack/backend/database/technologies/dynamodb.md)
- [Neo4j](full_stack/backend/database/technologies/neo4j.md)

### 3.3.3 Data Access Technologies

- [ORM & ODM](full_stack/backend/database/technologies/orm_odm.md)

### 3.3.4 API Development and Communication

- API Communication
  - [Remote Procedure Call (RPC)](full_stack/backend/api/communication/rpc.md)
  - Real-Time Communication
    - [Polling vs. WebSockets](full_stack/backend/api/communication/web_sockets.md)
  - Real-Time Peer-to-Peer (P2P) Communication
    - [WebRTC](full_stack/backend/api/communication/webrtc.md)

- API Architectural Styles 
  - [RESTful APIs](full_stack/backend/api/arch_styles/restfull_api.md)

- API Query Languages
  - [GraphQL](full_stack/backend/api/query_langs/graphql.md)
    - [Apollo GraphQL](full_stack/backend/api/query_langs/apollo.md)

- API Frameworks
  - [gRPC](full_stack/backend/api/frameworks/grpc.md)

- API Security & Access Control
  - Authentication
    - [JSON Web Tokens](full_stack/backend/api/security/authentication/jwt.md)
    - [Refresh Tokens](full_stack/backend/api/security/authentication/refresh_tokens.md)
  - Browser Security
    - [CORS](full_stack/backend/api/security/cors.md)

  </details>
  <!-- #endregion -->

---

  <!-- #region System Design -->
  <details>
    <summary><h2 id="system_design">3.4 System Design</h2></summary>

### 3.4.1 Infrastructure & Networking

- [URL/DNS/SSL](full_stack/system_design/url_dns.md)
  - URL
  - Domain Name vs Domain Name System
  - Round-robin DNS
  - GeoDNS
  - SSL vs TLS
  - SSL Pinning
  - Infoblox Threat Defense
  - Registry vs Registrar

- [Servers](full_stack/system_design/servers.md)
  - Stateful Servers
  - Stateless Servers
  - Hybrid Systems

- [Vertical Scaling](full_stack/system_design/vertical_scaling.md)
  - Introduction

- [Horizontal Scaling](full_stack/system_design/horizontal_scaling.md)
  - Introduction
  - Load Balancer
  - Routing Algorithms
    - Round Robin
    - Weighted Round Robin
    - Least Connections
    - Weighted Least Connections
    - IP Hash
    - Least Response Time
    - Random
    - Least Bandwidth
  - Consistent Hashing

- [Database Replication](full_stack/system_design/replication.md)
  - Introduction
  - Multi-master Replication
  - Bidirectional Replication
  - Circular Replication

- Resiliency & High Availability
  - [Over-Provisioning for Resiliency](full_stack/system_design/over_provisioning.md)
  - [Cold standby](full_stack/system_design/cold_standby.md)
  - [Warm standby](full_stack/system_design/warm_standby.md)
  - [Hot standby](full_stack/system_design/hot_standby.md)

- [Prefetching and Caching](full_stack/system_design/prefetching_and_caching.md)
  - Prefetching
  - Caching
    - Expiration Policy
    - Eviction Cache Policies
    - The Cold-Start Problem
    - Cache Misses
    - Caching Technologies

- [Content Delivery Network (CDN)](full_stack/system_design/cdn.md)
  - Introduction
  - Use Cases

- [The Celebrity (a.k.a. Hot Spot) Problem](full_stack/system_design/celebrity.md)
  - Introduction

### 3.4.2 Concurrency

- [Deadlocks](full_stack/system_design/concurrency/deadlocks.md)
- [Contention](full_stack/system_design/concurrency/contention.md)

### 3.4.3 Performance

- [Thundering Herd](full_stack/system_design/performance/thundering.md)

### 3.4.4 Distributed System Architecture

- [Monolithic Architecture](full_stack/system_design/architecture/mono.md)
- [Microservices Architecture](full_stack/system_design/architecture/micro.md)

### 3.4.5 Distributed System Patterns

- Service Integration Patterns
  - [Backend for Frontend Pattern](full_stack/system_design/patterns/bff.md)
  - [API Composition Pattern](full_stack/system_design/patterns/api_cp.md)

- Data Management Patterns
  - [Database-Per-Service Pattern](full_stack/system_design/patterns/dpsp.md)
  - [Shared-Database-Per-Service Anti-Pattern](full_stack/system_design/patterns/sdpsap.md)
  - [CQRS Pattern](full_stack/system_design/patterns/cqrs.md)
  - [Event Sourcing Pattern](full_stack/system_design/patterns/event_sourcing.md)

- Distributed Transaction Patterns
  - [Saga Pattern](full_stack/system_design/patterns/saga.md)

- Messaging Patterns
  - [Publish-Subscribe Pattern](full_stack/system_design/patterns/ps.md)
  - [Dead-Letter Queue (DLQ)](full_stack/system_design/patterns/dlq.md)

- Reliability Patterns
  - [Retry Pattern](full_stack/system_design/patterns/retry.md)
  - [Circuit Breaker Pattern](full_stack/system_design/patterns/circuit_breaker.md)
  - [Bulkhead Pattern](full_stack/system_design/patterns/bulkhead.md)

  </details>
  <!-- #endregion -->

---

  <!-- #region SDLC -->
  <details>
    <summary><h2 id="sdlc">3.5 Software Development Life Cycle (SDLC)</h2></summary>

3.5.1 [Big Bang Model](full_stack/sdlc/big_bang.md)

3.5.2 [Waterfall Model](full_stack/sdlc/waterfall.md)

3.5.3 [Validation and Verification Model (V-Model)](full_stack/sdlc/v_model.md)

3.5.4 [Iterative Model](full_stack/sdlc/iterative.md)

3.5.5 [Incremental Model](full_stack/sdlc/incremental.md)

3.5.6 [Rapid Application Development (RAD) Model](full_stack/sdlc/rad.md)

3.5.7 [Spiral Model](full_stack/sdlc/spiral.md)

3.5.8 [Agile Model](full_stack/sdlc/agile.md)

3.5.9 [DevOps Model](full_stack/sdlc/devops.md)

  </details>
  <!-- #endregion -->

---

  <!-- #region AWS Roadmap -->
  <details>
    <summary><h2 id="aws">3.6 AWS Roadmap</h2></summary>
      
  - [AWS Roadmap + Technical Interview](https://github.com/camponogaraviera/aws) 
  
  </details>
  <!-- #endregion -->

</details>
<!-- #endregion -->

---

<!-- #region 4. AI Roadmap -->
<details>
  <summary><h1 id="ai">4. AI Roadmap </h1></summary>

(Open-source)

## 4.1 [TensorFlow & PyTorch: API Tutorial](https://github.com/camponogaraviera/tf-torch)

(Private)

## 4.2 [Deep Learning Roadmap + Technical Interview](https://github.com/camponogaraviera/deep-learning-intro)

## 4.3 [Reinforcement Learning Roadmap: Theory and Implementations of Deep Reinforcement Learning Algorithms in TensorFlow and PyTorch](https://github.com/camponogaraviera/rl-roadmap)

## 4.4 [Large Language Model Roadmap: End-to-end Large Language Models in PyTorch + Technical Interview](https://github.com/camponogaraviera/llm-roadmap)

</details>
<!-- #endregion -->

---

<!-- #region 5. Mock System Design Interviews with AWS -->
<details>
  <summary><h1 id="mock">5. Mock System Design Interviews with AWS </h1></summary>

- [Design a Restaurant Reservation System](mock_system/rest_reservation.md)
- [Design a YouTube-like On-Demand Video Streaming Service](mock_system/youtube.md)
- [Design a Search Engine like Google](mock_system/google_search_engine.md)

</details>
<!-- #endregion -->

---

<!-- #region 6. Production-Grade Applications -->
<details open>
  <summary><h1 id="apps">6. Production-Grade Applications</h1></summary>

(Private)

- [Chat Horizon: Full-Stack AI Web Chat Application with In-Browser LLM Inference](https://github.com/camponogaraviera/chat-horizon-web)

<p align="center">
  <img src="https://github.com/camponogaraviera/camponogaraviera.github.io/blob/main/assets/chat_horizon/frontend1.png" width="45%" />
  <img src="https://github.com/camponogaraviera/camponogaraviera.github.io/blob/main/assets/chat_horizon/frontend2.png" width="45%" />
</p>

<p align="center">
  <img src="https://github.com/camponogaraviera/camponogaraviera.github.io/blob/main/assets/chat_horizon/frontend3.png" width="45%" />
  <img src="https://github.com/camponogaraviera/camponogaraviera.github.io/blob/main/assets/chat_horizon/frontend4.png" width="45%" />
</p>

<p align="center">
  <img src="https://github.com/camponogaraviera/camponogaraviera.github.io/blob/main/assets/chat_horizon/frontend5.png" width="45%" />
  <img src="https://github.com/camponogaraviera/camponogaraviera.github.io/blob/main/assets/chat_horizon/frontend6.png" width="45%" />
</p>

- [Social Eats: Full-Stack Social Food Discovery Mobile App with 3D Interactivity](https://github.com/camponogaraviera/social-eats)

<p align="center">
  <img src="https://github.com/camponogaraviera/camponogaraviera.github.io/blob/main/assets/social_eats/frontend1.jpg" width="25%" />
  <img src="https://github.com/camponogaraviera/camponogaraviera.github.io/blob/main/assets/social_eats/frontend2.jpg" width="25%" />
</p>

<p align="center">
  <img src="https://github.com/camponogaraviera/camponogaraviera.github.io/blob/main/assets/social_eats/frontend3.jpg" width="25%" />
  <img src="https://github.com/camponogaraviera/camponogaraviera.github.io/blob/main/assets/social_eats/frontend4.jpg" width="25%" />
</p>

</details>
<!-- #endregion -->

---

<!-- #region References -->
<details>
  <summary><h1 id="references">References</h1></summary>

[1] Python: [Python Documentation](https://docs.python.org/3/)

[2] Modern JavaScript (ES6+):

- [ECMA-262/ECMAScript](https://tc39.es/ecma262/)
- [MDN Web Docs](https://developer.mozilla.org/en-US/docs)

[3] TypeScript:

- [TypeScript Documentation](https://www.typescriptlang.org/docs/)
- [TypeScript GOTO Conference Keynote](https://youtu.be/3dqZW_DqHIQ?si=NB8Pmr8YDg5qn3Ge)

[4] C++:

- [Bjarne Stroustrup's homepage](https://www.stroustrup.com/)
- [Programming -- Principles and Practice Using C++ (3rd Edition)](https://www.stroustrup.com/programming.html)

[5] React: [React Documentation](https://react.dev/reference/react)

[6] React Native: [React Native Documentation](https://reactnative.dev/docs/getting-started)

[7] RESTful APIs:

- [MDN HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP)
- [RFC 9110: STD 97: HTTP Semantics](https://www.rfc-editor.org/info/rfc9110/)

[8] GraphQL APIs:

- [Learn GraphQL](https://graphql.org/learn/)
- [GraphQL Tools Documentation](https://the-guild.dev/graphql/tools/docs/introduction)
- [AWS AppSync GraphQL](https://docs.aws.amazon.com/appsync/latest/devguide/designing-a-graphql-api.html)

[9] Apollo GraphQL: [Apollo GraphQL Documentation](https://www.apollographql.com/docs/)

[10] gRPC APIs: [gRPC Documentation](https://grpc.io/docs/)

[11] Polling vs. WebSockets:

- [MDN WebSocket API](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
- [Socket.IO Documentation](https://socket.io/docs/v4/)
- [AWS AppSync Events](https://docs.aws.amazon.com/appsync/latest/eventapi/event-api-welcome.html)

[12] DynamoDB: [Amazon DynamoDB Documentation](https://docs.aws.amazon.com/dynamodb/) and [Awesome DynamoDB](https://github.com/alexdebrie/awesome-dynamodb)

[13] AWS: [AWS Documentation](https://docs.aws.amazon.com/)

</details>
<!-- #endregion -->

---

# License

© This work is licensed under the [Apache License 2.0](LICENSE) license.
