+++
title = "CloudNativeSolution 06: ARCHITECTING CLOUD NATIVE APPLICATION"
author = ["Bruce"]
date = 2024-10-24
series = ["Cloud Native Solution"]
tags = ["Cloud Native Solution", "SWE5001"]
draft = false
+++

# Cloud Native Application Essentials

## Definition
Cloud Native Applications are designed for cloud environments, leveraging microservices, containerization, and DevOps to ensure scalability, resilience, and maintainability.

### Core Properties
- **Resilient**: Can self-heal and automatically recover from failures.
- **Scalable**: Adjusts resources based on demand.
- **Observable**: Integrated logging and monitoring to track performance.
- **Automated**: Continuous integration and deployment (CI/CD) pipelines for rapid updates.
- **Open Standards**: Reduces vendor lock-in and fosters interoperability.

## Key Principles of Cloud Native Design (The Three Ps)
- **Platforms**: Runs on distributed, dynamic environments (public, private, hybrid clouds).
- **Properties**: Designed to be loosely coupled, manageable, observable, scalable, and resilient.
- **Practices**: Continuous delivery, DevOps, and automation ensure frequent, reliable updates.

## Traditional vs. Cloud Native Architecture

### Traditional Architecture
- **Characteristics**: Tightly coupled, monolithic, server-centric, designed for stability.
- **Drawbacks**: Harder to scale, maintain, and deploy changes quickly.

### Cloud Native Architecture
- **Characteristics**: Loosely coupled, microservices-based, container-centric, API-driven.
- **Benefits**: Enhanced scalability, flexibility, and fault tolerance; supports rapid iteration.

## The Twelve-Factor App Principles for Cloud Native Applications

1. **Codebase**: Single codebase for each app tracked in version control.
2. **Dependencies**: Explicitly declare dependencies.
3. **Config**: Store configuration in the environment.
4. **Backing Services**: Treat services like databases as attached resources.
5. **Build, Release, Run**: Separate stages for building, releasing, and running.
6. **Processes**: Stateless, share-nothing processes.
7. **Port Binding**: Self-contained services accessible via port binding.
8. **Concurrency**: Scale by adding instances.
9. **Disposability**: Fast startup and graceful shutdown.
10. **Dev/Prod Parity**: Maintain similarity across development, testing, and production.
11. **Logs**: Treat logs as event streams.
12. **Admin Processes**: Run management tasks as one-off processes.

## Key Cloud Native Architectures

### Monolithic Architecture
- **Characteristics**: Single, tightly coupled application; easy to develop initially.
- **Drawbacks**: Limited scalability, hard to modify individual components.

### Microservices Architecture
- **Characteristics**: Decouples application into independent services; each service has a specific responsibility.
- **Benefits**: Scalable, flexible, supports technology diversity.
- **Challenges**: Complex to manage; requires robust inter-service communication.

### Hexagonal Architecture
- **Structure**: Uses ports and adapters to separate business logic from external systems, allowing flexibility and easier testing.

### Serverless Architecture
- **Characteristics**: Runs on cloud functions (e.g., AWS Lambda) that execute code based on events.
- **Benefits**: Cost-effective, auto-scalable, minimal management.
- **Drawbacks**: Vendor lock-in, limited control, potential latency from cold starts.

## Scaling Approaches in Cloud Native Applications

### Scaling Cube (AKF)
- **X-axis Scaling**: Cloning application instances to distribute load.
- **Y-axis Scaling**: Breaking down by functionality (e.g., microservices).
- **Z-axis Scaling**: Partitioning data by customer or region.

### Horizontal and Vertical Scaling
- **Horizontal Scaling**: Add more instances to distribute the load (suitable for microservices).
- **Vertical Scaling**: Add resources to existing servers (more limited in cloud-native environments).

## Data Processing Architectures

### Lambda Architecture
- **Characteristics**: Combines batch and real-time processing layers for handling large volumes of data and historical processing.
- **Pros**: Manages both real-time and batch data processing.
- **Cons**: Complexity due to dual pipelines.
### 示例应用：实时推荐系统

假设一个电子商务网站希望实现个性化推荐功能，同时希望既能考虑用户的长期偏好，又能迅速响应用户的实时行为（如刚刚点击了某商品）。

#### 批处理层
存储并处理所有用户的历史数据。每晚运行一次批处理任务，分析每个用户的历史点击、购买记录，生成偏好标签（如用户喜欢的商品类别、品牌等）。

#### 实时层
处理实时的用户行为数据。当用户在网站上点击商品时，实时层会记录点击并根据用户的最新行为偏好（例如最近浏览的商品类别）更新推荐结果。

#### 服务层
当用户访问推荐系统时，服务层会将批处理层提供的长期偏好和实时层提供的即时偏好结合，为用户生成最新的推荐列表。

### Lambda架构的优缺点

- **优点**：结合批处理和实时处理，能够兼顾数据的完整性和实时性；即使实时层出现故障，系统依然可以通过批处理层提供历史数据。
- **缺点**：需要维护两套代码（批处理和实时处理），实现复杂；在一些应用中，批处理层的延迟可能会导致推荐不够实时。



