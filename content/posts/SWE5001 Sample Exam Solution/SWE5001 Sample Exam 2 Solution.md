+++
title = "SWE5001 Sample Exam 2 Solution"
author = ["Bruce"]
date = 2024-11-02
series = ["SWE5001 Sample Exam Solution"]
tags = ["Platform Engineering", "Architecting software solution", "Cloud Native Solution", "SWE5001"]
draft = false
+++

# SA Qa 
## Question Recap
The platform must be scalable to handle 10,000 updates of arrival times and passenger loads per minute in Phase 1, with a target response time of less than 3 seconds for 90% of requests. Additionally, the platform’s architecture must support high user growth, including 100,000 public users and 10,000 registered users, with updates synchronized approximately every 30 seconds.

## Reference Answer

Two crucial aspects of the architecture for achieving this scalability and responsiveness are:

1. **Event-Driven Architecture for Real-Time Updates**: 
   An event-driven architecture can be employed to handle frequent updates from the buses (arrival times, passenger loads, etc.) efficiently. By using a publish-subscribe model (e.g., message queues or event streams), the platform can decouple data ingestion from processing and serving data to users. This approach allows real-time updates to be processed asynchronously, ensuring that arrival times and passenger loads are updated quickly and efficiently without blocking.

2. **Auto-Scaling and Load Balancing**:
   Given the anticipated growth in user numbers and update frequency, the system must leverage auto-scaling and load balancing in the cloud to handle fluctuating loads. Cloud services that support auto-scaling (e.g., managed Kubernetes, load-balanced server instances) can dynamically adjust resources based on traffic, ensuring the system meets the 3-second response time SLA for most requests, even during peak hours.

---

## 考点与思考切入点

### 考点:
这道题主要考查学生对**系统可扩展性（scalability）和高并发（high concurrency）架构设计的理解，以及如何从架构层面满足系统响应时间要求**。考官关注学生是否能够识别并设计有效的**事件驱动架构（event-driven architecture）和自动扩展（auto-scaling）**策略来处理不断增长的请求和更新需求。

### 切入点:

1. **考虑数据流与实时更新需求**：
   - 题目提到每分钟10,000次更新和100,000个用户量，系统需要能够实时、高效地处理和分发这些更新。因此，从数据流和事件驱动的角度入手，思考如何让系统能够处理高频率的实时数据更新。

2. **考虑弹性扩展性**：
   - 由于用户数量和负载会随时间增长，必须设计出能够自动调整资源的架构。学生可以考虑云平台提供的自动扩展和负载均衡策略，确保在高峰期间系统可以自动调节资源分配，满足大部分请求的响应时间要求。

# SA Qb
## ArrivalTimeComputer Service (Speedy Computation)

- **Cause**: The ArrivalTimeComputer service may be overloaded if it is designed to compute arrival times for all buses and routes in a single, centralized component. This can create a bottleneck, especially under high load, leading to delays in processing each update.
- **Countermeasure**: One solution is to distribute the computation of arrival times across multiple instances or microservices, perhaps by dividing the workload by bus route or region. This way, each instance only handles a subset of the computations, improving overall processing speed.
- **Drawbacks**: The drawback of this approach is increased complexity in managing distributed instances, including synchronization across services. Additionally, there may be a higher cost due to the need for additional computational resources.

## BusNetworkManager Service (Memory Usage)

- **Cause**: The BusNetworkManager may have high memory usage if it retains data for all bus stops, routes, and trips in memory. This can lead to inefficient memory consumption, especially as the number of routes and bus stops increases.
- **Countermeasure**: One way to reduce memory usage is to implement lazy loading or caching mechanisms. For example, the service could load only the data needed for active trips and offload inactive data to a storage service, retrieving it only when necessary.
- **Drawbacks**: A potential drawback is increased latency when retrieving data that isn’t cached, which could impact real-time performance. Another downside is the added complexity in managing cache expiration and data retrieval logic.

### 考点与思考切入点:

- **考点**: 此题的主要考点是性能优化和资源管理，特别是在大型分布式系统中如何优化计算速度和内存使用效率。考官希望学生能够识别当前架构中的瓶颈，并提出合理的解决方案，同时能分析解决方案的潜在弊端。
  
