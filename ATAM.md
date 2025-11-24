# ATAM Assessment Phase 3
## Utility Tree
```
Utility
├── Performance
│   ├── Dashboard Latency
│   │   └── (H,M) Dashboard loads in < 2 seconds under normal load
│   └── Notification Throughput
│       └── (M,M) Notifications queued within 1 second
│
├── Reliability
│   └── Notification Delivery Reliability
│       └── (H,M) Notification queue avoids any message loss
│
├── Availability
│   └── Dashboard uptime
│       └── (H,L) System accessible 99.5% of the time monthly
│
└── Usability
    └── Personalized Dashboard View
        └── (M,L) Dashboard content is accurate & relevant per‑user
```


## Gathering Information
### Architectural Decisions
| AD ID | Architectural Decisions |
|:-----:|--------------------------|
| AD1   | Add DashboardCache |
| AD2   | Add NotificationManager |
| AD3   | Use async notification retrieval |
| AD4   | DashboardController prompts notifications on dashboard |

### Sensitivity Points
| Sensitivity ID | Sensitivity Point Description |
|:--------------:|------------------|
| S1 | The system performance depends on having the Dashboard Cache working. |
| S2 | The dashboard's responsiveness depends on whether notifications are delivered asynchronously or synchronously.  |
| S3 | The accuracy of the data depends on how cache refresh and invalidation are handled.  |

### Tradeoffs
| Tradeoff ID | Tradeoff Description |
|:-----------:|----------|
| T1 | Caching improves the dashboard speed but also introduces the risk of users temporarily seeing old information. |
| T2 | Asynchronous notification dispatch helps improve responsiveness but also adds complexity. |
| T3 | Aggressive caching helps improve performance under heavy load but may reduce accuracy. |

### Architectural Risks
| Risk ID | Risk Description |
|:-------:|------------------|
| R1 | The cache invalidation rules are not defined yet. |
| R2 | A failure in the notification queue can result in some notifications not being delivered. |
| R3 | If the cache fails, there is no fallback mechanism implemented. |

### Architectural Non‑Risks
| Non‑Risk ID | Non‑Risk Description |
|:-----------:|----------------------|
| N1 | Separating the dashboard and notification concerns removes the risk of one feature slowing down the other. |
| N2 | Placing cache on the server side ensures fast access without exposing any sensitive data to the clients. |
| N3 | Asynchronous notification dispatch is separated from the page rendering, so the UI stays responsive even when many notifications are being sent. |

## Assessment Table
| Analyzing Scenario | QA-2 |  |  |  |
|--------------------|------|--|--|--|
| Scenario | Student opens dashboard after new announcement |  |  |  |
| Attributes | Performance, Reliability, Availability, Usability |  |  |  |
| Stimulus | User loads personalized dashboard |  |  |  |
| Environment | Normal operating load |  |  |  |
| Response | Cached dashboard is returned and the notification is queued asynchronously |  |  |  |
| **Architecture Decision** | **Sensitivity** | **Tradeoff** | **Risk** | **Non risk** |
| AD1 - Add DashBoardCache | S1, S3 | T1 | R1 | N2 |
| AD2 - Add NotificationManager | S2 | T2 | R2 | N1 |
| AD3 - Use async notification delivery | S2 | T2, T3 | R2, R3 | N3 |
| AD4 - DashboardController triggers dashboard and notifications |  |  |  | N1 |



| Analyzing Scenario | QA-2 |
|-------------------|------|
| Scenario | Student opens dashboard after new announcement |
| Attributes | Performance, Reliability, Availability, Usability |
| Stimulus | User loads personalized dashboard |
| Environment | Normal operating load |
| Response | Cached dashboard is returned and the notification is queued asynchronously |

| Architecture Decision | Sensitivity | Tradeoff | Risk | Non risk |
|-----------------------|-------------|----------|------|----------|
| AD1 - Add DashboardCache | S1, S3 | T1 | R1 | N2 |
| AD2 - Add NotificationManager | S2 | T2 | R2 | N1 |
| AD3 - Use async notification delivery | S2 | T2, T3 | R2, R3 | N3 |
| AD4 - DashboardController triggers dashboard and notifications |  |  |  | N1 |


