+++
title = "04 Reusable Services and API Gateway_v2.3"
author = ["Bruce"]
date = 2024-10-14
series = ["Platform Engineering"]
tags = ["Platform Engineering", "SWE5001"]
draft = false
+++

## 1. Characteristics of Microservices:
- Decoupled Architecture: Independent services, each dedicated to a business capability, communicate typically through HTTP APIs.
- Independently Deployable: Allows separate deployments with minimal dependencies, enhancing scalability and maintenance.
- Decentralized Data Management: Each service may have its own database, enabling tailored data storage approaches per service.
- Infrastructure Automation: Continuous Integration/Continuous Deployment (CI/CD) pipelines are common, supporting rapid, reliable updates.
- Failure Isolation: Designed to tolerate individual service failures, with monitoring and resilience strategies to ensure minimal disruption.

## 2. Decoupling Reusable Services
- Bounded Contexts: Each service operates within a "bounded context," establishing a unique language and boundaries.
- Context Mapping:
- Shared Kernel: Shared model between two teams.
- Partnership: High collaboration to align integrations.
- Customer-Supplier: The upstream service provides data that the downstream service consumes.
- Anticorruption Layer: Translates from an upstream service's language to the downstream service's language.
- Published Language: Uses standardized data formats (e.g., JSON, XML) for easier cross-context consumption.

## 3. Aggregates
- Definition: Clusters of entities within the same bounded context treated as a single unit (aggregate root).
Principles:
- Transactional Consistency: All elements within an aggregate must be consistent at the end of a transaction.
- Rules of Thumb:
    - Design small aggregates for quicker load and processing.
    - Reference other aggregates by ID only, not by object reference.
    - Update other aggregates using eventual consistency unless real-time updates are crucial.

## 4. Domain Events
- Definition: Record of a business-significant event within a bounded context.
- Characteristics:
    - Named in past tense (e.g., ProductCreated).
    - Contains all relevant data to inform subscribers.
- Usage:
    - Loosely coupled systems can use domain events to propagate changes.
    - Ensure causal consistency, meaning events occur in logical order across services.

### Domain Events

**Domain Events** 是在软件开发中用于表示业务系统中发生的特定事件，通常由系统内部的不同组件使用。Domain Events 的设计目的是解耦系统，允许各个组件独立工作并接收对业务有意义的事件信息。

---

### 具体解释和例子

- **定义**：Domain Event 代表在一个 **bounded context**（即一个明确定义的业务领域）内发生的、对业务有意义的事件。
  - 比如在电商系统中，有一个 "ProductCreated" 的 Domain Event，表示系统中新产品已创建完毕。
  - 这一事件记录了该产品的所有相关信息，确保接收者能了解事件细节，例如产品名称、类别、库存量等。

- **命名**：Domain Event 的名字一般用 **过去时** 表达，表示该事件已经完成。
  - 例如 “OrderShipped” 表示订单已经发货，而不是正在发货中，这样订阅该事件的其他系统可以明确该订单状态。

- **包含相关数据**：每个 Domain Event 都包含所有必要的信息，确保订阅方可以基于这些数据作出响应。
  - 比如 “OrderPaid” 事件可能包含订单 ID、支付时间、支付金额、用户信息等，以确保订阅者了解订单的支付情况。

- **使用**：Domain Events 在系统之间传播变化信息，使得各个子系统松散耦合（loosely coupled）。
  - 例如，当订单系统中的 “OrderShipped” 事件发生后，库存管理系统可以订阅该事件，更新库存。
  - 同样，通知系统可能订阅该事件，给用户发送“已发货”短信通知。

- **因果一致性**：确保事件的发生顺序保持一致，防止逻辑混乱。
  - 比如在订单系统中，“OrderCreated” 应先于 “OrderShipped” 发生，接收系统需要按顺序处理这两个事件，以确保业务逻辑不出错。

---

### 具体应用场景

设想一个电商系统中，用户完成下单流程后，订单系统会生成一个 "OrderPlaced" 事件。

- **库存系统** 订阅了 "OrderPlaced" 事件，当收到该事件后，它会自动减少相关商品的库存。
- **通知系统** 也订阅了 "OrderPlaced" 事件。收到事件后，它会发送一封确认邮件给用户，表示订单已成功生成。
- **推荐系统** 还可以订阅这个事件，根据用户购买的产品类型，推荐相关产品。


