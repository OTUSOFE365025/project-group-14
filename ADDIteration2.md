# ADD Iteration 2

---

## ADD Step 2
The goal is to address the general architectural concern of identifying structures to support primary functionality. 

In this second iteration, the architect considers the system's primary use cases:  
- UC1
- UC2
- UC3
- UC5

---

## ADD Step 3

In this iteration, the elements that will be refined are the modules located in the layers defined by the two reference architectures from the previous iteration. In general, the functionality of this system is supported by the collaborations of components associated with the modules from the defined layers.

---

## ADD Step 4
| Design Decisions and Location | Rationale|
|------------------------------|----------------------------|
|Create a Domain Model for AIDAP                              |Before starting a functional decomposition, it is necessary to create an initial domain model for the system. The main entities in AIDAP (users, courses, materials, announcements, schedules, chat sessions, interaction history, external systems, notifications). Without this, the domain structure would use ad hoc during development.                            |
|Identify Domain Objects that map to functional requirements                              |All the primary use cases and distinct functional elements are associated with at least one domain object that the system works with.                             |
|Decompose Domain Objects into general and specialized Components                                |Each domain object is decomposed across layers into modules. The client handles user interaction (screens and formatting the request), while the server handles business logic and saving to the database. This separation keeps the system easy to change and easier to understand. There are no good alternatives to decomposing the layers into modules to support functionality.                              |
|Use REST/JSON-based APIs between the client and API Gateway                              |REST/JSON is a widely used formatting tool. Using REST/JSON provides a clear separation between the client and server. This makes it easier to replace or test the individual modules because their interactions happen through the API requests and responses, which supports accurate integration with external systems and the AI service. Alternatives were not considered because they would add complexity at this stage.  |

---

## ADD Step 5
| Design Decisions and Location | Rationale  |
|------------------------------|----------------------------|
|Create an initial domain model for AIDAP                              |The entities required for AIDAP’s primary use cases, such as users, courses, materials, announcements, schedules, interaction history, notifications, and external university systems are identified and represented in an initial domain model. This is created without needing the entire domain to be completed in advance.                             |
|Map primary system use cases to domain objects                              |The initial domain objects are identified by analyzing all the primary use cases for the AIDAP system (student interaction, instructor interaction, AI assistance, and data synchronization). These domain objects allow information to be stored, accessed, and shared for a better understanding.                            |
|Decompose domain objects across the layers to identify layer-specific modules and interfaces                              |This technique ensures that all the functionalities and their associated modules are identified. This decomposition is only done for the primary use cases. As the modules are separated across the layers, the architect ensures that the communication between the layers is consistent. This leads to a new architectural consideration of the interactions between layer-specific modules and interfaces.                            |
|Use REST/JSON-based APIs for communication                              |This communication style uses a request and response which allows all parts of the system to interact with each other. This allows the modules to be tested and changed independently.                            |
|Associate frameworks with modules in the data layer and external integration                              |Communication with the LMS, registration, and calendar systems is encapsulated in the AI Service Agent and synchronization modules.                            |

---

## ADD Step 6

Figure 5 shows an initial domain model for the system.
Figure 6 shows the domain objects that are instantiated for the use case
model.
Figure 7 shows a sketch of a module view with modules that are derived
from the business objects and associated with the primary use cases. Note
that explicit interfaces are not shown but their existence is assumed. 

---

<img width="1740" height="882" alt="image" src="https://github.com/user-attachments/assets/cf2bbd12-0177-4f9d-98bf-674c4462dd0b" /> <br>
Figure 5.Initial Domain Model for the System

---

<img width="1316" height="1074" alt="image" src="https://github.com/user-attachments/assets/5b150b2c-19f0-47e5-bd6a-64476f1574ca" /> <br>
Figure 6. Domain objects associated with the use case mode

---

<img width="1122" height="1172" alt="image" src="https://github.com/user-attachments/assets/ff093361-dddf-446a-8669-af41fbe60edb" />
Figure 7. Modules that support the primary use cases

---

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
| AIDataMapper               | Saves queries and AI responses to form an interaction history. Uses this information to make AI more accurate. |

---

### Sequence Diagram UC-1: Publish Course Materials, Announcements and View Analytics

