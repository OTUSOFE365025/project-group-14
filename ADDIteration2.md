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
| Design Decisions and Location | Rationale and Assumptions |
|------------------------------|----------------------------|
|Create a Domain Model for AIDAP                              |Before starting a functional decomposition, it is necessary to create an initial domain model for the system. The main entities in AIDAP (users, courses, materials, announcements, schedules, chat sessions, interaction history, external systems, notifications). Without this, the domain structure would use ad hoc during development.                            |
|Identify Domain Objects that map to functional requirements                              |All the primary use cases and distinct functional elements are associated with at least one domain object that the system works with.                             |
|Decompose Domain Objects into general and specialized Components                                |Each domain object is decomposed across layers into modules. The client handles user interaction (screens and formatting the request), while the server handles business logic and saving to the database. This separation keeps the system easy to change and easier to understand. There are no good alternatives to decomposing the layers into modules to support functionality.                              |
|Use REST/JSON-based APIs between the client and API Gateway                              |REST/JSON is a widely used formatting tool. Using REST/JSON provides a clear separation between the client and server. This makes it easier to replace or test the individual modules because their interactions happen through the API requests and responses, which supports accurate integration with external systems and the AI service. Alternatives were not considered because they would add complexity at this stage.  |
