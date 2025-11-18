# ADD Iteration 1 — AIDAP

## Table of Contents
- [Step 1: Review Inputs](#step-1-review-inputs)
- [Step 2: Select the Drivers](#step-2-select-the-drivers)
- [Step 3: Choose Elements to Refine](#step-3-choose-elements-to-refine)
- [Step 4: Select Design Concepts](#step-4-select-design-concepts)
- [Step 5: Instantiate Architectural Elements](#step-5-instantiate-architectural-elements)
- [Step 6: Sketch Views](#step-6-sketch-views)
- [Step 7: Analysis](#step-7-analysis)

---

## Step 1 Review Inputs

| **Category** | **Details** |
|-------------|-------------|
| **Design Purpose** | This is a greenfield system from a mature domain. The purpose is to produce a sufficiently detailed design to support the construction of the AI-Powered Digital Assistant Platform (AIDAP). |
| **Primary Functional Requirements** | **UC1:** Because it represents core lecturer workflows<br>**UC2:**  Because it represents core student interaction<br>**UC3:**  Because of the technical issues associated with data synchronization<br>**UC5:** Because it represents the core AI interaction capability of the system|

---

## 2. Quality Attribute Scenarios

| **Scenario ID** | **Importance to the Customer** | **Difficulty of Implementation according to Architect** |
|-----------------|-------------------------------|---------------------------------------------|
| **QA1** | High | High |
| **QA2** | High | Medium |
| **QA3** | High | High |
| **QA4** | High | High |
| **QA5** | Medium | Medium |

From this list, QA1, QA2, QA3, and QA4 are selected as drivers.

---

## 3. Constraints

All of the constraints discussed are included as drivers.

---

## 4. Concerns

All of the architectural concerns discussed are included as drivers.

---

## Step 2: Select Drivers

This is the first iteration in the design of a greenfield system, the architect keeps in mind all of the drivers that influence the general structure of the system, however, the architect must be mindful of the following:

### Quality Attributes
- QA-1: Security,Usability,Performance
- QA-2: Performance,Reliability,Availability,Usability
- QA-3: Reliability,Interoperability
- QA-4: Performance,Usability

### Constraints
- CON-1: System must be deployed in the cloud and scale with the number of users.
- CON-2: System must connect to external university systems through standard APIs.
- CON-5: Dashboards must provide real-time data and be responsive on both web and mobile.
- CON-8: The AI responses must be generated within approximately two seconds.

### Concerns
- CRN-2: System must remain scalable even under increasing load.
- CRN-3: Strong access control must be enforced all over.
- CRN-4: Integration with external university systems must be reliable.
- CRN-7: Personalization must be supported using stored interaction history

---
<img width="1288" height="892" alt="image" src="https://github.com/user-attachments/assets/39e8ca4b-3aa4-4fb4-bf77-771e76bb03ac" />
Figure 1. Context Diagram of AIDAP System

---

## ADD Step 3: Choose One or More Elements of the System to Refine
Refine the entire system in Iteration 1 because it is greenfield.

---

## ADD Step 4: Choose One or More Design Concepts that Satisfy the Selected Drivers

| Design Decisions and Location | Rationale |
|------------------------------|-----------|
| Logically structure the client part using the Rich Internet Application (RIA) reference architecture | The RIA reference architecture allows AIDAP to run directly in the browser without installation and still provides a rich user responsive interface. It supports web and mobile dashboards (CON-5) and keeps interactions fast and usable (QA-2, QA-4). It also allows for interactive dashboards and features from the browser (UC-2, UC-5) which involves processing on the client side. This helps the system stay responsive even under heavy load. |
| Logically structure the server using a Service-Based Application architecture | A service-based backend is selected because the AIDAP server does not include its own user interface.This separates the major responsibilities such as authentication, dashboard generation, AI processing, and data synchronization, and it supports integration with external university systems using the standard APIs (CON-2, UC-3). It also helps keep the system reliable and have fast AI responses (QA-3, QA-4). |
| Physically structure the system using a Three-Tier Deployment Pattern | A three-tier structure is selected because AIDAP will run in the cloud and must scale as the number of users grows (CON-1), and also keep academic data stored securely in the database layer (CON-3). This deployment supports availability and reliability (QA-3) and keeps responsibilities clearly separated; the client focuses on interaction, the application tier handles AI and synchronization logic, and the database tier manages persistent records. |
| Deploy the system as a Cloud-Hosted Web Service | Cloud deployment satisfies CON-1. It also simplifies integration with external university systems (CON-2) and supports the performance requirements of the AI component (QA-4). |
| Use API-Based Integration for Communication with External University Systems | The system needs to connect to LMS, registration, and calendar systems through standard APIs (CON-2). Using API connectors creates a clear separation between AIDAP and those external systems, and it helps keep data synchronization reliable by allowing retry logic when something temporarily fails (QA-3, UC-3). |

---

## Discard Alternatives:

| Discarded Alternative | Reason for Discarding |
|----------------------|-----------------------|
| Web-application | This reference architecture focuses on server side rendering and requires full page refreshes. This makes it difficult to provide a rich user interface with real-time updating dashboards. |
| Mobile Application | The system needs to work the same way on laptops and browsers too, not just on phones. Lecturers are more likely to use computers for publishing course material, so limiting AIDAP to mobile would prevent consistency across platforms. |
| Microservices Architecture | Although microservices is scalable, it adds complexity for Iteration 1, and the drivers can be achieved using a simpler service-based structure. |

---

## ADD Step 5: Instantiate Architectural Elements, Allocate Responsibilities, and Define Interfaces

| Design Decision and Location | Rationale |
|------------------------------|-----------|
| Instantiate a Client Data Access Module in the RIA client | A dedicated module is added to the client to manage all communication with the application tier. Using AJAX/Fetch API calls, the module communicates with the backend ensuring real-time dashboard updates, without requiring full page refreshes. This keeps the UI focused on interaction rather than data handling and avoids storing academic information locally. It also supports UC-2 by allowing dashboards to load data on demand without mixing presentation logic and data retrieval. |
| Instantiate a Synchronization Service in the application tier | A separate component is used to manage the connections with external university systems, as required by CON-2 and UC-3. This separates integration logic for future improvements in reliability. |
| Instantiate a Security Component for authentication and authorization | Because users can only access data they are allowed to see, an early security component is needed to manage login and permissions (CRN-3). Detailed interfaces will be defined in later iterations. |
| Instantiate an AI Interaction Component in the application tier | To support UC-5, a component is added to handle communication with the AI model and separate conversational logic from other services. |

---

## ADD Step 6: Sketch Views and Record Design Decisions

---

<img width="670" height="1184" alt="image" src="https://github.com/user-attachments/assets/ddf5bf51-5917-4132-85b1-09ce23bd101e" /> <br>
Figure 2. Module View of AIDAP System

---

| Element | Responsibility |
|--------|----------------|
| Chatbot UI | Displays the chat messages and also handles the user inputs and outputs for the conversation queries, sending them to the backend. |
| Dashboard | The dashboard shows personalized data like the grades, schedules and important announcements. |
| Client Data Processor | Manages the client side data logic, like validating user input, handling the sessions, formatting requests before they get sent to the API, and also managing language and preference settings. |
| Local Cache | Temporarily stores any recent messages, announcements, and dashboard data for quick and offline access, or under weaker network conditions. |
| API Gateway | The main entry point in between the client and the server, receives API requests, validates them and directs them through the backend, all while maintaining security. |
| Message Handler | Manages how the messages and data are exchanged between the client and backend by structuring and interpreting the API requests as well as the response formats. |
| Interaction Controller | Coordinates client requests to the appropriate business module, it also coordinates workflows between the different backend components that exist. |
| Chat Flow Manager | Keeps track of context, manages the dialogue flow, and makes sure that the user interactions are consistent. |
| AI Execution Engine | Runs the AI and natural language processing logic responsible for interpreting user questions and generating responses using the stores and live data. |
| Data Entities | Represent the core business entities like users, courses, schedules, and announcements |
| Data access module | Manages the connection between the business logic and the database, performing all the data retrieval and updates in a secure way; this includes reading, writing, updating user data, logging chats, etc. |
| Helpers and Utilities | Provides the shared helper functions such as error handling, data validation, and logging, these are used across multiple layers to help keep the code clean and support backend operations. |
| AI service agent | Connects the AIDAP with the external university systems like the LMS, calendar, registration, and email platforms to both sync and recover data from failed connection attempts. |

---

<img width="1778" height="840" alt="image" src="https://github.com/user-attachments/assets/3b5ce2ff-6829-4b30-89b3-894cd700b8a2" /> <br>
Figure 3. Initial deployment diagram for the FCAPS system 

---

| Element | Responsibility |
|--------|----------------|
| User Device (web/mobile browser) | Hosts the client side application that is used by students and lectures to access AIDAP on a web or mobile browser. Uses HTTPS to send requests to the server. |
| AIDAP Application Server (cloud-deployed) | Hosts all the server-side business logic, including service interfaces, the API gateway, security, and all iteration components. Handles dashboard display and rendering, notifications, AI interaction, and manages synchronization. Checks requests for routing to external systems. |
| Cloud Database Server | Stores external system’s synchronized information, user profiles, dashboard data, interaction history with the AI model, notification preferences. Accessed through SQL/ORM using the application server. |
| LMS System | LMS system is one of the external university learning platforms from which the AIDAP system retrieves the course assignments, materials, grades and analytics using REST API. |
| Registration System | An external system that provides academic enrollment records, and course registration information retrieved by the AIDAP using REST API. |
| Calendar System | External calendar that provides academic deadlines, events and schedules that AIDAP syncs using REST API. |
| Authentication System | External identification checker, that verifies user credentials allowing for secure login before accessing the AIDAP. |

---

<img width="792" height="1042" alt="image" src="https://github.com/user-attachments/assets/a3534d1c-9a03-40da-aad4-5061059e12fc" /> <br>
Figure 4. Reference Architecture diagram for the FCAPS system

---

| Relationship | Description |
|-------------|-------------|
| Between User Device and AIDAP Application Server | Communication takes place over secure HTTPS for all client–server requests |
| Between AIDAP Application Server and Cloud Database Server | The server communicates with the cloud database using SQL and ORM for storing and retrieving any academic data. |
| Between AIDAP Application Server and LMS System | Integration is done using the REST APIs to fetch the course materials, grades, and analytics. |
| Between AIDAP Application Server and Registration System | The AIDAP retrieves the enrollment and course registration data using REST APIs. |
| Between AIDAP Application Server and Calendar System | The AIDAP retrieves the academic events and schedules by using REST APIs. |
| Between AIDAP Application Server and Authentication System | Login authentication is performed using an Auth API |

---

## ADD Step 7: Perform Analysis of Current Design and Review Iteration Goal and Achievement of Design Purpose.

| Not Addressed | Partially Addressed | Completely Addressed | Design Decisions made during the iteration |
|---------------|---------------------|----------------------|-------------------------------------------|
|               |                     | **UC-1**             | The RIA client architecture properly supports uploading any content & viewing analytics functionalities. |
|               |                     | **UC-2**             | Fully supported through the responsive web/mobile dashboard that is implemented. |
|               | **UC-3**            |                      | Partially supported because synchronization components do exist but the retry and conflict resolution are not defined yet. |
|               |                     | **UC-5**             | Fully supported by the AI Execution Engine and the Chat Flow Manager. |
|               | **QA-1**            |                      | Partially supported because the Security Component isn’t defined yet. The API Gateway makes the access controlled and reliable and the RIA architecture ensures that the interface stays fast and also user-friendly. |
|               |                     | **QA-2**             | The RIA client, cloud deployment, and separation of concerns improves responsiveness and helps dashboards load quickly, but the iteration has not yet defined consistent performance guarantees. |
|               | **QA-3**            |                      | Partially supported since reliability is addressed conceptually but the failure recovery is not designed yet. |
|               |                     | **QA-4**             | AI processing is isolated in its own component, which helps keep responses fast and helps improve usability, but the specific performance guarantees have not been defined yet. |
|               |                     | **CON-1**            | Designing the system with a three-tier deployment with client, application, and database layers supports scaling in the cloud environment. |
|               |                     | **CON-2**            | Fully satisfied through API integration and synchronization components for the external systems. |
|               | **CON-3**           |                      | A Security Component was added to manage login and access control, however, the detailed role based permissions and secure session handling are not defined yet. |
|               | **CON-4**           |                      | The system includes notification handling, and notification preferences are stored in the database. However, specific notification delivery rules are not designed yet. |
|               |                     | **CON-5**            | Fully satisfied using the RIA architecture, and responsive dashboard. |
|               | **CON-6**           |                      | A Synchronization Service and API based integration were created, allowing for data syncing, but retry handling and conflict handling have not been defined yet. |
|               | **CON-7**           |                      | The Security Component and Data Access Module provide a secure boundary for retrieving student data, but detailed rules have not been defined yet. |
|               | **CON-8**           |                      | Partially addressed because the AI is isolated for faster responses, but the performance guarantees and load-balancing are not specified or defined yet. The AI Engine allows fast processing, but the set of rules for ensuring consistent 2-second responses are still to be set. |
| **CON-9**     |                     |                      | No relevant decisions made |
| **CON-10**    |                     |                      | No relevant decisions made |
| **CON-11**    |                     |                      | No relevant decisions made |
| **CON-12**    |                     |                      | No relevant decisions made |
| **CRN-1**     |                     |                      | No relevant decisions made |
|               |                     | **CRN-2**            | The cloud based deployment and the service based backend allow the components to scale as the usage continues to grow. |
| **CRN-3**     |                     |                      | No relevant decisions made |
|               | **CRN-4**           |                      | The system supports reliable external communication, but decisions for retries and handling failures are still to be implemented. |
| **CRN-5**     |                     |                      | No relevant decisions made |
| **CRN-6**     |                     |                      | No relevant decisions made |
|               | **CRN-7**           |                      | The interaction history will be stored, but the decision of how personalization will work from a user perspective has not been designed yet. |
|               |                     | **CRN-8**            | The RIA architecture ensures very consistent interface behavior across browsers and devices. |
| **CRN-9**     |                     |                      | No relevant decisions made |