Figure 8. shows the initial sequence diagram for UC-1 (publish course materials and announcements). It shows how the lecturer submits new content for publishing and how the system then processes and distributes it. Once the lecturer initiates the upload, the Client Data Processor formats all the material and forwards the request through the API Gateway for authentication. After validated the Message Handler processes the request and passes it to the Interaction Controller, storing the material through the Data Access Module and recording the publish event. Notifications are sent to the students, and the analytics are updated, then a success message is returned to the lecturer to confirm the update.
<img width="1722" height="1068" alt="image" src="https://github.com/user-attachments/assets/b3e8428f-a615-4b7d-9517-8f8a90e8a2df" />

---

| Element | Method | Description |
|--------|--------|-------------|
| Lecturer | uploadCourseMaterial/postAnnouncement() | Initiates action to upload course material or post an announcement |
|  | viewAnalytics() | Displays summarized course analytics |
| Dashboard | prepareMaterial() | Collects uploaded file content and formats it to send to the client data processor |
|  | updateDashboard() | Updates dashboard once content is published successfully |
| Client Data Processor | sendUploadRequest() | Sends prepared data to the backend through the API gateway |
|  | successMessage() | Displays success message when the backend receives the request |
| API Gateway | authenticateUser() | Sends authentication request to security to verify the lecturer |
| Security | authenticateUser() | Verifies the lecturer and provides access |
|  | authenticationConfirmed() | Confirms authentication |
| Message Handler | formatRequest() | Formats the client side request so it’s compatible with the backend |
|  | processPublishRequest() | Passes the formatted request to the Interaction Controller for execution |
|  | formatResponse() | Formats the backend response to forward to the API gateway |
| Interaction Controller | saveMaterial() | Saves uploaded course material and posted announcements |
|  | notifyStudents() | Sends notifications to students when course material or announcements are posted |
|  | recordEvent(‘material_published’) | Sends analytics and data to the Dashboard Monitor |
|  | publishSuccess | Sends a success message when all backend operations are complete |
| Data Access Module | insertMaterial() | Inserts the uploaded course material data/ posted announcement data into the database |
|  | materialSaved | Confirms that the data was saved |
| Communication Manager | notificationsSent() | Confirms that students received notifications |
| Dashboard Monitor | logSuccess() | Updates analytics with the logged success event |
| Database | confirmation | Confirms that the data storage was successful |

---

### Sequence Diagram UC-2: Personalized Dashboard and Notifications

Figure 9. presents the initial sequence diagram for UC-2 (personalized dashboard and notifications). It shows how the system first prepares and then displays the dashboard information after a student requests access. The interaction starts when the student opens the dashboard, which prompts the Client Data Processor to check the Local Cache for previously stored data. If data needs to be retrieved, the request is passed through the API Gateway for validation and then it is sent to the Message Handler. The Interaction Controller collects grades, events, and notifications through the Data Access Module and then returns them to the client. The Local Cache is updated, and then the completed dashboard is rendered for the student.
<img width="1710" height="1084" alt="image" src="https://github.com/user-attachments/assets/41cec8ae-3e58-4149-9da8-d9764be6364a" />

---

| Element | Method | Description |
|--------|--------|-------------|
| Student | openDashboard() | Initiates the interaction so that the student can access their personalized alerts and data. |
| Dashboard | requestDashboard() | Requests the client side processor to fetch or load the dashboard |
|  | renderDashboard() | Displays the dashboard interface by using the provided dataset/sets. |
| Client Data Processor | sendRequest() | After the data is prepared, this sends the dashboard request to the API Gateway |
|  | sendResponse() | Receives the response from the API Gateway and triggers the dashboard rendering. |
| Local Cache | readCache() | Retrieves the stored data from the local memory for faster and quicker access. |
|  | updateCache() | Updates the cached dashboard data with the most latest information. |
| API Gateway | validateUser() | Sends the users login token and session info to the Security component to confirm if they are allowed to access the system. |
|  | routeRequest() | Passes on the validated request to the Message Handler where it is processed by its respective backend component. |
|  | sendResponse() | Returns the final dashboard info from the server back to the client. |
| Security | validateUser() | Checks that the users credentials are valid and that they have the permission to continue interaction. |
| Message Handler | getDashboard() | Receives the dashboard access request from the API Gateway and sends it to the Interaction Controller for further processing. |
|  | response() | Sends the completed dashboard data back to the API Gateway after all the backend processing is finished. |
| Interaction Controller | getUserData() | Retrieves the students grades and any upcoming events from the Data Access Module. |
|  | getNotifications() | Requests notifications from the Notification Manager so that they can be included in the dashboard view. |
| Notification Manager | getNotifications() | Collects all the notification records from the Data Access Module and then organizes them for the display. |
| Data Access Module | executeQuery() (for grades, events) | Runs SQL queries to collect all of the students grades and event info from the database. |
|  | executeQuery() (for notifications) | Runs SQL queries to collect all of the students notifications from the database. |
| Database | executeQuery() | Executes the SQL commands it receives from the Data Access Module and sends back the resulting data. |

