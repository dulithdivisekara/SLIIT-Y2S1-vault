---
title: "IT2140 - ER and EER to Relational Model Mapping"
date: 2026-06-25
module: "IT2140"
author: Dulith Divisekara
institution: SLIIT
tags: [information-technology, relational-mapping, eer-diagrams, semester-notes]
---
## Study Tracker
- [ ] Understand the core components of the Relational Model.
- [ ] Map standard Entities, Attributes, and N-ary relationships to relational schemas.
- [ ] Evaluate and apply the four ISA (Superclass/Subclass) mapping options based on constraints.

## 1. The Relational Model

The relational model, introduced by Codd in 1970, is the foundation of modern Database Management Systems (DBMS) due to its simplicity in data representation . 

> [!NOTE]
> A relational database is a collection of relations with distinct names. A relation consists of two parts: a **Relational Schema** (the structure/columns) and a **Relational Instance** (the actual data/rows) .

* **Relational Schema:** Describes the relation's name, the name of each field, and the domain of each field (e.g., `Students(sid: string, name: string, age: integer)`) .
* **Relational Instance:** A set of tuples (records or rows) where each tuple contains the same number of fields defined by the schema .

### Example: Relational Instance of "Students"

| sid | name | login | age | gpa |
| :--- | :--- | :--- | :--- | :--- |
| 53831 | Madayan | madayan@music | 11 | 1.8 |
| 53832 | Guldu | guldu@music | 12 | 2.0 |
| 53688 | Smith | smith@ee | 18 | 3.2 |

## 2. Mapping Entities and N-ary Relationships

When mapping standard entities, the entity becomes a table, the attributes become columns, and the key attribute becomes the Primary Key.


When dealing with an **N-ary Relationship** (a relationship connecting three or more entities), it cannot be mapped by simply adding a foreign key to one of the entities.
* **The Mapping Rule:** An N-ary relationship is mapped into a brand new "Relationship" relation (table) .
* **The Keys:** The primary keys of all participating entities are added as Foreign Keys into this new relation. Collectively, these foreign keys form the composite Primary Key of the new relation .

## 3. Mapping ISA (Superclass/Subclass) Relationships

When converting Extended ER (EER) models containing specialization/generalization (ISA relationships), database designers must choose one of four mapping options depending on the business constraints (Disjoint vs. Overlapping, Total vs. Partial) .

> [!IMPORTANT]
> The choice of mapping strategy dictates the database's performance and data integrity rules. 

| Mapping Strategy | Architecture | Constraint Suitability | Pros / Cons |
| :--- | :--- | :--- | :--- |
| **Option 1: Multi-Relation** | Create a table for the Superclass and separate tables for each Subclass. The Superclass PK is used as the PK and FK in the Subclass tables . | **Universal** (Works for disjoint, overlapping, total, and partial) . | Highly flexible, but requires complex `JOIN` queries to retrieve full data. |
| **Option 2: Multi-Relation** | Create separate tables *only* for the Subclasses. Each subclass table inherits all attributes of the superclass . | **Total Participation ONLY** (Subclasses must completely cover the superclass) . | Eliminates joins, but cannot store entities that don't belong to a specific subclass. |
| **Option 3: Single-Relation** | Create a single table containing all attributes from the superclass and all subclasses, plus a new `type` column . | **Disjoint ONLY** (An entity can only belong to one subclass) . | Fast retrieval (no joins), but creates many `NULL` values for irrelevant subclass attributes. |
| **Option 4: Single-Relation** | Create a single table containing all attributes, plus multiple **Boolean** columns to indicate subclass membership (e.g., `is_student`, `is_faculty`) . | **Overlapping** (An entity can belong to multiple subclasses) . | Supports overlapping roles cleanly, but suffers from the same `NULL` value bloat as Option 3. |

