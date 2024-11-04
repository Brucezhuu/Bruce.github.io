+++
title = "PE_02: PE02 Reusable Assets and Frameworks_v2.1"
author = ["Bruce"]
date = 2024-10-14
series = ["Platform Engineering"]
tags = ["Platform Engineering", "SWE5001"]
draft = false
+++

## 1. Understanding Reusable Assets in Platforms
Reusable assets are components or services that can be leveraged across multiple systems or applications within a platform. They streamline development by providing ready-to-use functionalities and frameworks.

Types of Reusable Assets:

- Services: Components that provide specific, well-defined functionalities (e.g., user authentication, data storage).
- Frameworks: Provide a structured environment for building applications, offering partial implementations of common functions.
- Shared Libraries: Collections of code used across different services and applications, typically containing helper functions, data structures, or protocols.
{{< figure src="/img/in-post/Platform Engineering/02-01.png" caption="<span class=\"figure-number\">Figure 1: </span>Other resusable assets" width="1000px" >}}

## 2. Characteristics of Reusable Services
Reusable services must be maintainable, extensible, and resilient to support long-term usage across platforms:

- Single Responsibility: Each service should focus on one specific functionality, following the Single Responsibility Principle (SRP).
- Loose Coupling: Services should minimize dependencies on other services to ensure modularity and flexibility.
- Scalability: Design services to be horizontally scalable, meaning they can scale independently of each other.
- Stable and Cohesive Functions: Services should provide a stable interface and cohesive functions, minimizing the need for frequent changes.

- Horizontally Scalable（水平扩展）
水平扩展是指通过增加更多的独立节点或服务器来增加系统的容量。例如，在一个服务器群中加入更多的服务器，以分散处理负载。水平扩展的优势是灵活性更高，系统可以更好地处理大量并发请求，因为多个节点可以协同工作来分担负担。常见的水平扩展例子是数据库的分片或使用负载均衡器来分配流量。

    - 例子：当网站访问量增加时，增加更多的服务器节点，来支撑增长的用户访问。

- Vertically Scalable（垂直扩展）
垂直扩展是指通过增加现有服务器的资源（如CPU、内存、硬盘）来提升系统的容量。这种方式通常比水平扩展简单，因为不涉及复杂的分布式架构，但受限于硬件的性能上限，一台服务器可以提升的资源总是有限的。

    - 例子：将一台服务器的内存从16GB升级到32GB，以提高其处理能力

## 3. Identifying and Designing Reusable Services
- **Domain-Driven Design (DDD)** is a common approach to **identifying reusable services**:

    - **Bounded Contexts:** Define boundaries around domain areas, such as User Management or Payment Processing, to ensure each service operates within a specific context.
    - **Ubiquitous Language:** Standardize terminology across the team and codebase for clarity and consistency.
    - **Entity Segmentation:** Identify and separate entities based on business requirements to keep services manageable and focused.
{{< figure src="/img/in-post/Platform Engineering/02-02.png" caption="<span class=\"figure-number\">Figure 2: </span>Two levels of design conducted in DDD" width="1000px" >}}
- All the entities in a BC are functionally cohesive to the BC
- Non-cohesive entities are pushed out to other BCs
- The entities in a BC are specified by one UL (Different languages are used in different BCs)
- Multiple BCs prevent the design of monolithic applications, hence
    - Smaller set of test cases for a BC And other similar benefits
    - The boundaries of BCs can be the boundaries for services, source code repositories and databases
    - Teams can be assigned on a per BC basis
### Strategic Design
Strategic design addresses high-level, coarse-grained design issues with the goal of dividing the overall business domain into distinct areas, ensuring each area can operate independently. Key concepts in strategic design include:

- Bounded Contexts
    - Explanation: Bounded Contexts are a core concept in DDD that define specific boundaries within the business domain. Each context focuses on a particular business function, reducing the risk of mixing responsibilities across areas.
    - Example: In an e-commerce platform, there might be separate bounded contexts for "User Management" and "Order Processing." The User Management context handles user registration and login, while Order Processing focuses on order creation and payment. This separation allows each system to focus on its own responsibilities without interfering with the other.
- Ubiquitous Language

    - Explanation: Ubiquitous Language ensures that within a bounded context, all team members (developers, business analysts, etc.) use the same terminology for key business concepts, aligning communication and code with business needs.
    - Example: In the "Order Processing" context, the team consistently uses terms like "Order," "Payment," and "Refund." These terms mean the same thing to everyone on the team and in the code, reducing miscommunication and ensuring the design is aligned with business requirements.
- Subdomains

    - Explanation: A domain can be further divided into subdomains, which can include the core domain (the most crucial part of the business) and supporting or generic subdomains.
    - Example: In a financial system, "Payment Processing" might be the core domain, as it's the primary function of the system. "Report Generation," on the other hand, could be a supporting subdomain, providing necessary analytics but not being central to the business logic.
- Context Mappings

    - Explanation: Context Mappings define how different bounded contexts integrate and relate to each other, allowing them to work together within a system.
    - Example: In an e-commerce platform, "User Management" and "Order Processing" may need to share user information. Context mappings specify how these contexts interact, possibly via APIs or events, to ensure smooth data sharing between them.
### Tactical Design
Tactical Design addresses fine-grained, detailed design issues with the goal of implementing business logic within specific bounded contexts. The primary tools here include:

- Aggregates

    - Explanation: Aggregates are clusters of related entities that are treated as a single unit to ensure data consistency within a bounded context.
    - Example: In the "Order Processing" context, an order and its associated order items (products) might form an aggregate. This ensures that when updating order details, the state of the associated order items is also updated, maintaining data integrity.
