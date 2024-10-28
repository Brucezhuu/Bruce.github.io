+++
title = "02: Architect the solution architecture (Asset)"
author = ["Bruce"]
date = 2024-10-04
series = ["Architecting software solution"]
tags = ["Architecting software solution", "SWE5001"]
draft = false
+++

## 1. Objectives of the Lecture

- Reusable Architectural Assets: Understand how to use architectural assets to improve design efficiency by reusing proven structures and components.
- Architecture Styles and Components: Learn about different architectural styles and the components within them.
- Integration of Components: Explore methods to integrate different parts of the system.
- Benefit from Assets: Understand how to leverage reusable assets in defining a robust solution architecture.

## 2. Key Topics to Focus On

- Metamodel of Architectural Assets: This defines how different architectural styles, reference architectures, and descriptions of these architectures come together to form a coherent model. It’s essentially the “big picture” that ties together different design elements (slide 5).
- Architectural Styles: Learn about different architectural styles that guide the design of systems:
- Client-Server Architecture: Common in web and app development, it separates client interactions from backend services (slide 9).
- Tiered Architecture: Multi-layer designs like the 3-tier or n-tier architecture where each layer is responsible for specific services, such as presentation, business logic, and data layers.
- Network Components: Explore the role of network components like firewalls, proxies, and server zones, often used in enterprise solutions (slide 11).
- Server Components: Learn the roles of application servers, portal servers, and data persistence mechanisms (SQL and NoSQL databases), and how to integrate them (slides 14-15).
- CAP Theorem: This theorem explains the trade-off between consistency, availability, and partition tolerance in distributed systems (slide 16).
- ACID vs. BASE Models: Important for understanding transactional integrity (ACID) versus more flexible consistency models (BASE) (slide 17).

## 3. Architecture Styles in Detail
### Net work components:
- Demilitarized Zone (DMZ):

	- Definition: A DMZ is a zone in a network that acts as a buffer between a purely internal network (like an intranet) and an external network (like the internet). 即将部分用于提供对外服务的服务器主机划分到一个特定的子网——DMZ内，在DMZ的主机能与同处DMZ内的主机和外部网络的主机通信，而同内部网络主机的通信会被受到限制。这使DMZ的主机能被内部网络和外部网络所访问，而内部网络又能避免外部网络所得知。
	- Purpose: It helps to protect the internal network by controlling access. External users, such as customers accessing a public website, are routed through the DMZ to interact with servers located there, while the internal network remains shielded behind additional firewalls.
- Reverse Proxy:

	- Definition: A reverse proxy is an intermediate server that sits between internet users and internal servers.
	- Purpose: It is used to forward requests from external users to internal servers. One key function is hiding the identity of internal servers, providing an additional layer of security. For example, when users access a website, they are actually communicating with the reverse proxy, which in turn communicates with the internal servers, shielding the internal network.

- Forward Proxy:

	- Definition: A forward proxy acts on behalf of internal clients to access external servers, such as websites on the internet.
	- Purpose: It allows internal clients (users on the intranet) to access the internet even if they are otherwise restricted by a firewall. The forward proxy hides the identity of these internal clients from the external systems they are accessing.
- Summary:

	- DMZ acts as a protective buffer between the internal and external networks.
	- Reverse Proxy hides internal servers from external users, offering an extra security layer.
	- Forward Proxy hides internal clients when they access external networks, allowing restricted users to access external resources while maintaining internal security.
- Example of Use:

	1.	DMZ: In this diagram, internet users first encounter an outer firewall that directs them to a web server (located in the DMZ), which is a reverse proxy. The internal systems, such as the application server, database server, and internal users, are behind an inner firewall in the internal zone.
	2.	Reverse Proxy: When external users make requests, such as visiting a website, their requests are handled by the reverse proxy (web server in the DMZ) before being forwarded to the application server and database server behind the inner firewall.
	3.	Forward Proxy: If internal users (from the intranet zone) want to access the internet, they may pass through a forward proxy that mediates their connection and shields their identity from the external websites they visit.
### server components:
- Service Execution (e.g., Application Servers)

	- Definition: An application server is a server dedicated to running applications. It acts as a middle layer between the front-end user interface and the back-end databases, processing business logic and managing data transactions.
	- Function:
	    - Executes the core functionality of applications.
	    - Provides services like load balancing, database connections, and security handling.
	    - Hosts applications, typically in a multi-tier architecture, allowing users to interact with the system via client interfaces (web, mobile, etc.).
    - Example: In a web application, the application server might process login requests, retrieve data from the database, and return results to the user.

