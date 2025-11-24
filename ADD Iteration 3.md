# Iteration 3

## Step 2: Establish Iteration Goal by Selecting Drivers
The goal for this iteration is to address the **QA-2 quality attribute scenario**. A student accesses their personalized dashboard during normal operation. The dashboard must remain responsive and available **at least 99.5% of the time per month**, and notifications should be delivered **quickly and without delay** under normal load.  
This iteration focuses on improving dashboard responsiveness, notification delivery performance, and system availability during typical usage.

---

## Step 3: Choose One or More Elements of the System to Refine
For the chosen scenario the elements that will be refined directly affect the availability and usability of the personalized dashboard and notification system:
- DashboardController 
- NotificationManager

---

## Step 4: Choose One or More Design Concepts that Satisfy Selected Drivers

| Design Decision and Location | Rationale |
|------------------------------|-----------|
| Introduce caching in the DashBoardController | Reduces response time and frequent database queries, which allows for quicker load time. |
| Introduce asynchronous notification retrieval in NotificationManager | Prevents delay in dashboard requests by asynchronously processing notifications. |
| Apply load balancing of requests to multiple instances of the DashBoardController | Creating replications of the DashBoardController and distributing dashboard requests among them, improves quick response under a high load. |

## Step 5: Instantiate Architectural Elements, Allocate Responsibilities, and Define Interfaces

| Design Decisions and Locations | Rationale |
|-------------------------------|-----------|
| DashboardCache | Allows DashboardController to use cached responses quickly |
| AsyncNotificationDispatcher | Allows notification to be delivered outside the main request flow |
| NotificationStatusTracker | Identifies notification delays and failures without affecting the client UI. |

## Step 6: Sketch Views and Record Design Decisions 
| Element | Responsibility |
|--------|----------------|
| DashboardCache | Stores recently generated dashboard data to reduce database queries and improve response time. |
| AsyncNotificationDispatcher | Delivers notifications asynchronously to prevent blocking any dashboard requests. |
| NotificationStatusTracker | Records if each notification was successfully delivered and retries any failed ones without interrupting user experience. |

## Step 7: Perform Analysis of Current Design and Review Iteration Goal and Achievement of Design Purpose

|  | Not Addressed | Partially Addressed | Completely Addressed | Design decision made during iteration |
|--|---------------|---------------------|----------------------|----------------------------------------|
| QA-1 |  |  |  | No direct authentication or authorization changes were introduced in this iteration |
| QA-2 |  |  |  | Dashboard caching & async notification improve responsiveness & availability, but implementation technologies haven’t been selected yet. |
| QA-3 |  |  |  | No restart, retry, or synchronization recovery changes were introduced. |
| QA-4 |  |  |  | No changes introduced that affect AI performance. |
| CON-1 |  |  |  | Performance improvements support scalability constraints. |
| CON-5 |  |  |  | Dashboard is more responsive, but real-time freshness has not been addressed. |
| CRN-2 |  |  |  | The system can handle more requests, but scalability isn’t fully addressed. |
| CRN-4 |  |  |  | No new failure-handling mechanisms introduced this iteration. |



