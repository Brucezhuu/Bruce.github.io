+++
title = "PE_03: API Design"
author = ["Bruce"]
date = 2024-10-14
series = ["Platform Engineering"]
tags = ["Platform Engineering", "SWE5001"]
draft = false
+++

## 1. API Design Mindset: Product over Project
- Product Mindset: APIs should be treated as a long-term product with regular updates, focusing on consumer needs and ease of use. Success metrics include adoption rate, developer satisfaction, and business impact.
- Core Elements:
    - Outside-In Approach: Design based on the needs of consumers (e.g., developers, end-users).
    - Data-Driven: Use analytics to monitor API usage and improve based on user behavior.
    - Time to Market: Release a Minimum Viable Product (MVP) quickly, refining with consumer feedback.
## 2. API Standards and Documentation
- OpenAPI Specification (OAS): Standard format for describing REST APIs, ensuring compatibility across platforms.
- Documentation Tools:
    - Swagger: Tools for designing, documenting, and testing APIs with OAS.
    - RAML: Focuses on API lifecycle management, merging with OpenAPI.
    - API Blueprint: Promotes a “design-first” approach, decoupling API design from backend implementation.
## 3. RESTful Guiding Principles
- Client-Server Model: Separation of concerns between client and server enhances modularity.
- Statelessness: No client session state on the server, enabling easier scaling.
- Uniform Interface: Resources are identified by URLs, with operations defined by HTTP methods.
- Layered System: The API’s design should allow for intermediate layers (e.g., caching).
- Caching: Improves performance and reduces server load.
- HATEOAS (Hypermedia as the Engine of Application State): Clients navigate the API using links embedded in responses.
## 4. API Design Best Practices
- Resource Naming:

    - Use nouns (e.g., /customers) rather than verbs (avoid /getCustomers).
    - Use plural nouns (e.g., /products).
    - Kebab-case for multi-word resources (e.g., /customer-orders).
- HTTP Methods:

    - GET: Retrieve resource.
    - POST: Create new resource.
    - PUT: Update existing resource (idempotent).
    - PATCH: Partially update resource.
    - DELETE: Remove resource.
- Idempotency:

    - GET, PUT, and DELETE should be idempotent (repeated requests yield the same result).
    - POST is not idempotent as it creates new resources.

## 5. API Response and Error Handling
- Status Codes:
    - 2xx: Success (e.g., 200 OK, 201 Created).
    - 4xx: Client errors (e.g., 400 Bad Request, 404 Not Found).
    - 5xx: Server errors (e.g., 500 Internal Server Error, 503 Service Unavailable).
- Error Responses:
Use meaningful messages, and provide details for developers, such as error codes and potential fixes.

## 6. API Maturity Model (Richardson Maturity Model)
- Level 0 (POX): Plain Old XML, limited use of HTTP capabilities.
- Level 1 (Resources): Defines different resources with a single HTTP method.
- Level 2 (HTTP Verbs): Full use of HTTP methods on resources.
- Level 3 (HATEOAS): Includes hypermedia links for state transitions, fully RESTful.

## 7. GraphQL as an Alternative
Key Features:
    - Single Endpoint: Consolidates multiple endpoints into a single entry point. （"Single Endpoint" refers to a design pattern in APIs where multiple functionalities or resources are accessed through a single entry point instead of having multiple, specific endpoints for each function or resource. ）
    - Client-Driven Data Fetching: Clients can specify exactly the data they need, reducing over-fetching or under-fetching.
    - Common Use Cases: Ideal for complex applications with multiple services or when fine-grained data fetching is necessary.
### Example of Single Endpoint:
Imagine an e-commerce API with a single endpoint /api:

- Request 1: Retrieve user information
    - POST /api
    - Body: { "query": "user", "userId": 123 }
- Request 2: Retrieve order information
    - POST /api
    - Body: { "query": "order", "orderId": 456 }
- Request 3: Retrieve product information
    - POST /api
    - Body: { "query": "product", "productId": 789 }
Instead of having /users, /orders, and /products, everything is handled by /api. The server interprets the request body to determine what data to return.

## 8. API Versioning
- Versioning Rules:
    - Avoid abrupt removal of features.
    - Ensure backward compatibility.
    - Add features as optional.
- Approaches:
URI Versioning: Add version to the URL (/v1/customers).
Header Versioning: Use custom headers for versioning (Accept-Version: v1).

## 9. API Security
- OAuth 2.0: Industry standard for authorization, using tokens to grant access to API resources without sharing credentials.
- CORS (Cross-Origin Resource Sharing): Controls access to resources based on origin, scheme, and port.
- Payload Inspection: Validate content, headers, and parameters for security.

## 10. API Performance Optimization
- Caching Layers:
    - Client-side: Caches data locally.
    - Content Delivery Network (CDN): Caches API data closer to the user.
    - API Gateway: Manages API requests and responses.
