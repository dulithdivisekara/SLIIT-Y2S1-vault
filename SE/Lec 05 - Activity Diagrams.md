---
title: "IT2030 - Lecture 5: Activity Diagrams"
date: 2026-06-25
module: "IT2030"
author: Dulith Divisekara
institution: SLIIT
tags: [information-technology, software-engineering, uml-diagrams, semester-notes]
---
## Study Tracker
- [ ] Define the purpose of Activity Diagrams in modeling workflows and system behavior.
- [ ] Identify core elements: Actions, Transitions, Decision/Merge nodes, and Forks/Joins.
- [ ] Distinguish between conditional branching (Decision/Merge) and parallel processing (Fork/Join).
- [ ] Apply Swimlanes (Partitioning) to assign responsibilities to organizational units or actors.

## 1. Overview and Purpose of Activity Diagrams

An Activity Diagram is a unified modeling language (UML) diagram used to model the dynamic workflows, dependencies, and sequential/concurrent activities of a system .


They are primarily utilized during the analysis and design phases for:
* Modeling business processes and operational workflows .
* Analyzing system functionality to identify and detail use cases .
* Clarifying complex concurrency and parallel processing issues .

## 2. Core Notational Elements

An Activity Diagram consists of discrete actions connected by control flows (transitions).

> [!NOTE]
> An **Action** represents a single, atomic task (e.g., `Validate Membership`), while an **Activity** is a sequence or grouping of multiple actions .

* **Initial (Start) Node:** Represented by a filled black circle. Marks the entry point of the workflow .
* **Activity Final (End) Node:** Represented by a filled circle nested within a hollow circle. Signifies the termination of the entire activity .
* **Action Node:** Represented by a rounded rectangle containing a verb phrase in the present tense .
* **Transition (Control Flow):** A directed arrow connecting nodes, showing the path of execution token flow .
* **Call Action (Sub-Activity):** Represented by a standard action node containing a "rake-style" symbol. This indicates that the action encapsulates a more detailed sub-workflow defined in a separate activity diagram .

## 3. Control Flow Mechanics: Branching vs. Concurrency

Properly distinguishing between conditional logic and parallel execution is critical. A common modeling mistake is confusing Merge nodes with Join nodes .

| Flow Type | Split Node | Convergence Node | Mechanism |
| :--- | :--- | :--- | :--- |
| **Conditional Branching** | **Decision Node** (Diamond) | **Merge Node** (Diamond) | Control flow follows *only one* path based on mutually exclusive Guard Conditions (e.g., `[Valid Member]`, `[Else]`) . |
| **Parallel Processing** | **Fork Node** (Thick Bar) | **Join Node** (Thick Bar) | Control flow splits into multiple concurrent threads that execute simultaneously. All threads must arrive at the Join Node before execution proceeds . |

> [!WARNING]
> **Looping/Iteration:** While an asterisk (`*`) inside an action node indicates iteration, it is best practice to explicitly model loops using Decision Nodes and Guard Conditions to clearly define termination criteria .

```mermaid
graph TD
    classDef init fill:#333,stroke:#333,stroke-width:2px,color:#fff;
    classDef node fill:#e1f5fe,stroke:#0288d1,stroke-width:2px,color:#01579b;
    classDef sync fill:#000,stroke:#000,stroke-width:4px,color:#fff;
    classDef cond fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#e65100;

    Start(( )):::init --> Receive["Validate Membership"]:::node
    Receive --> Dec{" "}:::cond
    
    Dec -->|"[Else]"| Cancel["Cancel Order"]:::node
    Dec -->|"[Valid Member]"| Fork[" "]:::sync
    
    Cancel --> End(( )):::init
    
    Fork --> Process["Take Payment"]:::node
    Fork --> Pack["Issue DVD"]:::node
    
    Process --> Join[" "]:::sync
    Pack --> Join
    
    Join --> Merge{" "}:::cond
    Merge --> End
````

## 4. Activity Partitioning (Swimlanes)

To add organizational context to the behavioral model, Activity Diagrams utilize **Partitions**, commonly referred to as **Swimlanes** .

- Swimlanes group actions that share a common characteristic, typically representing a specific organizational unit (e.g., `Order Dept`) or a business actor (e.g., `Customer`) .
    
- They are drawn using parallel horizontal or vertical lines .
    
- **Sub-Partitioning:** Swimlanes can be nested to show hierarchy (e.g., an `Order Dept` sub-partition within a larger `Department` partition) .
    

## Technical Implementation

Below is a self-contained Python script simulating the fundamental control flow mechanics of an Activity Diagram. It utilizes standard `if/else` logic to represent **Decision/Merge** nodes and the `threading` library to represent **Fork/Join** parallel synchronization bars.

```Python
import threading
import time
import random

def execute_action(action_name: str, delay: float):
    """Simulates the execution of a UML Action Node."""
    print(f"[{threading.current_thread().name}] Action Started: {action_name}")
    time.sleep(delay)
    print(f"[{threading.current_thread().name}] Action Completed: {action_name}")

def parallel_payment_task():
    # Belongs to the Finance Swimlane
    execute_action("Take Payment", random.uniform(0.5, 1.0))

def parallel_inventory_task():
    # Belongs to the Inventory Swimlane
    execute_action("Issue DVD", random.uniform(0.5, 1.0))

def simulate_activity_diagram():
    print("--- (Initial Node) Workflow Started ---")
    
    # Sequential Action
    execute_action("Validate Membership", 0.5)
    
    # 1. Decision Node (Diamond)
    is_valid_member = True # Guard Condition Evaluation
    print(f"\n--- (Decision Node) Evaluated [Valid Member: {is_valid_member}] ---")
    
    if not is_valid_member:
        # [Else] Transition Path
        execute_action("Cancel Order", 0.2)
    else:
        # [Valid Member] Transition Path
        print("\n--- (Fork Node) Splitting into Parallel Concurrent Flows ---")
        
        # Spawning parallel threads to simulate Forking
        t1 = threading.Thread(target=parallel_payment_task, name="Finance_Swimlane")
        t2 = threading.Thread(target=parallel_inventory_task, name="Inventory_Swimlane")
        
        t1.start()
        t2.start()
        
        # 2. Join Node (Synchronization Bar)
        # The main thread blocks until both parallel threads complete
        t1.join()
        t2.join()
        print("--- (Join Node) All Parallel Flows Merged ---")
        
        # Merge Node Convergence (Implicit in code structure)
        execute_action("Update DVD Status", 0.3)

    print("\n--- (Activity Final Node) Workflow Terminated ---")

if __name__ == "__main__":
    simulate_activity_diagram()
```

## Active Recall Self-Check

> [!TIP]
> 
> The rake symbol indicates that the action is a **Call Action** or **Sub-Activity**. This means the specific action is complex and its internal workflow is defined in detail on a completely separate activity diagram .

> [!TIP]
> 
> When a token hits a **Decision Node**, it evaluates the guard conditions and travels down _only one_ single valid path. When a token hits a **Fork Node**, it splits, and execution flows down _all_ connected parallel paths simultaneously .

> [!TIP]
> 
> A diamond Merge Node is exclusively used to bring divergent, mutually exclusive conditional paths back together. Concurrent parallel paths generated by a Fork Node must be synchronized and merged using a **Join Node** (a thick bar), which waits for all threads to finish before proceeding .