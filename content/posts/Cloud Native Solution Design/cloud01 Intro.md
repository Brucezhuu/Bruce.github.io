+++
title = "CloudNativeSolution 01: Cloud Overview"
author = ["Bruce"]
date = 2024-10-24
series = ["Cloud Native Solution"]
tags = ["Cloud Native Solution", "SWE5001"]
draft = false
+++
{{< figure src="/img/in-post/Cloud Native Solution Design/overview.png" caption="<span class=\"figure-number\">Figure 1: </span>Overview" width="1000px" >}}

# Overview of Components in Modern Software Development

## 1. Text Messaging
### Popular Platforms:
- **WeChat**: Described as a "Super App," integrating various services beyond messaging.
- **Telegram, WhatsApp, Signal, Messenger**: Known for privacy features and cross-platform messaging.
- **Insta (SM Platform)**: Likely refers to Instagram, a social media platform with messaging features.

### Technologies Used:
- **TS (TypeScript)**: A frontend language offering strict typing on top of JavaScript.
- **Channel (Swift, Kotlin)**: Indicates mobile channel development using Swift (iOS) and Kotlin (Android).
- **Search GREP**: Refers to searching or pattern matching in text.
- **Erlang**: Known for building scalable and fault-tolerant systems, often used in messaging apps.
- **ELK (Elastic)**: A stack for managing and visualizing log data.
- **BEAM**: Refers to the Erlang virtual machine, optimized for concurrent processes.

## 2. Platforms
### Various Platform-Based Applications and Frameworks:
- **Shopee, Grab, Lazada**: E-commerce and ride-hailing platforms.
- **Uber, Airbnb, Netflix, Spotify**: Platforms that provide services such as transportation, accommodation, streaming, and music.

### Technology:
- **GoLang (Go)**: A language popular for scalable backend services, often used in microservices architectures.

## 3. Legacy Streams
### Technologies and Platforms Considered Legacy:
- **YouTube**: As a traditional streaming platform.
- **Languages**:
  - **Python, Java, C++, JS**: Widely used in various systems, though some may be seen as legacy depending on the context.

## 4. Challenges in Modern App Development
- **Multiple PL (Programming Languages)**: Modern apps often incorporate several programming languages within a single application.
- **Scaling is Challenging**: Scalability is a key issue as user bases grow.

### Modern Apps Characteristics:
- **Full Stack**: Developers work across both frontend and backend.
- **Web/Native**: Support for both web and native mobile applications.
- **Multiple DB**: Usage of various databases depending on data needs.
- **Multiple Tech Stacks**: Incorporating a range of frameworks, libraries, and tools.
- **Multiple PL**: Utilization of different programming languages.

## 5. IT Architecture and Blueprint
### IT Architecture:
- **Focuses on Non-Functional Requirements**: Such as Scalability and other "ilities" (like reliability, availability).
- **Priority**: Indicates a focus on prioritizing these requirements.

### Blueprint:
- **Components and their Interactions**: Emphasizes understanding the building blocks of a system and how they interact.
- **Perspective**: Implies a high-level view of architecture.
- **Covers Application Solution, Data Security, and Cloud**: Highlighting security and cloud-based solutions.

{{< figure src="/img/in-post/Cloud Native Solution Design/Architect.png" caption="<span class=\"figure-number\">Figure 2: </span>a holistic view of cloud-native architecture" width="1000px" >}}

# Overview of Cloud Native Solution Architecture and Infrastructure

## 1. Human2Human (H2H)
### Purpose:
This section emphasizes the importance of clear and compliant communication in architectural design, ensuring that all stakeholders understand the project requirements and structure.

### Key Aspects:
- **Comms (Communication)**: Facilitates information sharing among team members.
- **Clarity**: Ensures that diagrams and documentation are easily understood.
- **Compliance**: Adheres to standards and regulatory requirements.

