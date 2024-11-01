+++
title = "CloudNativeSolution 09: Cloud Patterns and Practices"
author = ["Bruce"]
date = 2024-10-24
series = ["Cloud Native Solution"]
tags = ["Cloud Native Solution", "SWE5001"]
draft = false
+++

# Introduction to Cloud Patterns

### Definition
Architectural patterns capture design ideas as archetypal and reusable descriptions to address common challenges like performance limitations and business risks.

### Types
- **Design Patterns**: General solutions for recurring problems in software design.
- **Architectural Patterns**: Framework-based solutions implemented within software to improve scalability, resilience, and flexibility.

## Key Cloud Patterns and Their Use Cases

- **Ambassador**: Acts as an intermediary to offload client requests and implement policies.
- **Anti-Corruption Layer**: Protects legacy systems from new microservices by translating between interfaces.
- **Bulkhead**: Isolates different parts of the system to prevent failures from cascading.
- **Cache-Aside**: Data is loaded into cache on-demand, improving data retrieval performance.
- **Circuit Breaker**: Detects and stops calls to unresponsive services to avoid cascading failures.

## Event-Driven Microservices

- **Event Sourcing**: Stores state changes as a sequence of events, useful for maintaining history and audit.
- **Service Mesh**: Uses proxies to route requests between microservices, enhancing control and observability.
- **Compensating Transactions**: Uses compensatory actions for rollback in distributed transactions, promoting decoupling and scalability.

## Data Storage Patterns

- **Shared Data Store**: Multiple services access a single database, beneficial within a domain boundary.
- **Data Store per Service**: Each service has its own database, ensuring encapsulation and API-based access.
- **Command Query Responsibility Segregation (CQRS)**:
  - Separates data modification and read operations into different models.
  - Ideal for systems with high read/write loads, improves scalability and performance.

## Saga Pattern for Distributed Transactions

- **Choreography-Based Saga**: Each service publishes domain events to trigger actions in other services.
{{< figure src="/img/in-post/Cloud Native Solution Design/Choreography- based saga.png" caption="<span class=\"figure-number\">Figure 1: </span>Choreography- based saga" width="1000px" >}}
- **Orchestration-Based Saga**: A central orchestrator directs the sequence of service calls.
{{< figure src="/img/in-post/Cloud Native Solution Design/Orchestration- based saga.png" caption="<span class=\"figure-number\">Figure 2: </span>Orchestration- based saga" width="1000px" >}}

## Sharding Patterns for Data Management

- **Lookup Strategy**: Routes requests to the correct shard using a key.
- **Range Strategy**: Stores data in ordered groups, suitable for sequentially related items.
- **Hash Strategy**: Distributes data to balance load across shards, reducing hotspots.

## Multi-Tenancy Models

### Isolation Options
- **Physical Isolation**: Each tenant has dedicated resources, ensuring strong isolation.
- **Application Isolation**: Tenants share infrastructure but have separate application instances.
- **Database Isolation**: Tenants have separate databases for enhanced security and customization.

### Schema Continuum （把schema理解为数据库中数据表）
- **Separate Database**: Maximum isolation, suitable for tenants requiring data separation.
- **Separate Schema**: Shared database with tenant-specific schemas.
- **Shared Schema**: All tenant data in the same schema, most economical but least isolated.

## Content Aggregation Patterns

- **Aggregation by Client**: Client-side aggregation for faster perceived performance but requires advanced UI capabilities.
- **API Aggregation**: API Gateway aggregates responses, beneficial in bandwidth-constrained scenarios.
- **Microservice Aggregation**: A microservice aggregates data, ideal for real-time business logic application.
- **Database Aggregation**: Pre-aggregates data at the database level, enabling complex analytics but requires more storage.

## Capabilities for Microservices Coordination

- **State Management**: Manages the output state of coordinated services, stored in a persistent store.
- **Transaction Control**: Compensating transactions are preferred over distributed transactions for scalability.
- **Timeout Handling**: Monitors service response times to prevent bottlenecks.
- **Configurability**: Allows parameterized configuration for various microservice flows.

## Checklist for Cloud Native Design

### App Design & Architecture
- Composite, loosely coupled architecture
- Fault-tolerant, environment-centric, scalable, and upgradeable without downtime

### Data Store
- Scalability, redundancy, recovery, partitioning, and caching

### Security
- Multi-token authentication, field-level security, and single sign-on

### Integration
- Flexible response formats (JSON, XML) and Service-Level Agreements (SLAs)
