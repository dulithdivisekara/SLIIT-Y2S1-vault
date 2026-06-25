---
title: "IT2140 - Database Design and Development: SQL"
date: 2026-06-25
module: "IT2140"
author: Dulith Divisekara
institution: SLIIT
tags: [information-technology, sql, database-management, semester-notes]
---
## Study Tracker
- [ ] Understand the role and components of Structured Query Language (SQL).
- [ ] Use Data Definition Language (DDL) to create and modify database structures.
- [ ] Use Data Manipulation Language (DML) to insert, delete, and update records.
- [ ] Construct complex `SELECT` queries utilizing logical operators, groupings, and aggregate functions.

## 1. Introduction to SQL

SQL, originally called SEQUEL (Structured English QUEry Language), is a comprehensive database language developed for the experimental System R database system . Standardized by ANSI and ISO in 1986, SQL allows users to define, manipulate, and query relational databases .

The language encompasses several subsets:
* **Data Definition Language (DDL):** For structural modifications .
* **Data Manipulation Language (DML):** For data modification and retrieval .
* **Security & Authorization Facilities** .
* **Transaction Processing Facilities** .

## 2. Data Definition Language (DDL)

> [!IMPORTANT]
> DDL statements (`CREATE`, `ALTER`, `DROP`) define the schema, modifying the actual tables and views rather than the data inside them . 

### Database Constraints
Constraints ensure data integrity when defining tables .

| Constraint | Purpose |
| :--- | :--- |
| **Not Null** | Ensures a column cannot contain empty/null values . |
| **Unique** | Ensures all values within a column are distinct, preventing duplicates . |
| **Primary Key** | Uniquely identifies each record in the table . |
| **Foreign Key** | Establishes a link to the primary key of another table to maintain referential integrity . |
| **Default** | Provides a fallback value if none is explicitly provided during insertion . |
| **Check** | Validates that data meets a specific condition before insertion (e.g., `age > 18`) . |

### DDL Implementation Example

```sql
-- Creating a table with various constraints
CREATE TABLE STUDENT (
    studentId INTEGER PRIMARY KEY,
    sName VARCHAR(30) NOT NULL,
    nic CHAR(10) UNIQUE,
    gpa FLOAT,
    progId VARCHAR(10) DEFAULT 'IT',
    CONSTRAINT student_prog_fk FOREIGN KEY (progId) 
        REFERENCES programs (id) ON DELETE SET DEFAULT ON UPDATE CASCADE,
    CONSTRAINT gpa_ck CHECK (gpa <= 4.0)
);

-- Modifying the structure of an existing table
ALTER TABLE STUDENT ADD age INT;
ALTER TABLE STUDENT ADD CONSTRAINT chk_age CHECK (age > 18);
ALTER TABLE STUDENT DROP COLUMN age;
````

## 3. Data Manipulation Language (DML)

> [!NOTE]
> 
> DML statements (`INSERT`, `UPDATE`, `DELETE`) allow users to interact with the actual rows of data stored within the schema established by DDL .

- **Inserting Rows:**
    
    
    
    ```SQL
    -- Insert providing all values
    INSERT INTO student VALUES (1000, 'Amal', '123456789V', 3.2, 'BM');
    -- Insert providing specific column values
    INSERT INTO student(studentId, sName, nic) VALUES (1001, 'Nimali', '234567890V');
    ```
    
- **Updating Rows:**
    
    
    
    ```SQL
    UPDATE student SET gpa = 2.8 WHERE studentId = 1001;
    ```
    
- **Deleting Rows:**
    
    
    
    ```SQL
    DELETE FROM student WHERE studentId = 1000;
    ```
    

## 4. Data Retrieval: The SELECT Clause

The `SELECT` statement is the fundamental command for querying information from a database .

### The Anatomy of a Query

The general structure of a comprehensive SQL query follows a specific order of execution :



```SQL
SELECT <attribute-list>
FROM <table-list>
[WHERE <condition>]
[GROUP BY <group attribute(s)>]
[HAVING <group condition>]
[ORDER BY <attribute list>];
```

### Operators and Filtering

|**Operator**|**Function**|**Example Usage**|
|---|---|---|
|`LIKE`|Pattern matching. `%` = any sequence of chars, `_` = single character .|`SELECT Name FROM student WHERE Name LIKE 'A%'`|
|`IS [NOT] NULL`|Checks for the absence of data .|`SELECT studentId FROM student WHERE gpa IS NULL`|
|`DISTINCT`|Eliminates duplicate values from the result set .|`SELECT DISTINCT progId FROM student`|
|`BETWEEN`|Checks if a value falls within an inclusive range .|`SELECT studentId FROM student WHERE gpa BETWEEN 3.7 AND 4.00`|
|`ORDER BY`|Sorts results. Default is Ascending (`ASC`), use `DESC` for descending .|`SELECT Name, gpa FROM student ORDER BY gpa`|

### Aggregation and Grouping

Aggregate functions (`SUM`, `COUNT`, `AVG`, `MIN`, `MAX`) summarize the results of an expression over multiple rows, returning a single scalar value .



```SQL
-- Find overall statistics
SELECT AVG(gpa), MIN(gpa), MAX(gpa) FROM student;
```

To aggregate data into subsets, use the `GROUP BY` clause. This produces a single summary row for each group . Note that when using `GROUP BY`, every item in the `SELECT` list must either be an aggregate function or appear in the `GROUP BY` clause itself .

To filter the _grouped_ results (unlike `WHERE` which filters individual rows before grouping), use the `HAVING` clause .



```SQL
-- Count how many students are enrolled in each course, 
-- but only show courses with more than 2 students.
SELECT CID, Count(SID)
FROM course
GROUP BY CID
HAVING count(SID) > 2;
```

## Active Recall Self-Check

> [!TIP]
> 
> The `WHERE` clause applies conditions to filter individual rows _before_ any grouping occurs. The `HAVING` clause applies conditions to filter aggregate groups _after_ the `GROUP BY` operation has been executed .

> [!TIP]
> 
> While the lecture specifically shows `ON DELETE SET DEFAULT` , omitting this specification typically defaults to `RESTRICT` in most relational database management systems. This means the system will prevent the deletion of the parent record if there are child records actively referencing it, thereby protecting referential integrity.

> [!TIP]
> 
> You would use the underscore `_` wildcard, which matches exactly one single character . The pattern would be `LIKE '___n'` (three underscores followed by an 'n').