- **切入点**:
  - **关注系统负载分布**：对于 ArrivalTimeComputer 服务，需要考虑如何有效地分散负载以提高速度。思考可以通过微服务化或分布式计算来解决。
  - **关注内存管理**：对于 BusNetworkManager 服务，要考虑如何在不影响性能的前提下节省内存，例如惰性加载或缓存机制。学生应注意分析在性能和资源管理之间的权衡。

## Qb (i) 另解： 考虑跨服务调用带来的延迟

### Cause of the Issue
The current domain design separates ArrivalTimeComputer as an independent service, which can lead to delays due to inter-service communication. In scenarios requiring real-time data updates, this separation introduces network latency and dependency issues, making the response slower.

### Solution (User's Approach)
To address this, we can combine arrivalTimeComputing with BusNetworkManager as a single bounded context (BC). This approach will reduce dependency on external service calls, allowing the computation of arrival times to be handled within the same service that manages bus routes and stops. By colocating these functions, the platform can achieve faster response times in real-time data scenarios.

### Benefits
- **Reduced Latency**: By consolidating these services, we minimize the network latency that occurs with inter-service communication, leading to faster computation of arrival times.
- **Simplified Dependency Management**: With both computations and bus management in the same context, the system no longer relies on external service calls, making the design simpler and more responsive.

### Drawbacks
- **Increased Complexity**: Combining these two bounded contexts could make the BusNetworkManager BC more complex, as it will now have both data management and computational responsibilities.
- **Scalability Constraints**: In the future, if either the arrival time computation or bus management functions require individual scaling, having them tightly coupled might limit flexibility. Additional modifications could be necessary to separate them again for scalability.

# SA Qc

## Question Recap: 
This question addresses performance issues with the Receive Current Position & Speed and Receive Passenger Entrance/Exit APIs. Functionality and load testing revealed a 20-30 second latency when handling large concurrent updates, which impacts real-time accuracy. You need to identify the likely reasons for this latency, refactor the APIs, and provide a concrete example

## Reference Answer:

1. Likely Causes of Latency:

- High Volume of Synchronous Calls: The existing design uses synchronous HTTP PUT requests, which are blocking in nature and can cause delays when there are high volumes of concurrent requests from multiple vehicle onboard units.
- Single Processing Queue: If the API relies on a single processing queue or thread to handle updates, it may become a bottleneck under heavy load, causing significant delays in processing each update.

2. Proposed Refactor: To improve performance, refactor the APIs by introducing an asynchronous event-driven architecture. Rather than handling each update synchronously, we can:

- Use a message queue (e.g., Kafka, RabbitMQ) to receive and enqueue updates from vehicle onboard units.
- Implement event-driven consumers that process updates from the queue asynchronously, allowing multiple consumers to handle updates in parallel.
3. Concrete Example:

- Instead of directly using PUT requests to /arrivaltime and /passengerload, configure the vehicle onboard units to send updates as messages to /eventQueue.
- An example message format might look like this:
```json
Copy code
{
  "busTrip": "TR123",
  "busRoute": "RT456",
  "position": { "latitude": 1.300, "longitude": 103.800 },
  "speed": 45,
  "passengerEvent": "entrance"
}
```
- The event queue would then distribute these messages to multiple processing consumers that handle the updates in parallel, reducing latency.

| API                              | HTTP Action | Inputs                                                                                                                                                      | Outputs                                |
|----------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------|
| Receive Current Position & Speed | POST        | `{ "busTrip": "TR123", "busRoute": "RT456", "position": { "latitude": 1.300, "longitude": 103.800 }, "speed": 45 }`                                         | `{ "status": "received" }`, 202 Accepted |
| Receive Passenger Entrance/Exit  | POST        | `{ "busTrip": "TR123", "busRoute": "RT456", "passengerEvent": "entrance" }`                                                                                | `{ "status": "received" }`, 202 Accepted |
4. Benefits of the Refactor:

- Scalability: Message queues and asynchronous processing allow horizontal scaling, so the system can handle more concurrent updates without creating a bottleneck.
- Improved Real-Time Accuracy: By processing updates concurrently, the system can respond faster, ensuring users receive more accurate real-time data.
5. Drawbacks:

- Complexity: This design requires additional infrastructure for message queuing and event handling, which increases system complexity.
- Message Ordering: As messages are processed asynchronously, maintaining strict order may be challenging, which could affect the accuracy of sequential updates.


## 考点与思考切入点:

- 考点: 这道题考查学生对高并发和实时处理的理解，尤其是在API设计中的异步处理和事件驱动架构。考官关注学生是否能识别出同步请求的瓶颈，并能设计出一个基于消息队列的解决方案来提高系统的响应速度。

### 切入点:

- 分析高并发下的性能瓶颈：题目提到在高并发的负载下产生了延迟，应思考同步API调用在大规模更新下可能造成的延迟问题，考虑采用异步处理来缓解压力。

- 设计事件驱动的解决方案：考虑使用消息队列和并行处理来优化响应时间，这样可以分散负载并实现实时性目标。需要权衡异步处理带来的系统复杂性和顺序性问题

# SA Qd

### Scalability

- **Justification**: The platform is expected to support a large number of concurrent users and handle frequent updates in real-time (e.g., 10,000 updates per minute for arrival times and passenger loads). Scalability is therefore crucial, as it ensures that the database can handle this growing load without compromising performance.
- **Implementation**: By using a scalable database solution, the platform can expand seamlessly as the number of users and the frequency of updates increase. This can be achieved through distributed database solutions that allow horizontal scaling, like NoSQL databases (e.g., MongoDB, Cassandra) or cloud-based managed databases with auto-scaling capabilities.

### Low Latency

- **Justification**: The platform has strict response time requirements, with a target of updating 90% of requests in under 3 seconds. Low latency is essential to meet these real-time requirements and provide a smooth experience for users checking arrival times and passenger loads.
- **Implementation**: A low-latency database ensures fast read and write operations, which is critical for real-time data systems. In-memory databases (e.g., Redis) or databases optimized for low-latency access can help meet these demands, particularly for frequent updates.

### 考点与思考切入点

- **考点**: 此题考查学生对**质量属性（quality attributes）**的理解，尤其是如何选择合适的数据库技术来满足系统的关键需求。考官关注学生是否能够识别并优先考虑与项目场景最相关的数据库质量属性，例如可扩展性和低延迟。

- **切入点**:
  - **考虑并发和扩展需求**：题目描述了系统需要支持大量并发用户和频繁更新，因此需要考虑数据库的扩展能力。学生应结合 NoSQL 数据库或云端扩展服务来支持横向扩展。
  - **考虑实时响应的需求**：由于系统有严格的实时要求，应优先选择低延迟的数据库。学生可以考虑使用内存数据库或优化低延迟访问的数据库来确保实时更新。


### Scalability

- **High Concurrency Requirements**: The platform is expected to support up to 100,000 public users, 10,000 registered users, and 10,000 vehicle onboard units concurrently. With such a large user base and high-frequency updates, a scalable database is essential to ensure that the system can handle increasing loads without degradation in performance.
- **Projected Growth**: As the platform grows, additional modes of transportation and services (e.g., rail transit, private hires) will be integrated. Scalability is thus critical to allow the database to handle more data, users, and use cases without requiring significant re-architecture.
- **Alternative Qualities Consideration**: Other qualities like maintainability or flexibility are important, but without scalability, the platform would struggle to handle its fundamental load requirements, leading to degraded performance and user experience.

### Low Latency

- **Real-Time Requirements**: The platform needs to update users with arrival times and passenger loads in less than 3 seconds for 90% of requests. Low latency in the database layer is essential to meet these response time SLAs, ensuring that users receive near-instantaneous updates.
- **User Experience**: For real-time transportation data, latency is critical to provide accurate and timely information. If users experience delays, it can significantly impact their perception of the platform’s reliability and usability.
- **Alternative Qualities Consideration**: Qualities like data consistency are important but can often be managed in real-time systems through eventual consistency models. However, without low latency, the platform would fail to meet its fundamental requirement for real-time updates, leading to a poor user experience.