- B2C Integration (e.g., Portal Servers)

	- Definition: A portal server provides a centralized point of access for users to interact with various services or applications.
	- Function:
	- Facilitates business-to-consumer (B2C) integration by providing a unified interface for customers to access different services.
	- Can offer personalized services, customer support, and other functionalities through a web portal.
    - Example: An online banking portal where customers log in to view their account balances, make transfers, and manage their profiles.
- Service Management and Governance (e.g., Mobile Device Management (MDM), Application Management (MAM), Service Registry)
- Data Persistence (e.g., SQL, NoSQL Databases)

### CAP Theory
The CAP theorem (also known as Brewer’s theorem) states that in a distributed system, it is impossible to simultaneously achieve all three of the following properties:
- 	1. Consistency (C):
	- Definition: Every read request will return the most recent write. In other words, all nodes in the system have the same, up-to-date data.
	- Explanation: If a system is consistent, it ensures that whenever data is written or updated, all subsequent reads will reflect that change, regardless of which node is being queried.
	- Example: In a banking system, if you deposit money at one branch, that update is immediately reflected across all branches, ensuring the balance is always accurate.
- 2. Availability (A):
	- Definition: The system is **always available** to respond to requests, even if some components are down. Every request (read/write) will receive a response.
	- Explanation: In a highly available system, the system ensures that even in the face of failures or network issues, it will continue to respond to requests. However, this may come at the cost of consistency.
	- Example: In an e-commerce website, users should always be able to browse products and place orders, even if some database nodes are temporarily unavailable.
- 3. Partition Tolerance (P):
	- Definition: The system can continue to operate even if communication between nodes is interrupted (i.e., network partitions occur).
	- Explanation: In distributed systems, network partitions are inevitable (e.g., network failures, latency). A partition-tolerant system can continue to process requests even if some nodes cannot communicate with others.
	- Example: In a globally distributed content delivery network (CDN), if a network partition isolates some data centers, the system can still serve content from the available data centers.

#### Applications of the CAP Theorem:

1. Financial Systems (CP): Systems that require strong consistency, like banking applications, may prefer Consistency and Partition Tolerance over availability. This ensures that the data is always accurate, even if it means temporarily rejecting requests during network issues.
2. E-commerce and Social Media (AP): These systems prioritize Availability and Partition Tolerance to handle large numbers of requests and ensure high uptime, even at the cost of temporary inconsistencies. For example, an e-commerce website may allow users to continue browsing and placing orders even if some updates (like stock levels) are not immediately reflected.

### ACID vs BASE
ACID and BASE are two different models used to ensure data integrity, consistency, and performance in database systems, especially in the context of transactions. They represent two distinct philosophies for handling data management in distributed or non-distributed systems.

#### ACID (used primarily in traditional relational databases)

The ACID model is designed to ensure reliable and consistent transactions in a database system, particularly in relational databases (e.g., MySQL, PostgreSQL). ACID stands for:

1.	Atomicity:
	- Definition: A transaction is treated as a single “atomic” unit, meaning all operations within the transaction must either complete entirely or not at all.
	- Example: In a banking system, if you transfer money from Account A to Account B, both the debit from Account A and the credit to Account B must succeed. If one part fails (e.g., debit succeeds, but credit fails), the entire transaction is rolled back.
2. Consistency:
	- Definition: A transaction must take the database from one valid state to another, ensuring that all predefined rules (like constraints, triggers, etc.) are met.
	- Example: If you transfer money between accounts, the total balance across all accounts must remain consistent. The system ensures that constraints, such as no negative balances, are respected.
3. Isolation:
	- Definition: Transactions should not interfere with each other. Each transaction should appear to be executed in isolation, meaning intermediate states within one transaction are not visible to other transactions.
	- Example: If two users are transferring money simultaneously, one user’s transaction should not affect the outcome of the other, ensuring no incorrect balances are seen during the process.
4. Durability:
	- Definition: Once a transaction has been committed, it will remain in the system, even in the event of a crash, power failure, or other issues. Data is safely written to disk.
	- Example: Once your money transfer is confirmed, the system guarantees that the transfer will not be lost, even if the database server crashes immediately afterward.