## 2. Diagram
### Diagram Types:
- **UML (Unified Modeling Language)**: A visual language for modeling software systems, often used for designing and documenting object-oriented systems.
  - **Class Diagram**: Defines the structure of the system by showing the system's classes, attributes, and relationships.
  - **Use Case (UC) and Activity Diagrams**: Capture functional requirements and the dynamic behavior of the system.
  - **Deployment and Package Diagrams**: Show physical deployment of artifacts on nodes and logical groupings of classes or components.
  - **C4 Diagrams**: A set of architecture diagrams representing different levels of detail (Context, Container, Component, and Code), which helps in visualizing the system's structure and its interactions.

## 3. Human2Machine (H2M)
### Executable Elements:
- **Code Repository (GIT)**: Centralized location for version-controlled code, allowing teams to collaborate efficiently.
- **Software**: Refers to the actual code and applications that are part of the architecture.
- **Infrastructure as Code (IaC)**: A methodology for managing and provisioning computing infrastructure through code rather than manual processes.
- **Configuration Files (YAML)**: YAML files are used for configuration, as they are easy to read and edit.
- **Context Maps**: Visual representations or files that define relationships and contexts within the system.
- **Secrets (Passwords, Encryption)**: Secure storage and management of sensitive information such as passwords and encryption keys.

## 4. IaC Tools and Practices
### Tools for Infrastructure Deployment:
- **Terraform, Ansible, Puppet, Chef**: Popular tools for managing infrastructure in code. These tools automate the deployment of servers, virtual machines, and other resources.
- **Virtual Machines**: Used for server-based deployments and are a staple in traditional cloud setups.

### Components:
- **Servers, Storage, Networking**: Core components of cloud infrastructure managed through IaC tools, enabling scalable and repeatable deployments.

## 5. Containers and Orchestration
### Infrastructure Elements:
- **RT (Runtime) + PL (Programming Language) + FW (Framework)**: Indicates the stack of runtime, language, and framework needed to support containerized applications.
- **Containers**: Lightweight, standalone, and executable software packages that include everything needed to run a piece of software.

### Technologies:
- **Kubernetes**: An orchestration platform for managing containerized applications in clusters.
- **Docker**: A containerization platform that packages applications into containers.
- **Rocket and Linkerd**: Alternative container runtimes and service meshes that enable secure, reliable, and observable communication within microservices.

### Container Management Tools:
- **Kubectl**: A command-line tool for interacting with Kubernetes clusters.
- **Kind**: A tool for running local Kubernetes clusters.
- **Helm**: A package manager for Kubernetes, used to define, install, and upgrade complex Kubernetes applications.

---

This diagram provides a holistic view of cloud-native architecture, highlighting the integration of communication, diagramming, infrastructure management, and container orchestration. It reflects the layered approach required for managing modern cloud environments and the tools used for efficient and secure deployments.


{{< figure src="/img/in-post/Cloud Native Solution Design/Traditional Web App.png" caption="<span class=\"figure-number\">Figure 3: </span>Traditional Web App" width="1000px" >}}

# Architecture of a Traditional Web Application

## Components

### Browser
- **Description**: Represents the client-side interface through which users interact with the web application.

### Firewall
- **Description**: Acts as a security barrier to protect the internal network, filtering incoming and outgoing traffic based on predefined security rules.

### AD or LDAP
- **AD (Active Directory)** and **LDAP (Lightweight Directory Access Protocol)** are used for:
  - Authentication
  - Managing user information within the application, especially in corporate environments.

### JNDI (Java Naming and Directory Interface)
- **Description**: A Java API for directory services that allows Java applications to discover and look up data and resources, such as databases or services.

### DNS (Domain Name System)
- **Description**: Translates domain names (e.g., www.example.com) into IP addresses, allowing users to access the application using easy-to-remember names rather than numeric addresses.

### Load Balancer
- **Description**: Distributes incoming traffic evenly across multiple servers, ensuring high availability and reliability by preventing any single server from becoming overwhelmed.

### Apache Web Server
- **Description**: Handles HTTP requests from clients, delivering web pages and static content. Apache is a widely used web server known for its flexibility and performance.

## Backend Architecture

### Web Servers
- **Function**: Handle incoming requests from clients (via the load balancer) and route them to the appropriate application server.
- **Purpose**: Serve static content directly or forward requests for dynamic content.

