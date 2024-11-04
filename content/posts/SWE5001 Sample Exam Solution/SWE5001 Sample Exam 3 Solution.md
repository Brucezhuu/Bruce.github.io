+++
title = "SWE5001 Sample Exam 3 Solution"
author = ["Bruce"]
date = 2024-11-02
series = ["SWE5001 Sample Exam Solution"]
tags = ["Platform Engineering", "Architecting software solution", "Cloud Native Solution", "SWE5001"]
draft = false
+++

# SA Qa
**参考答案**

为了实现平台生态系统的扩展并增加用户和交互数量，架构的一个关键方面是采用模块化和微服务架构。这种架构允许药品处方、医疗设备和未来扩展的其他服务彼此独立但可互操作。通过微服务实现的模块化架构使得平台可以在不同的功能之间实现松耦合，从而更便捷地添加或移除功能模块，如新的设备种类或健康应用。此外，使用API暴露这些服务，使得外部系统或应用可以通过标准化接口与平台进行交互，这将进一步增强平台的生态系统。

---

**考点与切入点**

这道题的考点是**扩展性**和**架构的可演进性**。关键在于理解如何通过架构设计来支持平台功能的扩展，尤其是支持新的用户类型和交互。回答时可以从模块化设计和微服务架构入手，强调它们如何帮助平台适应新的需求。模块化设计可以减少各模块之间的依赖关系，使得平台可以更灵活地添加新服务。此外，通过提供API接口，平台可以更好地支持外部应用或服务的接入，这对于生态系统的成长至关重要。

---

**English Sample Answer and Explanation**

**Sample Answer**

To support the growth of the platform's ecosystem by expanding customer reach and interactions, one crucial aspect of the architecture is to adopt a **modular and microservices architecture**. This architecture allows for **medicine prescription, medical equipment**, and future additional services to operate independently yet be interoperable. A modular architecture achieved through microservices enables the platform to maintain **loose coupling** between different functionalities, making it easier to add or remove modules, such as new types of equipment or health applications. Additionally, **exposing these services via APIs** enables external systems or applications to interact with the platform through standardized interfaces, further enhancing the platform's ecosystem.

---

**Key Points and Approach**

The focus of this question is on **scalability** and **architectural evolvability**. The key is to understand how architectural design can support expanding platform functionalities, especially in terms of accommodating new customer types and interactions. In answering, you can focus on **modular design and microservices architecture**, emphasizing how these support adaptability to new requirements. **Modular design minimizes dependencies** between components, allowing flexibility in adding new services. Additionally, **providing APIs** for these services allows for easier integration with external applications, which is crucial for ecosystem growth.

# SA Qb
**中文参考答案与解析**

**参考答案**

(i) **领域对象及其关系 UML 类图**

根据案例中的描述，领域对象可以包括以下主要实体：

- **患者 (Patient)**：包含患者信息，如ID、姓名、地址等。
- **医生 (Doctor)**：负责上传处方。
- **处方 (Prescription)**：与患者关联，包含药品信息。
- **药品 (Medicine)**：由供应商提供，与处方相关。
- **医疗设备 (Equipment)**：由设备供应商提供。
- **订单 (Order)**：与患者、药品或设备相关联，包含订单详细信息。
- **跑步者 (Runner)** 和 **瑜伽练习者 (Yoga Practitioner)**：用于跟踪健康活动数据。
- **挑战 (Challenge)**：跑步者和瑜伽练习者可以参与的健康目标。
- **支付 (Payment)**：订单完成时的支付信息。
- **供应商 (Supplier)**：包括药品供应商和设备供应商。

这些领域对象的关系可以在UML类图中展示，例如：

- **患者** 关联到 **处方**，可以有多个 **订单**。
- **供应商** 提供 **药品** 或 **设备**，这些订单属于患者。
- **跑步者** 和 **瑜伽练习者** 可以跟踪 **挑战**。

(ii) **建议的共享服务**

基于平台的需求，可以推荐以下共享服务：

