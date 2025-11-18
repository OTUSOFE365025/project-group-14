# ADD Iteration 2
## ADD Step 2
The goal is to address the general architectural concern of identifying structures to support primary functionality. 

In this second iteration, the architect considers the system's primary use cases:  
- UC1
- UC2
- UC3
- UC5
## ADD Step 3
In this iteration, the elements that will be refined are the modules located in the layers defined by the two reference architectures from the previous iteration. In general, the functionality of this system is supported by the collaborations of components associated with the modules from the defined layers.
## ADD Step 4
| Design Decisions and Location | Rationale|
|------------------------------|----------------------------|
|Create a Domain Model for AIDAP                              |Before starting a functional decomposition, it is necessary to create an initial domain model for the system. The main entities in AIDAP (users, courses, materials, announcements, schedules, chat sessions, interaction history, external systems, notifications). Without this, the domain structure would use ad hoc during development.                            |
|Identify Domain Objects that map to functional requirements                              |All the primary use cases and distinct functional elements are associated with at least one domain object that the system works with.                             |
|Decompose Domain Objects into general and specialized Components                                |Each domain object is decomposed across layers into modules. The client handles user interaction (screens and formatting the request), while the server handles business logic and saving to the database. This separation keeps the system easy to change and easier to understand. There are no good alternatives to decomposing the layers into modules to support functionality.                              |
|Use REST/JSON-based APIs between the client and API Gateway                              |REST/JSON is a widely used formatting tool. Using REST/JSON provides a clear separation between the client and server. This makes it easier to replace or test the individual modules because their interactions happen through the API requests and responses, which supports accurate integration with external systems and the AI service. Alternatives were not considered because they would add complexity at this stage.  |
## ADD Step 5
| Design Decisions and Location | Rationale  |
|------------------------------|----------------------------|
|Create an initial domain model for AIDAP                              |The entities required for AIDAP’s primary use cases, such as users, courses, materials, announcements, schedules, interaction history, notifications, and external university systems are identified and represented in an initial domain model. This is created without needing the entire domain to be completed in advance.                             |
|Map primary system use cases to domain objects                              |The initial domain objects are identified by analyzing all the primary use cases for the AIDAP system (student interaction, instructor interaction, AI assistance, and data synchronization). These domain objects allow information to be stored, accessed, and shared for a better understanding.                            |
|Decompose domain objects across the layers to identify layer-specific modules and interfaces                              |This technique ensures that all the functionalities and their associated modules are identified. This decomposition is only done for the primary use cases. As the modules are separated across the layers, the architect ensures that the communication between the layers is consistent. This leads to a new architectural consideration of the interactions between layer-specific modules and interfaces.                            |
|Use REST/JSON-based APIs for communication                              |This communication style uses a request and response which allows all parts of the system to interact with each other. This allows the modules to be tested and changed independently.                            |
|Associate frameworks with modules in the data layer and external integration                              |Communication with the LMS, registration, and calendar systems is encapsulated in the AI Service Agent and synchronization modules.                            |
## ADD Step 6
Figure 5.Initial Domain Model for the System

Figure 6. Domain objects associated with the use case mode

FIGURE 7. Modules that support the primary use cases

| Element                     | Responsibility |
|----------------------------|----------------|
| DashboardView              | Displays the personalized dashboard including upcoming deadlines, grades summary, announcements, and notifications to the user. |
| CoursePublisingView        | Allows lecturers to publish course materials and announcements, and review analytics related to students. |
| LoginView                  | Provides the user interface for authentication and allows users to enter their credentials. |
| DashboardController        | Retrieves dashboard information from the server and forwards it to the DashboardView. Handles dashboard-related user actions. |
| CourseController           | Handles user actions related to the course publishing and analytics viewing and also transfers data to/from the CoursePublishingView. |
| LoginController            | Processes authentication requests from the LoginView and forwards the credentials to the server side for verification. |
| RequestManager             | Manages the communication between client controllers and the server. Sends the formatted requests and receives responses for all client-side operations. |
| RequestService (Server)    | Receives all client requests and transfers them to the correct controller. Focuses on validation so the client only interacts with a single entry point. |
| CourseAndAnalyticsController | Manages operations related to course materials, announcements, and analytics. Processes analytics and stores information. |
| DasboardController         | Creates personalized dashboard content using analytics, notifications, course materials, announcements, and deadlines. |
| SecurityController         | Verifies user authentication and allows role-based access. Responsible for only allowing valid users to use the AIDAP system. |
| SystemReliabilityController | Manages overall system health, latency, and recovery actions. Ensures the system stays reliable and consistent, by backing up user data. Allows multiple users to be using the AIDAP system at the same time. |
| SyncController             | Handles synchronization and communication with external university systems. Utilizes procedures and retires synchronization if the connection fails. |
| CourseDataMapper           | Saves, loads, and updates course information and analytics. |
| DashboardDataMapper        | Retrieves personalized dashboard data such as grades, deadlines, and notification preferences. |
| UserDataMapper             | Stores user credentials and permissions based on the type of user. Provides a secure database that requires user authentication to enter. |
| SyncDataMapper             | Stores records related to synchronization and failed connections. Ensures that connection is retired when needed. |
| AIDataMApper               | Saves queries and AI responses to form an interaction history. Uses this information to make AI more accurate. |