### App Servers (e.g., Tomcat)
- **Description**: Application servers, often using platforms like Apache Tomcat, process dynamic requests by running server-side code.
- **Function**: Handle business logic, interact with databases, and generate dynamic content for users.

### Distributed Cluster
- **Description**: Refers to a group of servers or nodes working together to provide a unified service.
- **Benefits**: Improves scalability, performance, and reliability by distributing tasks across multiple machines.

## Databases

### RDBMS (Relational Database Management Systems)
- **Function**: The primary database system storing structured data in tables with relationships between them.
- **Examples**:
  - **MSSQL**: Microsoft SQL Server, a popular enterprise database.
  - **PostgreSQL**: An open-source database known for advanced features.
  - **MySQL**: A widely-used open-source relational database known for speed and ease of use.

### Backup Database
- **Description**: A secondary database used to store backup data, ensuring data availability and resilience in case of failure in the primary database.

## Technology Stack (Products)

- **Apache**: Handles HTTP requests.
- **Tomcat**: A Java servlet container that serves Java-based applications.
- **.NET**: A framework by Microsoft, commonly used for building enterprise applications, especially on Windows servers.

---

## Summary
This architecture exemplifies a traditional, monolithic approach to web applications, where different components are often tightly coupled. It contrasts with modern cloud-native approaches, which prioritize microservices, containerization, and dynamic scaling. This setup, however, is effective for applications that require stable, predictable environments with consistent resources.

{{< figure src="/img/in-post/Cloud Native Solution Design/Cloud Enable.png" caption="<span class=\"figure-number\">Figure 4: </span>Cloud Native Solution Design/Cloud Enable" width="1000px" >}}
# Cloud-Enabled Architecture with "Lift, Shape & Shift" Approach

This architecture represents a Cloud-Enabled setup under a "Lift, Shape & Shift" approach, where existing applications are migrated to the cloud with minimal changes to leverage cloud infrastructure benefits.

## Components

### Browser
- **Description**: Acts as the client interface, sending requests to the cloud-enabled application.

### AWS R53 (Route 53)
- **Description**: Amazon Route 53, a scalable Domain Name System (DNS) service, directs traffic to AWS services based on domain names.

### IAM (Identity and Access Management)
- **Description**: Manages user permissions and controls access to AWS resources, ensuring that only authorized users can access specific services.

### AWS ELB (Elastic Load Balancer)
- **Description**: Distributes incoming traffic across multiple instances (e.g., virtual machines or containers) to ensure high availability and fault tolerance. It also aids in scaling by managing the distribution of traffic as demand increases.

### WS (Web Server)
- **Description**: Handles HTTP requests from clients. It can be hosted on a virtual machine (VM) instance, such as AWS EC2 (Elastic Compute Cloud), or within a container service.

### VM (AWS EC2)
- **Description**: AWS EC2 provides scalable virtual machines in the cloud, allowing applications to run with flexible computing power that can scale up or down based on demand.

### Containers (AWS ECS - Elastic Container Service)
- **Description**: AWS ECS enables running containerized applications in a managed environment, making it easy to deploy, manage, and scale containerized services.

### Replicas
- **Description**: Replicas provide redundancy by running multiple instances of the same service. This ensures high availability and load distribution across the replicas.

### Cluster
- **Description**: A grouping of VMs or containers to run distributed applications. In this context, it represents a cloud-hosted cluster that provides scalability and fault tolerance.

### CW (CloudWatch) and CF (CloudFront)
- **AWS CloudWatch**: Monitors AWS resources and applications, providing data on performance, errors, and usage.
- **AWS CloudFront**: A content delivery network (CDN) that accelerates content distribution by caching it at edge locations closer to users.

### AS (Auto Scaling)
- **Description**: Automatically adjusts the number of VM or container instances based on demand. Auto Scaling ensures that the application maintains performance during high traffic and reduces costs during low demand by scaling down.

### AWS RDS (Relational Database Service)
- **Description**: A managed database service that supports relational databases such as MySQL, PostgreSQL, and MSSQL. RDS simplifies database maintenance tasks like backups, patching, and scaling.

