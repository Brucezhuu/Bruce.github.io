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
### 1. Shared Kernel

**Shared Kernel** 指的是两个团队之间共享的一部分模型或代码库。这种模式适用于紧密耦合的业务逻辑或数据模型，需要确保一致性。通常只有少量共享内容，因为共享内容可能导致更多依赖，增加变更带来的复杂度。

**例子**：
在一个电商系统中，“订单服务”和“支付服务”可能会共享“订单”模型的部分定义。这样，当订单状态更新时，支付服务可以直接了解最新的订单信息，而不必重新定义订单的结构。在这种模式中，“订单状态”可能会是一个 Shared Kernel，供两者共同使用，以便保持一致性。

---

### 2. Partnership

**Partnership** 是两个团队或服务之间的高度协作关系。这种关系意味着两个团队需要密切合作，以确保彼此集成的顺利进行。它通常用于两个团队之间有较强依赖的场景，双方协作一致，以确保系统的成功。

**例子**：
假设在银行系统中，“贷款服务”团队和“信用评估服务”团队采用 Partnership 模式，因为贷款审批需要依赖信用评分结果。这两个团队需要经常交流、协同开发，以确保接口一致性和逻辑对齐。这样可以在贷款流程中高效地调用信用评估数据。

---

### 3. Customer-Supplier

**Customer-Supplier** 模式适用于上游服务（Supplier）向下游服务（Customer）提供数据或服务，双方关系类似于客户和供应商。上游服务负责提供数据，下游服务依赖这些数据进行处理。通常由下游服务来推动需求变化。

**例子**：
在一个新闻平台中，“内容管理服务”（CMS）为“推荐服务”提供文章数据。推荐服务作为 Customer，根据 CMS 的提供的内容数据生成个性化推荐。CMS 作为 Supplier，需要确保文章数据更新的及时性和完整性，但不需要了解推荐服务的具体处理流程。

---

### 4. Anticorruption Layer

**Anticorruption Layer** 是一种隔离层，用于翻译和转换不同上下文之间的数据结构、术语或业务逻辑。这种模式用于避免直接依赖和引入上游系统的复杂性或不一致性。Anticorruption Layer 使下游服务能够独立于上游服务的实现方式。

**例子**：
假设在零售系统中，采购系统需要对接外部的供应商系统。由于供应商系统有独特的产品分类和编码方式，采购系统使用 Anticorruption Layer 将供应商的编码和结构转换为自己的内部标准，以保持一致性。这样即使供应商系统发生变化，也不会直接影响采购系统。

---

### 5. Published Language

**Published Language** 使用标准化的数据格式和协议（如 JSON、XML、ProtoBuf 等），从而实现不同上下文之间的数据传递和集成。这种模式适用于跨服务或跨团队的数据共享，尤其是在需要与外部系统集成时。

**例子**：
在跨境电商平台中，各个供应商使用不同的系统。平台通过定义标准化的 JSON 格式接口，实现供应商信息和库存数据的统一发布，供平台中的不同模块使用。这种标准化的 Published Language 使得不同供应商能够轻松集成到平台上，同时也减少了数据传输时的复杂性和误解。

### 6. Conformist

**Conformist** 模式指的是下游服务完全依赖上游服务的模型和规则。下游服务（Conformist）不对上游的数据结构做任何修改，而是接受并使用上游服务的结构和格式。此模式适用于下游服务无权改变上游模型、或上游服务控制力较强的场景。

**例子**：
在一个多商家电商平台中，“商品展示服务”（展示商家的商品信息）作为下游服务完全依赖“商品管理服务”的数据结构。商品管理服务由平台统一管理，下游的商品展示服务直接采用其结构，而不做转换。因此，如果商品管理服务的模型发生改变，商品展示服务也必须适应这些变化。这里的商品展示服务就充当了 Conformist 的角色。

---

### 7. Open Host Service

**Open Host Service** 是一种开放服务模式。它通过公开一个标准化的接口，让不同服务可以轻松访问其提供的功能和数据。通常会使用标准协议（如 REST API）和文档化的接口。这种模式适用于希望支持多个下游服务访问的情况，以减少每次集成的重复工作。

**例子**：
假设一个支付网关服务为多个电商平台（例如电子商务网站、移动应用等）提供支付处理服务。支付网关定义了一个通用的 REST API，允许电商平台通过 API 接口直接调用支付功能，无需为每个客户端创建独特接口。通过使用 Open Host Service 模式，支付网关服务为每个平台提供了一致且标准化的访问方式，方便了各个平台的集成。

