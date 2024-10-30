+++
title = "Archss_01: Introduction to solution architecture"
author = ["Bruce"]
date = 2024-10-04
series = ["Architecting software solution"]
tags = ["Architecting software solution", "SWE5001"]
draft = false
+++

## 1.	Definition of Architecture:
Architecture refers to the fundamental organization of a system, the components, their relationships, and the principles guiding its design and evolution. 

This definition highlights that architecture isn’t just about components but also the way they interact with each other and the environment.

## 2.	Enterprise Architecture vs. Solution Architecture:

- Enterprise Architecture is a high-level framework that guides the overall structure and operations of an organization. It focuses on aligning IT infrastructure and business processes to meet strategic goals. It serves as a blueprint for how an organization can achieve its objectives.

- Solution Architecture, on the other hand, focuses on specific projects or systems. It defines how individual IT systems should be designed and implemented to meet the specific needs of a business or project. Solution architecture takes the strategic direction from enterprise architecture and applies it in a more detailed, practical context.

## 3.	Architect’s Role and Responsibilities:
- Architects are responsible for the **viability** of a solution, meaning they ensure the designed solution can be implemented, meets the necessary requirements, and aligns with enterprise architecture.

- Architects also **support** the project manager, assess business requirements, evaluate technical solutions, define the architecture, and ensure security, reliability, and maintainability of the solution.

## 4.	Benefits of Architecture Reviews:
Architecture reviews help identify potential risks, assess quality attributes like scalability and performance, and promote good design practices. These reviews also reduce project costs by uncovering problems early and help stakeholders resolve conflicting requirements.
##	5.	ISO 25010: Software Quality Attributes:
This international standard defines a set of quality attributes for evaluating software products, including:
- Functionality: The degree to which the system meets user needs.
- Performance Efficiency: How well the system performs with respect to resource usage.
- Compatibility: The ability of a system to work with other systems.
- Usability: The ease with which users can achieve their goals.
- Reliability: The ability of the system to perform under specified conditions for a specified time.
- Security: Protection of data and information from unauthorized access.
- Maintainability: How easily the system can be modified.
- Portability: The ability to transfer the system from one environment to another.
### three types of relationships among software quality attributes:
#### 1. Direct Relationship:

Definition: If a software system is good at one attribute, it is likely to also be good at another attribute.

Example:

- Security and Reliability often have a direct relationship. A system that has strong security measures (e.g., encryption, authentication) is also likely to be reliable because it will be more resilient against attacks, unauthorized access, and data corruption. Improving security often enhances the reliability of the system since fewer breaches or failures are expected.

#### 2. Neutral Relationship:

Definition: Two attributes are independent of each other, meaning improving one does not necessarily affect the other.

Example:

- Usability and Scalability are often neutral. A system that is highly usable (e.g., easy for users to navigate) does not necessarily affect its ability to scale (e.g., handle more users or larger data volumes). The design decisions for usability may not have a direct impact on scalability.

3. Inverse Relationship:

Definition: If a software system is good at one attribute, it is often not good at the other attribute.

Example:

- Performance and Security can have an inverse relationship. For instance, adding more security layers (e.g., encryption, frequent authentication checks) can degrade system performance because these processes add overhead. Therefore, a system optimized for high security might experience slower performance, and vice versa.

## 6.	Architectural and Design Characteristics:
- Abstraction: Helps focus on significant details, ignoring unnecessary complexity.
- Encapsulation: Hides details that are not relevant to other components.
- Cohesion (high is good): High cohesion means components work well together, improving maintainability.
- Coupling (low is good) : Low coupling reduces the interdependence between components, making the system easier to modify and maintain.

## 7.	Solution Architecture Deliverables:
These include conceptual and physical models of data, services, and technologies. Solution architects create high-level designs that guide detailed implementation and ensure alignment with business goals and enterprise architecture.

## Appendix: ISO25010 Product Quality Model
1. Functional Suitability:

- Definition: The degree to which the software meets the stated and implied functional needs.
- Sub-attributes:

	• Functional Completeness: The extent to which all the required functionalities are provided.

	• Functional Correctness: Accuracy of the results produced by the system.

	• Functional Appropriateness: The usefulness of the functionalities in achieving specific tasks.

2. Performance Efficiency:

- Definition: Performance relative to the amount of resources used.
- Sub-attributes:

	•	Time Behavior: How quickly the system responds or performs.

	•	Resource Utilization: Efficient use of system resources like CPU, memory, etc.

	•	Capacity: The system’s ability to handle the required load (e.g., number of users, data volume).

3. Compatibility:

- Definition: The ability to function well alongside other products, systems, or environments.
- Sub-attributes:

	• Co-existence: Ability to perform efficiently in a shared environment.

	• Interoperability: The ability to exchange information and use it across systems.

4. Usability:

- Definition: The degree to which the software can be used effectively and efficiently by specific users.
- Sub-attributes:

	• Appropriateness Recognizability: How easily users can recognize if the system meets their needs.

	• Learnability: Ease of learning to use the system.

	• Operability: How easy the system is to operate and control.

	• User Error Protection: Prevention of user errors.

	• User Interface Aesthetics: How visually appealing the interface is.

	• Accessibility: Usability by people with disabilities or different characteristics.

5. Reliability:

- Definition: The system’s ability to perform its intended functions under specific conditions for a specified period.
- Sub-attributes:

	• Maturity: How reliable the system is under normal operation.

	• Availability: The system’s availability when required for use.

	• Fault Tolerance: Ability to continue functioning despite the presence of faults.

	• Recoverability: Ability to recover and restore functions after failure.

6. Security:

- Definition: Protection of data and information to ensure appropriate access and modification.
- Sub-attributes:

	• Confidentiality: Ensures data is only accessible to authorized users.

	• Integrity: Prevents unauthorized modifications to data.

	• Non-repudiation: Ensures actions or events can be verified, and users cannot deny them.

	• Accountability: The ability to trace actions back to responsible entities.

	• Authenticity: Ensuring the identity of entities interacting with the system is genuine.

7. Maintainability:

- Definition: The ease with which the software can be modified to meet new requirements, fix defects, or improve performance.
- Sub-attributes:

	• Modularity: The system’s components can be modified independently.

	• Reusability: The ability to reuse parts of the system in other contexts.

	• Analyzability: Ease of diagnosing problems or evaluating the impact of changes.

	• Modifiability: Ease of making changes without introducing new issues.

	• Testability: The extent to which testing can be effectively conducted.

8. Portability:

- Definition: The ease with which the system can be transferred or adapted for use in different environments.
- Sub-attributes:

	• Adaptability: The system can be adapted to different environments without significant changes.

	•	Installability: Ease of installing and uninstalling the system in specific environments.
    
	•	Replaceability: Ability to replace the system with another one that performs the same function.
