---
title: "IT2011 - Expert Systems and Ontologies"
date: 2026-06-25
module: "IT2011"
author: Dulith Divisekara
institution: SLIIT
tags: [information-technology, expert-systems, ontologies, semester-notes]
---
## Study Tracker
- [ ] Understand the Data, Information, and Knowledge hierarchy.
- [ ] Identify the core architectural components of an Expert System.
- [ ] Differentiate between Forward Chaining and Backward Chaining inference mechanisms.
- [ ] Define Ontologies and their structural components.
- [ ] Contrast traditional Expert Systems with Modern AI techniques.

## 1. The Knowledge Hierarchy and Expert Systems

An Expert System is a computer program engineered to simulate the decision-making abilities of a human expert within a specialized domain (e.g., medicine or finance) . It operates using a set of explicit "if-then" rules rather than independent cognitive thought .

To understand how these systems process inputs, it is necessary to differentiate the levels of abstraction:
* **Data:** Raw, unprocessed facts (e.g., "98.6") .
* **Information:** Organized and structured data (e.g., "Temperature = 98.6°F") .
* **Knowledge:** Meaningful insights derived from information (e.g., "98.6°F is a normal body temperature") .

Unlike conventional computer systems where data and control logic are stored together, an expert system separates the control mechanism from the encoded knowledge and possesses the capability to explain its conclusions .

## 2. Architecture of an Expert System

> [!IMPORTANT]
> The performance of an Expert System relies on separating the raw domain knowledge from the reasoning algorithms that process it.

### Key Components
1.  **Knowledge Base:** The repository storing declarative facts (e.g., symptoms) and procedural rules (if-then statements) collected from domain experts .
2.  **Inference Engine:** The logical processing unit that applies the stored rules to the known facts to deduce new conclusions .
3.  **Working Memory / Fact Base:** A temporary memory bank that stores the current facts and user inputs during a specific reasoning session .
4.  **Explanation Facility:** A transparency module that explains how and why the system reached a specific conclusion, building user trust .
5.  **Interfaces:** The User Interface allows standard interaction, while the Developer (Knowledge Acquisition) Interface enables engineers to update the underlying rules .

```mermaid
graph TD
    classDef core fill:#e1f5fe,stroke:#0288d1,stroke-width:2px,color:#01579b;
    classDef process fill:#e8f5e9,stroke:#388e3c,stroke-width:2px,color:#1b5e20;
    classDef external fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#e65100;

    KB[Knowledge Base]:::core <--> IE[Inference Engine]:::process
    DB[Working Memory / Facts]:::core <--> IE
    IE <--> EF[Explanation Facility]:::process
    EF <--> UI[User Interface]:::external
    UI <--> U((User)):::external
    KB <--> DI[Developer Interface]:::external
````

## 3. Reasoning Mechanisms: Forward vs. Backward Chaining

The Inference Engine operates using two primary logical mechanisms .

|**Feature**|**Forward Chaining**|**Backward Chaining**|
|---|---|---|
|**Concept / Start Point**|Starts with known facts (data) .|Starts with a goal or hypothesis .|
|**Direction**|Moves toward conclusions (Goal) .|Moves toward supporting facts to verify the goal .|
|**Use Case**|Monitoring and control systems .|Diagnostic and expert advice systems .|
|**Execution Example**|Input symptoms → infer disease .|Assume disease → check if symptoms match .|

## 4. Ontologies in Artificial Intelligence

> [!NOTE]
> 
> An ontology is defined as "A formal, explicit specification of a shared conceptualization" (Gruber, 1993) . It structures knowledge so that machines can understand relationships and reason with semantic interoperability .

### Ontological Components

|**Element**|**Description**|**Example**|
|---|---|---|
|**Class**|A broad group or category of entities .|Student, Course .|
|**Instance**|A specific member belonging to a class .|John (Student) .|
|**Property**|A feature or attribute defining a class .|hasName, hasAge .|
|**Relationship**|The logical link between classes or instances .|enrolledIn (Student → Course) .|
|**Axiom**|A logical rule that must remain universally true .|"A course must have at least 1 instructor" .|

## 5. Limitations and the Shift to Modern AI

Expert Systems and Ontologies are highly structured but suffer from severe limitations. They require manual creation by domain experts, fail to adapt to evolving data, break when rules conflict, and completely lack the capacity to learn from experience . Furthermore, they cannot process unstructured, noisy real-world data like images or audio .

Consequently, the field has transitioned to data-driven Machine Learning (ML), unstructured-capable Deep Learning (DL), and Reinforcement Learning (RL) .

## Technical Implementation

Below is a self-contained Python script simulating a basic Forward Chaining Inference Engine. The system accepts initial facts (data) and evaluates them against a hardcoded knowledge base to reach a diagnostic goal.

```Python
class ForwardChainingEngine:
    def __init__(self):
        # The Knowledge Base containing procedural rules
        self.rules = [
            {"conditions": {"fever", "headache", "runny nose"}, "conclusion": "Flu"},
            {"conditions": {"fever", "cough"}, "conclusion": "Respiratory Infection"},
            {"conditions": {"headache", "nausea"}, "conclusion": "Migraine"}
        ]
        # Working memory to store known facts
        self.working_memory = set()

    def add_facts(self, facts: list):
        """Adds observed facts into the working memory."""
        for fact in facts:
            self.working_memory.add(fact)

    def infer(self) -> str:
        """Applies forward chaining to reach a conclusion based on working memory."""
        for rule in self.rules:
            # Check if all conditions for a rule exist in the working memory
            if rule["conditions"].issubset(self.working_memory):
                return f"Diagnosis Reached: {rule['conclusion']}"
        
        return "Insufficient data to reach a valid conclusion."

if __name__ == "__main__":
    # Initialize the Expert System
    expert_system = ForwardChainingEngine()
    
    # Simulate the User Interface taking inputs
    patient_symptoms = ["fever", "headache", "runny nose", "fatigue"]
    expert_system.add_facts(patient_symptoms)
    
    print(f"Working Memory (Facts): {expert_system.working_memory}")
    
    # Trigger the Inference Engine
    result = expert_system.infer()
    print(f"Inference Engine Output: {result}")
```

## Active Recall Self-Check

> [!TIP]
> 
> Declarative knowledge consists of static facts (e.g., "Fever is a body temperature over 100.4°F"), whereas procedural knowledge consists of logical rules guiding action (e.g., "IF fever AND sore throat THEN possible flu") .

> [!TIP]
> 
> The explanation facility provides transparency by explaining exactly how and why a particular conclusion was reached (e.g., justifying a diagnosis based on specific inputted symptoms). This transparency builds user trust in the system .

> [!TIP]
> 
> They are strictly rule-based and rely on explicit, well-structured logic. They cannot handle unstructured, ambiguous, or noisy data (like pixels or audio waves) and lack the ability to learn or adapt from experience, which is necessary for complex perception tasks .