---

### Sequence Diagram UC-3: University Data Synchronization

Figure 10. shows an initial sequence diagram for UC-3 (university data synchronization). It shows how the system updates local data by comparing stored data with the university’s external datasets. When triggerSync() is triggered, the AI Service Agent retrieves the current values from the Local Cache and requests updated records from the University API. The two datasets are then compared to determine necessary inserts or updates. The updated data is then forwarded to the Data Access Module and committed to the Database. After successful storage, the Local Cache is refreshed and the synchronization process completes.
<img width="1574" height="884" alt="image" src="https://github.com/user-attachments/assets/364f238e-8133-4dd1-9182-552b432680f0" />

---

| Element | Method | Description |
|--------|--------|-------------|
| Schedular | triggerSync() | Starts synchronization process |
| AI Service Agent | triggerSync() | Activated by the Scheduler element to start syncing |
|  | getLocalData() | Requests the existing university data from the Local Cache for comparing. |
|  | compareUpdate(localData,remoteData) | Compares data from the Local Cache and University API to determine differences and needed updates. |
|  | getUniversityData() | Requests data from University API and receives returned data |
|  | updateCache(remoteData) | Updates the Local Cache with new data to ensure consistency |
| Data Access Model | insertOrupdate() | Sends insert/update to the Database and receives confirmation when complete |
| Database | insertOrupdate() | Inserts or updates records from data access model. |
| University API | getUniversityData() | receives request for university data from AI Service Agent |

---

### Sequence Diagram UC-5: Access Academic Information

Figure 11. shows the initial sequence diagram for UC-5 (access academic information). It shows how a student can interact with the chatbot to retrieve any academic answers. When a question is submitted, the Chatbot UI prepares the message and checks the Local Cache for an existing answer. If no answer is found, the request is authenticated through the API Gateway and forwarded to the Message Handler. The Chat Flow Manager interprets the question and retrieves the required information through the AI Execution Engine and the Data Access Module. The new generated answer is returned through the server components, it is added to the Local Cache, and then displayed to the student.
<img width="1754" height="1068" alt="image" src="https://github.com/user-attachments/assets/bdbf8a88-a3c6-4895-9dc8-e1d02285a6c6" />

---

| Element | Method | Description |
|--------|--------|-------------|
| Student | askQuestion() | Initiates the interaction by asking an academic question. |
| Chatbot UI | prepareMessage() | Handles the user input, formats the question, and passes it to the client processor. |
|  | renderAnswer() | Displays the final answer returned from the backend. |
| Client Data Processor | getCachedAnswer() | Checks the Local Cache for previously retrieved answers. |
|  | returnCachedAnswer() | Returns cached results to determine if backend access is required. |
|  | sendRequest() | Sends the formatted request to the API Gateway. |
|  | receiveResponse() | Receives the backend response after processing is completed. |
|  | updateCache() | Stores new answers into the Local Cache. |
| Local Cache | Implicit method | Temporarily stores previous answers to increase speed. |
| API Gateway | authenticateUser() | Verifies that the user is allowed to request academic information. |
|  | forwardRequest() | After authentication, forwards the question to the Message Handler. |
| Security | authResult() | Returns the authentication decision back to the API Gateway. |
| Message Handler | processQuestion() | Translates and organizes the user question, directing it to the correct backend component. |
|  | sendResponse() | Sends the response back to the API Gateway. |
| Chat Flow Manager | interpretQuestion() | Sends the question to the AI Engine and finds conversation context. |
|  | buildResponse() | Prepares the final response object to send back through the server. |
| AI Execution Engine | getAcademicInfo() | Requests the necessary data from the Data Access Module to answer the question. |
|  | (returns) answer | Sends the interpreted and generated answer to the Chat Flow Manager. |
| Data Access Module | fetchData() | Retrieves the required academic information from the database. |
|  | returnInfo() | Sends the fetched academic data back to the AI Execution Engine. |
| Database | Implicit method | Stores academic records and provides data needed to answer the user’s question. |