ACID is used where strong consistency and reliability are required, such as in financial systems, inventory management, and mission-critical applications where data accuracy is paramount.

#### BASE (used primarily in NoSQL and distributed databases)

The BASE model is more flexible and designed to handle high availability and scalability, often at the cost of immediate consistency. It is commonly used in NoSQL databases (e.g., Cassandra, DynamoDB) and distributed systems. BASE stands for:

1. Basically Available:
	- Definition: The system guarantees basic availability, meaning that the system will always respond to requests, but the response might not contain the most recent or accurate data.
	- Example: In a social media platform, you might see slightly outdated posts or comments in your feed, but the system will remain responsive and available to users, even under heavy load.
2. Soft State:
	- Definition: The system allows for an intermediate state where data may change over time without an immediate guarantee of consistency. Data doesn’t have to be consistent across all nodes at all times.
	- Example: In a distributed system, updates might not be immediately propagated across all nodes, meaning one server might have outdated information, but over time, all nodes will synchronize.
3.	Eventual Consistency:
	•	Definition: The system guarantees that, eventually, all data will become consistent, but not immediately. After some period of time (usually seconds or minutes), all replicas will converge to the same value.
	•	Example: In an e-commerce website, if you update your shopping cart, some nodes might show an older version of the cart for a few seconds. However, after a short delay, all nodes will reflect the updated cart.

BASE is often used in high-availability, large-scale systems like social networks, e-commerce platforms, and distributed systems where the trade-off between consistency and availability is acceptable. It allows for higher scalability and resilience to failures, especially in environments with massive amounts of data.

#### How to Choose Between ACID and BASE:

- Choose ACID if:
	- You need strong consistency and reliable transactions.
	- Data accuracy is critical, such as in banking, inventory management, financial transactions, etc.
	- The system cannot afford inconsistencies at any point in time.
- Choose BASE if:
	- You need high availability and scalability over strict consistency.
	- You can tolerate eventual consistency, where small delays in data consistency won’t negatively impact user experience.
	- You are dealing with large distributed systems, like social media, e-commerce platforms, or IoT systems, where scalability and performance are more important than immediate consistency.

#### Conclusion:

- ACID is all about ensuring consistency and reliability in database transactions, ideal for situations where the integrity and correctness of data are paramount.
- BASE, on the other hand, focuses on scalability and availability, tolerating temporary inconsistencies in exchange for better performance and resilience in large distributed systems.

### peer to peer architecture style (structural):
Peer-to-peer (P2P) architecture is a decentralized network model where each participating device (peer) can act as both a client and a server.

#### Key Characteristics of Peer-to-Peer Architecture:

1. Each Peer Acts as Both Client and Server:
	- In P2P networks, there is no strict distinction between a client and a server. Every peer can both consume and provide resources. This contrasts with traditional client-server models where dedicated servers provide services, and clients consume them.
	- Example: In a file-sharing network like BitTorrent, each peer (user) can download files (acting as a client) and also upload parts of the files to others (acting as a server).
2. Direct Communication Between Peers:
	- Peers can communicate directly with one another without a central server. This reduces the need for intermediaries and allows for more flexible and scalable networks.
	- Example: In systems like Bitcoin or decentralized online games, participants can interact directly with each other for transactions or gameplay without a centralized authority coordinating every action.
3. Examples of P2P Systems:
	- Napster: One of the first P2P file-sharing networks where users could share and download music files directly from one another.
	- BitTorrent: A widely-used P2P file-sharing protocol where users share parts of a file with each other, distributing bandwidth usage across the network.
	- Bitcoin: A cryptocurrency that operates on a P2P network where transactions are verified by nodes (peers) without the need for a central authority like a bank.
	- Massively Multiplayer Online Games (MMOG): In certain MMOGs, players communicate directly with each other to coordinate in-game actions without always relying on a central game server.

#### Advantages of Peer-to-Peer Architecture:

1. Scalability:
	- Since each peer provides resources (like bandwidth and storage), the network can scale as more users join. The load is distributed across all peers rather than centralized on a single server.
2. Fault Tolerance:
	- The network is more resilient because there is no single point of failure. If one peer goes down, the system can still function as long as other peers are operational.
3. Decentralization:
	- P2P systems are decentralized, meaning that no central authority controls the network. This can lead to increased privacy, autonomy, and resistance to censorship or shutdowns.

#### Challenges of Peer-to-Peer Architecture:

1. Consistency:
	- Strict consistency can be a concern in P2P networks. Since data is distributed across many peers, it’s challenging to ensure that all peers have the latest, consistent version of the data at any given time.
	- Example: In a distributed ledger like Bitcoin, ensuring that all nodes agree on the same version of the blockchain can be complex.
2. Security:
	- Authentication and authorization can be tricky in a P2P network because there is no central server to enforce security rules. Each peer must be responsible for its own security.
	- Example: In file-sharing systems like BitTorrent, there is a risk of downloading malicious content because there is no central authority to verify the integrity of files.
3. Management:
    - Without a central authority, managing updates, security patches, and ensuring proper behavior across all peers can be more complicated.
### Discussion 1:

{{< figure src="/img/in-post/Architecting software solution/swe5001_1_02_Discussion1.png" caption="<span class=\"figure-number\">Figure 1: </span>Dissction 1" width="500px" >}}

Option 2 is better:

1. Increased Security with a Reverse Proxy:

- Reverse Proxy: In Option 2, a web reverse proxy server is added in the DMZ. This is a crucial security component because the reverse proxy sits between external clients (like the browser thin client) and the internal servers, filtering traffic and ensuring that only legitimate requests reach the internal IIS application server.
- This adds an additional layer of defense, preventing direct access from external clients to the internal application server, which reduces the exposure of internal systems to potential attacks.
- The reverse proxy also hides the identity of the internal servers, ensuring that external users cannot directly access or target them.

2. Multi-Layered Firewall Setup:

- In Option 2, the system includes Inner Firewall 2, which further isolates the internal network (intranet) from the internal zone. This adds a second layer of protection for Active Directory, MSSQL Database Server, and Internal Users. Even if an attacker breaches the first inner firewall, there is still another layer of protection for critical services.
- This separation of zones ensures that sensitive services, such as the database and Active Directory, are protected behind multiple layers of defense, making the system more robust and secure.

3. Defense in Depth:

- Option 2 adopts a more comprehensive defense-in-depth strategy, where multiple layers of security (outer firewall, reverse proxy, two inner firewalls) work together to protect the system. This reduces the risk of a single point of failure and makes it harder for attackers to penetrate the entire system.

4. Better Traffic Management:

- The reverse proxy in Option 2 can also handle load balancing, distribute traffic to different servers, and optimize network traffic, improving overall performance and resource utilization. It can also provide caching, SSL termination, and additional security features.

5. Summary:

Option 2 provides:

- Enhanced security by using a reverse proxy to protect internal servers from direct access and adding multiple firewalls for layered protection.
- Improved traffic management by using the reverse proxy for optimizing request handling.
- Better separation of concerns, which leads to stronger system protection and resilience against attacks.




### Data Flow Architecture (communication): 
Data flows through the system sequentially. Suitable for batch processing, this style is used in ETL (Extract, Transform, Load) scenarios (slide 22).
1.	Extract: Data is extracted from various sources like databases, APIs, or flat files.
2.	Transform: Data is cleaned, normalized, and formatted according to business rules. This can involve tasks like removing duplicates, converting data types, or summarizing information.
3.	Load: The transformed data is then loaded into a data warehouse or database for analysis.

Each step in the ETL process represents a component in the data flow, and the data flows through these components sequentially or in parallel until the desired result is achieved.

### Call and Return (communication): 
A simple interaction where one component invokes another and waits for a response. Common in layered system architectures (slides 27-29).
#### RESTful webservices (Representational State Transfer)
- Key Characteristics:

	1.	Resource-Based:
		- REST services are built around resources, and each resource is identified by a URL. For example, in a REST API, an HTTP GET request to /customers/123 might retrieve the details of a customer with ID 123.
	2.	Stateless:
		- REST is stateless, meaning each request from a client to a server must contain all the information needed to process that request. The server does not store any session information between requests.
	3.	Flexible Data Formats:
		- REST typically uses JSON for data exchange, though it can also use XML, HTML, or other formats. JSON is lighter and easier to parse than XML, which makes REST services faster in many cases.
	4.	Uses HTTP Methods:
		- REST heavily relies on standard HTTP methods like:
		- GET: Retrieve data.
		- POST: Submit new data.
		- PUT: Update existing data.
		- DELETE: Remove data.
	5.	Stateless and Scalable:
		- REST services are easier to scale because they do not require the server to maintain the state of each user session, making them ideal for web applications with many users.