### Kappa Architecture
- **Characteristics**: Uses a single real-time stream processing layer for simplicity.
- **Pros**: Simpler than Lambda; better for real-time applications.
- **Cons**: Limited to streaming data; challenging to reprocess large data sets.

## Cloud Native Design Principles

- **Automation**:
  - **CI/CD**: Automate testing, deployment, and delivery processes.
  - **Monitoring and Logging**: Use tools like Prometheus, Grafana, and ELK for observability.
- **Loose Coupling and Isolation**: Design services to interact with minimal dependencies, improving resilience.
- **Self-Healing**: Use orchestrators (e.g., Kubernetes) to restart failed containers or redistribute loads.
- **Security**: Integrate security at all stages (DevSecOps), use role-based access control (RBAC), and secure inter-service communication.

## Reactive Manifesto for Cloud Native Applications
- **Responsive**: System responds in a timely manner.
- **Resilient**: Continues functioning even during failures.
- **Elastic**: Scales up or down based on demand.
- **Message-Driven**: Uses asynchronous messaging for decoupled components.

## Design Considerations for Scalability, Availability, and Manageability

- **Scalability**: Evaluate capacity needs, database partitioning, asynchronous processing, and load balancing.
- **Availability**: SLA-driven design with redundancy, failover mechanisms, and data replication.
- **Manageability**:
  - **Monitoring**: Implement real-time monitoring and logs.
  - **Deployment**: Automate deployment and have rollback options.

# Serverless Architecture Overview

Serverless architecture is a cloud computing model where applications and services are built and run without the need to manage infrastructure. Instead, cloud providers manage the underlying infrastructure, allowing developers to focus on writing code that executes in response to specific events.

{{< figure src="/img/in-post/Cloud Native Solution Design/serveless.png" caption="<span class=\"figure-number\">Figure 1: </span>Serveless Architecture" width="1000px" >}}

** Serveless architecture is a kind of FaaS (Function as a Service)**

## Key Characteristics

- **On-Demand Execution**: Serverless functions are triggered by events (e.g., API requests, database updates) and execute only when needed. This eliminates the need for a persistent server, reducing operational overhead.
- **Automatic Scaling**: Serverless architectures automatically adjust resources based on the workload, scaling up during peak times and scaling down when demand decreases.
- **Event-Driven**: Functions are invoked in response to specific events. This event-driven model suits use cases like processing user requests, performing scheduled tasks, or responding to data changes.

## Components Illustrated in the Diagram

- **Event Sources**: Various client interfaces, including mobile, IoT devices, and web applications, interact with the system through an API Gateway.
- **API Gateway**: Acts as the entry point, routing requests to appropriate serverless functions and services. It handles request validation, authorization, and response formatting.
- **Serverless Functions**:
  - **Authentication Function**: Handles user authentication, validating credentials before granting access.
  - **Vehicle Bid Function**: Processes vehicle bidding requests, likely involving bid validation and storage in a database.
  - **Vehicle Management Service**: Manages vehicle-related operations, such as listing available vehicles or updating vehicle details.
- **Database**: A backend database stores data, accessible by the functions and services as needed.

## Pros of Serverless Architecture

- **Cost-Efficiency**: You only pay for the compute time consumed, which can be more economical than maintaining dedicated servers.
- **Scalability**: Serverless functions scale automatically with demand, ideal for applications with variable loads.
- **Reduced Operational Overhead**: No need to manage servers, enabling teams to focus on development rather than infrastructure maintenance.

## Cons of Serverless Architecture

- **Vendor Lock-In**: Serverless platforms can tie you to a specific cloud provider’s infrastructure and services, which may limit flexibility in the future.
- **Cold Starts**: The first request to an inactive function may experience delays due to initialization, known as a "cold start."
- **Limited Control**: Since the infrastructure is managed by the cloud provider, there is limited access to the underlying environment, which can be a disadvantage for custom configurations.

## Common Serverless Providers

- **AWS Lambda**: Amazon's serverless computing service, widely used for integrating with other AWS services.
- **Google Cloud Functions**: Offers integration with Google Cloud’s ecosystem and supports various programming languages.
- **Azure Functions**: Part of Microsoft Azure, providing serverless functions with extensive tooling and support for enterprise applications.
- **IBM Functions**: A serverless platform by IBM, built on Apache OpenWhisk, suitable for IoT, web, and mobile applications.

## Example Use Cases

- **API-Driven Microservices**: Applications where each function serves a specific microservice, such as a user authentication or data processing service.
- **Real-Time Data Processing**: Processing data streams from IoT devices or handling transactions for applications.
- **Batch Processing**: Performing scheduled or on-demand tasks such as image processing, file conversions, and data analysis.



## Summary
- **Cloud Native Benefits**: Scalability, resilience, agility, and efficient automation, essential for modern applications.
- **12-Factor Principles**: Core to building scalable, maintainable cloud-native applications.
- **Key Architectures**: Microservices, serverless, and event-driven models support cloud-native requirements.
- **Design Considerations**: Prioritize automation, security, and observability for efficient, reliable cloud-native applications.
