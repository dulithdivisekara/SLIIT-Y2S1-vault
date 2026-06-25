---
title: "IT2140 - Conceptual Database Modeling: ER Model"
date: 2026-06-25
module: "IT2140"
author: Dulith Divisekara
institution: SLIIT
tags: [information-technology, database-management, er-diagrams, semester-notes]
---
## Study Tracker
- [ ] Understand the role of Conceptual Database Design in the development lifecycle.
- [ ] Identify and define Entities, Attributes, and their various sub-types.
- [ ] Define Relationships, Cardinality Ratios, and Participation Constraints.
- [ ] Construct comprehensive Entity-Relationship (ER) Diagrams for given scenarios.

## 1. Conceptual Database Design

Following the Requirement Analysis phase, the next critical step is **Conceptual Database Design**. This involves creating a high-level conceptual schema for the database using a semantic data model, most commonly the Entity-Relationship (ER) model. This abstracts the technical complexities, focusing entirely on data structure and business logic.

## 2. Entities and Attributes

### Entities and Entity Sets
* **Entity:** An object in the real world with an independent existence (e.g., a specific student, a specific car). Represented graphically by a **Rectangle**.
* **Entity Set:** A collection of similar entities (e.g., all Students, all Cars). Entity names are typically singular nouns with the first letter capitalized.

### Attributes
An entity is described using a set of properties called **Attributes**, represented graphically by an **Oval**. The **Domain** of an attribute specifies the set of valid, possible values it can hold (e.g., Age must be an integer between 0-120).

#### Types of Attributes
| Attribute Type | Description | ER Notation |
| :--- | :--- | :--- |
| **Composite** | Can be divided into smaller subparts (e.g., `Name` divided into `First_Name`, `Middle_Name`, `Surname`). | Oval branching into sub-ovals. |
| **Multi-valued** | Contains multiple values for a single entity (e.g., `Email` where a student has Gmail, Yahoo, and University accounts). | **Double-lined Oval**. |
| **Derived** | Values are calculated from other attributes and not stored directly (e.g., `Age` calculated from `DOB` and current date). | **Dotted/Dashed Oval**. |

### Keys
* **Candidate Key:** Any minimal set of attributes that uniquely identifies an entity within its entity set.
* **Primary Key:** The single Candidate Key chosen by the database designer to uniquely identify records. Represented by an **Underlined** attribute name.
* **Composite Key:** A Primary Key made up of two or more attributes combined (e.g., `StudentID` + `UnitID`).
* **Super Key:** Any set of attributes that uniquely identifies a tuple. *Note: A primary key is a minimal super key.*

## 3. Weak Entities

A **Weak Entity** cannot exist independently; its existence depends entirely on another entity (the Owner Entity). 
* It lacks its own primary key and instead inherits part of its key from the owner entity. 
* The attributes it uses to distinguish itself among other weak entities of the same owner are called **Partial Keys** (represented by a dashed underline).
* Represented graphically by a **Double-lined Rectangle**.
* The relationship connecting the weak entity to its owner is called an **Identifying Relationship** (represented by a **Double-lined Diamond**).


## 4. Relationships and Constraints

A **Relationship** is an association among two or more entities, represented by a **Diamond**. The number of participating entities defines the **Degree** of the relationship (e.g., Binary = 2, Ternary = 3). 

Relationships can also possess **Descriptive Attributes** to record information about the association itself (e.g., a `startDate` attribute on a `Works_In` relationship between `Employee` and `Department`).

### Cardinality Ratios
Specifies the maximum number of relationship instances an entity can participate in.
* **One-to-One (1:1):** E.g., An Employee manages at most one Department; a Department has at most one Manager.
* **One-to-Many (1:N):** E.g., An Employee works in one Department; a Department has many Employees.
* **Many-to-Many (N:M):** E.g., An Employee works on many Projects; a Project has many Employees.

### Participation Constraints
Specifies whether the existence of an entity depends on it being related to another entity.
* **Total Participation (Mandatory):** The entity *must* participate in the relationship. Represented by a **Double Line** connecting the entity to the relationship diamond. (e.g., A Student *must* register for a Degree).
* **Partial Participation (Optional):** The entity *may or may not* participate. Represented by a **Single Line**.

### Recursive Relationships
A relationship where an entity type interacts with itself. It is crucial to label the lines to specify the role each entity plays in the relationship.
* *Example:* An `Employee` manages other `Employees`. The entity is `Employee`, the relationship is `Manages`. The roles are "Manager" (1) and "Subordinate" (N).

## Active Recall Self-Check

<details>
<summary>How does a Composite Attribute differ from a Multi-valued Attribute?</summary>

> [!TIP]
> A composite attribute can be broken down into distinct subparts that form a single logical unit (like an Address broken into Street, City, Zip Code). A multi-valued attribute contains a list or set of independent values of the same type for a single entity (like a person having three different phone numbers).
</details>

<details>
<summary>If a company deletes an `Employee` record, what happens to the associated `Dependent` records if `Dependent` is modeled as a Weak Entity?</summary>

> [!TIP]
> Because a Weak Entity's existence depends entirely on its Owner Entity, deleting the `Employee` (the owner) will result in the deletion of all associated `Dependent` records, as they cannot exist independently in the database.
</details>

<details>
<summary>In an ER diagram, what visual notation distinguishes Total Participation from Partial Participation?</summary>

> [!TIP]
> Total Participation (mandatory involvement) is indicated by a double line connecting the entity rectangle to the relationship diamond. Partial Participation (optional involvement) is indicated by a standard single line.
</details>