- Use Cases:

	- Web services and APIs used by web and mobile applications (e.g., social media platforms, cloud services).
	- Microservices architectures that need to scale rapidly.
	- Lightweight applications with simple data interactions.


### Event-Based Architectures (communication):
- Point-to-Point Event Handling: Used for real-time applications where messages are sent asynchronously from producer to consumer.
- Publish/Subscribe Model: The producer publishes events, and multiple subscribers consume them based on their interest (slides 32-33).
#### Discussion2:

{{< figure src="/img/in-post/Architecting software solution/swe5001_1_02_Discussion2.png" caption="<span class=\"figure-number\">Figure 2: </span>Dissction 2" width="500px" >}}
1.	Required Quality Attributes:
	- Usability: The system should be easy for the general public and tourists to access and understand, especially with extensive visual graphics and illustrations.
	- Performance: The system must deliver data quickly and efficiently, especially if real-time haze data is involved, ensuring timely information access.
	- Scalability: The system must handle potentially large numbers of users, especially during haze events when public interest is high.
	- Availability: The system should be highly available and reliable, as the public depends on timely access to this critical information.
	- Security: Ensure the system is secure from unauthorized access, particularly to prevent tampering with haze data or sensitive information.
	- Interoperability: The system should be able to integrate and work smoothly with existing haze sensors and databases that store the haze data.
2.	Appropriate Communication Style(s):
	- RESTful Web Services: Given the public access and likely need for scalability and flexibility, RESTful web services are appropriate. REST is stateless and easily scalable, making it a good fit for public-facing web services delivering data in formats like JSON.
	- WebSocket (for real-time data): If the system requires real-time haze data updates, WebSocket could be used to provide live updates to users with minimal latency.
	- API-based integration: If other systems (like mobile apps or external agencies) need to consume the data, providing an API would allow smooth integration, likely using REST as the API style for ease of use and broad compatibility.
3. Recommended Communication Flow:

	- Client (browser/mobile app) → HTTP/HTTPS REST API for haze data retrieval.
	- Server hosts and provides RESTful services and real-time data updates using WebSockets or Pub-Sub architecture.
	- Data visualization can be rendered on the client side using a combination of static assets delivered by CDN (content Delivery Network)and dynamic data from the API.

4. Summary:

	- RESTful Web Services for most interactions due to their scalability and ease of integration.
	- WebSockets for real-time data streaming from haze sensors.
	- Pub-Sub for event-driven notifications about important haze updates.
	- CDN for delivering static content like graphics and visualizations.
## 4. Reference Architectures

- Web Architecture: Traditional and modern client-centric web architectures are compared (slide 41).

- Mobile Architecture: Discusses how to build backend services for mobile apps, including AWS-hosted architectures (slide 44).

- Broker Architecture: Introduces ESB (Enterprise Service Bus) and the role of brokers in mediating interactions between components (slide 50).

- SOA and Microservices: Service-Oriented Architectures (SOA) and microservices, highlighting the decoupling of functionality into independent services (slides 52-58).
SOA（Service-Oriented Architecture）是一种面向服务的软件架构风格，它是一种基于服务的软件设计和开发方法，将应用程序组织为一组松散耦合的、可重用的、自治的服务，这些服务通过标准化的接口进行通信，以实现各种业务流程和功能。

- Cloud and Serverless Architectures: Explore cloud models (IaaS, PaaS, SaaS) and serverless computing, focusing on how services are provided without dedicated servers (slides 61-67).
### Discussion 3:
{{< figure src="/img/in-post/Architecting software solution/swe5001_1_02_Discussion3.png" caption="<span class=\"figure-number\">Figure 3: </span>Dissction 3" width="500px" >}}

1. Mobile Architecture: Key Decision Drivers for Mobile Application Development

When determining the architecture for mobile application development, the following factors should be considered:

- Platform Choice:
	- Native vs. Cross-Platform: Deciding whether to develop natively (iOS and Android separately) or use cross-platform frameworks like Flutter or React Native.
	- Device Compatibility: Ensuring the app works across different devices with varying screen sizes, hardware capabilities, and OS versions.
	- User Experience (UX) and Performance:
	- Responsive Design: The architecture should support a seamless user experience, offering smooth navigation, fast response times, and intuitive design.
	- Offline Capabilities: The need for offline functionality should influence decisions about local storage and data synchronization.
	- Security:
		- Data Security: Ensure data encryption for sensitive information (e.g., using SSL/TLS).
		- Authentication: Implement secure login methods, such as OAuth, fingerprint, or facial recognition.
	- Scalability:
		- As user demand grows, the app should be able to scale without performance degradation. Backend services (e.g., cloud-based services) should be designed to handle increasing load.
		- Integration with Backend Services:
		- API Design: The mobile app architecture must include well-structured APIs for fetching data from the backend, handling authentication, and more.
		- Cloud Services: Integration with cloud services for data storage, user management, or even serverless features can reduce infrastructure complexity.
	- Maintenance and Updates:
		- Consider the ease of deploying updates (bug fixes, new features) to users. A good architecture allows for smooth and timely updates with minimal disruption to users.
	- Battery and Performance Efficiency:
		- The architecture should optimize for resource usage, minimizing battery consumption, and handling background processes efficiently.
	- Push Notifications:
		- Integrating push notifications is often a key requirement, especially for real-time interactions with users.

2. SOA / Microservices Architecture / Serverless Architecture: Key Factors and Considerations

For an enterprise adopting SOA, Microservices, or Serverless Architecture, the following factors must be considered:

- Scalability and Flexibility:
	- Microservices and serverless architectures offer independent scaling of components, making it easier to scale parts of the application as needed without affecting other services.
	- SOA enables scalability at the service level but may have more overhead than microservices.
- Decoupling and Reusability:
	- Microservices and SOA allow for modular, loosely coupled services that can be developed, deployed, and maintained independently. This also enables reusability of services across different parts of the organization.
- Communication Overhead:
	- In microservices and SOA, services often communicate over the network (via REST, gRPC, or messaging systems). The latency and network reliability must be taken into account.
	- With serverless, functions typically have even shorter lifespans, so their design should focus on minimizing unnecessary communication between components.
- Cost Considerations:
	- Serverless architecture has a pay-per-execution pricing model, which may lead to cost savings for applications with irregular or unpredictable traffic. However, for constant high traffic, this may be more expensive compared to microservices.
	- SOA and microservices typically involve higher upfront infrastructure costs but can be cost-efficient at scale.
- Deployment and Management Complexity:
	- Microservices introduce complexity in deployment, monitoring, and debugging due to the distributed nature of services. Proper orchestration tools (e.g., Kubernetes) and monitoring solutions are necessary.
	- SOA typically follows a more structured deployment model with central management, while serverless offers simplified deployment by abstracting the infrastructure layer.
- Security and Governance:
	- Microservices and SOA require strong security measures, especially for inter-service communication, ensuring authentication and authorization (e.g., OAuth, JWT).
	- Serverless requires careful management of security roles and permissions, ensuring least privilege access across Lambda functions and API Gateway resources.
- Data Consistency:
	- Both SOA and microservices must address data consistency challenges, particularly in distributed systems. Techniques such as eventual consistency or saga patterns are used.
	- In serverless architectures, data persistence must be handled carefully between stateless functions, often using a mix of databases like DynamoDB or relational databases.

3. Cloud Architecture: Considerations for Cloud Deployment

When evaluating cloud adoption, the following considerations are important:



- Types of Applications Suitable for Cloud:
	- Web Applications: Websites or web services that need to scale according to demand. Cloud services can handle traffic spikes efficiently.
	- Data-Intensive Applications: Applications that require massive storage and data processing (e.g., big data analytics, machine learning). Cloud services can provide scalable storage and computing power.
	- DevOps/CI-CD Pipelines: Applications that require continuous integration and deployment workflows can benefit from cloud automation tools like AWS CodePipeline, Azure DevOps, or GitHub Actions.
	Big Data Analytics:
	- Applications that process large datasets (e.g., data warehousing, data lakes, and machine learning models) are ideal for cloud deployment due to the elasticity and high-performance compute resources available in the cloud.
	- Customer Relationship Management (CRM) and Enterprise Resource Planning (ERP):
	Applications like CRMs and ERPs can leverage the cloud’s scalability, particularly for businesses that need to manage and analyze extensive customer or enterprise data. SaaS versions of CRMs and ERPs are widely available and optimized for cloud use.
	- Disaster Recovery and Backup Solutions:Cloud services offer robust backup and disaster recovery solutions, providing cost-effective, reliable, and highly available options for data replication and recovery in different geographic regions.
	- Mobile and Collaboration Apps: Applications that require frequent updates, remote accessibility, and multi-user collaboration (e.g., file sharing, team collaboration tools) are highly suitable for cloud deployment.
	- Content Distribution and Media Streaming: Applications that need global distribution, such as media streaming and content delivery networks (CDNs), benefit from cloud’s global presence and CDN services for low-latency delivery.

