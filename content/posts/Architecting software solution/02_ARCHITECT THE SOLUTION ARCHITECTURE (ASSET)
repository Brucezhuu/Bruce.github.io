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

### peer to peer architecture style:
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




- Data Flow Architecture: Data flows through the system sequentially. Suitable for batch processing, this style is used in ETL (Extract, Transform, Load) scenarios (slide 22).
- Call and Return: A simple interaction where one component invokes another and waits for a response. Common in layered system architectures (slides 27-29).
- Event-Based Architectures:
    - Point-to-Point Event Handling: Used for real-time applications where messages are sent asynchronously from producer to consumer.
    - Publish/Subscribe Model: The producer publishes events, and multiple subscribers consume them based on their interest (slides 32-33).

## 4. Reference Architectures

	•	Web Architecture: Traditional and modern client-centric web architectures are compared (slide 41).
	•	Mobile Architecture: Discusses how to build backend services for mobile apps, including AWS-hosted architectures (slide 44).
	•	Broker Architecture: Introduces ESB (Enterprise Service Bus) and the role of brokers in mediating interactions between components (slide 50).
	•	SOA and Microservices: Service-Oriented Architectures (SOA) and microservices, highlighting the decoupling of functionality into independent services (slides 52-58).
	•	Cloud and Serverless Architectures: Explore cloud models (IaaS, PaaS, SaaS) and serverless computing, focusing on how services are provided without dedicated servers (slides 61-67).

5. Architectural Descriptions

	•	4+1 Architectural View Model: This model uses multiple views (logical, process, physical, development) to capture different perspectives of the architecture, helping to meet the needs of various stakeholders (slide 74).
	•	UML (Unified Modeling Language): A common language for visualizing and documenting software design (slide 73).

6. Important Concepts

	•	Microservices Discovery and API Gateways: How microservices communicate with each other and how API gateways manage this communication (slides 57-58).
	•	Multitenancy and Elasticity: Relevant for cloud architectures, these concepts refer to the sharing of resources among multiple customers and adjusting resources based on demand (slide 66).

7. Applying What You’ve Learned

	•	CAP Theorem: Consider its application in real-world scenarios, such as e-commerce systems. For example, when a user adds items to a cart, should the system prioritize Consistency or Availability if network partitioning occurs (slide 18)?
	•	Event-Based Architecture: Think about scenarios where asynchronous communication is necessary, like sending notifications or integrating with legacy systems.
	•	Cloud Adoption: Reflect on what types of applications are suitable for the cloud and what considerations (e.g., security, latency) should guide cloud architecture decisions (slide 69).

8. Practical Steps for Using Architectural Assets

	•	Reference Architectures: These serve as a blueprint for designing systems in specific domains (web, mobile, cloud). By using pre-existing reference architectures, you can save time and ensure the system follows best practices.
	•	Reuse Patterns: Look for common patterns (like data flow or event-driven architectures) that can be reused across different projects to accelerate development.