### Backup
- **Description**: Refers to data backup storage, ensuring that critical data is stored redundantly and can be recovered in case of failure.

## Additional Considerations

### Vendor Specific
- **Description**: This architecture leverages AWS-specific services, making it dependent on AWS as a cloud provider. This dependency can limit flexibility if a multi-cloud strategy is desired later.

### Migrate
- **Description**: "Lift, Shape & Shift" involves moving existing applications to the cloud without re-architecting the entire system. This migration enables cloud scalability and reliability without fully re-engineering the application.

### Hybrid / Multi-Cloud
- **Description**: Although this setup is AWS-centric, it can be extended to hybrid or multi-cloud environments where certain services are hosted on-premises or across multiple cloud providers.

---

This cloud-enabled architecture allows traditional applications to take advantage of cloud scalability, reliability, and managed services with minimal refactoring, making it a suitable approach for companies transitioning gradually to a cloud-native environment.

{{< figure src="/img/in-post/Cloud Native Solution Design/Cloud Native 1.png" caption="<span class=\"figure-number\">Figure 4: </span>Cloud Native 1" width="1000px" >}}
{{< figure src="/img/in-post/Cloud Native Solution Design/Cloud Native.png" caption="<span class=\"figure-number\">Figure 5: </span>Cloud Native 2" width="1000px" >}}

# Cloud-Native Architecture

This architecture represents a Cloud-Native setup built entirely in the cloud from scratch, leveraging containerization, service registries, and cloud-native tools. Below is a detailed breakdown of each component.

## Components

### Ingress (DNS)
- **Description**: Ingress manages and routes external traffic to services within the cluster.
- **Function**: Uses DNS to map domain names to IP addresses, ensuring accessible and well-defined endpoints.

### IAM (Identity and Access Management)
- **Description**: Provides fine-grained access control to resources in the cloud.
- **Function**: Manages user permissions and secures application access.

### Service Registry
- **Description**: Maintains a directory of available services and their endpoints.
- **Function**: Supports dynamic service discovery within the cluster, crucial for microservices architectures.

### Gateway (GW)
- **Description**: Serves as an entry point for client requests, handling authentication, routing, and load balancing.
- **Types**:
  - **API Gateway**: Manages and routes API calls.
  - **Service Gateway**: Handles inter-service communication.
  - **App Gateway**: Manages application-specific routing and access control.

### Endpoints
- **Description**: Defined interaction points for each service, allowing clients or other services to connect, typically within a cloud-native cluster.

## Cloud-Native Components

### Pod
- **Description**: The smallest deployable unit in Kubernetes, hosting one or more containers.
- **Function**: Provides network and storage configurations for the containers it contains.

### Container
- **Description**: A runtime instance within a Pod, built from Docker Images.
- **Function**: Packages applications and dependencies to run consistently across environments.

### Docker Image
- **Description**: A pre-configured snapshot used to create containers.
- **Function**: Essential for versioning and deploying applications, stored in a Container Registry.

### Node
- **Description**: A worker machine in Kubernetes where Pods are deployed.
- **Function**: Manages containerized applications and provides necessary runtime environments.

### Service
- **Description**: In Kubernetes, services abstract a logical set of Pods and define policies for accessing them.
- **Function**: Ensures reliable client connections to appropriate Pods, even as Pods are dynamically created or destroyed.

### Volume Claim
- **Description**: Persistent Volume Claims (PVCs) request storage for containers.
- **Function**: Allows data persistence beyond the lifespan of individual Pods.

## Core Cloud-Native Components

### LB (Load Balancer)
- **Description**: Distributes incoming traffic across multiple instances or Pods.
- **Function**: Balances load, enhances reliability, and prevents single points of failure.

### WS (Web Server)
- **Description**: Manages HTTP requests and responses, serving content to clients.
- **Function**: Often part of a microservice in a cloud-native architecture.

### AS (Application Server)
- **Description**: Handles business logic and processes user requests.
- **Function**: Often stateless to facilitate scaling in a cloud-native environment.