# SA Qe

### Amazon DynamoDB

- **Justification**:
  - **Scalability**: DynamoDB is a fully managed NoSQL database that supports automatic scaling, making it suitable for handling high concurrency and large data volumes as the platform grows. DynamoDB can handle up to millions of requests per second, supporting the platform’s requirements as the number of users and vehicle updates increase.
  - **Low Latency**: DynamoDB is designed for low-latency operations, providing single-digit millisecond response times for both read and write operations. This meets the platform's real-time requirements, ensuring users receive timely updates on arrival times and passenger loads.

### Amazon ElastiCache (Redis)

- **Justification**:
  - **Low Latency**: ElastiCache with Redis is an in-memory data store that excels in low-latency data access. It can serve real-time data updates with sub-millisecond latency, making it ideal for caching frequently accessed data like arrival times and passenger loads. This helps offload read operations from the primary database, ensuring fast responses for users.
  - **Scalability**: ElastiCache supports clustering, allowing it to scale horizontally and manage a high number of concurrent connections. By caching data, it reduces the load on the main database, which helps the entire platform scale effectively.

### 考点与思考切入点

- **考点**: 此题考查学生对云数据库服务的选择能力，特别是在可扩展性和低延迟方面的考量。考官关注学生是否能够根据质量属性的需求，合理选择适合的平台数据库服务，并清晰地解释其与系统需求的适配性。

- **切入点**:
  - **结合所需质量属性选择云服务**：基于之前确定的可扩展性和低延迟两个核心质量属性，学生应选择具备自动扩展、低延迟访问特性的云服务。NoSQL 数据库如 DynamoDB 通常适合扩展性需求，而内存数据库如 Redis 则更适合低延迟访问。
  - **理解各服务的优势**：学生应清楚每个云服务在特定场景下的优势，例如 DynamoDB 的横向扩展能力和 Redis 的快速数据访问特性。选择时要强调这些服务如何具体帮助平台达到关键性能要求。

# SA Qf
### Colocate ArrivalTimeComputer and PassengerLoadComputer

- **Justification**:
  - **Similar Update Patterns**: Both services handle frequent updates from vehicle onboard units related to arrival times and passenger loads, and both need to respond to real-time data changes. By colocating them, these services can share data quickly without relying on external network calls, improving the response time.
  - **Reduced Latency**: Since both services depend on the same data (e.g., bus position, speed, and passenger events), colocating them reduces the latency associated with inter-service communication, as they can communicate directly within the same container.

### Colocate UserNotification and UserPreferenceManager

- **Justification**:
  - **Interdependent Functionality**: The UserNotification service often needs to know a user’s preferences to send relevant notifications, such as preferred bus stops or alert types. By colocating these services, the platform can streamline notifications based on user preferences, reducing the need for multiple network calls.
  - **Improved User Experience**: Colocating these services enhances performance for user interactions related to notifications and preferences, ensuring timely alerts and a smoother user experience.

### 考点与思考切入点

- **考点**: 这道题考查学生对容器化部署和服务组合的理解，尤其是在有限资源条件下如何合理地选择服务进行协同部署。考官关注学生是否能够识别具有协同作用的服务，并提出合理的服务组合建议，以提高系统性能并降低延迟。

- **切入点**:
  - **考虑服务间的依赖关系**：优先组合那些具有数据和调用依赖关系的服务，如 ArrivalTimeComputer 和 PassengerLoadComputer，因为它们处理类似的实时数据，放在一起可以减少延迟。
  - **关注用户交互和体验**：UserNotification 和 UserPreferenceManager 在用户通知和偏好设置方面存在关联，将它们放在一起可以减少网络调用，提升用户体验。通过减少跨服务的网络请求，可以显著提高通知的响应速度。

# SB Qa

# Cloud-Native Architecture

## Frontend Components
- **Mobile Application (BusInfo4Me)**: This component will interact with backend services via APIs and display real-time data, including journey mapping and arrival times.