- **用户管理服务 (User Management Service)**：支持患者、医生、供应商等用户的注册和验证。
- **订单管理服务 (Order Management Service)**：负责订单创建、更新、查询和状态跟踪。
- **支付服务 (Payment Service)**：处理支付请求和确认订单支付。
- **数据分析服务 (Analytics Service)**：用于生成无个人信息的分析报告。
- **健康数据管理服务 (Health Data Management Service)**：专用于处理跑步者和瑜伽练习者的数据。
- **目录服务 (Catalog Service)**：维护药品和设备目录信息，供患者浏览。

**考点与切入点**

本题的考点在于**领域驱动设计 (DDD)** 和平台架构中的**服务设计**。回答的关键是识别出系统的主要领域对象以及这些对象如何在平台上形成关系。第一部分考查的是考生的建模能力和领域理解，而第二部分侧重于设计可复用的服务，以支持平台内外部的不同应用和服务。这需要考生结合领域驱动设计的方法，**将功能抽象为服务**，保证服务的独立性和复用性。

---

**English Sample Answer and Explanation**

**Sample Answer**

(i) **Domain Entities and UML Class Diagram**

Based on the case study, the domain entities might include the following primary entities:

- **Patient**: Contains patient details, such as ID, name, and address.
- **Doctor**: Responsible for uploading prescriptions.
- **Prescription**: Associated with a patient and contains medicine information.
- **Medicine**: Supplied by suppliers and linked to prescriptions.
- **Equipment**: Provided by equipment suppliers.
- **Order**: Linked to a patient, medicines, or equipment, and contains order details.
- **Runner and Yoga Practitioner**: Users who track health activities.
- **Challenge**: Health goals for runners and yoga practitioners.
- **Payment**: Represents payment information for completed orders.
- **Supplier**: Represents both drug and equipment suppliers.

The relationships among these domain entities can be shown in a UML class diagram, for example:

- **Patient** is associated with **Prescriptions** and may have multiple **Orders**.
- **Supplier** provides **Medicines** or **Equipment**, which are included in **Orders**.
- **Runners** and **Yoga Practitioners** can participate in **Challenges**.

(ii) **Recommended Shared Services**

Based on the platform's requirements, the following shared services are recommended:

- **User Management Service**: Handles registration and authentication for patients, doctors, and suppliers.
- **Order Management Service**: Manages order creation, updates, queries, and tracking.
- **Payment Service**: Processes payment requests and confirms order payments.
- **Analytics Service**: Generates non-personal analytics reports.
- **Health Data Management Service**: Manages health data for runners and yoga practitioners.
- **Catalog Service**: Maintains catalogs of medicines and equipment for patient browsing.

**Key Points and Approach**

The focus of this question is on **Domain-Driven Design (DDD)** and **service design for platform architecture**. The key to answering is to identify the primary domain entities in the system and understand how they relate within the platform. The first part assesses the ability to model and understand the domain, while the second part focuses on designing reusable services to support both internal and external applications. This requires abstracting functionality into services, ensuring their independence and reusability within the platform.

# SA Qc
**中文参考答案与解析**

**参考答案**
### (i) 推荐开放的服务及其使用者

1. 订单管理服务 (Order Management Service)：该服务可以开放给患者（消费者）和药品/设备供应商（生产者）。患者可以通过API查询他们的订单状态，而供应商可以更新订单的交付状态。此API为患者提供实时跟踪订单的功能，同时使供应商能够灵活管理订单状态，从而提升用户体验。

2. 支付服务 (Payment Service)：该服务可以开放给患者（消费者）和支付网关（合作方）。患者可以使用此API完成订单支付，确保支付过程的便捷性和安全性。通过与支付网关集成，平台可以满足PCI-DSS标准，确保支付信息的安全性。


### (ii) API设计 
- **订单管理服务（符合RMM Level 3，包含Hypermedia Controls）**

- **URI**: `/api/v1/orders/{order_id}`

- **HTTP操作**:
  - `GET /api/v1/orders/{order_id}`: 用于患者获取订单状态。
  - `PUT /api/v1/orders/{order_id}/status`: 由供应商更新订单状态。

