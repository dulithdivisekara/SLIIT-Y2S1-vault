---
title: "IT2030 - Lecture 4: Use Case Diagrams"
date: 2026-06-25
module: "IT2030"
author: Dulith Divisekara
institution: SLIIT
tags: [information-technology, software-engineering, uml-diagrams, semester-notes]
---
## Study Tracker
- [ ] Understand the purpose of Use Case Diagrams in capturing functional requirements.
- [ ] Identify and define the four core components: System Boundary, Actors, Use Cases, and Relationships.
- [ ] Construct Use Case Diagrams utilizing specific relationships (Association, Generalization, Include, Extend).
- [ ] Formulate detailed Use Case Scenarios (Textual Specifications).

## 1. Overview of Use Case Diagrams

A Use Case Diagram provides a high-level graphical representation of a system's proposed functionality . It acts as a powerful communication tool between developers and non-technical stakeholders (like management and clients) because it illustrates *WHAT* a system will do without getting bogged down in the technical details of *HOW* it will accomplish it .

## 2. Core Components


A valid Use Case Diagram is built using four foundational components :

1. **System Boundary:** Represented by a rectangle. It defines the physical or logical scope of the system being built. Use cases are placed inside the boundary; actors are placed outside .
2. **Actors:** Represented by stick figures. An actor is an external entity that interacts directly with the system. Actors can be human users, external hardware devices, or other software systems .
3. **Use Cases:** Represented by an oval. A use case is a high-level unit of behavioral interaction between an actor and the system. It should be named using a verb-noun phrase (e.g., `Reserve a Room`) .
4. **Relationships:** Represented by various connecting lines/arrows that define how actors and use cases interact and depend on one another .

## 3. Types of Relationships

Understanding how to link components is the most critical part of constructing accurate Use Case Diagrams.

### 3.1 Actor Relationships
* **Association:** A solid line connecting an Actor to a Use Case, indicating that the actor participates in or initiates that specific use case .
* **Actor Generalization:** Drawn with a solid line ending in a hollow triangle pointing to the "Parent" actor. Used when a "Child" actor inherits all characteristics and access rights of a parent, but can also have unique access to other specific use cases .

### 3.2 Use Case to Use Case Relationships

| Relationship Type | UML Notation | Description |
| :--- | :--- | :--- |
| **`<<include>>`** | Dashed arrow pointing *toward* the included use case. | The Base use case explicitly incorporates the behavior of another use case. The included use case is a **mandatory** sub-routine that never stands alone (e.g., `Withdraw Money` strictly `<<includes>>` `Validate User`) . |
| **`<<extend>>`** | Dashed arrow pointing *toward* the Base use case. | The Base use case stands alone, but under certain specific conditions, its behavior is modified or extended by an **optional** use case. A condition must be met for this to trigger (e.g., `Enroll in Uni` is `<<extended>>` by `Perform Visa Check` *only if* the student is foreign) . |
| **Generalization** | Solid line with hollow triangle pointing *toward* the Parent use case. | A Child use case inherits the fundamental behavior of a general Parent use case but adds specific implementation details (e.g., `Pay via PayPal` is a generalized child of `Make Payment`) . |

## 4. Use Case Scenarios (Textual Specifications)

While the diagram provides a visual overview, a **Use Case Scenario** provides the formal, step-by-step textual description of the event flow during execution .

A standard template includes:
* **Pre-conditions:** What must be true *before* the use case executes (e.g., User has logged into the ATM) .
* **Post-conditions:** What will be true *after* successful execution .
* **Main Scenario (Happy Path):** The idealized, successful step-by-step sequence of events assuming no errors occur .
* **Extensions (Alternative Flows):** Describes what happens when variations or errors arise during the main scenario (e.g., "Step 5a: System notifies user of insufficient funds") .

```json
{
  "Use Case ID": "UC-001",
  "Name": "Withdraw Money",
  "Primary Actor": "Bank Customer",
  "Pre-conditions": "User has securely logged into the ATM.",
  "Post-conditions": "User account is debited, and cash is dispensed.",
  "Main Scenario": [
    "1. System asks for amount to withdraw.",
    "2. User enters amount.",
    "3. System debits user's account.",
    "4. System dispenses money and receipt."
  ],
  "Extensions": {
    "3a": "If funds are insufficient, System displays error and ejects card."
  }
}
````

_(Example of a structured Use Case Specification)_

## Active Recall Self-Check

> [!TIP]
> 
> The System Boundary rectangle represents the absolute limits of the physical or logical software system. Actors are, by definition, _external_ entities (humans, hardware, or third-party APIs) that interact with the system from the outside. Therefore, an Actor must always be drawn outside the boundary box .

> [!TIP]
> 
> An `<<include>>` relationship means the Base use case is _dependent_ on the included use case; it cannot finish its execution without running that mandatory sub-routine . Conversely, an `<<extend>>` relationship means the Base use case is completely independent; it can execute successfully on its own, and the extended use case is purely an optional addition triggered only under specific conditions .

> [!TIP]
> 
> You can create a parent Actor called `User` that is associated with the generic `Login` and `Update Profile` use cases. Then, draw Generalization arrows (solid line, hollow triangle) from both `Student` and `Teacher` pointing to `User`. They will inherit those generic abilities while retaining their own specific, independent use cases .