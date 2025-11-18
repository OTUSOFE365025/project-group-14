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

## Step 1: Review Inputs

| Category | Details |
|---------|---------|
| **Design Purpose** | AIDAP is a greenfield system in a mature domain. The purpose is to produce a sufficiently detailed architecture to support the construction of the AI-Powered Digital Assistant Platform. |
| **Primary Functional Requirements** | UC-1 Lecturer workflows, UC-2 Student interaction, UC-3 Data synchronization, UC-5 AI conversational support |
| **Quality Attribute Scenarios** | **Selected drivers:** QA-1, QA-2, QA-3, QA-4 |
| **Constraints** | All collected constraints are included as drivers |
| **Concerns** | All architectural concerns are included as drivers |


## Step 2: Select Drivers

### Quality Attributes
- QA-1: Security / Usability / Performance
- QA-2: Performance / Reliability / Availability
- QA-3: Reliability / Interoperability
- QA-4: Performance / Usability

### Constraints
- CON-1: Cloud-scalable deployment
- CON-2: API-based integration with university systems
- CON-5: Real-time dashboards responsive on web/mobile
- CON-8: AI must respond within ~2 seconds

### Concerns
- CRN-2: System must scale under high load
- CRN-3: Strong access control
- CRN-4: Reliable synchronization with external systems
- CRN-7: Personalization using interaction history