## Backend Components
- **API Gateway**: The API gateway acts as a single entry point for all incoming requests from the mobile app, routing requests to the appropriate services and enforcing security and rate-limiting policies.

### Microservices
- **JourneyMappingService**: This service handles the commuter’s multi-segment journey mapping, breaking down the route between bus stops and updating routes as needed.
- **RealTimeDataService**: This service retrieves and updates real-time data on bus arrival times and passenger loads.
- **NotificationService**: Sends alerts and notifications to users based on preferences (e.g., bus arrival alerts).
- **UserPreferenceService**: Stores and retrieves user settings, such as favorite locations and notification preferences.

### Data Layer
- **NoSQL Database (e.g., DynamoDB)**: Stores real-time and historical data for arrival times and passenger loads to support scalable, low-latency access.
- **In-Memory Cache (e.g., Redis)**: Caches frequently accessed data (e.g., popular routes and real-time arrival times) to reduce load on the main database.

### Load Balancer
- Distributes incoming traffic evenly across service instances to ensure reliability and scalability.

## Deployment
- Use **container orchestration** (e.g., Kubernetes) to manage microservices, ensuring efficient resource utilization and high availability.

---

# Hybrid Architecture (Microservices with Some Monolithic Elements)

A **hybrid architecture** is preferred, as it combines microservices for core real-time data processing and journey mapping with a simpler monolithic approach for less complex, less frequently updated features like user preferences. This approach balances the benefits of modularity and scalability with ease of management.

### Benefits:
- **Modularity for Real-Time Services**: Key real-time services (e.g., JourneyMappingService, RealTimeDataService) operate independently, allowing for rapid scaling based on demand.
- **Reduced Complexity**: Keeping user management and preference data in a monolithic service simplifies interactions and reduces the management overhead associated with microservices, improving maintainability.

---

# 考点与思考切入点:

## 考点
这道题考查学生对云原生架构设计和微服务与混合架构选择的理解，尤其是在移动应用和实时数据环境下，如何有效设计架构。考官关注学生是否能够识别核心服务的可扩展性需求，并能设计合适的模块化架构，同时能权衡纯微服务和混合架构的利弊。

## 切入点
- **关注实时数据服务的架构需求**：实时数据更新和行程映射服务需要快速响应，因此应考虑基于微服务的模块化设计来保证扩展性。
- **平衡模块化和管理简便性**：在选择微服务或混合架构时，需考虑系统的复杂性管理。混合架构可以在保证关键模块独立扩展的同时，减少不常变化模块的管理开销，如用户偏好设置服务。

# SB Qb

# Choice: Containers

## Justification:
- **Scalability and Flexibility**: Containers are lightweight and designed to scale horizontally, making them ideal for the high-concurrency demands of the platform. They can be orchestrated and scaled easily with tools like Kubernetes, which automatically allocates resources as demand fluctuates, ensuring the platform can handle sudden spikes in user activity.
- **Resource Efficiency**: Containers consume fewer resources than virtual machines (VMs) because they share the host OS kernel, allowing for more instances to run on the same hardware. This makes them more cost-effective and efficient for managing microservices.
- **Faster Deployment and Recovery**: With containers, deployments are faster, as containers start up and shut down more quickly than VMs. This reduces downtime and improves the platform's resilience, ensuring users experience minimal service interruption during scaling operations.

## Scaling Design:
- **Use Kubernetes** to manage container orchestration, allowing for auto-scaling based on real-time metrics like CPU and memory usage. For example:
  - **Horizontal Pod Autoscaling (HPA)** can automatically add or remove container instances of critical microservices (e.g., RealTimeDataService, JourneyMappingService) to meet the demand.
- **Load Balancer**: Distributes traffic evenly across all container instances, ensuring no single instance is overwhelmed.
- **Caching Layer**: Implement an in-memory caching solution like Redis to further reduce the load on backend services and databases, allowing frequently accessed data to be served quickly without querying the database repeatedly.

---

# 考点与思考切入点:

## 考点
此题主要考查学生对容器化与虚拟机的优缺点以及扩展性设计的理解。考官关注学生是否能够识别并解释容器在现代分布式应用中相较于虚拟机的优势，尤其在高并发环境下的效率和灵活性。

