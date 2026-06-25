---
title: "IT2030 - Lecture 2: Software Development Models"
date: 2026-06-25
module: "IT2030"
author: Dulith Divisekara
institution: SLIIT
tags: [information-technology, software-engineering, process-models, agile, semester-notes]
---
## Study Tracker
- [ ] Differentiate between the overarching SDLC and specific Software Process Models.
- [ ] Analyze traditional and structured models (Waterfall, V-Model, Incremental).
- [ ] Understand risk-driven (Spiral) and iterative methodologies (Prototyping, Agile).
- [ ] Evaluate and select appropriate process models based on project requirements, size, and risk.

## 1. SDLC vs. Software Process Models

While often used interchangeably, the Software Development Life Cycle (SDLC) and Software Process Models serve distinct architectural purposes in engineering .

> [!IMPORTANT]
> **SDLC** dictates *what* phases must be executed (e.g., Requirements, Design, Implementation, Testing, Deployment, Maintenance) as a general structure . 
> **Software Process Models** dictate *how* those phases are executed, organized, and iterated (e.g., sequentially in Waterfall, or iteratively in Agile) .

## 2. Software Process Model Architectures

Choosing the correct structural framework depends on requirement volatility, system complexity, and organizational risk tolerance .

### Structural Comparison Matrix

| Model | Architectural Approach | Best Use Case | Primary Drawback |
| :--- | :--- | :--- | :--- |
| **Waterfall** | Strictly linear and phase-based. Each phase must finish before the next begins . | Simple projects with well-defined, static requirements (e.g., Payroll systems) . | No user feedback until final delivery; costly to adapt to changes . |
| **Iterative Waterfall** | Linear, but allows backward transitions to correct errors found in later phases . | Medium-sized projects with mostly clear requirements . | Still suffers from limited overarching flexibility compared to Agile . |
| **V-Model** | Development phases (left) map directly to matched validation/testing phases (right) . | Safety-critical systems (e.g., Medical devices, Avionics) . | Very rigid; fixing foundational changes late is extremely costly . |
| **Prototyping** | Builds a rough, early version to gather user feedback before full implementation . | Projects where initial requirements are highly ambiguous . | Scope creep; users may mistake the prototype for the final product . |
| **Incremental** | The system is developed, tested, and delivered in small functional units over time . | Large projects needing early delivery of core features (e.g., Windows OS updates) . | Requires a highly stable foundational architecture upfront . |
| **Spiral** | Iterative cycles heavily focused on continuous Risk Analysis before engineering . | High-risk, highly complex, large-scale systems (e.g., Aerospace) . | Process is complex, expensive, and requires deep risk-assessment expertise . |

### The V-Model Architecture

```mermaid
graph TD
    classDef dev fill:#e1f5fe,stroke:#0288d1,stroke-width:2px,color:#01579b;
    classDef test fill:#e8f5e9,stroke:#388e3c,stroke-width:2px,color:#1b5e20;
    classDef code fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#e65100;

    Req["Requirement Gathering"]:::dev --> Sys["System Analysis"]:::dev
    Sys --> Arch["Software Design"]:::dev
    Arch --> Mod["Module Design"]:::dev
    
    Mod --> Coding["Implementation (Coding)"]:::code
    
    Coding --> UT["Unit Testing"]:::test
    UT --> IT["Integration Testing"]:::test
    IT --> ST["System Testing"]:::test
    ST --> AT["Acceptance Testing"]:::test

    Req -.-> |"Validation Phase"| AT
    Sys -.-> |"Validation Phase"| ST
    Arch -.-> |"Verification Phase"| IT
    Mod -.-> |"Verification Phase"| UT
````

## 3. The Agile Methodology

Agile is not a single framework, but a philosophy based on the 2001 Agile Manifesto. It prioritizes team collaboration, continuous customer feedback, and flexibility to adapt to shifting requirements over rigid planning .

> [!NOTE]
> 
> **Common Agile Frameworks:**
> 
> - **Scrum:** Executes work in fixed-length iterations called _Sprints_ (1-4 weeks). Utilizes distinct roles (Product Owner, Scrum Master) and structured meetings .
>     
> - **Kanban:** Focuses on continuous flow and visual task management (using a Kanban board) rather than strict timeboxes. Best for maintenance and support workflows .
>     

## Technical Implementation

While process models dictate workflow, teams use programmatic tools to manage these processes. Below is a self-contained Python script simulating a basic Agile Scrum Kanban board, demonstrating how work items move through an incremental lifecycle.

```Python
from datetime import datetime

class AgileTask:
    def __init__(self, task_id: str, description: str):
        self.task_id = task_id
        self.description = description
        self.status = "BACKLOG"
        self.updated_at = datetime.now()

    def advance_status(self, new_status: str):
        valid_statuses = ["BACKLOG", "IN_PROGRESS", "TESTING", "DONE"]
        if new_status in valid_statuses:
            self.status = new_status
            self.updated_at = datetime.now()
            print(f"[{self.task_id}] moved to -> {self.status}")
        else:
            print(f"Error: {new_status} is an invalid Agile state.")

class ScrumBoard:
    def __init__(self, sprint_name: str):
        self.sprint_name = sprint_name
        self.tasks = []

    def add_task(self, task: AgileTask):
        self.tasks.append(task)
        print(f"Added '{task.description}' to {self.sprint_name} Backlog.")

    def print_board(self):
        print(f"\n--- Current State: {self.sprint_name} ---")
        for task in self.tasks:
            print(f"[{task.status}] {task.task_id}: {task.description}")
        print("----------------------------------\n")

if __name__ == "__main__":
    # Simulate an Agile Sprint
    sprint_board = ScrumBoard("Sprint 1: User Authentication")
    
    # Requirements added to backlog
    task1 = AgileTask("AUTH-101", "Design Login UI")
    task2 = AgileTask("AUTH-102", "Implement JWT Token Logic")
    
    sprint_board.add_task(task1)
    sprint_board.add_task(task2)
    sprint_board.print_board()
    
    # Simulate developers picking up work incrementally
    task1.advance_status("IN_PROGRESS")
    task2.advance_status("IN_PROGRESS")
    
    # Simulate testing and completion
    task1.advance_status("TESTING")
    task1.advance_status("DONE")
    
    sprint_board.print_board()
```

## Active Recall Self-Check

> [!TIP]
> 
> You should select the **V-Model** (or Spiral). The V-Model is explicitly designed for safety-critical systems because it mandates that strict verification and validation tests are designed and executed for every single phase of development, minimizing fatal defects .

> [!TIP]
> 
> While both are iterative, the Spiral Model's defining characteristic is its heavy emphasis on **Risk Analysis** during every single cycle. It is used primarily to identify and mitigate technical or financial risks in highly complex projects before moving to the engineering phase .

> [!TIP]
> 
> In Waterfall, the SDLC phases (Requirements, Design, Code, Test) are executed linearly and only once for the entire project . In Scrum, the entire SDLC is compressed and executed repeatedly in short, fixed-length cycles called Sprints (1-4 weeks), delivering a small working increment each time .