- **输入定义**:
  - `order_id`（路径参数）: 订单的唯一标识。
  - `status`（请求体参数，仅用于PUT操作）: 新的订单状态（如“已发货”、“正在配送”等）。

- **输出定义 (包含Hypermedia Controls)**:
  - `order_id`: 订单ID。
  - `status`: 当前订单状态。
  - `estimated_delivery`: 预期的配送日期和时间。
  - `_links`: 包含相关链接的对象
    - `self`: { "href": "/api/v1/orders/{order_id}" }
    - `update_status`: { "href": "/api/v1/orders/{order_id}/status", "method": "PUT" }
    - `track_delivery`: { "href": "/api/v1/orders/{order_id}/track", "method": "GET" }

- **目的解释**: 该服务允许患者实时查看订单状态，并根据需要调用其他相关操作。例如，患者可以通过`_links.track_delivery`链接查看配送的实时状态，供应商则可以利用`_links.update_status`链接更新订单状态。这种设计方式使API客户端能够动态探索可能的下一步操作，提升了交互灵活性。

---

**考点与切入点**

本题的关键在于实现**Richardson Maturity Model**的**第3级别要求**，确保API具备**Hypermedia Controls**。回答时需要特别关注如何通过嵌入的**超媒体链接（Hypermedia Controls）**为API的消费者提供指导，使其能根据资源状态动态调用其他关联操作。通过提供`_links`对象，API客户端可以在不额外查询文档的情况下理解可能的操作，这提高了可用性和适应性，特别是在高度交互的平台生态系统中。

---

**English Sample Answer and Explanation**

**Sample Answer**
### (i) Recommended Exposed Services and Their Consumers

1. Order Management Service: This service can be exposed to patients (consumers) and drug/equipment suppliers (producers). Patients can use this API to check their order status, while suppliers can update the delivery status. This API enables real-time tracking for patients and allows suppliers to manage order statuses efficiently, improving user experience.

2. Payment Service: This service can be exposed to patients (consumers) and the payment gateway (partner). Patients can use this API to complete their order payments, ensuring convenience and security. Integration with the payment gateway helps the platform meet PCI-DSS standards, ensuring payment information security.
### (ii)**API Design 
- Order Management Service (Compliant with RMM Level 3, including Hypermedia Controls)**

- **URI**: `/api/v1/orders/{order_id}`

- **HTTP Actions**:
  - `GET /api/v1/orders/{order_id}`: For patients to retrieve order status.
  - `PUT /api/v1/orders/{order_id}/status`: For suppliers to update order status.

- **Input Definition**:
  - `order_id` (path parameter): Unique identifier for the order.
  - `status` (body parameter, for PUT only): New status for the order (e.g., "shipped," "in transit").

- **Output Definition (with Hypermedia Controls)**:
  - `order_id`: The order ID.
  - `status`: Current order status.
  - `estimated_delivery`: Estimated delivery date and time.
  - `_links`: Object containing related links
    - `self`: { "href": "/api/v1/orders/{order_id}" }
    - `update_status`: { "href": "/api/v1/orders/{order_id}/status", "method": "PUT" }
    - `track_delivery`: { "href": "/api/v1/orders/{order_id}/track", "method": "GET" }

- **Purpose Explanation**: This service allows patients to view their order status in real time and explore related actions based on resource state. For example, patients can use `_links.track_delivery` to view live tracking of their delivery, while suppliers can use `_links.update_status` to change the order status. This design empowers API clients to dynamically discover possible next actions, enhancing flexibility in interactions.

---

**Key Points and Approach**

The focus of this question is on achieving **Level 3 of the Richardson Maturity Model**, specifically ensuring that the API includes **Hypermedia Controls**. The answer should focus on embedding hypermedia links (`_links`) that guide API consumers, enabling them to dynamically discover related actions based on the resource state. Providing `_links` within the API response allows clients to understand available operations without needing additional documentation, improving usability and adaptability, especially in a highly interactive platform ecosystem.

