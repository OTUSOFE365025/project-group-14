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


