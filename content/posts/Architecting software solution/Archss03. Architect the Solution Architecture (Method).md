+++
title = "Archss_03: Architect the solution architecture (Method)"
author = ["Bruce"]
date = 2024-10-04
series = ["Architecting software solution"]
tags = ["Architecting software solution", "SWE5001"]
draft = false
+++

## 1. Defining Requirements

- Tasks:
	1.	Collect Stakeholder Requests.
	2.	Capture Common Vocabulary.
	3.	Define System Context.
	4.	Outline Functional & Non-Functional Requirements.
	5.	Prioritize Requirements.
	6.	Detail Functional & Non-Functional Requirements.
	7.	Update the Solution Architecture Document (SAD).
	8.	Review Requirements with Stakeholders.

Example of detaile functional requirements using use case:
{{< figure src="/img/in-post/Architecting software solution/reserve slot use case1.png" caption="<span class=\"figure-number\">Figure 1: </span>reserve slot use case 1" width="1000px" >}}

{{< figure src="/img/in-post/Architecting software solution/reserve slot use case2.png" caption="<span class=\"figure-number\">Figure 2: </span>reserve slot use case 2" width="1000px" >}}

Example of detaile non-functional requirements:

{{< figure src="/img/in-post/Architecting software solution/Availability Requirement.png" caption="<span class=\"figure-number\">Figure 3: </span>Availability Requirement" width="1000px" >}}
## 2. Logical Architecture
### Logical Architecture activities (Task summary)
- 1. Survey Architecture Assets
	{{< figure src="/img/in-post/Architecting software solution/Logical-Architect-task1.png" caption="<span class=\"figure-number\">Figure 4: </span>Logical-Architect-task1" width="1000px" >}}
- 2. Define Architecture Overview
	{{< figure src="/img/in-post/Architecting software solution/Logical-Architect-task2.1.png" caption="<span class=\"figure-number\">Figure 5: </span>Logical-Architect-task2.1" width="1000px" >}}
	{{< figure src="/img/in-post/Architecting software solution/Logical-Architect-task2.2.png" caption="<span class=\"figure-number\">Figure 6: </span>Logical-Architect-task2.2" width="1000px" >}}
- 3. Outline Functional Elements
	{{< figure src="/img/in-post/Architecting software solution/Logical-Architect-task3.1.png" caption="<span class=\"figure-number\">Figure 7: </span>Logical-Architect-task3.1" width="1000px" >}}
	{{< figure src="/img/in-post/Architecting software solution/Logical-Architect-task3.2.png" caption="<span class=\"figure-number\">Figure 8: </span>Logical-Architect-task3.2" width="1000px" >}}
- 4. Outline Deployment Elements
- 5. Verify Architecture
- 6. Detail Functional Elements
- 7. Detail Deployment Elements
	{{< figure src="/img/in-post/Architecting software solution/Logical-Architect-task7.png" caption="<span class=\"figure-number\">Figure 9: </span>Logical-Architect-task7" width="1000px" >}}
- 8. Validate Architecture
- 9. Update Solution Architecture Document
- 10. ReviewArchitecturewith Stakeholders

## 3. Physical Architecture
### Physical Architecture activities (Task summary)
- 1. SurveyArchitectureAssets
- 2. Define Architecture Overview
- 3. Outline Functional Elements
- 4. Outline Deployment Elements
- 5. VerifyArchitecture
- 6. Detail Functional Elements
- 7. Detail Deployment Elements
- 8. ValidateArchitecture
- 9. Update Solution Architecture Document
- 10. Review Architecture with Stakeholders
详细见打印的PPT

## 4. Architect SHS Workshop (4 Exercise)
### Exercise 1:
{{< figure src="/img/in-post/Architecting software solution/03_exercise1.png" caption="<span class=\"figure-number\">Figure 4: </span>03_exercise 1" width="1000px" >}}

{{< figure src="/img/in-post/Architecting software solution/03-exercise1-briefing.png" caption="<span class=\"figure-number\">Figure 5: </span>03_exercise-1" width="1000px" >}}
- Step 1: Outline the Functional Requirements and Create the System Context
	- Functional Requirements:

	Based on the case study, the functional requirements are as follows:

	- Retrieve Historical Records: The system must allow medical staff to access student historical medical records from the mainframe system.
	- Perform Health Screenings: Enable medical staff to conduct health screenings (e.g., vision, hearing, weight, height) and record results.
	- Manage Immunization Records: Track and update immunization records during screenings.
	- Appointment Scheduling: Allow medical staff to schedule appointments for further check-ups or missed screenings, with data exchange between SHS and the mainframe system.
	- Generate and Print Screening Results: Print results on-site for insertion into student health booklets.
	- Statistical Reporting: Enable SHB staff to consolidate and analyze screening data, generating monthly reports.
	- Manage Student Profiles: Add, modify, or update student profiles within the system.

	- System Context:
		The system context defines the key actors interacting with SHS and the flow of information. Here’s a simplified view:

		- Actors:
			- Medical Staff: Interacts with SHS at schools to perform screenings, schedule appointments, and print results.
			- SHB Staff: Accesses SHS to generate reports and manage student profiles.
			- Mainframe System: Provides historical data and processes appointment requests.
		- Interactions:

			SHS interacts with medical staff on-site at schools and connects with the mainframe system for historical records and appointments.
		- Data flows include reading historical records, writing new screening results, scheduling appointments, and producing reports.

