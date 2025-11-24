# ATAM Assessment Phase 3
## Utility Tree

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