```mermaid
graph TD
    classDef super fill:#e1f5fe,stroke:#0288d1,stroke-width:2px,color:#01579b;
    classDef sub fill:#e8f5e9,stroke:#388e3c,stroke-width:2px,color:#1b5e20;
    classDef link fill:#f5f5f5,stroke:#9e9e9e,stroke-width:2px,color:#616161;

    P["Person Table<br/>PK: NIC"]:::super --- FK1(("FK: NIC")):::link
    P --- FK2(("FK: NIC")):::link
    FK1 --- S["Student Table<br/>PK: NIC<br/>Attributes: GPA"]:::sub
    FK2 --- F["Faculty Table<br/>PK: NIC<br/>Attributes: Salary"]:::sub
````

_(Diagram illustrating the architecture of Option 1: Multi-Relation mapping)_

## Technical Implementation

Below is a self-contained Python script demonstrating how to programmatically implement **Option 3 (Single-Relation with a Type column)** and **Option 1 (Multi-Relation)** using SQLite.


```Python
import sqlite3

def demonstrate_isa_mapping():
    """
    Demonstrates the SQL DDL implementation of ISA Mapping Options.
    """
    conn = sqlite3.connect(":memory:")
    cursor = conn.cursor()

    print("--- Implementing Option 3 (Single Table, Disjoint) ---")
    # A single table handles all attributes. 'role_type' dictates the subclass.
    cursor.execute("""
        CREATE TABLE Person_Single (
            NIC TEXT PRIMARY KEY,
            name TEXT NOT NULL,
            age INTEGER,
            gpa REAL,        -- Nullable if not a student
            salary REAL,     -- Nullable if not a faculty member
            role_type TEXT CHECK(role_type IN ('STUDENT', 'FACULTY', 'OTHER'))
        )
    """)
    
    # Inserting a disjoint student
    cursor.execute("INSERT INTO Person_Single (NIC, name, age, gpa, role_type) VALUES ('123V', 'Alice', 20, 3.8, 'STUDENT')")
    print("Successfully inserted disjoint entity into Single-Relation table.")

    print("\n--- Implementing Option 1 (Multi-Relation, Universal) ---")
    # Superclass Table
    cursor.execute("""
        CREATE TABLE Person_Super (
            NIC TEXT PRIMARY KEY,
            name TEXT NOT NULL,
            age INTEGER
        )
    """)
    
    # Subclass Table 1
    cursor.execute("""
        CREATE TABLE Student_Sub (
            NIC TEXT PRIMARY KEY,
            gpa REAL,
            FOREIGN KEY (NIC) REFERENCES Person_Super(NIC)
        )
    """)
    
    # Subclass Table 2
    cursor.execute("""
        CREATE TABLE Faculty_Sub (
            NIC TEXT PRIMARY KEY,
            salary REAL,
            FOREIGN KEY (NIC) REFERENCES Person_Super(NIC)
        )
    """)

    # Inserting an overlapping entity (Both Student and Faculty)
    nic = "999V"
    cursor.execute("INSERT INTO Person_Super (NIC, name, age) VALUES (?, ?, ?)", (nic, "Bob", 25))
    cursor.execute("INSERT INTO Student_Sub (NIC, gpa) VALUES (?, ?)", (nic, 3.9))
    cursor.execute("INSERT INTO Faculty_Sub (NIC, salary) VALUES (?, ?)", (nic, 50000))
    
    # Querying the overlapping data using JOINs
    cursor.execute("""
        SELECT p.name, s.gpa, f.salary 
        FROM Person_Super p
        JOIN Student_Sub s ON p.NIC = s.NIC
        JOIN Faculty_Sub f ON p.NIC = f.NIC
    """)
    row = cursor.fetchone()
    print(f"Successfully retrieved Overlapping Entity via JOIN: Name={row[0]}, GPA={row[1]}, Salary=${row[2]}")

    conn.close()

if __name__ == "__main__":
    demonstrate_isa_mapping()
```

## Active Recall Self-Check

> [!TIP]
> 
> **Option 2** (Creating separate relations for all subclasses only) is optimized for this scenario. Because the participation is **Total**, there are no generic "superclass-only" entities, meaning a dedicated superclass table is unnecessary and would just cause redundant joins .

> [!TIP]
> 
> Option 3 uses a single column (e.g., `type`) to store the subclass identifier. If an entity were overlapping (e.g., both a Student AND a Faculty member), a single `type` column could not accurately store both states simultaneously without violating atomic data principles. For overlapping data in a single table, Option 4 (multiple Boolean columns) must be used .

> [!TIP]
> 
> It is converted by creating a completely new Relation (table). The Primary Keys of all the participating entities are inserted into this new relation as Foreign Keys. Collectively, these Foreign Keys combine to act as the Composite Primary Key for the new relationship table .