---

### 8. Separate Ways

**Separate Ways** 模式适用于不同的上下文或团队有完全不同的业务需求时。此模式意味着两个上下文可以完全独立工作，彼此没有依赖关系，不需要共享模型或业务逻辑。通常用于两个团队或服务间的业务需求差异较大，不需要数据交互的场景。

**例子**：
假设一个公司同时运营在线商店和线下实体店的会员系统。在线商店有一个“会员管理服务”，主要用于处理用户的线上活动（如积分、折扣等）；而实体店有一个完全独立的“会员积分管理系统”，管理线下会员积分规则。由于线上和线下会员系统的需求和规则完全不同，它们互不依赖，没有共享的模型，各自独立发展。这样的场景中，线上和线下的会员系统采用了 Separate Ways 模式，各自保持独立。


## 3. Aggregates
- Definition: Clusters of entities within the same bounded context treated as a single unit (aggregate root).
Principles:
- Transactional Consistency: All elements within an aggregate must be consistent at the end of a transaction.
- Rules of Thumb:
    - 1. Protect Business Invariants Inside Aggregate Boundaries
    Rule: Business invariants—rules or constraints that must be maintained throughout an aggregate's lifecycle—should be protected within aggregate boundaries to ensure data consistency and correct business logic.
    Example: In a task management system, when the hoursRemaining for each task within a BacklogItem (a backlog entity) reaches zero, the BacklogItem status should automatically be set to DONE. This invariant ensures that once all tasks are complete, the backlog item reflects a completed state.
    - 2. Design Small Aggregates
    Rule: Keep aggregates small to reduce memory usage, improve loading speed, and enhance transaction processing efficiency. Smaller aggregates are also easier to test and maintain.
    Example: Consider a Product aggregate that contains multiple BacklogItems, Releases, and Sprints. If all related entities are included directly in Product, the aggregate would be overly large. Breaking down each BacklogItem and Release as smaller, independent aggregates minimizes memory consumption and transactional complexity.
    3. Reference Other Aggregates by Identity Only
    Rule: Avoid directly referencing other aggregates' objects within an aggregate. Instead, use only unique identifiers. This design simplifies data persistence and aggregate relationships while reducing unnecessary coupling.
    Example: Aggregates like BacklogItem, Release, and Sprint should reference the Product aggregate only by ProductId rather than holding a full object reference. This avoids unnecessary memory load and maintains aggregate independence.
    4. Use Eventual Consistency When Updating Other Aggregates
    Rule: When an aggregate needs to synchronize data with other aggregates, use “eventual consistency” instead of immediate updates. Trigger a domain event in one aggregate, which another aggregate processes later.
    Example: In a Sprint aggregate, when a BacklogItem is committed to a sprint, it can update the BacklogItem in one transaction and then publish a BacklogItemCommitted event. The Sprint aggregate can process this event in another transaction to maintain synchronization. This approach minimizes tight coupling between aggregates.
    5. Avoid an Anemic Domain Model
    Rule: Aggregates should contain business logic rather than serving as mere data access objects with only getters/setters. Embedding business logic within aggregates prevents the dispersion of rules across application layers or helper classes.
    Example: In an order system, the Order aggregate should not only expose fields but also contain business logic like calculating prices, generating order summaries, and checking stock. This ensures that business logic is encapsulated within the aggregate.
    6. One Repository for Persistence of One Aggregate
    Rule: Each aggregate should have a single repository responsible for its persistence, consolidating all storage logic for the aggregate. Factory methods may be used for instantiating complex aggregates.
    Example: In a user management system, the User aggregate, including all its entities and value objects, should be managed by a single UserRepository. This focused repository structure simplifies persistence and maintains the integrity of the aggregate.

## 4. Domain Events
- Definition: Record of a business-significant event within a bounded context.
- Characteristics:
    - Named in past tense (e.g., ProductCreated).
    - Contains all relevant data to inform subscribers.
- Usage:
    - Loosely coupled systems can use domain events to propagate changes.
    - Ensure causal consistency, meaning events occur in logical order across services.
    - For each of the context mappings of Messaging type
        - Identify the business functions that rely on the context mapping
        - For each of the business functions
            - Identify the domain events that are required
    -  For each propagation of changes from one aggregate to another
        - Identify the domain events that is required

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
    - can rely on rule engine
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