## 切入点
- **考虑系统需求与扩展方式的匹配**：题目要求支持高并发访问和实时响应，因此应优先考虑容器的轻量化和快速扩展能力，学生应说明容器相对虚拟机的资源效率和弹性。
- **设计有效的扩展方案**：使用容器编排工具（如 Kubernetes）实现自动扩展，并结合缓存等措施减轻后端压力，确保系统在高负载时仍能稳定运行。

# SB Qc

# Architecture Overview

## Data Ingestion
- **Amazon Kinesis**: Use Kinesis for real-time data streaming. Kinesis collects data from the Vehicle Onboard Units (bus location, speed, etc.) and sends it to the processing pipeline.

## Data Storage
- **Amazon S3**: Store historical data for arrival times, passenger loads, and environmental data. S3 is cost-effective and integrates well with analytics and ML services.
- **Amazon DynamoDB**: Use DynamoDB to store and quickly retrieve recent arrival times and passenger counts, as it provides low-latency access.

## Data Processing and Feature Engineering
- **AWS Lambda**: Set up Lambda functions to process and transform real-time data, preparing it for ML model input. Lambda provides serverless, on-demand compute that scales automatically.

## Machine Learning Model
- **Amazon SageMaker**: Use SageMaker to train and deploy the prediction model. SageMaker can use historical time series data and additional features to forecast arrival times accurately.

## Prediction Serving
- **API Gateway**: Expose the prediction model as a REST API through API Gateway, allowing RealTimeDataService to fetch predictions for commuter updates.
- **Amazon ElastiCache (Redis)**: Cache recent predictions to reduce repeated model queries, enhancing response times.

---

# Alternative Choices

- **Data Ingestion**: Instead of Kinesis, **Apache Kafka** (managed on AWS as MSK) is another option for real-time streaming if there are complex partitioning requirements or multi-data center needs.
- **Machine Learning**: For simpler models, **Amazon Forecast** could be used as a managed service specifically optimized for time series forecasting, though it may lack the customization of SageMaker.
- **Data Storage**: For more complex analytics or if SQL querying is required, **Amazon Redshift** can be used alongside S3 to provide a data warehousing solution.

---

# 考点与思考切入点:

## 考点
此题考查学生对机器学习与数据管道架构设计的理解，尤其是在云环境中如何选择合适的服务来实现实时预测。考官关注学生是否能够根据实时需求和预测精度要求，选择适当的服务以优化数据处理和模型训练。

## 切入点
- **选择合适的数据处理和预测服务**：根据数据特性和实时需求，考虑使用流处理（如 Kinesis）和快速缓存（如 Redis），并选用机器学习服务（如 SageMaker）来构建和部署模型。
- **提供替代方案**：考虑到不同场景下的需求，学生可以说明选择替代服务的原因，以体现对服务的灵活应用和兼容性理解。

# SC Qa

### Evaluation of the Logical Architecture in Figure 6:

**Identified Undesirable Architectural Decision:**

- **Issue**: The architecture does not separate real-time components (e.g., TripEventComputer and BusTripOptimizer) from batch processing or analytics components (e.g., AnalyticsReport and AnalyticsConfiguration). Placing real-time components together with batch or analytics components leads to tight coupling, reducing scalability and flexibility. This lack of separation could result in performance bottlenecks, as real-time processes might be delayed by analytics tasks and vice versa.

**Proposed Corrective Action:**

- **Solution**: Separate the real-time components into a dedicated subsystem, such as RealTimeProcessingService, and the analytics-related components into another subsystem, such as AnalyticsService. This separation allows the system to independently scale real-time services as demand increases, without affecting batch processing or analytics tasks.

- **Justification**:
  - **Improved Scalability**: By creating a separate RealTimeProcessingService for real-time components, the platform can independently scale this subsystem to meet the demands of real-time processing without interfering with analytics operations.
  - **Enhanced Maintainability and Modularity**: Decoupling real-time processing from analytics makes it easier to maintain and optimize each subsystem individually. It allows each team to focus on specific requirements without overlapping, and upgrades to one subsystem won’t disrupt the functionality of the other.