- Domain Events

    - Explanation: Domain Events capture significant business occurrences within a bounded context and can be used to communicate information or trigger actions across contexts.
    - Example: In the "Payment Processing" context, when a payment is successfully completed, a "Payment Successful" domain event might be generated. Other contexts, such as Order Management, can listen for this event and update the order status accordingly.
{{< figure src="/img/in-post/Platform Engineering/02-03.png" caption="<span class=\"figure-number\">Figure 3: </span>DDD" width="1000px" >}}
- DDD vs. Object-Oriented Analysis and Design (OOAD):

    - DDD emphasizes domain objects and focuses on aligning design with domain logic.DDD强调领域对象和与业务逻辑的对齐，即更关注业务语境和业务需求的细节，确保系统的设计能够反映业务的实际需求。
    - OOAD centers around functional requirements and the relationships between them, catering to a broader architectural style.OOAD则更关注功能需求和类之间的关系，是一种更广泛的设计方法。它侧重于功能和类的结构设计，更适合一些没有复杂业务逻辑的系统设计。
### workshop：maid2Order (ppt 29 ~ 32)
{{< figure src="/img/in-post/Platform Engineering/maid2Order-sample-solution.png" caption="<span class=\"figure-number\">Figure 4: </span>Sample solution" width="1000px" >}}
## 4. Frameworks: Characteristics and Benefits
A framework is a partially implemented structure that can be extended to develop applications with specific domain logic, such as user interfaces or data access layers.

- Characteristics of Reusable Frameworks:

    - Focused on Solving Domain Problems: Frameworks solve specific domain issues (e.g., data management, user interaction).
    - Multiple Layer Support: Frameworks can be designed for various layers of an application, such as the data layer (e.g., Spring Data) or the web layer (e.g., Spring MVC).
    - Consistency and Integration: Frameworks should be consistent across layers and integrate seamlessly with other development tools.

## 5. Qualities of a Well-Designed Framework
- Simplicity: Avoid complexity to ensure ease of use. Simplicity allows developers to quickly understand and implement the framework.
- Adaptability and Evolution: Design frameworks with flexibility to accommodate future changes, preventing obsolescence.
- Consistency: Maintain a cohesive design to promote productivity and minimize errors.
- Testability: Ensure applications built with the framework are easy to test, ideally following test-driven development principles.
- Principles of Framework Design:

    - Scenario-Driven: Base the design on common use cases and focus on usability.
    - Low Barrier to Entry: Provide straightforward APIs that are easy to experiment with for new users.
    - Self-Documenting: Design objects and methods that are self-explanatory, minimizing the need for external documentation.

## 6. Framework Design Process
- Domain Analysis: Collect examples and understand domain requirements before framework design.
- Whitebox to Blackbox Transition: Start with a clear, modular framework that provides control over its components, gradually refining it into a more general, reusable tool.
- Iterative Improvement: Use feedback from early adopters to identify and address weak points in the framework design.

## 7. Shared Libraries: Advantages and Limitations
Shared libraries contain reusable code across clients and services within a platform. They support various functionalities, such as data structures or authentication protocols.

- Characteristics of Shared Libraries:

    - State Specification: Define class states through attributes, ensuring consistency across components.
    - Behavioral Implementation: Provide reusable functions to handle common processes (e.g., database connections).
- Pitfalls:

    - Coupling: Libraries shared across services can create dependencies, making it difficult to update individual services independently.
    - Loss of Technology Heterogeneity: Shared libraries are often platform or language-specific, limiting flexibility in choosing different technology stacks.
- Guidelines:

    - Don’t Repeat Yourself (DRY): Follow DRY principles within a service, but avoid excessive cross-service code sharing that creates dependencies.
    - Client Responsibility: Let clients manage updates to client libraries, allowing flexibility and minimizing unwanted disruptions.

## 8. Common Pitfalls in Reusable Frameworks and Services
- Immature Domain Understanding: Misinterpretation of domain needs leads to frequent design changes.
- Unstable Interfaces: Frequent interface changes create unnecessary ripple effects on dependent applications.
- Premature Publication: Releasing frameworks too early can lead to breaking changes. Validate frameworks with pilot projects before broad deployment.
- Over-Coupling: Avoid coupling services too tightly to shared libraries or frameworks to maintain service independence and flexibility.

## 9. Practical Examples of Reusable Frameworks and Services
- Case Study: Singapore Government Tech Stack (SGTS)

    - SGTS provides **a standardized suite of tools** for government agencies to build secure, integrated digital services.
    -Reusable Services: Centralized services like identity verification (MyInfo) and payment processing enable agencies to avoid duplicating effort and achieve consistency.
- Case Study: Netflix’s Client Libraries

    - Service Discovery and Failure Handling: Netflix’s client libraries manage service discovery and failover, ensuring client-server reliability while maintaining decoupling.
    - Drawbacks: Tight coupling between client libraries and services can limit flexibility when scaling or updating services independently.
- AWS SDKs: AWS SDKs provide flexibility for users to decide when to upgrade client libraries, ensuring services can evolve independently without forcing client changes.

## 10. Summary 
- Reusable assets are instrumental in boosting the productivity of the development teams of a software platform
- Reusable services and frameworks enable larger scale reuse while shared libraries allow smaller scale but ad-hoc reuse
- DDD techniques help to identify and design services
- Guidelines are available for selecting and designing frameworks
- Overuse of shared libraries could unnecessarily constrainthe development of components of a software platform

