---
title: "IT2030 - Lecture 1: Introduction to Software Engineering, Ethics & SDLC Overview"
date: 2026-06-25
module: "IT2030"
author: Dulith Divisekara
institution: SLIIT
tags: [information-technology, software-engineering, sdlc, ethics, semester-notes]
---
## Study Tracker
- [ ] Define Software Engineering and contrast it with traditional programming.
- [ ] Identify the core phases of the Software Development Life Cycle (SDLC).
- [ ] Analyze the five dimensions of a Feasibility Study (Technical, Economic, Legal, Operational, Schedule).
- [ ] Categorize software requirements (Functional, Non-Functional, Constraints) and identify stakeholders.
- [ ] Recognize the necessity of Software Engineering Ethics and professional codes (IEEE/ACM).

## 1. Software Engineering vs. Programming

Software Engineering (SE) is the application of structured engineering principles to build software that is reliable, efficient, and easy to maintain . 

During the "Software Crisis" (1960s–1990s), numerous projects failed catastrophically due to a lack of structured processes, resulting in budget overruns, missed deadlines, and severe system disasters (e.g., the Therac-25 radiation overdoses and the Ariane 5 rocket explosion) . SE emerged to solve these issues.

> [!IMPORTANT]
> Programming is a narrow, task-oriented activity focused on writing code. Software Engineering is a broad discipline encompassing the entire lifecycle: analysis, design, architecture, coding, testing, deployment, and maintenance .

### Paradigm Comparison

| Aspect | Programming | Software Engineering |
| :--- | :--- | :--- |
| **Focus** | Writing code for specific tasks or features . | Developing complete, reliable, and maintainable systems . |
| **Process** | May be ad hoc or informal . | Follows structured, well-defined processes (e.g., SDLC) . |
| **Teamwork** | Often done individually . | Requires collaboration and coordination in teams . |
| **End Goal** | Working code . | Working, reliable, scalable, and user-validated software . |

## 2. Software Development Life Cycle (SDLC)

The SDLC provides a structural roadmap for building software, ensuring teams meet user expectations while mitigating risks and delivery delays .

```mermaid
graph TD
    classDef phase fill:#e1f5fe,stroke:#0288d1,stroke-width:2px,color:#01579b;
    classDef decision fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#e65100;

    FS{"Feasibility Study"}:::decision --> R["1. Requirements Analysis"]:::phase
    R --> D["2. System Design"]:::phase
    D --> I["3. Implementation (Coding)"]:::phase
    I --> T["4. Testing"]:::phase
    T --> DP["5. Deployment"]:::phase
    DP --> M["6. Maintenance"]:::phase
    M --> R
````

### The Feasibility Study

Before initiating the SDLC, teams must determine if the system _should_ be built by evaluating five criteria :

1. **Technical:** Do we possess the required hardware, software, and skills? 2. **Economic:** Do the benefits outweigh the expenses? 3. **Legal:** Does the system comply with regulations (e.g., GDPR)? 4. **Operational:** Will end-users adapt to and utilize the new system? 5. **Schedule:** Can the project be delivered within the realistic timeline? ## 3. Requirements Engineering & Stakeholders
    

Requirements gathering forms the foundation for design, implementation, and testing, acting as a functional contract between the client and development team .

> [!NOTE]
> 
> A **Stakeholder** is any person or group with an interest in the system. This includes End Users, Internal Users (admins), External Partners (payment gateways), Regulatory Bodies, and Project Sponsors .

### Categorizing Requirements

|**Requirement Type**|**Definition**|**Example (Attendance System)**|
|---|---|---|
|**Functional**|Defines exactly _what_ the system should do .|"The system should allow teachers to mark daily attendance."|
|**Non-Functional**|Defines _how_ the system should perform (quality attributes) .|"The system should be accessible on both desktop and mobile."|
|**Constraints**|Rules, boundaries, or limitations the system must strictly follow .|"The system must run on the local university network only."|

## 4. Software Engineering Ethics

Ethics in SE govern the moral values that dictate right and wrong in the design, development, testing, and utilization of software systems .

Key ethical values include **Responsibility**, **Transparency**, **Privacy**, and **Fairness** . To enforce these standards, international bodies have established formal Codes of Ethics:

- **IEEE/ACM Joint Code:** Established in 1999, it outlines 8 core principles governing public interest, product quality, and professional judgment .
    
- **Other Professional Codes:** BCS (UK), ACS (Australia), and IFIP .
    

## Technical Implementation

While this module focuses on theoretical architecture, requirements gathering must eventually be formalized into structured, machine-readable artifacts for modern CI/CD tracking tools (like Jira or Azure DevOps). Below is a JSON representation of how a Software Requirement is technically modeled in an agile management database.


```JSON
{
  "requirement_id": "REQ-ATT-001",
  "type": "Functional",
  "category": "Attendance Management",
  "description": "The system shall allow authenticated teachers to mark daily attendance for their assigned students.",
  "acceptance_criteria": [
    "Teacher must be logged in with role 'INSTRUCTOR'.",
    "Attendance interface must display all students registered to the selected course.",
    "System must persist attendance records to the central database immediately upon submission."
  ],
  "traceability": {
    "source_stakeholder": "Academic Administration",
    "linked_design_doc": "DES-UI-004",
    "associated_tests": ["TC-ATT-101", "TC-ATT-102"]
  },
  "priority": "HIGH",
  "status": "APPROVED"
}
```

## Active Recall Self-Check

> [!TIP]
> 
> **Unit Testing** isolates and tests individual small pieces of code (like a single function or module) to ensure they work correctly on their own . **Integration Testing** verifies that those different modules work together correctly when combined .

> [!TIP]
> 
> This is a **Non-Functional Requirement**. It does not describe a specific feature or action the system takes, but rather defines a performance metric or quality attribute (how fast it must behave) .

> [!TIP]
> 
> The Ariane 5 rocket exploded 40 seconds after launch because the engineers reused legacy code from an older model without properly adapting or testing it for the new system's specifications .