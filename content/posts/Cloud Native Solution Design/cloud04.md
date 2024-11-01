+++
title = "CloudNativeSolution 04: Cloud Persistence"
author = ["Bruce"]
date = 2024-10-24
series = ["Cloud Native Solution"]
tags = ["Cloud Native Solution", "SWE5001"]
draft = false
+++


# Overview of Cloud Persistence
**Purpose**: Cloud persistence ensures data storage and retrieval for applications, tailored for cloud scalability and flexibility.

**Requirements**:
- Efficient data storage and processing to handle various data types and volumes.
- Selection of appropriate cloud storage options based on application needs, from structured to unstructured data.

# Evolution of Database Models
1. **Navigational DBMS (1960s)**:
   - **Hierarchical Model**: Organizes data in a tree-like structure; efficient but inflexible for complex queries.
   - **Network Model**: Allows complex relationships with multiple parent-child links, but lacks simplicity.

2. **Relational DBMS (1970s)**:
   - Based on Edgar Codd’s relational model, focusing on data organization via tables and ACID transactions (Atomicity, Consistency, Isolation, Durability).
   - **Normalization**: Data is split into smaller tables to reduce redundancy and improve data integrity.

3. **Object-Oriented DBMS (1990s)**:
   - Stores complex data structures directly, without normalization.
   - Compatible with Object-Oriented Programming but lacked widespread adoption.

4. **NoSQL Databases**:
   - Designed for modern data needs, emphasizing scalability and handling of unstructured data.

# Types of Data and Their Persistence Needs
- **Structured Data**: Highly organized data with predefined schema, ideal for RDBMS.
- **Unstructured Data**: Lacks rigid structure; often stored in document or key-value stores.
- **Semi-Structured Data**: Has some organizational schema but is flexible (e.g., JSON, XML).

# Key Cloud Persistence Strategies
1. **File Storage**:
   - **Local File System**: Temporary storage attached to a VM; suitable for IaaS.
   - **Network File System (NFS)**: Shared storage accessible by multiple instances, e.g., AWS EFS.
   - **Mounted Volume**: Persistent block storage for single-instance access, e.g., AWS EBS.

## File Storage – Local File System

- **The local file system of your VM instance**: Refers to storage that is directly managed and accessed by the virtual machine instance.

- **Physical Storage Location**: This storage is located on disks that are physically attached to the host computer.

- **Data Durability**: Data is not durable in the implementation of many cloud providers, meaning it can be lost under certain conditions.

- **Data Loss Conditions**:
  - If the underlying drive fails
  - When the instance stops
  - When the instance terminates

- **Ideal Use Cases**: Suitable for temporary storage of information that changes frequently, such as buffers, caches, scratch data, and other temporary content. Also useful for data that is replicated across a fleet of instances, such as in a load-balanced pool of web servers.

- **Applicable Only for IaaS**: Designed to work with Infrastructure as a Service (IaaS) environments.

- **Analogy**: Similar to the local hard disk of a personal notebook, providing temporary, non-durable storage.
## File Storage – Network File System

- **Network file system** that can be accessed by multiple instances
- Some cloud providers like **AWS** provide a **managed network file system service**
- Allows **simultaneous read-write access** from multiple virtual machine instances, similar to a typical network file system
- Ideal for **sharing files** across multiple instances
- Applicable for **IaaS** and possibly **PaaS** (if it allows file system access)
- Analogy: a **shared folder over a network**

## File Storage – Mounted Volume

- **Block-level storage volume** that can be mounted to any running VM instances, similar to mounting a virtual hard disk
- **Persists independently** from the life of the VM instances
- Can only be attached to **one VM instance for read/write access** (for obvious reasons)
- Some cloud providers may allow the volume to be **attached to multiple VM instances for read-only access**
- Ideal when **data must be quickly accessible** and requires **long-term persistence**
- Analogy: **portable USB hard disk**


2. **Object Storage**: Stores data as objects accessible via APIs, ideal for scalability and web-based access, e.g., Amazon S3.

3. **Database Storage**:
   - **RDBMS**: Structured storage with ACID compliance, e.g., Amazon RDS.
   - **NoSQL**: Flexible and scalable, with types including document, key-value, column-family, and graph databases.

# NoSQL Database Types and Use Cases
1. **Key-Value Store** (e.g., Redis, DynamoDB): Simple storage with fast retrieval by key; ideal for caching, session storage, and real-time applications.
2. **Column-Family Store** (e.g., Cassandra, HBase): Organizes data by rows and columns, optimized for large datasets; suitable for analytics and time-series data.
{{< figure src="/img/in-post/Cloud Native Solution Design/Column Family Data Store.png" caption="<span class=\"figure-number\">Figure 1: </span>Column-Family Data Store" width="1000px" >}}
### When to Choose Column Family Databases (e.g., Cassandra, DynamoDB)

- **Clear Structure and Static Schema**: Ideal for data with a relatively clear and static schema that doesn’t frequently change.

- **Large Data Volumes**: Suitable for applications handling large volumes of data that require efficient data storage and retrieval.

- **Scalability Across Servers**: Best when data needs to be distributed across multiple servers for horizontal scalability.

- **Data Compression**: Column-family databases often support data compression, which is beneficial for reducing storage costs in large datasets.

- **Frequent Querying of Collective Data Attributes**: Useful when there is a small group of attributes in the data that are frequently queried together.

- **Aggregation and Analytics**: Designed for scenarios involving significant aggregation queries, such as analytics and time-series data processing.


3. **Document Store** (e.g., MongoDB, CouchDB): Stores hierarchical data as documents (e.g., JSON); supports flexible schemas and is ideal for content management.
### When to Use Document Store? Eg: MongoDB

