---
title: "IT2140 - Introduction to DBMS, Database Design Process and Data-Intensive Applications"
date: 2026-06-25
module: "IT2140"
author: Dulith Divisekara
institution: SLIIT
tags: [information-technology, database-management, system-design, semester-notes]
---
## Study Tracker
- [ ] Differentiate between Data and Information, and define a DBMS.
- [ ] Identify the components of a Database System Environment and its advantages.
- [ ] Outline the 6-step Database Design Process.
- [ ] Understand Cloud Database Hosting and Data-Intensive Applications.

## 1. Fundamentals of Databases

To understand database systems, it is essential to distinguish between raw inputs and processed outputs:
* **Data:** Raw, unprocessed facts or values (numbers, text, symbols) that do not carry standalone meaning .
* **Information:** Data that has been processed, organized, or structured into a meaningful context .

> [!NOTE]
> A **Database** is a collection of related data . A **Database Management System (DBMS)** is specialized software that facilitates defining, constructing, manipulating, and sharing databases among various users and applications .

## 2. Database System Environment & Advantages

A complete Database System Environment consists of five major components: **Hardware, Software, People, Procedures, and Data** .

Transitioning from traditional File Processing Systems to a Database Approach provides several critical advantages:

| Advantage | Description |
| :--- | :--- |
| **Data Independence** | Application programs are insulated from how data is physically structured and stored . |
| **Efficient Access** | Utilizes sophisticated indexing and storage techniques for rapid retrieval . |
| **Data Integrity** | Enforces defined constraints (e.g., ensuring a 'Name' field only accepts strings) . |
| **Concurrency & Security** | Allows simultaneous access by multiple users while restricting unauthorized entry . |
| **Backup & Recovery** | Protects the system and users from catastrophic hardware or software failures . |

## 3. The Database Design Process


A poorly designed database results in redundant data, slow query response times, and inconsistent results . A rigorous 6-step process is required for enterprise-grade systems:

1. **Requirement Collection and Analysis:** Gathering data requirements, identifying objects/relationships, and determining constraints through interviews, existing documentation, and system reviews .
2. **Conceptual Database Design:** Translating requirements into a high-level description or Semantic Data Model (e.g., Entity-Relationship Model) .
3. **Logical Database Design:** Selecting a specific DBMS and converting the conceptual schema into a compatible data model (e.g., Relational Data Model) .
4. **Schema Refinement:** Normalizing the logical schema to eliminate redundancies and potential update anomalies .
5. **Physical Database Design:** Defining physical storage structures and creating indexes to meet performance criteria .
6. **Security Design:** Defining user groups, roles, and access levels (e.g., read-only vs. update permissions) .

```mermaid
graph TD
    classDef process fill:#e1f5fe,stroke:#0288d1,stroke-width:2px,color:#01579b;
    classDef refine fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#e65100;
    classDef deploy fill:#e8f5e9,stroke:#388e3c,stroke-width:2px,color:#1b5e20;

    R[1. Requirement Analysis]:::process --> C[2. Conceptual Design]:::process
    C --> L[3. Logical Design]:::process
    L --> SR[4. Schema Refinement]:::refine
    SR --> P[5. Physical Design]:::deploy
    P --> S[6. Security Design]:::deploy
````

## 4. Hosting & Data-Intensive Applications

### Database Hosting Models

Database deployment relies on balancing scalability, cost, security, and performance .

- **On-Premise:** Infrastructure is hosted locally.
    
- **Cloud Database:** Hosted on remote servers. Can be **Self-managed** (IaaS) or **DBaaS** (Fully Managed Database-as-a-Service) . Examples include AWS RDS (Relational) and MongoDB Atlas (NoSQL) .
    

### Data-Intensive Applications

Data-intensive applications prioritize handling massive volumes of data over complex CPU computations .

> [!IMPORTANT]
> 
> These applications require fast, consistent access and extreme scalability to handle heavy user loads (e.g., Facebook, Amazon, Online Banking) . A robust DBMS is critical here for indexing, concurrency control, and query optimization .

## Technical Implementation

Below is a self-contained Python script demonstrating how a programmatic application interacts with a DBMS to enforce the concepts of Logical Design, Data Integrity, and Data Retrieval.


```Python
import sqlite3

def setup_and_query_database():
    """
    Demonstrates creating a schema, enforcing data integrity, 
    and querying a local SQLite database.
    """
    # Connect to an in-memory DBMS instance
    connection = sqlite3.connect(":memory:")
    cursor = connection.cursor()

    # Step 3 & 5: Logical & Physical Design (Schema & Types)
    # Enforcing integrity constraints (NOT NULL, UNIQUE)
    cursor.execute("""
        CREATE TABLE Students (
            StudentID INTEGER PRIMARY KEY AUTOINCREMENT,
            Name TEXT NOT NULL,
            Course TEXT NOT NULL,
            Grade INTEGER CHECK(Grade >= 0 AND Grade <= 100)
        )
    """)

    # Insert raw data into the structured schema
    sample_data = [
        ("John", "IT101", 85),
        ("Alice", "IT101", 92),
        ("Bob", "IT2140", 78)
    ]
    cursor.executemany("INSERT INTO Students (Name, Course, Grade) VALUES (?, ?, ?)", sample_data)
    connection.commit()

    # Step: Data Retrieval / Query Processing
    print("Executing Query: Retrieve students with Grade > 80")
    cursor.execute("SELECT Name, Course, Grade FROM Students WHERE Grade > 80")
    
    results = cursor.fetchall()
    for row in results:
        # Transforming raw stored data into formatted Information
        print(f"[Information] Student {row[0]} scored {row[2]} in {row[1]}.")

    connection.close()

if __name__ == "__main__":
    setup_and_query_database()
```

## Active Recall Self-Check

> [!TIP]
> 
> Conceptual Design creates a high-level description of data and relationships independent of any specific technology (e.g., an ER Model) . Logical Design maps that conceptual model to a specific structural paradigm, such as the Relational Data Model, preparing it for a specific DBMS .

> [!TIP]
> 
> Data-intensive apps (like social media or banking) process massive volumes of data rather than heavy computation . They rely on a DBMS to provide efficient storage, fast indexing, concurrent user transaction handling, and backup/recovery mechanisms .

> [!TIP]
> 
> Schema Refinement occurs after Logical Design. It involves analyzing the proposed schema to identify and eliminate potential problems, primarily structural redundancies and update anomalies, often through normalization techniques .