### RDBMS (Relational Database Management System)
- **Description**: A managed database system storing structured data.
- **Function**: Often used with distributed storage solutions in cloud-native architectures.

---

## Summary
This cloud-native architecture emphasizes containerization, microservices, and dynamic scaling. Each component is designed to work cohesively within a cloud-native environment, providing benefits such as scalability, resiliency, and efficient resource management. Unlike traditional architectures, this setup is built specifically for the cloud, allowing for flexibility, modularity, and seamless integration with cloud services.

# Cloud Enabled vs. Cloud Native

## 1. Cloud Enabled (云启用)

### 定义
Cloud Enabled 是指将传统应用迁移到云端，而无需对其进行大规模重构或重新设计。这种方法通常称为“Lift and Shift”（抬起并迁移），即直接将现有应用从本地数据中心迁移到云端。

### 特点
- **最小化改动**：通常不改变应用程序的代码或架构，只是利用云基础设施来提供计算、存储和网络资源。
- **使用云服务但不优化**：虽然应用在云中运行，但可能无法充分利用云原生功能，如自动扩展、容器化、无服务器计算等。
- **主要目标**：节省成本和提高基础设施的灵活性，而不改变应用程序的核心架构。

### 适用场景
- 适合已有的遗留系统，尤其是传统企业应用。这种方法允许企业在迁移到云端时最小化开发时间和成本。

### 示例：Lift and Shift 迁移到 AWS EC2
- **传统环境**：一个使用 Apache Web Server、Tomcat Application Server 和 MySQL 数据库的传统 Web 应用程序运行在专有的数据中心中。
- **迁移到云**：
  - 使用 AWS EC2 实例来运行 Apache 和 Tomcat。
  - 使用 AWS RDS 来管理 MySQL 数据库。
  - 使用 Elastic Load Balancer (ELB) 进行流量负载均衡。
- **改动**：代码不需要改变，应用程序只是迁移到了 AWS 的虚拟机上。利用了云的弹性计算资源，但未使用容器或微服务等现代云原生功能。

### 优缺点
- **优点**：
  - 迁移过程简单快速，通常需要的开发工作量较少。
  - 在云上运行时仍可以节省成本（例如，只按需付费）。
- **缺点**：
  - 无法充分利用云原生架构带来的灵活性和自动扩展能力。
  - 应用仍然可能依赖特定的硬件资源或旧的架构，不容易实现高效的弹性伸缩和自动化。

---

## 2. Cloud Native (云原生)

### 定义
Cloud Native 是专为云环境设计和构建的应用架构。云原生应用通常使用微服务架构，并部署在容器和编排系统（如 Kubernetes）上，能够利用云的优势，如自动扩展、故障隔离和持续交付。

### 特点
- **微服务架构**：应用被划分为多个小型服务，每个服务都可以独立部署和扩展。
- **容器化**：每个微服务通常都运行在容器中，容器提供独立的运行环境，有助于实现一致性和隔离。
- **自动化与编排**：使用容器编排工具（如 Kubernetes）管理容器的部署、扩展和容错。
- **弹性和可扩展性**：云原生应用可以根据需求自动调整资源，提高应用的可用性和性能。

### 适用场景
- 适合现代云环境中构建的新应用，尤其是高可用性和快速迭代的需求场景，如电商、社交媒体和金融科技应用。

### 示例：基于 Kubernetes 的电商应用
- **需求**：一个高可用性、快速扩展的电商平台，需要独立部署不同的功能模块（如用户服务、商品服务、订单服务等）。
- **架构**：
  - **微服务**：将用户服务、商品服务、订单服务等模块划分为独立的微服务。
  - **容器化**：每个微服务打包成 Docker 容器，确保开发、测试和生产环境的一致性。
  - **Kubernetes**：使用 Kubernetes 管理微服务的部署、扩展和更新。
  - **自动扩展**：Kubernetes 根据用户流量的变化自动增加或减少 Pod 的数量。
  - **服务发现和负载均衡**：使用 Kubernetes 的内部 DNS 进行服务发现，并在服务之间进行负载均衡。
- **改动**：需要从头设计和开发应用代码，以适应微服务和容器化的需求。