# SA Qd
### 中文参考答案与解析
#### 参考答案
推荐共同部署在同一容器中的服务：

- **订单管理服务 (Order Management Service) 和 支付服务 (Payment Service)**：这些服务在订单确认和付款阶段的通信频率较高，且需要紧密协作来确保订单状态更新与支付信息一致。将它们部署在同一容器中可以减少延迟，提高交互效率。

- **用户管理服务 (User Management Service) 和 健康数据管理服务 (Health Data Management Service)**：这两个服务在跑步者和瑜伽练习者的用户管理方面有较高的交互需求。例如，健康数据的更新往往需要验证用户身份，将这两个服务放在同一容器中可以简化认证请求的处理，提升用户体验。

#### 考点与切入点
本题的考点是服务通信的亲和性和容器化部署的优化。回答时需要识别那些具有高通信亲和性的服务，即通信频率高或数据交互密集的服务。将高亲和性的服务部署在同一容器中可以减少网络延迟和带宽使用，从而提升系统性能。同时需要权衡容器之间的隔离性与通信效率。考生应结合服务之间的依赖关系来判断哪些服务适合共同部署，以实现性能和资源的最佳平衡。

---

### English Sample Answer and Explanation
#### Sample Answer
Recommended Services for Co-location in the Same Container:

- **Order Management Service and Payment Service**: These services have high communication affinity during order confirmation and payment processing. They need to coordinate closely to ensure consistent order status updates and payment records. Deploying them in the same container can reduce latency and improve interaction efficiency.

- **User Management Service and Health Data Management Service**: These services frequently interact, especially for user authentication related to runners and yoga practitioners' health data updates. Placing them in the same container can simplify authentication processing, enhancing user experience by reducing response times for identity verification requests.

#### Key Points and Approach
This question focuses on communication affinity between services and optimization in containerized deployment. The key is to identify services with high communication affinity, meaning services that interact frequently or exchange significant amounts of data. Co-locating these services in the same container reduces network latency and bandwidth usage, thus improving system performance. It's essential to balance isolation between containers with communication efficiency. Candidates should consider service dependencies to determine which services are suitable for co-location, aiming for optimal performance and resource usage.

# SB Qa

### 中文参考答案与解析
#### 参考答案
在分析WeFitRun和WeFitYoga系统的逻辑部署图时，发现了以下三个架构决策问题及其改进建议：

- **问题：前端与数据库的直接连接**

  - **原因**：这会导致安全隐患，因为前端系统可以直接访问数据库，可能导致数据泄露或不一致性。
  - **改进建议**：应通过后端API层进行数据访问，以便控制数据访问权限和确保一致性。这样前端只能通过受控的接口与数据交互，提升安全性和可维护性。

- **问题：缺乏负载均衡器**

  - **原因**：没有负载均衡器意味着所有请求直接发送到相同的服务器，可能导致在高峰期时的瓶颈和服务中断。
  - **改进建议**：引入负载均衡器，将用户请求分配给多个服务器实例，提升系统的可扩展性和故障容忍能力，确保在高负载时系统能够平稳运行。

- **问题：日志和监控服务缺失**

  - **原因**：缺乏日志和监控会让故障排查和性能优化变得困难，尤其是在多服务架构中。
  - **改进建议**：添加集中式日志记录和监控服务，如ELK或Prometheus，这样可以实时监控系统性能并在出现故障时快速诊断问题，提升系统的可靠性和运维效率。

#### 考点与切入点
本题的考点是架构分析与优化，关键在于识别逻辑部署中的潜在问题并提出合理的解决方案。回答时应结合系统的性能、可维护性、安全性和扩展性等方面的要求。首先分析架构是否符合基本的安全和性能需求，找出前端和数据库的直接连接或缺少负载均衡等常见问题；然后通过引入后端API层、负载均衡器及监控服务来优化系统设计，以提升整个系统的可靠性、性能和维护性。

---