## 5. Service Integration Patterns
- RESTful HTTP: Resource-based and commonly used with standardized HTTP methods (GET, POST, PUT, DELETE).
- Messaging: Services subscribe to events, useful for asynchronous communication and resilience to network failures.
- RPC with SOAP: Remote calls with SOAP protocol, but requires strong coupling and is sensitive to network latency.

## 6. Orchestrating vs. Choreographing Services
- Orchestration:
    - Managed by a central service.
    - Simpler, with explicit control but can lead to "god services."
- Choreography:
    - Services respond to events independently.
    - More complex, requires monitoring but offers loose coupling.
### Orchestration（服务编排）

**Orchestration** 指由一个中央控制的服务负责管理和调度其他服务。这种方式像一个指挥家（central orchestrator），安排各服务在什么时间做什么，流程通常是由中央服务控制的。

#### 特点
- **中央服务控制**：所有操作由中央服务明确指示。
- **简单易管理**：可以清晰地看到流程步骤，便于维护。
- **潜在缺点**：中央控制服务可能会变成“God Service”（上帝服务），即它变得非常庞大和复杂，影响系统的灵活性。

#### 举个例子

设想一个电商系统中，有一个用户下单流程。此流程可以由一个 Orchestration 服务（例如 “Order Service”）来控制。流程如下：

1. 用户创建订单。
2. **Order Service** 发送请求给库存服务来检查库存并减少库存。
3. 如果库存充足，**Order Service** 再发送请求给支付服务来处理付款。
4. 付款完成后，**Order Service** 再通知配送服务安排发货。

在这里，**Order Service** 充当中央控制器，直接管理每个步骤的顺序。这种方式可以保证流程的清晰性和控制性，因为 Orchestration 可以清楚地安排各个步骤，但可能导致 **Order Service** 越来越复杂。

---

### Choreography（服务编排）

**Choreography** 是没有中央控制的。相反，每个服务根据事件触发和独立响应。各服务相互观察和订阅事件，决定如何在流程中配合。这种方式更像舞蹈中的协同，不需要指挥，服务会根据事件自行响应。

#### 特点
- **去中心化**：没有中央服务管理，服务基于事件来触发响应。
- **松散耦合**：服务彼此独立，减少了单点依赖，使得系统更灵活。
- **潜在缺点**：事件之间的依赖关系更复杂，调试和监控更具挑战性。

#### 举个例子

还是以电商系统的用户下单流程为例，但这次使用 Choreography：

1. 用户创建订单，触发“OrderPlaced”事件。
2. **库存服务** 监听到“OrderPlaced”事件，检查库存并减少库存。减少成功后，触发“InventoryChecked”事件。
3. **支付服务** 监听到“InventoryChecked”事件，处理用户付款。如果付款成功，触发“PaymentCompleted”事件。
4. **配送服务** 监听到“PaymentCompleted”事件，安排发货。

在这种方式中，每个服务独立监听事件，并基于事件作出反应。比如，库存、支付和配送服务并不直接知道整个流程，而是基于各自需要监听的事件执行特定操作。

---

### 总结对比

| 特性       | Orchestration                     | Choreography                          |
|------------|-----------------------------------|---------------------------------------|
| **控制方式** | 由中央服务控制                    | 由事件驱动，每个服务独立响应            |
| **耦合性**   | 相对较高，受限于中央控制           | 松散耦合，服务独立                     |
| **难度**     | 较简单，流程更直观                | 较复杂，需要处理事件依赖               |
| **缺点**     | 容易产生“God Service”             | 监控、调试较复杂                       |

在实践中，复杂的系统可能会结合使用 Orchestration 和 Choreography 来处理不同的流程。例如，核心业务流程用 Orchestration 确保稳定和清晰，而子流程用 Choreography 提高灵活性和独立性。

## 7. API Gateways
- Purpose: Acts as a router in a microservice architecture.
- Functions:
    - Aggregates APIs for clients, allowing single API requests to access multiple services.
    - Protocol Translation: Translates client requests to internal protocols.
    - Examples: AWS API Gateway: Manages, monitors, and scales API calls with tools for traffic management and access control.