### 优缺点
- **优点**：
  - 能充分利用云的弹性和自动化优势，实现自动扩展、容错和高可用性。
  - 可以更快速地进行应用的迭代和部署，适应不断变化的业务需求。
- **缺点**：
  - 开发和维护的复杂度较高，需要投入更多资源来设计和管理云原生架构。
  - 学习成本较高，团队需要具备 DevOps、容器编排等技能。

---

## 总结对比

| 特点         | Cloud Enabled                 | Cloud Native                    |
|--------------|-------------------------------|---------------------------------|
| **架构风格** | 单体应用或传统架构            | 微服务架构                      |
| **部署方式** | 虚拟机或简单的云服务（如 EC2）| 容器化，通常使用 Kubernetes    |
| **开发成本** | 低（直接迁移）                | 高（重新设计和开发）            |
| **自动化**   | 较少（主要依赖基础自动化）    | 高（自动扩展、持续交付等）      |
| **弹性**     | 有限，需手动调整 VM 或配置     | 高度弹性，自动扩展               |
| **适用场景** | 现有应用迁移，节约成本        | 新应用，高可用性、快速迭代      |

---

## 总结
- **Cloud Enabled** 更适合已有的遗留系统，这些系统通常是单体应用，直接迁移到云端后可以利用云的成本和基础设施优势。
- **Cloud Native** 则是为现代云环境量身打造的，利用微服务、容器化和编排工具的优势，实现高效的自动化和弹性，适用于快速变化的业务需求。

# Cloud Computing Overview: Open-Book Exam Quick Reference

## 1. Cloud Computing Definition & Core Concepts

- **Definition**: Cloud computing provides on-demand access to a shared pool of computing resources (networks, servers, storage, applications, and services) that can be provisioned with minimal management.
- **Philosophy**:
  - Rent resources instead of owning them.
  - Scale resources up or down based on need.
- **Essential Characteristics (NIST)**:
  - **On-demand self-service**: Users can provision resources as needed.
  - **Broad network access**: Accessible from anywhere via the internet.
  - **Resource pooling**: Shared resources among multiple consumers.
  - **Rapid elasticity**: Resources can scale quickly based on demand.
  - **Measured service**: Pay for what you use (metered usage).

## 2. Reasons for Adopting Cloud Computing

- **Business Agility**: Quick response to new opportunities, faster time to market.
- **Cost Savings**: Lower capital and operational expenses; pay-as-you-go.
- **Efficiency**: Improved resource utilization and scalability.
- **Innovation**: Access to cutting-edge technology without large upfront investment.

## 3. Cloud Service Models

- **Infrastructure as a Service (IaaS)**:
    - **Definition**: IaaS allows users to rent computing resources instead of purchasing them outright, with services typically billed on a usage basis.
    - **Core Concept**: This model essentially provides bare metal hardware on the cloud.

### How it Operates

- **Application Hosting & Data Storage**:
  - Applications run and data is stored on the cloud provider's infrastructure.
  
- **Usage**:
  - Users utilize the cloud infrastructure for compute processing, storage, networking, and other IaaS services based on their needs.
  - Enables deployment and running of software applications, middleware, tools, data storage, and more.

- **User Control**:
  - Users do not manage or control the underlying cloud infrastructure.
  - Users have control over the operating systems on their compute instances (cloud-based computers), storage configurations, and network setup.

- **Platform as a Service (PaaS)**:

    - **Definition**: PaaS provides a solution stack over cloud infrastructure, enabling software development and deployment entirely in the cloud.

### How it Operates

- **Software Development & Deployment**:
  - Software developers can develop and run applications on the cloud provider’s platform.
  
- **Usage**:
  - Users can use self-built or acquired applications created using the programming languages and tools supported by the cloud provider.

- **User Control**:
  - Users have control over the deployed applications.
  - Users do not manage or control the underlying cloud infrastructure (e.g., servers, operating systems, or storage).

- **Software as a Service (SaaS)**:
  - Delivers software applications over the internet (e.g., Salesforce, Google Workspace).
  - Users access applications without managing infrastructure or application resources.