### English Sample Answer and Explanation
#### Sample Answer
Upon analyzing the logical deployment diagram of the WeFitRun and WeFitYoga systems, the following three architectural issues and recommendations were identified:

- **Issue: Direct connection between frontend and database**

  - **Reason**: This poses security risks as the frontend has unrestricted access to the database, which may lead to data breaches or inconsistency.
  - **Recommendation**: Introduce a backend API layer to mediate data access. This allows the frontend to interact with data only through controlled interfaces, enhancing security and maintainability.

- **Issue: Lack of a load balancer**

  - **Reason**: Without a load balancer, all requests are directed to the same server, potentially creating bottlenecks and service interruptions during peak times.
  - **Recommendation**: Implement a load balancer to distribute requests across multiple server instances, improving scalability and fault tolerance to ensure smooth performance under high load.

- **Issue: Missing logging and monitoring services**

  - **Reason**: Without logging and monitoring, troubleshooting and performance optimization become challenging, especially in a multi-service architecture.
  - **Recommendation**: Introduce centralized logging and monitoring services, such as ELK or Prometheus. This enables real-time system monitoring and quick diagnostics during failures, improving system reliability and operational efficiency.

#### Key Points and Approach
The focus of this question is on architectural analysis and optimization, specifically identifying potential issues in logical deployment and suggesting feasible solutions. Answering should involve assessing the system's performance, maintainability, security, and scalability requirements. Start by examining whether the architecture meets essential security and performance needs, identifying common issues like direct frontend-database connections or absence of load balancing. Solutions such as introducing an API layer, load balancer, and monitoring services improve system reliability, performance, and maintainability.

# SB Qb
### AWS Services Recommendation for WeFitRun and WeFitYoga Systems

Based on the requirements for building a scalable, secure, and high-performing architecture for the WeFitRun and WeFitYoga systems, here’s a recommendation of AWS services to use and the reasoning behind each choice.

#### 1. AWS API Gateway
- **Use**: Acts as the primary access point for all external and internal APIs, securing access to backend services.
- **Justification**: API Gateway provides centralized API management, including request routing, authentication, and rate limiting. It allows for secure API exposure and scales easily, ensuring that the system can handle growing traffic from mobile and web applications.

#### 2. AWS ElastiCache (Redis or Memcached)
- **Use**: Cache frequently accessed data (such as user data, fitness statistics, and leaderboards) to reduce the load on the database and improve response times.
- **Justification**: By caching frequently accessed information, ElastiCache helps reduce latency and improve application performance. It’s particularly useful for session data, caching leaderboard data for gamification, and other frequently requested information, thus improving the user experience.

#### 3. AWS Lambda
- **Use**: For serverless functions to handle asynchronous tasks, such as processing workout data, sending notifications, or handling event-driven actions.
- **Justification**: Lambda’s serverless nature allows the platform to execute code without provisioning or managing servers, saving costs. It's ideal for background tasks that don’t require continuous server uptime, such as processing workout updates or sending user notifications.

#### 4. AWS RDS
- **Use**: Primary relational database for storing structured data, such as user profiles, workout records, and application metadata.
- **Justification**: RDS provides a managed, scalable database solution with automated backups, snapshots, and multi-AZ deployment for high availability. It’s suitable for transactional data requiring consistency, like user and workout records.

#### 5. AWS Cognito
- **Use**: For user authentication, registration, and access control, especially for handling user identity and managing access to services.
- **Justification**: Cognito provides secure sign-in and sign-up capabilities, supports social and SAML logins, and scales to handle millions of users. It’s an easy way to manage authentication and integrate access control without building custom authentication mechanisms, enhancing both security and user experience.

#### 6. AWS S3
- **Use**: For storing large, static data files, such as user-uploaded workout videos, photos, and other media files.
- **Justification**: S3 offers scalable, durable, and cost-effective storage for static assets. It’s optimized for large data volumes and provides high availability, making it ideal for media files and backups. Its integration with CloudFront can further improve content delivery.