- Step 2: Identify and Detail Key Functional Requirements Influencing Architecture
Key Functional Requirements:
	- Use Case 1: Retrieve Historical Records
		- Description
		Medical staff need access to a student's historical medical records to ensure continuity and accuracy in health assessments and treatments.

		- Primary Actor
		Medical Staff (Nurses, Doctors)

		- Secondary Actor
		Mainframe System

		- Main Flow of Events
			- Medical staff initiates a request for a student’s historical records in SHS by entering the student’s ID or NRIC.
			- SHS sends a query to the mainframe to retrieve the student’s historical data.
			- The mainframe system processes the request and sends back the historical records to SHS.
			- SHS displays the historical records on the medical staff’s device for review.
			- Medical staff reviews the data to proceed with the screening process.
		- Alternative Flow of Events
			- Mainframe System Unavailable:If the mainframe is unavailable, SHS informs the medical staff that historical records cannot be accessed.SHS enters a "degraded mode," allowing the medical staff to proceed with the screening without access to historical data.Once the mainframe connection is restored, the medical staff can attempt to retrieve the records again.
			- Student Record Not Found:If the entered student ID or NRIC does not match any records in the mainframe, SHS notifies the medical staff that no records were found.Medical staff may re-enter the ID or NRIC for confirmation or proceed without historical data.
		- Pre-Conditions
			- The medical staff has a valid login and access rights to view student records.
			- The mainframe system is online and accessible from the school’s location.
		- Post-Conditions
			- Historical medical data is displayed on the SHS interface for the medical staff’s review.
			- In cases where data was unavailable, SHS logs the issue for later retrieval attempts or reports to SHB staff.
	- Use Case 2: Generate and Print Screening Results
		- Description
		After completing a health screening, medical staff must print a summary of the screening results to insert into the student’s health booklet, ensuring that the student and their guardians have an accessible copy of the results.

		- Primary Actor
		Medical Staff (Nurses, Doctors)

		- Secondary Actor
		Printer (Local on-site printer)

		- Main Flow of Events
			- Medical staff completes a student’s health screening and saves the results in SHS.
			- Medical staff selects the option to generate a screening summary report.
			- SHS compiles the recorded screening data into a formatted report for printing.
			- SHS sends the formatted report to the local on-site printer.
			- The local printer prints the report.
			- Medical staff retrieves the printed report and places it in the student’s health booklet.
	- Alternative Flow of Events
		- Printer Unavailable:If the printer is offline or unavailable, SHS notifies the medical staff that the report cannot be printed immediately.SHS stores the printable report in a queue or local storage to print later once the printer becomes available.Medical staff receives a notification once the report is successfully printed.
		- Incomplete Screening Data: If the screening data is incomplete, SHS alerts the medical staff and prevents the printing of the report. Medical staff can review and complete missing data entries before attempting to print the report again.
	- Pre-Conditions
		- Medical staff has completed the student’s health screening and saved all necessary data.
		- The local printer is set up and connected to SHS.
	- Post-Conditions
		- A printed report is generated and available for insertion into the student’s health booklet.
		- If printing was unsuccessful, the report remains available in SHS for a later printing attempt.

- Step 3: Outline Non-Functional Requirements
Grouped Non-Functional Requirements:
	- Performance: The system should handle high-volume screening data and allow medical staff to access historical records within a few seconds.
	- Availability: SHS should provide 99% availability and support offline mode to allow uninterrupted operation during outages.
	- Usability: Designed for low IT-literacy users, with an intuitive interface and minimal steps for core tasks.
	- Security: Ensure data confidentiality and integrity during transmission, storage, and archival.
	- Scalability: The system should scale to handle over 10 million student records annually.

- Step 4: Identify and Detail Key Non-Functional Requirements Influencing Architecture
Key Non-Functional Requirement:
Availability (Degraded Mode for Offline Operation):
	- Scenario Details:
	- Stimulus: Network or system outage at the school during a screening session.
	- Source: Network provider or internal system failure.
	- Response: The system enters a degraded mode, allowing data entry for essential functions (e.g., screenings) without network connectivity. Data syncs with the mainframe once the network is restored.
	- Response Measure: Data entry must continue for up to 5 hours offline, with data seamlessly synced post-restoration.
### Exercise 2:
{{< figure src="/img/in-post/Architecting software solution/03-exercise2.png" caption="<span class=\"figure-number\">Figure 6: </span>03_exercise-2" width="1000px" >}}

