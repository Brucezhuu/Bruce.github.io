+++
title = "05 Platform management"
author = ["Bruce"]
date = 2024-10-14
series = ["Platform Engineering"]
tags = ["Platform Engineering", "SWE5001"]
draft = false
+++

# Platform Overview and Core Components

## 1. Platform Overview
**Definition**: A platform is a business model that creates value by facilitating interactions between two or more interdependent groups (e.g., consumers and producers). It often merges infrastructure and application needs, handling service discovery, resource coordination, container orchestration, etc.

**Key Functions**:
- **Scalability**: Enables increased interaction and resource scaling.
- **Availability**: Ensures reliable user experiences.
- **Evolvability**: Supports rapid updates, bug fixes, and new features.

## 2. Core Platform Tools
- **Service Discovery**: Allows services to dynamically locate each other within the platform.
- **Deployment Strategies**: Handles version rollout methods to minimize downtime and control user impact.
- **Circuit Breaker**: Protects system resilience by managing request failures and retry logic.
- **Distributed Tracing**: Tracks user requests across microservices, aiding in debugging.
- **Distributed Transaction Management (Saga Pattern)**: Manages consistency across transactions in a distributed environment.
- **Logging and Monitoring**: Collects metrics and logs for performance analysis, error tracking, and optimization.

## 3. Service Discovery
**Purpose**: Ensures services can locate each other without hard-coded IPs.

**Process**:
1. **Register**: Each service registers its location with a registry (e.g., DNS).
2. **Lookup**: Services query the registry for addresses.
3. **Connect**: Services use the discovered addresses to interact.

**High Availability**: Service discovery systems like Zookeeper, etcd, and Consul typically use clustering with consensus protocols for fault tolerance.

## 4. Deployment Strategies
- **Blue/Green Deployment**: Both versions run simultaneously; traffic is switched only after confirming the new version works.
- **Rolling Update**: Gradual replacement of the old version with the new, without downtime.
- **Canary Release**: New version serves a small percentage of requests (e.g., 5-10%) to assess impact before full rollout.
- **Shadowing**: New version processes requests but does not impact the user; useful for testing and comparing outcomes.

## 5. Circuit Breaker
**Objective**: Prevents cascading failures by stopping requests to failing services temporarily.

**Mechanism**:
- Detects when a service fails consistently.
- Stops requests to the failing service for a specified interval, allowing it to recover.
- **Retry with Exponential Backoff**: Attempts reconnection after gradually increasing intervals (e.g., 50ms, 100ms).

**Tech Options**: Common libraries include Resilience4J, Sentinel, and Istio service mesh.

## 6. Rate Limiting
**Purpose**: Limits the number of requests from users or services to prevent overload or abuse (e.g., DDoS attacks).

**Implementation**:
- **Conditional Rate Limits**: Prioritize certain users over others based on quotas.
- **Interceptors**: Extract request details and check quotas from an in-memory database for fast decision-making.

**Tools**: Options include Istio, Sentinel, and cloud provider tools.

## 7. Distributed Tracing
**Goal**: Trace a request as it travels through multiple services, essential for debugging distributed systems.

**Process**:
1. Each request is tagged with a unique identifier (trace ID).
2. Services add child spans to the trace as they process the request.

**Tech Options**: Jaeger (CNCF), Spring Sleuth, Zipkin, and Datadog support standard distributed tracing.

## 8. Distributed Transaction Management (Saga Pattern)
**Challenge**: Consistency across multiple services in a distributed system where traditional ACID transactions aren’t feasible.

**Saga Pattern**:
- **Orchestrator-Based**: A central orchestrator coordinates and manages the entire transaction.
- **Choreography-Based**: Each service knows how to handle events independently, offering more resilience but adding complexity.

**Considerations**: Sagas do not guarantee read isolation, meaning users may see intermediate transaction states.

## 9. Metrics Collection
**Purpose**: Enables monitoring of system health and performance.

**Types of Metrics**:
- **Infrastructure Metrics**: CPU usage, memory, IOPS, uptime, average response time, etc.
- **Application Metrics**: Task success rates, user engagement, and frequency of visits.

**Key Availability Metrics**:
- **Uptime**: (MTBF - MTTR) / MTBF, typically measured in nines (e.g., 99.9%). 
    - MTBF (Mean Time Between Failures)：平均故障间隔时间，即系统从一次故障到下一次故障的平均时间。它衡量的是系统的可靠性。
    - MTTR (Mean Time to Repair)：平均修复时间，即系统发生故障后恢复到正常运行状态的平均时间。它衡量的是系统恢复的速度。
- **Yield**: Ratio of successful requests to total requests.
- **Harvest**: Availability of data from servers.

**Tools**: Prometheus with Grafana, TICK stack (Telegraf, InfluxDB), and Splunk.

## 10. Logging and Monitoring
**Importance**: Centralized logging is critical for analyzing performance, tracking errors, and auditing.

**Common Log Types**:
- Authentication successes/failures.
- Data access and changes.
- External resource access.

**Log Aggregation Patterns**:
- **Centralized Server**: Logs are formatted and sent to a central server. （Application ships transformed/formatted logs to Central log server）
- **Transformation Server**: Raw logs are parsed, transformed, and stored centrally. 

**Tools**: ELK stack (Elasticsearch, Logstash, Kibana), FluentD, Splunk, and cloud-based solutions like AWS and Google Stackdriver.
