+++
title = "06 Data management"
author = ["Bruce"]
date = 2024-10-14
series = ["Platform Engineering"]
tags = ["Platform Engineering", "SWE5001"]
draft = false
+++

# Data Management Overview

## 1. Data Management Goals
**Objective**: To understand different data models and their strengths and weaknesses, ensuring that applications are:
- **Highly Available**: Accessible when needed.
- **Fault-Tolerant**: Resilient to errors and failures.
- **Performant**: Fast and responsive.
- **Evolvable**: Capable of adapting to new requirements without major overhauls.

## 2. Types of Data
- **Structured Data**: Well-defined schema (e.g., tables in relational databases).
- **Semi-Structured Data**: Text with some structure, like JSON or XML.
- **Unstructured Data**: No inherent structure, such as plain text, images, and PDFs.

## 3. Key Properties for Data Management
- **Scalability**: Ability to scale vertically (increase power) or horizontally (add more instances).
- **Consistency**: Data should reflect the most recent updates.
- **Availability**: Systems should remain accessible even during partial failures.
- **Low Latency**: Fast response time for read and write operations.
- **Security**: Data should be protected from unauthorized access.

## 4. CAP Theorem
- **Consistency**: All reads receive the latest write.
- **Availability**: Every request receives a response, even if outdated.
- **Partition Tolerance**: System continues to operate despite network failures.
- **Trade-Off**: For distributed systems, you must choose between Consistency and Availability in the presence of a network partition.

## 5. RDBMS (Relational Database Management Systems)
- **Strengths**: Structured data, strong ACID compliance, flexible querying with SQL.
- **Scaling**:
  - **Vertical Scaling**: Increase server power.
  - **Horizontal Scaling (Sharding)**: Split data across multiple machines, though challenging to maintain consistency.
- **Limitations**: Difficult to handle schema evolution and less suitable for high-write applications or complex, non-tabular data.

## 6. Scaling Databases
- **Read Replication**: Creates read-only copies for scalability but introduces replication lag.
- **Sharding**: Splits data across multiple nodes to improve write scalability.
- **Caching**: Uses in-memory data storage for faster access but risks data loss if cache isn’t flushed to the disk.

## 7. NoSQL Databases
**Purpose**: Addresses limitations of RDBMS in handling large-scale, flexible, and schema-less data.

**Types of NoSQL Databases**:
- **Key-Value Stores**: Simple and fast, ideal for caching and session management (e.g., Redis).
- **Document Stores**: Stores data as documents (usually JSON); great for semi-structured data with flexible schema (e.g., MongoDB).
- **Column Family Stores**: Optimized for high-write loads with sparse data (e.g., Cassandra, HBase).
- **Graph Databases**: Designed for relationship-based queries, like social networks and logistics (e.g., Neo4j).

## 8. Key-Value Stores
- **Usage**: Extremely fast key-based lookups, commonly used in shopping carts and cache layers.
- **Limitations**: Poor performance on range queries and needs significant memory.
- **Example**: Redis, which stores key-value pairs across multiple servers.

## 9. Document Stores
- **Usage**: Supports complex queries on semi-structured data without a strict schema; useful for catalogs, 360-degree client views, etc.
- **Limitations**: Limited support for joins and complex transactions.
- **Example**: MongoDB, where each document represents an entity (e.g., JSON format).

## 10. Column Family Stores
- **Usage**: Ideal for write-heavy applications, with a flexible schema allowing sparse data storage.
- **Limitations**: No joins; secondary indices can be slow and costly.
- **Example**: Cassandra, used for applications like message stores and analytics backup.

## 11. Search Engines
- **Purpose**: Provides advanced querying capabilities such as ranking, filtering, and aggregations.
- **Usage**: Commonly used in applications needing robust search functions, like StackOverflow and GitHub.
- **Limitations**: Slow for insertions due to tokenization; not suitable as a primary datastore.
- **Example**: Elasticsearch, which supports a wide range of query types in JSON format.

## 12. Graph Databases
- **Purpose**: Optimized for data with complex relationships (e.g., social networks, logistics).
- **Usage**: Suited for queries that focus on the relationships between entities.
- **Limitations**: Horizontal scaling is difficult, and aggregations can be slow.
- **Example**: Neo4j, commonly used for analyzing connected clusters and social graphs.

## 13. SQL for Accessing Files
- **Purpose**: Allows SQL queries on files stored in systems like HDFS.
- **Common Tools**: Hive, Athena, BigQuery, Snowflake – these are not databases but enable SQL-like querying on structured files.

## 14. Choosing the Right Database
- **Application Requirements**: Match database type to the application’s read/write load, schema flexibility, query patterns, and scalability needs.
- **Business Decision**: Decide on consistency vs. availability based on user experience priorities (e.g., Amazon prioritizes availability in shopping carts).
