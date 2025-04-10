---
title: System Design Fundamentals
description: This document covers fundamental concepts of System Design
---

# System Design Fundamentals

## Client-Server Architecture

A computing model where tasks are distributed between clients (requesters) and
servers (providers of resources/services).

### Core Components

#### Client

**End-user (client) devices or applications** that communicates with **consumer applications** to make **requests** to servers.

!!! example "Examples"

    - **Client devices**: Web browser, phone, computer
    - **Consumer apps**: Web application, mobile application

#### Server

**Dedicated computers or software** that **provides services, resources or data** to clients over a network to **fulfill requests**.

### Application Layers

#### Frontend

**Web servers** that **serve static files** (HTML, CSS, JavaScript), **routes requests** to appropriate backend servers, often **acts as the entry point** for client application.

!!! info "Important Note"

    All frontend servers are [web servers](#web-server), but not all web servers that serve static files are considered frontend servers (Ex: CDN, Static File Servers).

!!! example "Examples"

    -   **Node.js with Express**: Often used to create custom frontend servers that serve React/Angular/Vue applications
    -   **Nginx as a frontend proxy**: Configured specifically to route and cache frontend resources

#### Backend

**Application servers** that **host server-side applications** that process business logic, handle data operations, and interact with databases and other services.

!!! example "Examples"

    - Application servers running Node.js, Python, Java, .NET, etc.

#### Browser/Frontend App

A **client-side application** (1) running on the user's device that **provides the interface users interact** with directly in their browser.
{ .annotate }

1. client-side application: A collection of static files (HTML, CSS, and JavaScript)

!!! example "Examples"

    - **Examples**: Web apps developed using frontend frameworks like React, Vue.js, Angular

#### Backend App

**Server-side applications** (typically stateless services) that run on backend servers, **implement business logic** and **data processing** functionalities, and **expose APIs** for clients to consume.

!!! example "Examples"

    - Applications built with ASP.NET Core, Django, Node.js

### Application Types

=== "Traditional Web Application"

    **Web applications** where **pages are fully rendered on the server** before being sent to the client.

    ```mermaid
    sequenceDiagram
        participant U as User
        participant B as Browser
        participant S as Server

        U->>B: Initial request
        B->>S: GET /page
        S->>B: Generate and return complete HTML
        B->>U: Display page

        U->>B: Click link/submit form
        B->>S: GET/POST new request
        S->>B: Generate and return new complete HTML
        B->>U: Display new page (full reload)
    ```

=== "Single Page Application (SPA)"

    **Web applications** that **load a single HTML page** and dynamically update content
    without full page reloads.

    ```mermaid
    sequenceDiagram
        participant U as User
        participant B as Browser
        participant S as Server

        U->>B: Initial request
        B->>S: GET index.html
        S->>B: Return HTML, CSS, JavaScript
        Note over B: SPA loads completely

        U->>B: Interact with page
        B->>B: Update DOM

        U->>B: Request new data
        B->>S: AJAX call to API
        S->>B: Return JSON data
        B->>B: Update view without page reload
    ```

    !!! tip
        SPAs use AJAX and JavaScript to fetch data from servers and modify the current page without requiring full page reloads.

=== "Progressive Web Application (PWA)"

    **Web applications** that use modern web technologies to **deliver app-like experiences** to users.

    ```mermaid
    flowchart LR
        Client[Client] -->|HTTP/HTTPS| SW[Service Worker]
        SW -->|Cache| CS[Cache Storage]
        SW -->|Sync| BS[Background Sync]
        SW -->|Push| PN[Push API]

        Client -->|Install| WM[Web Manifest]
        WM -->|Metadata| App[PWA]
        App -->|Offline| SW
        App -->|Responsive| Client
    ```

    !!! note "Key Features"
        PWAs are responsive, work offline, can be installed on home screens, support push notifications, and combine the best features of web and mobile applications.

### Deployment Architecture Patterns

=== "Single-Tier Architecture"

    A deployment strategy where **all application components (presentation, logic, data) run on a single server**. All components share the same computing resources and system environment.

    ```mermaid
    flowchart LR
        subgraph "Web server (IIS/Nginx)"
            UI[User Interface] --> BL[Business Logic]
            BL --> DB[(Database)]
        end
        User(("User/Browser")) -->|HTTP| UI
    ```

=== "Multi-Tier Architecture"

    A deployment strategy that **splits application components across multiple servers** by deploying them into separate tiers for web servers, application servers, and database servers to improves scalability, enhances maintainability, and strengthens security.

    ```mermaid
    flowchart LR
        User(("User/Browser")) -->|HTTP| LB[Load Balancer]

        subgraph "Presentation Tier"
            W1[Web Server 1]
            W2[Web Server 2]
        end

        subgraph "Application Tier"
            A1[App Server 1]
            A2[App Server 2]
        end

        subgraph "Data Tier"
            DB1[(Primary DB)]
            DB2[(Replica DB)]
        end

        LB --> W1
        LB --> W2
        W1 -->|API calls| A1
        W1 -->|API calls| A2
        W2 -->|API calls| A1
        W2 -->|API calls| A2
        A1 -->|SQL| DB1
        A2 -->|SQL| DB1
        DB1 -.->|Replication| DB2
    ```

=== "Containerized Architecture"

    A deployment strategy **using containerization (Docker, etc.) to package application components with their dependencies**. This enables consistent deployment across different environments and works with orchestration platforms like Kubernetes.

    ```mermaid
    flowchart TB
        User(("User/Browser")) -->|HTTP| API[API Gateway]

        subgraph "Kubernetes Cluster"
            subgraph "Node 1"
                C1{{UI Container}}
                C2{{Auth Container}}
            end

            subgraph "Node 2"
                C3{{Payment Container}}
                C4{{Product Container}}
            end

            subgraph "Node 3"
                C5{{Notification Container}}
                C6{{Analytics Container}}
            end

            subgraph "Persistent Storage"
                DB1[(Auth DB)]
                DB2[(Product DB)]
                DB3[(Payment DB)]
            end
        end

        API -->|Routes request| C1
        API -->|Routes request| C2
        API -->|Routes request| C3
        API -->|Routes request| C4
        API -->|Routes request| C5
        API -->|Routes request| C6

        C2 -.->|Read/Write| DB1
        C3 -.->|Read/Write| DB3
        C4 -.->|Read/Write| DB2

        C1 <-->|API calls| C2
        C1 <-->|API calls| C4
        C3 <-->|API calls| C4
    ```

    !!! tip
        Tools like Docker help create containers, while systems like Kubernetes help manage many containers.

## Server Infrastructure

### Infrastructure Components

#### Web Server

A server with a specialized software that **handles HTTP requests** from clients (typically browsers). It directly **serves static content** (HTML, CSS, JavaScript, images), **forwards requests** for dynamic content to backend servers for processing, and **returns appropriate responses** to the client.

!!! info "Primary Function"

    - Focus on **serving web content**, **handling HTTP protocols**, and **routing requests**

!!! example "Examples"

    - IIS, Nginx, Apache HTTP Server

#### Reverse Proxy

A server with a specialized software that sits between client devices and backend servers, **intercepting client requests** and **forwarding them to appropriate backend servers** while providing security, SSL termination, and caching.

!!! info "Primary Function"

    - Focus on request **forwarding**, **content caching**, and **security**

!!! example "Examples"

    - Nginx, IIS with Application Request Routing (ARR), Apache with mod_proxy

#### Load Balancer

A server with a specialized software that **distributes incoming client requests across multiple servers** to ensure high availability, avoid traffic overload, prevent single points of failure, and enable horizontal scaling.

!!! info "Primary Function"

    - Focus on **traffic distribution** and **high availability**

!!! example "Examples"

    - **Load Balancing Software**: Microsoft Network Load Balancing (NLB), Nginx, HAProxy
    - **Cloud-based Services**: AWS Elastic Load Balancing, Azure Load Balancer

#### API Gateway

A server with a specialized software that **acts as a single entry point** for client applications to access multiple backend services and APIs. It **serves as a reverse proxy for API requests** while providing additional functionality such as request routing, authentication, and monitoring.

!!! info "Primary Function"

    - Focus on **API traffic management**, **security**, and **request coordination** across multiple services

!!! example "Examples"

    - **Cloud-based Services**: Azure API Management, Amazon API Gateway
    - **Software**: Kong, Apigee, Tyk

#### CDN (Content Delivery Network)

**A distributed network of servers** that caches and delivers web content from the server closest to users, improving performance and user experience.

!!! note "Key Characteristic"

    - Distributed globally to optimize content delivery

!!! example "Examples"

    - CloudFlare, Akamai, Fastly

## Networking & Communication

### Network Addressing

#### IP Address

Numerical label assigned to devices on a network, used to identify and locate
them for communication.

!!! info "Types"

    - **IPv4**: 32-bit addresses (e.g., 192.168.1.1)
    - **IPv6**: 128-bit addresses (e.g., 2001:0db8:85a3:0000:0000:8a2e:0370:7334)

#### DNS (Domain Name System)

System that translates human-readable domain names into IP addresses computers
use to identify each other.

!!! example "Process"

    - When you enter "example.com" in a browser, DNS resolves this to an IP address like 93.184.216.34

### Communication Protocols

#### TCP

Transmission Control Protocol - provides reliable, ordered, and error-checked delivery of data between applications running on hosts communicating via an IP network.

!!! info "Characteristics"

    - Connection-oriented
    - Guarantees delivery and order
    - Flow control and congestion control

#### HTTP/HTTPS

Protocols for transmitting web content; HTTPS adds encryption for secure data
transfer.

!!! example "Common Methods"

    - **GET**: Retrieve data
    - **POST**: Submit data
    - **PUT**: Update existing resources
    - **DELETE**: Remove resources

#### TLS/SSL

Cryptographic protocols that provide secure communication over a computer
network, commonly used in HTTPS.

!!! warning "Security Best Practice"

    - Always use TLS 1.2 or higher; older versions have known vulnerabilities

#### WebSockets

Communication protocol that provides full-duplex communication channels over a
single TCP connection.

!!! tip "When to Use"

    - Ideal for real-time applications like chat, live notifications, and collaborative tools

### Performance Metrics

#### Latency

The time delay between a request being sent and the response being received,
often measured in milliseconds.

!!! example "Typical Values"

    - **Low latency**: < 100ms
    - **Medium latency**: 100-300ms
    - **High latency**: > 300ms

## API Development & Management

### API Architecture

=== "REST API"

    Architectural style for designing networked applications using HTTP methods and
    stateless operations.

    !!! info "Key Principles"
        - Stateless
        - Resource-based
        - Uses standard HTTP methods
        - Representation-oriented

=== "GraphQL"

    Query language and runtime for APIs that allows clients to request exactly the
    data they need.

    !!! tip "Advantages"
        - Single endpoint for all operations
        - Clients specify exactly what data they need
        - Strongly typed schema
        - Reduces over-fetching and under-fetching of data

=== "gRPC"

    High-performance, open-source universal RPC framework using Protocol Buffers.

    !!! info "Key Features"
        - Uses HTTP/2 for transport
        - Binary serialization with Protocol Buffers
        - Strongly typed contracts
        - Supports streaming: unary, server, client, and bidirectional

### API Communication Mechanisms

#### Polling

Client regularly checks the server for new data at fixed intervals.

!!! warning "Considerations"

    - Simple but inefficient for real-time data; generates unnecessary requests

#### Long Polling

Client makes a request to the server, and the server holds the connection open until new data is available.

!!! tip "Best For"

    - Simple real-time applications where WebSockets might be overkill

#### Webhooks

HTTP callbacks that deliver data to other applications in real-time when
specific events occur.

!!! example "Common Use Cases"

    - Payment processing notifications
    - CI/CD pipeline events
    - CRM updates

#### Circuit Breaker

Pattern that prevents cascading failures by stopping requests to failing services.

!!! info "States"

    - **Closed**: Requests flow normally
    - **Open**: Requests immediately fail without attempting to call the service
    - **Half-Open**: Limited requests allowed to test if service has recovered

### API Reliability & Protection

#### Rate Limiting

Strategy to restrict the number of API requests a user can make within a
specified time period.

!!! example "Common Approaches"

    - Fixed window
    - Sliding window
    - Token bucket
    - Leaky bucket

#### Throttling

Mechanism to control the usage of APIs by limiting the rate at which consumers can make calls.

!!! tip "Implementation"

    - Should return 429 (Too Many Requests) status code with Retry-After header

#### Idempotency

Property where multiple identical requests have the same effect as a single
request.

!!! example "HTTP Methods"

    - **Naturally idempotent**: GET, PUT, DELETE
    - **Not naturally idempotent**: POST (requires idempotency keys)

#### Backoff Strategies

Technique where a client progressively increases the delay between retries after failed requests.

!!! example "Common Types"

    - **Exponential backoff**: Wait times increase exponentially (1s, 2s, 4s, 8s...)
    - **Jittered backoff**: Adds randomness to avoid thundering herd problems

## Database & Storage

### Database Types

=== "SQL Database"

    Relational database that uses structured query language for defining and
    manipulating data.

    !!! example "Popular Implementations"
        - PostgreSQL
        - MySQL
        - SQL Server
        - Oracle

    !!! tip "Best For"
        Complex queries, transactions, and data with well-defined relationships

=== "NoSQL Database"

    Non-relational database designed for distributed data stores with diverse data
    models.

    !!! info "Common Types"
        - **Document stores**: MongoDB, Couchbase
        - **Key-value stores**: Redis, DynamoDB
        - **Column-family stores**: Cassandra, HBase
        - **Graph databases**: Neo4j, Amazon Neptune

    !!! tip "Best For"
        Scalability, flexibility with unstructured data, high write throughput

=== "TSDB (Time Series Database)"

    Database optimized for time-stamped or time series data.

    !!! example "Popular Implementations"
        - InfluxDB
        - TimescaleDB (PostgreSQL extension)
        - Prometheus

    !!! tip "Best For"
        Metrics, monitoring data, IoT sensor data, financial market data

### Data Partition

#### Partitioning (Vertical Partitioning)

Division of a database table into multiple tables by columns, with each table
having fewer columns.

!!! example "Use Case"

    - Splitting a wide table with customer profile data, purchase history, and preferences into separate tables by data category

#### Sharding (Horizontal Partitioning)

Database architecture pattern where rows of a database table are held separately
in different database nodes.

!!! info "Common Sharding Strategies"

    - **Range-based**: Partition by ranges of a key (e.g., customer_id 1-1000, 1001-2000)
    - **Hash-based**: Use a hash function on the key
    - **Directory-based**: Maintain a lookup table mapping keys to shards

### Database Optimization

#### Database Indexing

Data structure technique to improve the speed of data retrieval operations.

!!! warning "Tradeoffs"

    - Indexes speed up reads but slow down writes and require additional storage

!!! tip "Best Practices"

    - Index columns used in WHERE clauses and joins
    - Avoid over-indexing
    - Consider composite indexes for multi-column queries

#### Denormalization

Strategy to improve read performance by adding redundant data or grouping data.

!!! example "Techniques"

    - Materialized views
    - Precomputed aggregates
    - Redundant fields

### Storage Solutions

#### Blob Storage

System for storing large binary objects, such as images, videos, or documents.

!!! example "Services"

    - AWS S3
    - Azure Blob Storage
    - Google Cloud Storage

!!! tip "Best For"

    - Unstructured data like media files, backups, and static website content

## Scalability & Performance

### Scaling Strategies

#### Vertical Scaling

Increasing the capacity of a single server by adding more resources (CPU, RAM).

!!! info "Pros and Cons"

    - **Pros**: Simple, no distribution complexity
    - **Cons**: Limited by hardware capacity, expensive at high end

#### Horizontal Scaling

Adding more machines to a system to handle increased load, distributing
processing across multiple servers.

!!! info "Pros and Cons"

    - **Pros**: Linear cost scaling, high availability, fault tolerance
    - **Cons**: Increased complexity, data consistency challenges

### Performance Optimization

#### Replication

Process of creating and maintaining duplicate copies of data across multiple
nodes.

!!! info "Types"

    - **Master-slave**: One primary write node, multiple read replicas
    - **Multi-master**: Multiple nodes accept writes
    - **Peer-to-peer**: All nodes are equal

#### Caching

Temporary storage of copies of data to reduce retrieval time and server load.

!!! example "Caching Levels"

    - **Client-side**: Browser cache, mobile app cache
    - **CDN caching**: Edge locations
    - **Application caching**: In-memory caches like Redis
    - **Database caching**: Query cache, buffer pool

## Reliability & Consistency

### Consistency Models

#### ACID Properties

Set of database transaction properties (Atomicity, Consistency, Isolation,
Durability) that guarantee valid transactions.

!!! info "Components"

    - **Atomicity**: Transactions are all-or-nothing
    - **Consistency**: Transactions bring the database from one valid state to another
    - **Isolation**: Concurrent transactions don't interfere with each other
    - **Durability**: Committed transactions persist even after system failures

#### BASE Properties

Alternative to ACID for distributed systems: Basically Available, Soft state,
Eventually consistent.

!!! info "Components"

    - **Basically Available**: System guarantees availability
    - **Soft state**: State may change over time without input
    - **Eventually consistent**: System will become consistent over time

#### CAP Theorem

States that distributed systems can provide at most two of three guarantees:
Consistency, Availability, and Partition tolerance.

!!! example "System Classifications"

    - **CP (Consistent, Partition-tolerant)**: MongoDB, HBase
    - **AP (Available, Partition-tolerant)**: Cassandra, DynamoDB
    - **CA (Consistent, Available)**: Traditional RDBMS (not partition-tolerant)

### Reliability Patterns

#### Failover

Backup operational mode where functions are automatically transferred to standby
system components.

!!! example "Types"

    - **Active-passive**: Standby server only activates when primary fails
    - **Active-active**: Multiple servers handle traffic, survivors take over on failure

## Software Architecture & Design

### Software Architecture Patterns

=== "Monolithic Architecture"

    A software design pattern where **an application is built as a single, unified codebase** with tightly coupled components (user interface, business logic, data access).

    ```mermaid
    flowchart TD
        subgraph "Monolithic Application"
            UI[User Interface Layer]
            BL[Business Logic Layer]
            DAL[Data Access Layer]
            DB[(Database)]

            Client[Client] -->|Request| UI
            UI -->|Process| BL
            BL -->|Store/Retrieve| DAL
            DAL -->|Query| DB
        end

        note[Single Deployable Unit: All components <br> are packaged and deployed together]
        style note fill:#f9f,stroke:#333,stroke-width:1px
    ```

    !!! success "Pros"
        - Simplicity
        - Easier to debug
        - Faster initial development

    !!! failure "Cons"
        - Harder to scale
        - Can become complex and hard to maintain
        - Team coordination challenges

=== "Microservices Architecture"

    A software design pattern where **an application is built as a collection of small, independent services that communicate over a network**. Each microservice focuses on a specific business function and maintains its own data.

    ```mermaid
    flowchart TD
    Client[Client] --> API[API Gateway]

        subgraph "Microservice 1: User Service"
            US[User Service] --> UDB[(User DB)]
        end

        subgraph "Microservice 2: Order Service"
            OS[Order Service] --> ODB[(Order DB)]
        end

        subgraph "Microservice 3: Product Service"
            PS[Product Service] --> PDB[(Product DB)]
        end

        subgraph "Microservice 4: Payment Service"
            PAYS[Payment Service] --> PAYDB[(Payment DB)]
        end

        API --> US
        API --> OS
        API --> PS
        API --> PAYS

        OS -->|Communication| PS
        OS -->|Communication| PAYS

        note[Independent Services - <br>Each with its own database <br>and deployment pipeline]
        style note fill:#afd,stroke:#333,stroke-width:1px
    ```

    !!! success "Pros"
        - Independent scaling
        - Technology flexibility
        - Easier for team collaboration
        - Fault isolation

    !!! failure "Cons"
        - Complex to set up and manage
        - Harder to test end-to-end
        - Data consistency challenges
        - Network overhead

=== "Service-Oriented Architecture (SOA)"

    A software design pattern that **focuses on building reusable services to serve multiple applications across an entire organization**. These services typically communicate through standardized interfaces or an ESB (Enterprise Service Bus), enabling different applications to use these services regardless of the underlying technologies.

    ```mermaid
    flowchart TD
    App1[Application 1] --> ESB
    App2[Application 2] --> ESB
    App3[Application 3] --> ESB

        subgraph "Enterprise Service Bus (ESB)"
            ESB[Enterprise Service Bus]
        end

        ESB --> S1
        ESB --> S2
        ESB --> S3
        ESB --> S4

        subgraph "Shared Services"
            S1[Customer Service]
            S2[Product Catalog Service]
            S3[Order Processing Service]
            S4[Authentication Service]
        end

        S1 --> DB1[(Shared Database)]
        S2 --> DB1
        S3 --> DB1
        S4 --> DB2[(Auth Database)]

        note[Business-aligned services <br>with centralized communication <br>through Enterprise Service Bus]
        style note fill:#ccf,stroke:#333,stroke-width:1px
    ```

    !!! success "Pros"
        - Reusable services
        - Business-aligned modularity
        - Can integrate legacy systems
        - Standardized interfaces

    !!! failure "Cons"
        - Complex to manage
        - Centralized ESB can become a performance bottleneck and single point of failure
        - Often requires significant governance

### Microservices Design Patterns

#### [API Gateway](#api-gateway)

The API Gateway pattern provides a unified entry point for clients to interact with a system of microservices.

!!! tip "Implementation Considerations"
Should handle cross-cutting concerns like authentication, logging, and request routing

#### Service Discovery

Service that enables microservices to find and communicate with each other without hardcoded locations.

!!! example "Implementation Options"

    - **Client-side discovery**: Services query a registry (e.g., Netflix Eureka)
    - **Server-side discovery**: Load balancer routes requests (e.g., Kubernetes Service)

#### Saga Pattern

Sequence of local transactions where each transaction updates data within a
single service.

!!! info "Coordination Methods"

    - **Choreography**: Each service publishes events that trigger other services
    - **Orchestration**: Central coordinator directs the saga steps

#### [Circuit Breaker](#circuit-breaker)

Pattern that prevents cascading failures by stopping requests to failing
services.

!!! example "Libraries"

    - Netflix Hystrix
    - Resilience4j
    - Polly (.NET)

#### Sidecar Pattern

Deploys components of an application as separate processes or containers to
provide isolation and encapsulation.

!!! example "Common Uses"

    - Service mesh proxies (Istio, Linkerd)
    - Logging agents
    - Configuration agents

#### CQRS (Command Query Responsibility Segregation)

Pattern that separates read and write operations to optimize performance,
scalability, and security.

!!! tip "When to Use"

    - When read and write workloads have significantly different performance and scaling requirements

#### Event-Driven Architecture

Design paradigm where components communicate through events, promoting loose
coupling and scalability.

!!! example "Implementation Technologies"

    - Kafka
    - RabbitMQ
    - AWS EventBridge
    - Azure Event Grid

#### Strangler Pattern

A migration pattern/strategy for incrementally replacing a legacy system by gradually routing
functionality to new services.

!!! tip "Implementation Steps"

    1. Create a facade in front of the legacy system
    2. Gradually build new functionality behind the facade
    3. Incrementally redirect from legacy to new implementation
    4. Remove legacy code when no longer used

### Software Design Approaches (Design Methodologies)

#### Domain-Driven Design (DDD)

Software development approach focusing on the core domain and domain logic.

!!! info "Key Concepts"

    - Bounded contexts
    - Ubiquitous language
    - Aggregates
    - Domain events
    - Repositories

#### Test-Driven Development (TDD)

Development process where tests are written before code implementation, guiding
the design through failing tests that are later made to pass.

!!! info "TDD Cycle"

    1. Write a failing test
    2. Write minimal code to pass the test
    3. Refactor while maintaining passing tests

#### Event Sourcing

Storing changes to application state as a sequence of events rather than just
the current state.

!!! example "Benefits"

    - Complete audit history
    - Time travel (reconstruct past states)
    - Excellent for debugging and analysis