#### 7. AWS CloudFront
- **Use**: To distribute static content (like images, CSS, JavaScript) and media files to users globally with low latency.
- **Justification**: As a CDN, CloudFront caches content closer to users, improving load times for static assets and enhancing user experience by reducing latency. It works well with S3 for serving media content and static assets.

---

### Summary of Architecture with AWS Services

- **API Gateway**: Secure entry point for API requests, managing and routing traffic to backend services.
- **ElastiCache**: Caching layer to store frequently accessed data, reducing load on RDS and enhancing response time.
- **Lambda**: Serverless compute for handling asynchronous tasks, reducing costs by eliminating the need for dedicated servers for background processes.
- **RDS**: Relational database for persistent storage, managing user data and structured application data.
- **Cognito**: Provides user authentication and access management, simplifying user identity handling and enhancing security.
- **S3**: Scalable storage for static and large media files, integrated with CloudFront for content distribution.
- **CloudFront**: Content delivery network to cache static and media content closer to end users for faster load times.

---

### Explanation of Architectural Decisions

- **Security and Access Control**: API Gateway and Cognito provide a secure way to expose APIs while managing user authentication and authorization. This ensures that only authenticated users can access the system, and their access is tightly controlled.

- **Performance and Scalability**: The combination of ElastiCache, CloudFront, and Lambda ensures the system can handle high loads efficiently. ElastiCache reduces database load by caching frequently accessed data, while CloudFront caches static content closer to users. Lambda allows scalable event-driven processing without the need for a full-time server, reducing infrastructure costs.

- **Data Management and Reliability**: RDS and S3 ensure that data is reliably stored and accessible. RDS handles structured transactional data with consistency, while S3 offers scalable storage for large static files. Together, they meet data persistence and durability requirements, ensuring high availability and reliability across different data types.

### Three Significant Architectural Decisions for Deploying the WeFitRun and WeFitYoga Systems on AWS

Each architectural decision aligns with specific goals such as security, scalability, performance, and maintainability.

#### 1. Use of AWS API Gateway for Secure Access Control
- **Decision**: We decided to place an API Gateway in front of the API layer, which manages all interactions between external clients (like WeFitRun and WeFitYoga subsystems) and backend services.
- **Reasoning**: The API Gateway serves as a protective layer, enforcing security policies, managing authentication, and providing rate limiting to prevent abuse. It ensures that only authorized requests reach the internal services, which reduces the risk of unauthorized data access or system breaches.
- **Benefit**: By using API Gateway, we can control access to backend services in a centralized manner, making it easier to apply consistent security policies across the platform. This approach not only secures the system but also allows the architecture to scale as more services or consumers join the ecosystem.

#### 2. Elastic Load Balancer (ELB) for Scalability and High Availability
- **Decision**: An Elastic Load Balancer (ELB) is deployed in front of the API layer to distribute incoming requests across multiple API instances.
- **Reasoning**: The load balancer helps evenly distribute traffic, reducing the load on individual servers and preventing bottlenecks. It also supports auto-scaling, allowing the architecture to dynamically scale up during peak traffic periods and scale down during low traffic, optimizing costs and resource usage.
- **Benefit**: ELB enhances the system's availability by ensuring that if one API instance fails, traffic is rerouted to other instances without disrupting the user experience. This setup also allows the system to handle fluctuations in demand, providing a consistent and responsive experience for users.

#### 3. Separation of Persistent Data Storage (AWS RDS) and Caching (AWS ElastiCache)
- **Decision**: We use AWS RDS for relational data storage and AWS ElastiCache (Redis or Memcached) as a caching layer for frequently accessed data.
- **Reasoning**: Separating persistent storage from caching allows us to optimize data access based on usage patterns. RDS provides reliable, scalable, and managed relational database capabilities, while ElastiCache serves as a high-performance caching solution to reduce latency and relieve load on the database.
- **Benefit**: This separation improves both performance and cost efficiency. Frequently accessed data is served quickly from the cache, reducing the number of direct requests to the database. Additionally, RDS and ElastiCache both offer features for scaling, backups, and recovery, ensuring data integrity and availability even during high traffic or system failures.
