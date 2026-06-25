---
title: "IT2140 - Database Design and Development: SQL Joins"
date: 2026-06-25
module: IT2140
author: Dulith Divisekara
institution: SLIIT
tags:
  - information-technology
  - sql
  - database-management
  - semester-notes
---
## Study Tracker
- [ ] Understand the fundamental concept of SQL Joins and Primary Key-Foreign Key relationships.
- [ ] Construct `INNER JOIN` statements to retrieve intersecting data.
- [ ] Apply filtering conditions (`WHERE`) alongside join operations.
- [ ] Differentiate and implement `LEFT OUTER JOIN` and `RIGHT OUTER JOIN` to handle unmatched records.

## 1. Introduction to Relational Joins

In relational database management systems (RDBMS), data is normalized and distributed across multiple tables. Joins are utilized to combine data from two or more tables based on a related column, which is typically governed by a Primary Key-Foreign Key relationship . 

> [!IMPORTANT]
> A join acts as a bridge, allowing you to query attributes from disparate tables as if they existed in a single, unified table, provided there is a logical relational link between them.

## 2. Inner Joins

An `INNER JOIN` (or simply `JOIN`) returns only the rows where there is a match in *both* tables based on the specified join condition .

You can write an inner join using either implicit syntax (using the `WHERE` clause to link tables) or explicit syntax (using the `INNER JOIN ... ON` keywords) . Explicit syntax is the modern standard.

Furthermore, you can chain additional conditions using the `WHERE` clause to filter the joined result set .

### Syntax Comparison

| Syntax Type | SQL Structure |
| :--- | :--- |
| **Implicit Join** | `SELECT s.Name, p.OfferBy FROM Student s, Program p WHERE s.pid = p.progId;` |
| **Explicit Join** | `SELECT s.Name, p.OfferBy FROM Student s INNER JOIN Program p ON s.pid = p.progID;` |
| **With Conditions** | `SELECT s.Name FROM student s INNER JOIN program p ON s.pid = p.progId WHERE offerBy = 'SLIIT';` |

## 3. Outer Joins (Left & Right)

Unlike Inner Joins, Outer Joins do not require a strict match in both tables. They are used when you want to retain all records from one table, even if there is no corresponding relational data in the connected table.

### Left Outer Join
Returns **all rows** from the table on the left-hand side of the join, alongside the matching rows from the right-hand table . If there is no match, the result set will display `NULL` for the right side's attributes .

### Right Outer Join
Returns **all rows** from the table on the right-hand side of the join, alongside the matching rows from the left-hand table . If there is no match, the result set will display `NULL` for the left side's attributes .

```mermaid
graph LR
    classDef leftTable fill:#e1f5fe,stroke:#0288d1,stroke-width:2px,color:#01579b;
    classDef rightTable fill:#e8f5e9,stroke:#388e3c,stroke-width:2px,color:#1b5e20;
    classDef link fill:#f5f5f5,stroke:#9e9e9e,stroke-width:2px,color:#616161;

    S["Student Table<br/>(Left)"]:::leftTable --- FK(("pid = progId")):::link
    FK --- P["Program Table<br/>(Right)"]:::rightTable
````

## Technical Implementation

Below is a self-contained SQL script demonstrating table creation, mock data insertion, and all three primary join types discussed in the lecture.


```SQL
-- 1. Schema Setup
CREATE TABLE Program (
    progId VARCHAR(10) PRIMARY KEY,
    years INT,
    OfferBy VARCHAR(50)
);

CREATE TABLE Student (
    SID INT PRIMARY KEY,
    Name VARCHAR(50),
    gpa FLOAT,
    pid VARCHAR(10),
    FOREIGN KEY (pid) REFERENCES Program(progId)
);

-- 2. Mock Data Insertion
INSERT INTO Program (progId, years, OfferBy) VALUES 
('BM', 3, 'Curtin'),
('IT', 4, 'SLIIT'),
('SE', 3, 'SHU');

INSERT INTO Student (SID, Name, gpa, pid) VALUES 
(1000, 'Amal', 3.2, 'BM'),
(1001, 'Nimali', 2.8, 'IT'),
(1002, 'Aruni', 3.8, NULL),  -- Unassigned program
(1003, 'Surani', 2.5, 'IT');

-- 3. Query Implementations

-- INNER JOIN: Returns only students with assigned programs (Excludes Aruni, Excludes SE)
SELECT s.Name, p.OfferBy
FROM Student s 
INNER JOIN Program p ON s.pid = p.progId;

-- LEFT OUTER JOIN: Returns ALL students, filling NULL for Aruni's program
SELECT s.Name, p.OfferBy
FROM Student s 
LEFT OUTER JOIN Program p ON s.pid = p.progId;

-- RIGHT OUTER JOIN: Returns ALL programs, filling NULL for the unassigned 'SE' program
SELECT s.Name, p.OfferBy
FROM Student s 
RIGHT OUTER JOIN Program p ON s.pid = p.progId;
```

## Active Recall Self-Check

> [!TIP]
> 
> The student record will still be included in the final result set because it exists in the left table. However, any columns requested from the `Program` table (the right table) will simply display `NULL` for that specific student .

> [!TIP]
> 
> Explicit syntax separates the relationship logic (`ON`) from the filtering logic (`WHERE`). This makes complex queries significantly easier to read, debug, and maintain, reducing the likelihood of accidental Cartesian products (cross joins) if a `WHERE` condition is forgotten .

> [!TIP]
> 
> Yes. The join operation functionally creates a combined, temporary dataset. You can apply `GROUP BY` and aggregate functions to this joined dataset exactly as you would with a single standalone table.