- Pagination and Filtering:
    - Offset Pagination: Uses offset and limit for pagination.
    - Cursor Pagination: Uses cursors for efficient, scalable pagination.
- Compression: Enable gzip to reduce payload sizes.

## 11. API Testing Strategy
- Functional Testing: Verifies the API’s behavior, covering all valid and invalid input cases.
- Security Testing: Ensures API data is secure (e.g., deny-by-default policy, authentication and authorization).
- Performance Testing: Measures load handling, stress limits, and usability.

## 12. Logging and Monitoring
- Log Types:
    - Request Logs: Record incoming requests for debugging.
    - Error Logs: Capture error details for diagnosis.
- Performance Metrics:
    - Track Requests Per Minute (RPM), Latency, Failure Rate, and Uptime.
- Monitoring Tools: Moesif, Stackify, or custom tools to track API metrics and user interaction.

## 13. API Orchestration and Transaction Management
- Transaction Management:
    - Two-Phase Commit Protocol: Ensures distributed transactions are completed in multiple services.
    - SAGA Pattern: Manages transactions in a sequence with compensation actions if a transaction step fails.
- API Orchestration: Manages workflows across microservices, ensuring that the sequence and dependencies of API calls are handled.

### Transaction Management（事务管理）
在微服务架构中，一个业务操作可能涉及多个服务，例如用户下单可能涉及订单服务、支付服务和库存服务。为了保证这些操作的一致性和完整性，需要一种机制来管理分布式事务。这里提到了两种方法：两阶段提交协议和SAGA模式。

#### Two-Phase Commit Protocol（两阶段提交协议）
- 解释：两阶段提交协议是一种用于分布式事务的经典协议，分为两个阶段：准备阶段和提交阶段。所有涉及的服务首先会在“准备阶段”报告它们是否准备好完成操作；如果都准备好了，系统进入“提交阶段”，各服务同时提交操作，确保事务的整体一致性。
- 例子：假设一个电子商务应用的购买流程涉及多个微服务（如订单服务、支付服务和库存服务）。在两阶段提交中：
    - 准备阶段：订单服务创建订单并锁定库存；支付服务检查账户余额是否充足；如果各服务都准备好，它们返回“准备就绪”。
    - 提交阶段：一旦所有服务返回准备就绪，系统指示各服务正式提交操作。如果某个服务在准备阶段失败（例如库存不足），整个操作就会回滚。
- 优缺点：这种方式可以确保事务一致性，但会导致较高的耦合和性能开销，因此在微服务中较少使用。
#### SAGA Pattern（SAGA模式）
- 解释：SAGA是一种处理分布式事务的模式，通过将事务分解为一系列独立的步骤，每个步骤对应一个服务。如果某一步失败，则会触发“补偿操作”，用来回滚之前的步骤。SAGA通常用于微服务系统中的长事务。
- 例子：继续上面电子商务的例子，假设用户订单包含以下步骤：
    - 订单服务创建订单。
    - 库存服务锁定库存。
    - 支付服务扣款。 如果扣款失败，系统会执行补偿操作，比如：
    - 让库存服务释放锁定的库存，
    - 让订单服务取消订单。
- 优点：SAGA模式的事务管理灵活性更高，适合需要高并发和容错的微服务架构，但实现复杂，需要为每个步骤设计补偿逻辑。
#### API Orchestration（API编排）
- 解释：在微服务架构中，业务流程往往涉及多个服务。API编排负责管理跨服务的工作流程，确保API调用的顺序和依赖关系得以正确执行。例如，一个编排服务（Orchestrator）可以协调多个微服务的调用，确保流程按正确的顺序完成，并处理异常情况。

- 例子：假设有一个电商应用需要完成以下流程：用户下单 -> 付款 -> 减少库存 -> 发货。使用API编排的方式可以创建一个专门的编排服务来负责整个流程的控制：

    - 编排服务首先调用订单服务创建订单。
    - 创建订单成功后，调用支付服务进行付款。
    - 如果付款成功，调用库存服务减少库存。
    - 最后，调用物流服务完成发货。 如果任意步骤失败，编排服务可以根据业务逻辑进行补偿或重新尝试。
#### API编排 vs. API聚合：

- API编排更关注流程控制和执行顺序，确保依赖关系。适用于需要按顺序调用的复杂流程。
- API聚合通常指将多个API的结果组合成一个结果，适用于需要合并数据的场景。

## 14. API Design Workshop
RESTful API Design:

### 1. **Domain Analysis Based on Requirements**
- **Entities**:
  - **Customer**: The user ordering the service.
  - **CleaningAgency**: The provider offering the cleaning services.
  - **Order**: Represents each cleaning service order made by a customer.
  - **SubscriptionPlan**: Represents the subscription plan chosen by the customer, if applicable.

