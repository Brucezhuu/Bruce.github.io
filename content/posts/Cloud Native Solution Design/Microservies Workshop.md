+++
title = "CloudNativeSolution Workshop: Decomposite to microservices"
author = ["Bruce"]
date = 2024-10-25
series = ["Cloud Native Solution"]
tags = ["Cloud Native Solution", "SWE5001"]
draft = false
+++
# 
# Workshop 1: Decomposing TickIt Ticketing Application into Microservices Architecture

To redesign the TickIt ticketing application into a microservices architecture, let's analyze the existing system components and design a modular, scalable architecture that leverages cloud-native advantages.

## 1. Identify Core Services from Current Architecture
The case study outlines several key functions of the TickIt application:

- **Event Organization**: Organizing and managing events.
- **Event Publishing**: Publishing events to various channels.
- **Online Ticketing**: Facilitating ticket sales through mobile and web applications.
- **Point of Sale (POS) Tickets**: Physical ticketing at POS counters.
- **User Profile Management**: Managing customer profiles.
- **Admin**: Administrative control and event setup.
- **Accounting**: Handling transactions and financial records.
- **External Payment Gateway**: Processing payments through an external gateway.

Based on these functions, we can decompose TickIt into individual microservices that manage these distinct areas of functionality.

## 2. Proposed Microservices and Their Roles
Each microservice should be independently deployable and responsible for a specific function. Here’s a possible breakdown:

### Event Management Service
- Manages event creation, updates, scheduling, and cancellations.
- Stores event metadata, such as location, date, time, and capacity.

### Event Publishing Service
- Publishes events to the website, mobile app, and external news agencies.
- Handles integration with marketing and notification systems.

### Ticketing Service
- Manages ticket inventory, ticket sales, and seat reservations.
- Ensures real-time updates of ticket availability.
- Manages booking logic for individual and corporate clients.

### POS Service
- Provides APIs for POS systems at physical counters.
- Manages ticket issuance, validation, and sales at physical locations.
- Ensures synchronization with the Ticketing Service to prevent overbooking.

### User Profile Service
- Manages user accounts, personal details, preferences, and loyalty points.
- Supports user authentication and authorization.

### Admin Service
- Provides a dashboard for administrators to configure events, set up pricing, and manage clients.
- Includes access controls and audit logging.

### Accounting and Billing Service
- Processes transactions and manages billing records.
- Calculates fees, taxes, and discounts.
- Integrates with the payment gateway and generates reports for financial auditing.

### Payment Gateway Service
- Handles interactions with external payment providers.
- Processes payments securely, ensuring PCI compliance.
- Manages payment success/failure callbacks and updates the Accounting Service.

## 3. Supporting Services
### Notification Service
- Sends email, SMS, and in-app notifications to users about bookings, cancellations, and event updates.
- Integrates with external messaging services or APIs.

### Reporting Service
- Generates reports on ticket sales, revenue, user engagement, and event attendance.
- Provides insights for the admin dashboard and supports data analysis.

### Authentication and Authorization Service
- Manages secure login, access tokens, and permissions for different user roles (e.g., admin, POS staff, general user).
- Integrates with User Profile Service for user identity verification.

## 4. Service Interaction and Communication
### Event-Driven Communication
- Use an event bus or messaging system (e.g., Kafka or RabbitMQ) to enable asynchronous communication between services, such as between the Ticketing Service and Notification Service for sending booking confirmations.

### API Gateway
- Implement an API Gateway to route external requests to the appropriate microservices.
- The API Gateway will handle authentication, rate limiting, and load balancing.

### Service Discovery
- Use a service registry (e.g., Consul or Eureka) to help services locate each other dynamically within a distributed environment.

## 5. Database Strategy
Each microservice should manage its own database to maintain data independence. Below are suggested data stores for each service:

- **Event Management Service**: Relational database (e.g., PostgreSQL) for structured event metadata.
- **Ticketing Service**: NoSQL database (e.g., DynamoDB) for real-time ticket inventory.
- **User Profile Service**: Document database (e.g., MongoDB) for flexible user profile schema.
- **POS Service**: Relational database or shared cache (e.g., Redis) to sync POS ticket data quickly.
- **Accounting Service**: Relational database for financial transaction records.
- **Notification Service**: Queues (e.g., SQS) for handling messages and notifications.
- **Reporting Service**: Data warehouse (e.g., Redshift or BigQuery) for analytical processing.

## 6. Deployment Strategy
- **Containerization**: Package each microservice in a Docker container.
- **Orchestration**: Use Kubernetes or another orchestration tool to manage container deployment, scaling, and failover.
- **CI/CD Pipeline**: Automate builds, tests, and deployments for each microservice independently to enable continuous delivery.

## 7. Scalability and High Availability
- **Horizontal Scaling**: Each microservice can be scaled independently based on load, such as scaling the Ticketing Service during high-demand events.
- **Load Balancing**: Use load balancers to distribute requests evenly across service instances.
- **Replicas**: Maintain multiple replicas of critical services (e.g., Ticketing, Payment) to ensure high availability.

## 8. Proposed Microservices Architecture Diagram
The architecture diagram below illustrates the interaction between core services:

- **Event Management Service** ←→ **Event Publishing Service**
- **User Profile Service** ←→ **Authentication Service**
- **Ticketing Service** ←→ **POS Service**
- **Accounting Service** ←→ **Payment Gateway Service**
- **Notification Service** ←→ Event triggers from various services

