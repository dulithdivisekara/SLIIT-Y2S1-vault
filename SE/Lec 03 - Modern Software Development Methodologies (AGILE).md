---
title: "IT2030 - Modern Software Development Methodologies: AGILE"
date: 2026-06-25
module: "IT2030"
author: Dulith Divisekara
institution: SLIIT
tags: [information-technology, software-engineering, agile, scrum, semester-notes]
---
## Study Tracker
- [ ] Contrast Traditional (Plan-based) and Modern (Agile) software development methodologies.
- [ ] Memorize the 4 Key Values of the Agile Manifesto.
- [ ] Define the primary SCRUM Roles (Product Owner, Scrum Master, Development Team).
- [ ] Understand SCRUM Artifacts (Product Backlog, Sprint Backlog, Burn Down Chart) and formulate User Stories.
- [ ] Describe the purpose of key SCRUM Activities (Sprint Planning, Daily Scrum, Review, Retrospective).

## 1. Traditional vs. Modern (Agile) Methodologies

Traditional software development (like the Waterfall model) relies on heavy upfront planning and strict sequential phases . It suffers from high costs, low tolerance for requirement changes, and late defect detection .

Modern methodologies, specifically **Agile**, emphasize adaptability, teamwork, and iterative development . Agile delivers the highest business value in the shortest time with high quality by accommodating requirement changes throughout the lifecycle .

### The 4 Key Values of the Agile Manifesto

Agile operates on a foundational philosophy that prioritizes specific values over traditional corporate constraints :

| In Agile, we value... | Over Traditional Ways... |
| :--- | :--- |
| **Individuals and Interactions** | Processes and Tools |
| **Working Software** | Comprehensive Documentation |
| **Customer Collaboration** | Contract Negotiation |
| **Responding to Change** | Following a Plan |

> [!NOTE]
> Agile is an umbrella term encompassing various frameworks, including SCRUM, Kanban, eXtreme Programming (XP), Test Driven Development (TDD), and Lean Software Development .

## 2. The SCRUM Framework

SCRUM is the most popular Agile approach for developing innovative products and services. It breaks work into fixed-length iterations called **Sprints** (typically up to a calendar month) .

### 2.1 SCRUM Roles
A Scrum team is cross-functional and self-organizing .

* **Product Owner:** The empowered central point of product leadership and the client's representative. Responsible for maintaining the Product Backlog, deciding which features to build, and accepting/rejecting work results .
* **Scrum Master:** A servant-leader who ensures the team understands and follows Scrum values. They shield the team from external interferences and remove impediments/obstacles. *They do not exert control like a traditional project manager* .
* **Development Team:** A collaborative, self-organizing group of cross-functional professionals responsible for determining how to accomplish the goals set by the Product Owner and building the product .

### 2.2 SCRUM Artifacts

1.  **Product Backlog:** A prioritized, continuously evolving list of work (features, bugs, requirements) for the development team. High-value items are placed at the top .
2.  **Sprint Backlog:** A specific subset of items from the Product Backlog selected by the team to be completed during the current Sprint .
3.  **Burn Down Chart:** A graphical representation of the outstanding work (y-axis) versus time (x-axis). It tracks progress and predicts if the team will complete the sprint goals on time .

> [!TIP]
> **User Stories:** Backlog items are often written as User Stories to express business value in a format understandable to both business and technical stakeholders .
> *Template:* `As a <user role> I want to <goal> So that <benefit>` .

### 2.3 SCRUM Activities (Events)