- **Data Insert Consistency**: If there is a requirement of writing a huge amount of data without losing some data, then Document Store is best suited because of its high data insertion rate.

- **Data Corruption Recovery**: In Document Store, data repair can be done on a database level, and there is an automatic command to do so. However, this command reads all the data and rewrites it to a new set of files, which may be time-consuming if the database is huge, but it is preferred over losing the entire dataset.

- **Load Balancing**: Document Store supports faster replication and automatic load balancing configuration because of data placed in shards.

- **Avoid Joins**: If the developer wants to avoid normalization and joins, Document Store is best suited.

- **Changing Schema**: Document Store is schema-less, hence adding new fields will not result in any issues.

- **Not Relational Data**: If the data to be stored does not need to be relational, then Document Store should be selected.

- **Mapping**: If the mapping of application data objects is to be done directly into the document-based storage, Document Store is the best suited.

- **Creating Database Cluster**: If the database is geographically distributed and the user wishes to create a cluster to speed up data queries among remotely located databases, Document Store is suitable.

4. **Graph Store** (e.g., Neo4j): Uses nodes and edges to represent data relationships, best for social networks and recommendation systems.
### When to Choose Graph Databases (e.g., Neo4j)

- **Networking Relationships**: Ideal for data that requires entities to be linked in a networking fashion.

- **Social Networking**: Useful for applications like LinkedIn, where connections between people need to be tracked and queried.

- **Recommender Systems**: Effective for recommending similar products in e-commerce by finding relationships between items.

- **Complex Data Relationships**: Suitable for data that involves many-to-many relationships and requires low-latency performance at large scales.

- **Unstructured Relationships for Visualization**: Beneficial for visualizing unstructured relationships, such as time series and demographic data, especially when paired with AI algorithms.

- **Evolving Data Relationships**: Handles dynamically changing or evolving relationships, especially for highly structured and high-value connections.

- **Fast Navigation for Improved User Experience**: Allows for rapid navigation between entities to enhance user interactions and overall experience.

- **Moderate Data Size**: Most effective for datasets that are moderate in size compared to those stored in other types of databases.

- **Derived from Larger Datasets**: Graph data is often a subset derived from larger datasets in other storage systems.

# Polyglot Persistence
- **Definition**: Using multiple data storage technologies within a single application, each optimized for specific data needs.
- **Advantages**: Enables tailored storage solutions for different data types, improving efficiency and performance.
- **Challenges**: Increases complexity in development and maintenance.

# Cloud Data Storage Models in AWS
1. **File Storage**:
   - **Amazon EC2 Local Storage**: Temporary storage for VMs.
   - **Amazon EFS**: Managed NFS, suitable for applications needing shared access.
   - **Amazon EBS**: Persistent block storage, attached to individual EC2 instances.
   - **Amazon S3**: Object storage for unstructured data, scalable and accessible globally.

2. **Database Options**:
   - **RDS**: Managed relational database service supporting MySQL, PostgreSQL, etc.
   - **DynamoDB**: Managed NoSQL (key-value and document) database.
   - **Redshift**: Columnar storage for analytics and big data workloads.
   - **ElastiCache**: Managed in-memory caching service for improved application performance.
   - **Neptune**: Managed graph database service for connected data.

# Replication and Sharding Techniques
- **Replication**:
   - **Master-Slave**: Master handles writes; slaves sync for read operations.
   - **Peer-to-Peer**: All nodes are equal; suited for read and write distribution, but consistency can be challenging.
Master-slave replication reduces the chance of update conflicts but peer to- peer replication
avoids loading all writes onto a single point of failure

- **Sharding**:
   - **Horizontal Partitioning**: Data split across multiple servers to balance load and improve performance.
   - **Example**: Geographic sharding in e-commerce to keep data local to regions, improving access speed.

# Eventual Consistency and BASE Model
- **Eventual Consistency**: Ensures data consistency over time across nodes; ideal for applications where immediate consistency is unnecessary.
- **BASE Model (Basically Available, Soft State, Eventual Consistency)**: Alternative to ACID, used in distributed databases for higher availability and scalability.

# SQL, NoSQL, and NewSQL
- **SQL**: Traditional relational databases, suited for structured data and transactional consistency.
- **NoSQL**: Designed for large volumes of unstructured or semi-structured data; optimized for horizontal scalability.
- **NewSQL**: A hybrid model offering SQL capabilities with NoSQL scalability, suitable for modern transactional applications.

# Cloud Persistence Design Considerations
- **Data Type**: Choose based on structured, semi-structured, or unstructured data needs.
- **Scalability Requirements**: Horizontal scaling (NoSQL) vs. vertical scaling (SQL).
- **Consistency Needs**: ACID compliance for strict transactions, BASE for distributed environments.
- **Cost Efficiency**: Evaluate storage cost models for managed services.
- **Performance**: Use caching (ElastiCache) and distributed data strategies (sharding, replication) as needed.

# Choosing the Right Storage for Use Cases
- **Caching**: Use key-value stores like Redis or Memcached for high-speed data access.
- **High-Volume Reads/Writes**: Column-family stores (e.g., Cassandra) are ideal for time-series and analytics data.
- **Complex Data Relationships**: Graph stores like Neo4j excel for relationship-heavy applications.
- **File and Object Storage**: Use Amazon S3 for large-scale unstructured data that requires frequent access.

# Summary
- **Structured Data**: SQL (RDBMS) or NewSQL for high consistency needs.
- **Unstructured and Semi-Structured Data**: NoSQL with specific stores (document, key-value, column, graph) based on access patterns and data relationships.
- **Distributed and Scalable Architecture**: Use sharding and replication for high availability and performance.
- **Eventual Consistency Models**: Choose NoSQL and BASE-compliant models for non-critical transactional requirements.