{{< figure src="/img/in-post/Cloud Native Solution Design/Cloud Service Models.png" caption="<span class=\"figure-number\">Figure 6: </span>Cloud Service Models" width="1000px" >}}

{{< figure src="/img/in-post/Cloud Native Solution Design/Cloud Computing Service Model.png" caption="<span class=\"figure-number\">Figure 7: </span>Cloud Computing Service Model" width="1000px" >}}

{{< figure src="/img/in-post/Cloud Native Solution Design/cloud native service model.png" caption="<span class=\"figure-number\">Figure 8: </span>cloud native service model" width="1000px" >}}
## 4. Cloud Deployment Models

- **Public Cloud**:
  - Available to any organization or individual.
  - Infrastructure is owned and managed by the cloud provider.
- **Private Cloud**:
  - Operated solely for a single organization.
  - Can be on-premises or hosted off-site; managed by the organization or third party.
- **Hybrid Cloud**:
  - Combines public and private clouds for flexibility.
  - Allows data and applications to be shared across both environments.
- **Multi-Cloud**:
  - Uses multiple public cloud services from different providers.
  - Helps avoid vendor lock-in and improves reliability.
- **Virtual Private Cloud (VPC)**:
  - A private cloud within a public cloud with isolated resources.
  - Offers scalability and security similar to private cloud.
{{< figure src="/img/in-post/Cloud Native Solution Design/VPC.png" caption="<span class=\"figure-number\">Figure 9: </span>VPC" width="1000px" >}}
- **Proprietary Cloud**:
  - A custom-built cloud solution that includes hardware and software.
  - Used for on-premise deployment by large enterprises.

## 5. Benefits of Cloud Computing

- **Economic**:
  - Low entry and operational costs.
  - Converts CapEx to OpEx, with pay-as-you-go pricing.
- **Operational**:
  - Improved flexibility and mobility.
  - Faster deployment, prototyping, and scalability.
  - Access to automatic updates and security patches.
  - **Community Support**: Availability of shared knowledge and community resources.

## 6. Barriers to Cloud Adoption

- **Regulatory Compliance**: Industry regulations may restrict data storage.
- **Security Concerns**: Data breaches and unauthorized access.
- **Governance and Privacy**: Control over data and compliance with privacy laws.
- **Migration Complexity**: Integrating with legacy systems and data transfer challenges.

## 7. Cloud Industry Developments

- **Cloud-First Strategy**: Startups and tech-driven businesses adopt a cloud-first approach for innovation.
- **Mainstream Adoption**: Enterprises like DBS Bank in Singapore actively migrate to the cloud to drive digital transformation.
- **Gartner's Hype Cycle & Magic Quadrant**:
  - **Magic Quadrant for Cloud IaaS** shows leaders like AWS and Microsoft Azure.
  - **Gartner's Hype Cycle** illustrates trends and maturity of cloud technologies.

## 8. Major Cloud Providers & Selected Services

- **Amazon Web Services (AWS)**: Offers IaaS, PaaS, and SaaS with services like EC2, RDS, S3, and Lambda.
- **Microsoft Azure**: Provides virtual machines, app services, and a comprehensive suite for AI and machine learning.
- **Google Cloud Platform (GCP)**: Includes services like Compute Engine, App Engine, BigQuery, and Cloud AI tools.
- **IBM Bluemix**: Focuses on enterprise solutions with AI, IoT, and blockchain capabilities.

## 9. Application Development on Cloud

- **Cloud-Native Development**:
  - Uses microservices, containers (e.g., Kubernetes, Docker), and DevOps practices.
- **Hybrid Development**:
  - Combines cloud-based and on-premises resources for flexibility.
- **Serverless Computing**:
  - Executes code on-demand without provisioning or managing servers (e.g., AWS Lambda).

## 10. Common Cloud Myths (Gartner)

- Cloud is not always cheaper; cost savings depend on usage.
- Cloud does not fit every application type.
- A multi-cloud approach can help avoid reliance on a single provider.
- Cloud is secure when configured and managed properly.
- Migration to the cloud does not automatically enhance performance or efficiency.