- Types of Applications Not Suitable for Cloud:
	- Highly Latency-Sensitive Applications: Applications that require real-time processing (like high-frequency trading platforms) may not perform well in a cloud environment due to network latency.
	- Regulated or Legacy Systems: Systems that deal with highly regulated data (such as healthcare or financial data) may face legal or compliance issues when moved to the cloud, especially if data needs to be stored on-premise for compliance reasons.
	- Legacy Systems: Some older systems or applications that weren’t designed for distributed environments may not benefit from being moved to the cloud.
- Key Considerations for Cloud Adoption:
	- Cost Optimization: Cloud pricing models are complex, so it’s important to consider how much you will spend on compute, storage, and networking over time.
	- Security and Compliance: Moving to the cloud requires a new approach to security (e.g., encryption, identity management) and adherence to compliance requirements (e.g., GDPR, HIPAA).
	- Scalability and Elasticity: One of the key advantages of the cloud is auto-scaling. Ensure the application architecture can leverage the elastic nature of the cloud to scale based on demand.
	- Disaster Recovery: The cloud provides easy-to-implement disaster recovery and backup solutions. Consider how cloud platforms can enhance business continuity.
	- Vendor Lock-In: Ensure that the cloud services you choose do not create dependencies that prevent you from moving to another cloud provider or back to on-premise infrastructure if needed.
	- Network and Latency: Assess whether the cloud provider’s network performance is sufficient for your application needs, especially if your users are globally distributed.
	- Applications with Predictable, Fixed Workloads:Applications with stable and predictable resource demands may not benefit from the cloud’s scalability features, making an on-premises deployment potentially more cost-effective.
	- High-Performance Compute (HPC) for Specialized Workloads:Certain high-performance computing applications that demand specialized hardware (e.g., GPUs, FPGAs for scientific simulations or custom-built supercomputers) may require dedicated infrastructure that can be better optimized in an on-premises setup.
	- Applications Requiring Consistent Data Throughput and Low-Cost Storage:Applications requiring massive data throughput with high I/O demands (e.g., high-frequency transactional databases) or very low-cost storage may find cloud storage and data transfer fees prohibitive over time, making on-premises alternatives more attractive.

## 5. Architectural Descriptions

- 4+1 Architectural View Model: This model uses multiple views (logical, process, physical, development) to capture different perspectives of the architecture, helping to meet the needs of various stakeholders (slide 74).
- UML (Unified Modeling Language): A common language for visualizing and documenting software design (slide 73).

## 6. Important Concepts

- Microservices Discovery and API Gateways: How microservices communicate with each other and how API gateways manage this communication (slides 57-58).

The API Gateway is like a waiter in a restaurant that:

- Receives orders (client requests).
- Distributes them to the appropriate kitchens (microservices).
- Presents everything together in one neat response (aggregated data).
- Manages special requests (client-specific APIs).
- Ensures access control and security (authentication).
- Balances the load so that no single service (kitchen) gets overwhelmed.

- Multitenancy and Elasticity: Relevant for cloud architectures, these concepts refer to the sharing of resources among multiple customers and adjusting resources based on demand (slide 66).

## 7. Applying What You’ve Learned

- CAP Theorem: Consider its application in real-world scenarios, such as e-commerce systems. For example, when a user adds items to a cart, should the system prioritize Consistency or Availability if network partitioning occurs (slide 18)?
- Event-Based Architecture: Think about scenarios where asynchronous communication is necessary, like sending notifications or integrating with legacy systems.
- Cloud Adoption: Reflect on what types of applications are suitable for the cloud and what considerations (e.g., security, latency) should guide cloud architecture decisions (slide 69).

## 8. Practical Steps for Using Architectural Assets

- Reference Architectures: These serve as a blueprint for designing systems in specific domains (web, mobile, cloud). By using pre-existing reference architectures, you can save time and ensure the system follows best practices.
- Reuse Patterns: Look for common patterns (like data flow or event-driven architectures) that can be reused across different projects to accelerate development.