**Suggested Packaging for Functional Elements:**

- **RealTimeProcessingService**: This subsystem includes TripEventComputer, BusTripOptimizer, and any other real-time components. It is responsible for computing real-time events and optimizing bus trip adjustments.
- **AnalyticsService**: This subsystem includes AnalyticsConfiguration, AnalyticsReport, and related analytics components. It is focused on historical data analysis, report generation, and actionable insights.
- **NotificationService**: Manages user notifications, triggered by both real-time and analytics events, to alert users about relevant updates.
- **UserManagementService**: Responsible for handling user-related data (e.g., preferences, authentication) and interfacing with both the real-time and analytics subsystems as needed.

**Updated Architecture Diagram**: The revised architecture diagram would show separate subsystems for RealTimeProcessingService and AnalyticsService, with clearly defined boundaries and interfaces. Each subsystem would handle its respective functions, connected to other subsystems via appropriate interfaces.

---

### 考点与思考切入点

**考点**: 本题考查学生对模块化设计和服务解耦的理解，特别是在实时处理与批处理/分析之间的隔离。考官关注学生是否能够识别架构中的紧耦合设计，并提出合理的解决方案，以提升系统的可扩展性和灵活性。

**切入点**:

- **识别实时与批处理的耦合问题**：学生需要观察架构中实时处理和分析功能混杂在同一子系统中，并分析其对扩展性和性能的负面影响。
- **提出模块化优化方案**：建议将实时处理组件单独分离成 RealTimeProcessingService 子系统，以确保实时处理和分析处理能够独立扩展和优化，从而提升系统的效率和模块化水平。


# SC Qb

# Proposed Metric

### Mean Absolute Percentage Error (MAPE)
- **Definition**: MAPE is a widely used metric for measuring the accuracy of predictions in time series data. It calculates the average absolute error between the predicted and actual values, expressed as a percentage, providing a clear view of the model’s accuracy over time.

## Metric Calculation and Reporting

### Direct Capture
- **Requirement**: MAPE cannot be directly captured since it requires both predicted and actual arrival times. Therefore, it must be calculated based on the deviation between predicted values from the **PredictionService** and actual values received from vehicle onboard units.

### Calculation Components
- **PredictionService**: Stores the predicted arrival times and provides them as needed.
- **DataAggregator**: A separate component responsible for gathering actual arrival times and predicted arrival times. It computes MAPE periodically by comparing these values.

### Frequency of Calculation
- **Daily Calculation**: MAPE should be calculated daily to provide a consistent measure of prediction accuracy, which can then be analyzed to monitor and improve the model.

### Data Retention
- **Retention Period**: Store MAPE data for at least 6 months in a metrics database to analyze trends and model performance over time, helping the team to adjust and retrain the model if accuracy degrades.

---

# Supporting Metrics

### Mean Absolute Error (MAE)
- **Purpose**: Tracks the average absolute error in terms of minutes, providing a straightforward measure of prediction error.

### Prediction Bias
- **Purpose**: Measures if predictions consistently overestimate or underestimate arrival times. This helps to identify systematic issues in the model.

---

# Storage and Reporting

### Metrics Database
- **Storage Solution**: Store the calculated metrics in a metrics database (e.g., **Amazon Timestream**) optimized for time-series data.

### Visualization
- **Reporting Tools**: Use visualization tools like **Amazon QuickSight** or **Grafana** to report these metrics periodically to stakeholders.

---

# 考点与思考切入点:

## 考点
此题考查学生对机器学习模型评估指标的理解，尤其是如何监控和报告预测精度。考官关注学生是否能够选择合适的精度衡量标准（如 MAPE），并设计出一个有效的计算和存储方案来跟踪模型性能。

## 切入点
- **选择适合的精度指标**：结合时间序列预测任务的特点，学生应考虑如 MAPE 等合适的指标来量化预测误差。
- **设计度量计算和报告流程**：根据精度指标的计算需求，学生需设计数据收集、计算、存储和报告流程，并合理安排数据保留时间以支持长期监控。