- **Subscription Plans**:
  - **OnceAFortnight**
  - **OnceAWeek**
  - **TwiceAWeek**

### 2. **API Design - RESTful (Level 2)**

#### 1. **API: Place an Order for Cleaning Service**

- **URI**: `/api/v1/orders`
- **HTTP Verb**: `POST`
- **Description**: Allows the customer to place an order for cleaning service from a specific agency, either as a one-time order or under a subscription plan.
- **Parameters**:
  - `customerId` (String, required): ID of the customer placing the order.
  - `agencyId` (String, required): ID of the cleaning agency selected.
  - `planType` (String, optional): Subscription plan type (`OnceAFortnight`, `OnceAWeek`, `TwiceAWeek`).
  - `date` (Date, required): Date of the cleaning service.
  - `time` (String, required): Time of the service.
- **Request Body Example**:
  ```json
  {
    "customerId": "customer123",
    "agencyId": "agency456",
    "planType": "OnceAWeek",
    "date": "2024-10-15",
    "time": "10:00"
  }
  ```
- **ResResponse**：
- **200 OK** for successful retrieval.
- **201 Created** for successful creation.
- **204 No Content** for successful deletion.
- **400 Bad Request** for invalid input.
- **404 Not Found** for non-existent user.
- response body example: 
```json
{
  "orderId": "order789",
  "status": "Confirmed",
  "customerId": "customer123",
  "agencyId": "agency456",
  "planType": "OnceAWeek",
  "date": "2024-10-15",
  "time": "10:00"
}
```

### 2. API: View Subscription Plans
- **URI**: `/api/v1/plans`
- **HTTP Verb**: `GET`
- **Description**: Fetch available subscription plans and their details (e.g., frequency and pricing).

#### Response:
- **200 OK** on success

#### Response Body Example:
```json
[
  {
    "planType": "OnceAFortnight",
    "description": "Cleaning service once every two weeks",
    "rate": "100 USD"
  },
  {
    "planType": "OnceAWeek",
    "description": "Cleaning service once a week",
    "rate": "180 USD"
  },
  {
    "planType": "TwiceAWeek",
    "description": "Cleaning service twice a week",
    "rate": "320 USD"
  }
]
```

### 3. API: Retrieve Customer Orders
- **URI**: `/api/v1/customers/{customerId}/orders`
- **HTTP Verb**: `GET`
- **Description**: Retrieves all cleaning service orders for a specific customer, including both one-time and subscription-based orders.

#### Parameters:
- `customerId` (String, required): ID of the customer whose orders are being fetched.

#### Response:
- **200 OK** on success

#### Response Body Example:
```json
[
  {
    "orderId": "order789",
    "agencyId": "agency456",
    "date": "2024-10-15",
    "time": "10:00",
    "planType": "OnceAWeek",
    "status": "Confirmed"
  },
  {
    "orderId": "order1011",
    "agencyId": "agency456",
    "date": "2024-10-08",
    "time": "14:00",
    "planType": "OnceAWeek",
    "status": "Completed"
  }
]
```
- 404 Not Found if the customer or orders are not found.


### Alternative API Design - GraphQL

#### Types Definition
```
type Customer {
  customerId: ID!
  name: String
  orders: [Order]
}

type Order {
  orderId: ID!
  customer: Customer!
  agency: Agency!
  date: String!
  time: String!
  planType: PlanType
  status: String
}

type Agency {
  agencyId: ID!
  name: String
  orders: [Order]
}

enum PlanType {
  OnceAFortnight
  OnceAWeek
  TwiceAWeek
}

type SubscriptionPlan {
  planType: PlanType!
  description: String!
  rate: Float!
}

type Query {
  getCustomerOrders(customerId: ID!): [Order]
  getAvailablePlans: [SubscriptionPlan]
}
```

### Sample GraphQL Query
#### Query: Retrieve a customer’s orders and subscription plans
```
query {
  getCustomerOrders(customerId: "customer123") {
    orderId
    date
    time
    planType
    status
    agency {
      agencyId
      name
    }
  }

  getAvailablePlans {
    planType
    description
    rate
  }
}
```
#### Sample Response:
```json
Copy code
{
  "data": {
    "getCustomerOrders": [
      {
        "orderId": "order789",
        "date": "2024-10-15",
        "time": "10:00",
        "planType": "OnceAWeek",
        "status": "Confirmed",
        "agency": {
          "agencyId": "agency456",
          "name": "CleanHouse Ltd."
        }
      }
    ],
    "getAvailablePlans": [
      {
        "planType": "OnceAFortnight",
        "description": "Cleaning service once every two weeks",
        "rate": 100
      },
      {
        "planType": "OnceAWeek",
        "description": "Cleaning service once a week",
        "rate": 180
      },
      {
        "planType": "TwiceAWeek",
        "description": "Cleaning service twice a week",
        "rate": 320
      }
    ]
  }
}
```