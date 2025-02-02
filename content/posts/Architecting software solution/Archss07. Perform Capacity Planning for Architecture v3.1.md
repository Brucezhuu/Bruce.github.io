+++
title = "Archss_07. Perform Capacity Planning for Architecture v3.1"
author = ["Bruce"]
date = 2024-10-04
series = ["Architecting software solution"]
tags = ["Architecting software solution", "SWE5001"]
draft = false
+++

## 1. Why Capacity Planning?

The main goal of capacity planning is to ensure that the system has enough computing resources to handle the expected workload without compromising performance. Here are a few reasons why capacity planning is crucial:

- New Systems: Estimating resource requirements for a new system.
- Growth in User Base: As the number of users grows, so does the demand for system resources.
- Feature Additions: New functionalities may require additional storage or processing power.

Capacity planning helps architects predict when and how much additional capacity (like servers or storage) will be needed and allows the system to scale automatically to accommodate changes in workload.
## 2. Capacity Planning Tasks

The primary tasks in capacity planning involve:

- Predicting Required Resources: Using historical data or benchmarks to estimate the resources needed.
- Procurement Planning: Scheduling the purchase and installation of new hardware or cloud resources based on predicted needs.
- Automatic Scaling: Enabling systems to scale up or down automatically according to demand, especially in cloud environments.

This process involves a systematic approach to ensure that each component of the system can handle peak loads without degradation in performance.

## 3. Collecting and Analyzing Metrics

One of the key elements in capacity planning is data collection. Commonly used metrics include:

- Web/Application Server Metrics: Requests per second, active sessions, busy threads.
- System Metrics: Disk utilization, CPU usage, RAM usage, and storage usage.
- Database Metrics: Queries per second, cache hit rate, open connections.

Metrics can be collected from historical monitoring or benchmark tests. For example, Flickr's capacity planning monitors metrics like photo uploads per hour, average photo size (daily and cumulative), and API request traffic to anticipate future resource needs.

## 4. Predicting Hard Disk Usage

Planning for hard disk capacity is often cumulative and can be calculated based on the projected growth of data. For example:

Scenario: If a database grows from 1 million to 20 million transactions, each transaction averaging 1 MB, the required storage can be calculated accordingly.
Approaches:
Simple Estimation: Based on growth rates (e.g., 10% increase per month) and retention requirements (e.g., data kept for two years).
Forecasting Models: Using statistical tools like linear regression or moving averages to predict disk usage over time.

## 5. Predicting Server Capacity

Server capacity planning is often driven by peak usage requirements. For instance:

- Benchmark Testing: Running standard tests to assess server response times and throughput under various loads.
- TPC-C Benchmark: The TPC-C benchmark, which measures transactions per minute, is used to evaluate online transaction processing (OLTP) systems. This can be done using cloud benchmarks or other reference benchmarks to understand server performance.

A benchmark is the act of running a computer program, a set of programs, or other operations, in order to assess the
relative performance of a server (or any hardware), normally by running a number of standard tests and trials against it.


By comparing your system’s requirements with benchmark data, you can estimate how many servers are needed to handle specific loads.

## 6. Understanding Scalability with Amdahl’s Law and the Universal Scalability Law (USL)

- Amdahl’s Law: This law helps to predict the maximum speedup of a system when using multiple processors. It’s particularly useful in parallel computing and can be applied to estimate scalability limits in system architecture. According to Amdahl's Law:

    - If the application is entirely parallel, it scales linearly.
    - If application is 100% serial, it can’t scale
    - If there’s a serial component, it limits the scalability, regardless of how many additional processors are added.

- Universal Scalability Law (USL): The USL expands on Amdahl's Law, taking into account factors like resource contention (crosstalk) and serialization. For example:

    - Perfect Parallelization: Achieved when there’s no crosstalk or serialization.
    - Presence of Serialization and Crosstalk: These can significantly limit scalability, especially in complex applications.
    - The USL helps architects assess the realistic **scalability** of their systems and identify potential bottlenecks.

## 7. Forecasting Using Historical Data and Trend Analysis

Trend Analysis: Predicting future demand often requires analyzing historical patterns. Three main patterns are common:

- Random Patterns: Fluctuations without a predictable trend.
- Trending Patterns: Consistent growth or decline over time.
- Seasonal Patterns: Demand that fluctuates according to a seasonal cycle (e.g., daily, weekly, monthly).

Statistical Techniques:

- Linear Regression: A simple technique to predict capacity based on historical trends.
- Time Series Analysis: Techniques like moving averages and exponential smoothing are used when seasonality and trends are factors.
By using historical data, architects can predict future resource requirements more accurately, ensuring a system doesn’t run out of resources. 

## 8. Load Testing and Benchmarking Tools

If historical data isn’t available, load testing and benchmarking can simulate demand:

- Load Testing Tools: Tools like JMeter and Artillery.io allow architects to define scenarios, simulate traffic, and measure system response times.
- Benchmarking: Running performance tests based on a standard workload to assess how the system handles specific scenarios, such as high traffic or complex transactions.
These tools provide valuable data for determining when and how much capacity to scale.

## 9. Procurement Planning

Adding new capacity requires lead time for procurement, installation, testing, and integration into the production environment. A sample procurement timeline might include:

- Internal Process: Raise a procurement request.
- Delivery: Lead time from the hardware vendor.
- Setup: Installation and testing phases, including software installation and production setup.

With cloud computing, this lead time can be shortened through on-demand provisioning and automation, but careful planning is still necessary.

## 10. Elasticity in Cloud Environments

One of the most powerful tools in capacity planning for cloud environments is elasticity, or the ability to auto-scale based on demand. Key components of auto-scaling include:

- Scaling Policy: Defines when and how to scale resources.
- Scaling Adjustments: Determines the amount of resources to add or remove.
- Cooldown Period: Prevents constant scaling up and down (ping-pong effect).

Auto-scaling is often used alongside historical data to schedule scaling events in advance, ensuring that the application can handle peak loads without manual intervention.

## 11. Best Practices in Capacity Planning

Assumptions and Buffers: Since capacity planning relies on predictions, it’s wise to build in some buffer capacity to handle unexpected spikes.
Systematic Approach: Document the process and predictions to facilitate communication with stakeholders and adjust easily if assumptions change.
Regular Review: Capacity planning should be revisited regularly as system requirements, usage patterns, and technologies evolve.