These services communicate via APIs and are managed centrally through an API Gateway.

# Workshop2: Decomposing Zumata Travel Agent Portal into a Microservices Architecture

To redesign the Zumata Travel Agent Portal into a microservices architecture, let’s analyze its current monolithic architecture and identify key business capabilities and technical requirements to guide the decomposition process.

## 1. Understanding Zumata’s Current Architecture and Challenges

### Monolithic Structure:
- Zumata is currently a monolithic Java EE application composed of the user interface, service layer, data access layer, and data store.
- The application uses server-side load balancing and clusters, handling all business functions within a single deployable unit.

### Current Challenges:
- **Scalability**: The monolithic structure makes it difficult to scale individual components based on fluctuating demand.
- **Fault Isolation**: Failures in one part of the application impact the entire system, causing downtime.
- **Performance and Traffic Spikes**: The application struggles with uneven traffic loads, leading to performance issues during high-demand periods.
- **Diverse User Needs**: Different stakeholders require customized features with faster release cycles.

## 2. Microservices Architecture Proposal

To tackle these issues, the proposed microservices architecture aims to:
- Enable independent scaling and deployment of services.
- Improve resilience by isolating faults to specific services.
- Support different user requirements and customization for faster delivery.

## 3. Decomposition Strategy

Using business-driven decomposition, we’ll focus on separating business capabilities that generate distinct value. Key steps include identifying core business capabilities, related data stores, and value streams.

## 4. Identifying Key Microservices

Based on Zumata’s core business functions, we can outline the following microservices:

### a) Flight Booking Service
- **Purpose**: Manages flight reservations, seat selections, and ticketing.
- **Data Store**: Owns the flight inventory, reservation status, and ticket data.
- **Interactions**: Communicates with the Payment Service for transactions and the Notification Service to confirm bookings.

### b) Hotel Booking Service
- **Purpose**: Manages accommodation reservations, room availability, and booking details.
- **Data Store**: Maintains hotel inventory, booking records, and room allocations.
- **Interactions**: Works with the Payment Service and Notification Service for booking confirmations.

### c) Payment Service
- **Purpose**: Handles payment processing and manages transactions for both flights and hotels.
- **Data Store**: Stores transaction records, payment status, and customer billing information.
- **Interactions**: Communicates with external payment gateways, as well as the Flight and Hotel Booking services.

### d) User Management Service
- **Purpose**: Manages user profiles, authentication, and authorization.
- **Data Store**: Contains user account details, preferences, and access control policies.
- **Interactions**: Provides access tokens to other services based on user roles.

### e) Customer Relationship Management (CRM) Service
- **Purpose**: Manages customer interactions, tracks user behavior, and provides personalized recommendations.
- **Data Store**: Stores customer interaction history and preferences.
- **Interactions**: Integrates with the User Management Service for user data and the Notification Service for targeted marketing.

### f) Inventory Management Service
- **Purpose**: Manages and updates inventory for flights, hotels, and related resources.
- **Data Store**: Keeps inventory data separate from booking details to allow real-time updates.
- **Interactions**: Syncs with the Flight and Hotel Booking services to update availability.

### g) Notification Service
- **Purpose**: Sends notifications via SMS, email, or in-app messages for booking confirmations, cancellations, and promotional offers.
- **Data Store**: Maintains templates for different notification types.
- **Interactions**: Triggers notifications based on events from the Booking and CRM services.

### h) Admin Service
- **Purpose**: Allows administrative control over services like adding new hotels, flight routes, and updating inventory.
- **Data Store**: Limited data storage, focused on configurations and administrative logs.
- **Interactions**: Directly interfaces with all relevant services, including Inventory, Flight Booking, and Hotel Booking services.

## 5. Data Management and Communication Strategy

- **Service-Specific Databases**: Each service has its own database, enabling independent scaling and avoiding shared data bottlenecks.
- **Event-Driven Communication**:
  - Services like Notification and Payment will operate based on events triggered by the booking services.
  - Use a message broker (e.g., Kafka or RabbitMQ) to decouple services and ensure reliable communication.
- **API Gateway**:
  - Routes requests to the appropriate services and handles authentication, load balancing, and traffic management.

## 6. Example of Service Interactions

### Flight Booking Workflow:
1. A customer requests a flight reservation through the API Gateway.
2. The Flight Booking Service checks availability with Inventory Management.
3. If available, the Payment Service processes the payment.
4. Upon successful payment, the Notification Service sends a confirmation.

### CRM and Personalization:
- CRM tracks user behavior and provides recommendations for future bookings.
- Integrates with Notification Service to send personalized offers.

## 7. Deployment Strategy
- **Containerization**: Package each microservice using Docker.
- **Orchestration**: Deploy on Kubernetes to handle scaling, fault tolerance, and load balancing.
- **Continuous Integration/Continuous Deployment (CI/CD)**: Implement CI/CD pipelines for automated testing and deployment.

## 8. Scalability and Fault Tolerance

- **Independent Scaling**: Services like Flight Booking and Payment can scale independently based on demand.
- **Isolated Fault Domains**: Failures in one service (e.g., CRM) do not impact core functionalities (e.g., Flight Booking).

## Summary of Proposed Microservices Architecture

- **Flight Booking Service**
- **Hotel Booking Service**
- **Payment Service**
- **User Management Service**
- **CRM Service**
- **Inventory Management Service**
- **Notification Service**
- **Admin Service**