```mermaid
graph LR
    classDef process fill:#e1f5fe,stroke:#0288d1,stroke-width:2px,color:#01579b;
    classDef meeting fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#e65100;
    classDef artifact fill:#e8f5e9,stroke:#388e3c,stroke-width:2px,color:#1b5e20;

    PB["Product Backlog"]:::artifact --> SP["Sprint Planning"]:::meeting
    SP --> SB["Sprint Backlog"]:::artifact
    SB --> S["Sprint Execution<br/>(1-4 Weeks)"]:::process
    S -.-> DS["Daily Scrum<br/>(15 mins)"]:::meeting
    S --> SR["Sprint Review"]:::meeting
    SR --> SRet["Sprint Retrospective"]:::meeting
    SRet --> INC["Working Increment"]:::artifact
````

- **Sprint Planning:** The Product Owner and Development Team agree on a Sprint Goal and select items from the Product Backlog to form the Sprint Backlog .
    
- **Daily Scrum:** A strict 15-minute daily meeting where team members synchronize activities and discuss: _What did I accomplish yesterday? What will I do today? What are my impediments?_ .
    
- **Sprint Review:** Held near the end of the sprint to inspect the working product Increment and adapt the Product Backlog if necessary. Provides transparent visibility to stakeholders .
    
- **Sprint Retrospective:** The final sprint meeting where the team analyzes their internal workflows, identifies areas for improvement, and makes plans to implement those improvements in the next sprint .
    

## Technical Implementation

Agile teams rely heavily on metrics to track velocity and progress. Below is a self-contained Python script that simulates a **Sprint Burn Down Chart** calculator based on daily task completion data.

```Python
def calculate_burn_down(total_tasks: int, completed_per_day: list) -> None:
    """
    Calculates and displays the daily remaining tasks and actual velocity 
    for an Agile Sprint Burn Down analysis.
    """
    sprint_duration = len(completed_per_day)
    ideal_velocity = total_tasks / sprint_duration
    
    print(f"--- SPRINT BURN DOWN ANALYSIS ---")
    print(f"Total Tasks: {total_tasks}")
    print(f"Sprint Duration: {sprint_duration} Days")
    print(f"Ideal Velocity: {ideal_velocity:.1f} tasks/day\n")
    
    remaining_tasks = total_tasks
    actual_completed = 0
    
    print(f"{'Day':<5} | {'Completed Today':<15} | {'Remaining Tasks':<15}")
    print("-" * 40)
    
    for day, tasks_done in enumerate(completed_per_day, start=1):
        actual_completed += tasks_done
        remaining_tasks -= tasks_done
        # Ensure remaining tasks don't drop below zero logically
        remaining_tasks = max(0, remaining_tasks)
        print(f"Day {day:<3} | {tasks_done:<15} | {remaining_tasks:<15}")
        
    actual_velocity = actual_completed / sprint_duration
    print("\n--- SPRINT SUMMARY ---")
    print(f"Total Completed: {actual_completed}")
    print(f"Actual Average Velocity: {actual_velocity:.1f} tasks/day")
    
    if remaining_tasks == 0:
        print("Status: SUCCESS - Sprint Goals Met.")
    else:
        print(f"Status: WARNING - {remaining_tasks} tasks rolling over to backlog.")

if __name__ == "__main__":
    # Simulated data from the lecture activity (120 tasks over 5 days)
    # Tasks completed each day: Day 1=20, Day 2=50, Day 3=0, Day 4=20, Day 5=30
    sprint_data = [20, 50, 0, 20, 30]
    
    calculate_burn_down(total_tasks=120, completed_per_day=sprint_data)
```

## Active Recall Self-Check

> [!TIP]
> 
> The **Sprint Review** focuses on the _Product_. It inspects the working software increment built during the sprint and gathers feedback from stakeholders to adapt the Product Backlog . The **Sprint Retrospective** focuses on the _Process_. The internal team analyzes how they worked together and identifies workflow improvements for the next sprint .

> [!TIP]
> 
> **Working Software** is valued over comprehensive documentation. Agile focuses on delivering functional product increments quickly rather than spending excessive time writing exhaustive planning and design documents upfront .

> [!TIP]
> 
> _As a_ library member, _I want to_ renew my borrowed books through the online portal, _so that_ I do not have to travel to the physical library to extend my deadline .