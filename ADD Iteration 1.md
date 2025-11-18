I# ADD Iteration 1 — AIDAP

## Table of Contents
- [Step 1: Review Inputs](#step-1-review-inputs)
- [Step 2: Select the Drivers](#step-2-select-the-drivers)
- [Step 3: Choose Elements to Refine](#step-3-choose-elements-to-refine)
- [Step 4: Select Design Concepts](#step-4-select-design-concepts)
- [Step 5: Instantiate Architectural Elements](#step-5-instantiate-architectural-elements)
- [Step 6: Sketch Views](#step-6-sketch-views)
- [Step 7: Analysis](#step-7-analysis)

---

# Step 1 Review Inputs

| **Category** | **Details** |
|--------------|-------------|
| **Design Purpose** | This is a greenfield system from a mature domain. The purpose is to produce a sufficiently detailed design to support the construction of the AI-Powered Digital Assistant Platform (AIDAP). |
| **Primary Functional Requirements** | **UC1:** Represents core lecturer workflows  
**UC2:** Represents core student interaction  
**UC3:** Covers technical issues related to data synchronization  
**UC5:** Represents the core AI interaction capability of the system |

---

## 2. Quality Attribute Scenarios

| **Scenario ID** | **Importance to the Customer** | **Difficulty of Implementation (Architect)** |
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

## ADD Step 3: Choose One or More Elements of the System to Refine
Refine the entire system in Iteration 1 because it is greenfield.

## ADD Step 4: Choose One or More Design Concepts that Satisfy the Selected Drivers

| Design Decisions and Location | Rationale |
|------------------------------|-----------|
| Logically structure the client part using the Rich Internet Application (RIA) reference architecture | The RIA reference architecture allows AIDAP to run directly in the browser without installation and still provides a rich user responsive interface. It supports web and mobile dashboards (CON-5) and keeps interactions fast and usable (QA-2, QA-4). It also allows for interactive dashboards and features from the browser (UC-2, UC-5) which involves processing on the client side. This helps the system stay responsive even under heavy load. |
| Logically structure the server using a Service-Based Application architecture | A service-based backend is selected because the AIDAP server does not include its own user interface.This separates the major responsibilities such as authentication, dashboard generation, AI processing, and data synchronization, and it supports integration with external university systems using the standard APIs (CON-2, UC-3). It also helps keep the system reliable and have fast AI responses (QA-3, QA-4). |
| Physically structure the system using a Three-Tier Deployment Pattern | A three-tier structure is selected because AIDAP will run in the cloud and must scale as the number of users grows (CON-1), and also keep academic data stored securely in the database layer (CON-3). This deployment supports availability and reliability (QA-3) and keeps responsibilities clearly separated; the client focuses on interaction, the application tier handles AI and synchronization logic, and the database tier manages persistent records. |
| Deploy the system as a Cloud-Hosted Web Service | Cloud deployment satisfies CON-1. It also simplifies integration with external university systems (CON-2) and supports the performance requirements of the AI component (QA-4). |
| Use API-Based Integration for Communication with External University Systems | The system needs to connect to LMS, registration, and calendar systems through standard APIs (CON-2). Using API connectors creates a clear separation between AIDAP and those external systems, and it helps keep data synchronization reliable by allowing retry logic when something temporarily fails (QA-3, UC-3). |

## Discard Alternatives:

| Discarded Alternative | Reason for Discarding |
|----------------------|-----------------------|
| Web-application | This reference architecture focuses on server side rendering and requires full page refreshes. This makes it difficult to provide a rich user interface with real-time updating dashboards. |
| Mobile Application | The system needs to work the same way on laptops and browsers too, not just on phones. Lecturers are more likely to use computers for publishing course material, so limiting AIDAP to mobile would prevent consistency across platforms. |
| Microservices Architecture | Although microservices is scalable, it adds complexity for Iteration 1, and the drivers can be achieved using a simpler service-based structure. |

## ADD Step 5: Instantiate Architectural Elements, Allocate Responsibilities, and Define Interfaces

| Design Decision and Location | Rationale |
|------------------------------|-----------|
| Instantiate a Client Data Access Module in the RIA client | A dedicated module is added to the client to manage all communication with the application tier. Using AJAX/Fetch API calls, the module communicates with the backend ensuring real-time dashboard updates, without requiring full page refreshes. This keeps the UI focused on interaction rather than data handling and avoids storing academic information locally. It also supports UC-2 by allowing dashboards to load data on demand without mixing presentation logic and data retrieval. |
| Instantiate a Synchronization Service in the application tier | A separate component is used to manage the connections with external university systems, as required by CON-2 and UC-3. This separates integration logic for future improvements in reliability. |
| Instantiate a Security Component for authentication and authorization | Because users can only access data they are allowed to see, an early security component is needed to manage login and permissions (CRN-3). Detailed interfaces will be defined in later iterations. |
| Instantiate an AI Interaction Component in the application tier | To support UC-5, a component is added to handle communication with the AI model and separate conversational logic from other services. |