#### Architectural Decisions
Decision ID: 1

- Issue: Handling disconnected or low-network scenarios in schools due to network congestion.
Architectural Decision: Develop a mobile app (Android/iOS) for SHS that can work in both online and offline modes.
- Assumptions: Medical staff frequently carry mobile devices, which are portable and accessible, making them suitable for use in a school setting.
Offline access ensures continuity during network disruptions, storing data locally and syncing once a connection is re-established.
- Alternatives:
Tablet-based app: Provides a larger screen but may be less convenient to carry.
Laptop app: Offers higher processing power but lacks portability and battery efficiency.
- Justification: Mobile phones are universally accessible, battery-efficient, and provide sufficient processing power for SHS operations. They are portable and can handle intermittent network issues more effectively than laptops or tablets.

Decision ID: 2

- Issue: Supporting bulk file transfers and real-time transactions between SHS and the mainframe.
- Architectural Decision: Implement FTP for bulk data transfers and a Transaction Engine (TE) for real-time interactions.
- Assumptions:
Bulk data exchanges can be scheduled periodically, and the FTP protocol is reliable for handling large file transfers.
Real-time transactions will be handled by TE, enabling the system to handle immediate queries and responses efficiently.
- Alternatives:
FTP only: Suitable for bulk transfers but introduces delays in real-time transaction handling.
TE only: Effective for real-time interactions but inefficient for transferring large files.
- Justification: Combining FTP and TE optimizes both bulk data exchanges and real-time transactions, leveraging each protocol's strengths and avoiding the limitations of using only one.


{{< figure src="/img/in-post/Architecting software solution/03-exercise3.png" caption="<span class=\"figure-number\">Figure 7: </span>03_exercise-3" width="1000px" >}}

{{< figure src="/img/in-post/Architecting software solution/03-exercise4.png" caption="<span class=\"figure-number\">Figure 8: </span>03_exercise-4" width="1000px" >}}

### summary：
关于本章节，重点是 Logical Architecture 和 Physical Architecture 的部分， physical Architecture的部分要与Logical Architecture的部分相对应。
#### Logical Architecture： 
- Logical architecture: Overview:画出 `<<zone>>`和`tier`以及其可能包含的`subsystem`的架构

- Logical architecture: fuctional element outline (use case specific): 标明`Boundary`, `control`,`entity`

- Logical architecture: detailed functional element (use case specific): 在 fuctional element outline 的类的基础上对每个类尽量加上一些成员函数方法

- Logical architecure: deployment element outline (use case specific): 标明`location`以及其中的各种`server`, `engine`以及`client PC`或`Mobolie device`

- Logical architecure: detailed deployment element (use case specific): 标明`deploy`和`manifest`箭头，由此指明每个detailed functional element 部署或依赖在哪个`server`或`Engine`上， 其中：
	- Deploy Arrows: These arrows indicate where each component or subsystem is deployed within the physical infrastructure. For example, an arrow pointing from the Application Server to the Data Access Layer subsystem means that the Data Access Layer is deployed on the Application Server. The deploy arrow shows the physical location where each software component is hosted.

	- Manifest Arrows: These arrows signify that a particular layer, component, or boundary "contains" or "references" another element. For instance, in the diagram, the Integration Layer manifests the Payment System Interface, meaning the Payment System Interface is a part of or linked to the Integration Layer for the purpose of facilitating payment operations. Manifest arrows typically indicate a logical inclusion or relationship within the system architecture.
#### Physical Architecture：
- Overview: 在Logical Overview的基础上对每个`zone`划分子网，对`zone`中的每个`subsystem`指定服务器与端口号，并添加`firewall`以及`gateway`和`router`

- Physical functional element outline (usecase-specific): 对照 Logical fucntional element outline 画，可以在其基础上添加一些`xxxDelegate`(`<servelet>`)，利用JAVA EE指明每个component属于以下哪种：

	- For each boundary component representing interaction with persons, implement with JSPs and servlets
	- For each boundary component representing interaction with systems, implement with JMS
	- For each controller component, implement with Session EJB
	- For each entity component, implement with JPA

- Physical detailed functional element (usecase-specific): 在outline的基础上添加一些可能用到的成员函数方法

- pysical outline deployment: 在Logical deployement的基础上指明某个`location`具体是什么，以及一些`server`的合并和增减,并指明每个server的具体型号，CPU, RAM等

- Physical architecure: detailed deployment element (use case specific): 标明`deploy`和`manifest`箭头，由此指明每个detailed Physical element 部署或依赖在哪个`server`或`Engine`上

最后画 integration EndPoint图
{{< figure src="/img/in-post/Architecting software solution/Pysical-IntegrationEndpoint.png" caption="<span class=\"figure-number\">Figure 9: </span>Pysical-IntegrationEndpoint" width="1000px" >}}