## 8. SDKs for Service Consumption
- Purpose: SDKs simplify access to services by abstracting protocol complexities.
- Notable SDKs:
    - AWS SDKs: Offers SDKs across languages like Java, Python, and platforms like iOS.
    - Spring Boot Feign Clients: For REST-based microservices, Feign integrates with Eureka and Ribbon for load balancing and Hystrix for fault tolerance.
### SDKs（Software Development Kits）

**SDKs** 是开发工具包，提供一些封装好的接口和功能，帮助开发者更轻松地使用服务，而无需深入了解服务的底层协议（如 HTTP、TCP/IP）。SDK 就像一个简化工具，使服务的访问更直观、快速。

---

### 举例说明

#### 1. AWS SDKs

AWS（亚马逊云服务）提供了跨多种编程语言和平台的 SDK，帮助开发者快速连接和使用 AWS 的各种云服务。例如，假设一个开发者需要上传文件到 Amazon S3 存储服务，AWS 的 SDK 提供了封装好的方法，开发者只需调用这些方法，就能轻松地将文件上传到云端，而不需要自己编写复杂的网络请求代码。

**例子：使用 AWS SDK 上传文件到 S3**

假设你正在用 **Python** 开发一个应用，需要将图片上传到 AWS 的 S3 存储：

- 使用 AWS SDK for Python (称为 boto3)，可以直接调用 boto3 的 `upload_file` 函数完成上传，而不用自己写 HTTP 请求。

- 代码示例：
  
```python
  import boto3
  
  s3 = boto3.client('s3')
  s3.upload_file('path/to/photo.jpg', 'your-s3-bucket', 'photo.jpg')
```
这段代码背后，SDK 已经处理了文件如何通过 HTTP 上传到 S3，以及如何进行身份验证等复杂细节。开发者无需关注传输的协议细节，只需简单地调用 upload_file 方法即可。

### 2. Spring Boot Feign Clients

**Feign** 是 Spring Boot 中常用的一个工具，它帮助简化调用其他 REST 服务的过程。Feign 让开发者可以像调用本地方法一样去调用其他服务，而不必自己构建 HTTP 请求。

此外，Feign 集成了 **Eureka**（服务发现）、**Ribbon**（负载均衡）以及 **Hystrix**（容错处理），使得微服务调用更加可靠、方便。

---

**例子：使用 Feign 调用其他微服务**

假设一个订单服务（Order Service）需要从库存服务（Inventory Service）获取商品库存信息：
1. **定义 Feign 客户端接口**：使用 Feign，你只需定义一个接口，不用写实际的请求代码。

```java
@FeignClient(name = "inventory-service")
public interface InventoryClient {
    @GetMapping("/inventory/{productId}")
    ProductInventory getInventory(@PathVariable("productId") String productId);
}
2. 调用 Feign 客户端：然后，Order Service 可以像调用普通方法一样，调用 getInventory 方法。
```java
@Autowired
private InventoryClient inventoryClient;

public void processOrder(String productId) {
    ProductInventory inventory = inventoryClient.getInventory(productId);
    // 处理订单逻辑
}
```
3. Eureka、Ribbon 和 Hystrix 的作用：

- Eureka：在微服务架构中，Inventory Service 的地址可能动态变化，Eureka 会自动找到当前活跃的 Inventory Service 实例。
- Ribbon：如果有多个 Inventory Service 实例，Ribbon 会帮助分配请求，进行负载均衡。
- Hystrix：如果 Inventory Service 暂时不可用，Hystrix 会提供备用方案，避免整个服务崩溃。
通过 Feign 客户端，Order Service 不需要关心如何发起 HTTP 请求、处理负载均衡、服务发现、故障处理等细节，只需调用 Feign 提供的方法，大大简化了代码量和维护难度。

## 9. Failure Handling and Resilience
- Designing for Failure:
    - Microservices should handle potential failures in any service call gracefully, aiming to minimize user disruption.
    - Real-time Monitoring: Use metrics like database throughput and order rate to detect anomalies.
## 10. Automation in Microservices
- Infrastructure Automation:
    - Continuous Integration (CI) and Continuous Delivery (CD) are crucial for managing microservices.
- Production Management:
    - Automated deployments, tests, and monitoring are essential for scalable and reliable microservice architectures.