## 11. Summary
- After identifying the reusable services (i.e. BCs), they are **not totally decoupled**
  - There are still **relationships** and **interactions** between them

- **Context Mappings** model the **integration** between reusable services

- **Aggregates** **break the references** between reusable services and specify the **scope of transaction**

- **Domain Events** provide a means for **loosely coupled integration** between reusable services

- **More complex business logic** requires either **orchestration** or **choreography** of reusable services

- **API Gateway** provides **server-side discovery**, **load balancing**, **client-specific APIs**, **protocol translation**, etc.

- **Client SDKs** are a **convenient way for clients to consume services**

## 12. WorkShop

{{< figure src="/img/in-post/Platform Engineering/maid2Order-sample-solution.png" caption="<span class=\"figure-number\">Figure 1: </span>DDD sample solution" width="1000px" >}}
{{< figure src="/img/in-post/Platform Engineering/workshop.png" caption="<span class=\"figure-number\">Figure 2: </span>workshop" width="1000px" >}}

{{< figure src="/img/in-post/Platform Engineering/worshopsolution-Aggregate&DEs.png" caption="<span class=\"figure-number\">Figure 3: </span>solution" width="1000px" >}}

### In an aggregate, a Value Object

A **Value Object** is a type of object that represents a concept or attribute but does not have a distinct identity. Unlike entities, which are defined by a unique identifier, value objects are defined by their attributes and are typically immutable. They help model attributes of an entity, often describing or quantifying it, and they can be easily replaced or recreated when needed.

---

### Characteristics of Value Objects

- **Immutability**: Once created, the values cannot change. If a new value is needed, a new instance is created.
- **No Unique Identity**: Two value objects with the same attributes are considered equal.
- **Lightweight and Replaceable**: Since they lack identity, they’re easier to replace or recreate.

---

### Example

Consider an e-commerce application with an **Order** aggregate. Each order may include a **ShippingAddress** as a Value Object within the aggregate. The ShippingAddress value object could consist of:

- **Street**
- **City**
- **State**
- **ZipCode**
- **Country**

In this example, the ShippingAddress value object:

- **Describes an Attribute of the Order**: It provides the necessary details for where the order should be shipped.
- **Is Immutable**: If the address needs to be updated, a new ShippingAddress object is created with the updated values, rather than modifying the existing one.
- **Has No Unique Identity**: Two orders could have identical ShippingAddress values, but they do not require separate identities since they represent the same location.

# Criteria for Identifying the Root Entity within an Aggregate

To determine the root entity within an aggregate, consider the following criteria:

### 1. Ownership of Other Entities and Value Objects
The root entity serves as the main entry point and the owner of all other entities and value objects within the aggregate. Any modifications or access to entities within the aggregate should go through the root entity.

**Example**: In an Order aggregate, the Order entity is the root because it manages the lifecycle of related entities like OrderItem, ShippingAddress (a value object), and PaymentDetails. Any changes to items or addresses should be handled through the Order root.

### 2. Uniquely Identifiable within the Aggregate
The root entity has a unique identity (usually an ID) that represents the entire aggregate. This ID is used externally to reference the aggregate as a whole.

**Example**: In a Customer aggregate, the Customer entity is the root and has a unique CustomerID. All other entities, such as Address or OrderHistory, are accessed through Customer, making CustomerID the primary reference for the aggregate.

### 3. Transaction Boundaries
The root entity defines the boundaries of a transaction. Any operation within the aggregate should ensure consistency across the root and its associated entities.

**Example**: In a ProductCatalog aggregate, the Product entity can be the root if updates to related entities like ProductVariants and PricingDetails need to be kept within a single, consistent transaction.

### 4. Business Logic Control Point
The root entity enforces business invariants and rules within the aggregate. It should contain the core logic necessary to ensure that any changes made to the aggregate conform to business requirements.

**Example**: In a BankAccount aggregate, BankAccount is the root entity responsible for enforcing rules such as overdraft protection or withdrawal limits across associated entities like TransactionHistory.

### 5. Aggregate Lifecycle Management
The root entity controls the creation, modification, and deletion of the entire aggregate. When the root entity is deleted, all related entities within the aggregate should also be removed.

**Example**: In an Invoice aggregate, Invoice is the root entity. Deleting Invoice removes all associated LineItems and PaymentRecords, ensuring the entire aggregate is managed as a single unit.
