# SQL DML (Data Manipulation Language)

## 1. Introduction to DML

**Plain meaning:**  
Data Manipulation Language (DML) is the part of SQL that deals with **changing and accessing the actual data** stored in a database — not the database’s structure, but the *content*.  

Think of a database as a large, organized filing cabinet:  
- **DDL** (Data Definition Language) decides how many drawers and folders exist, and how they’re labeled.  
- **DML** is you putting papers inside, taking them out, updating them, or throwing them away.  

**Purpose of DML:**  
- Insert new data into tables.  
- Modify existing data.  
- Remove data you don’t need.  
- Retrieve (read) data for reports, analysis, or application use.

**Relationship to other SQL sublanguages:**  

| Type  | Purpose              | Example Commands            |
|-------|----------------------|-----------------------------|
| **DDL** | Structure & schema   | CREATE, ALTER, DROP         |
| **DML** | Content manipulation | SELECT, INSERT, UPDATE, DELETE |
| **DCL** | Permissions & security | GRANT, REVOKE              |
| **TCL** | Transaction control  | COMMIT, ROLLBACK            |

---

## 2. Primary DML Commands

DML commands are the **core tools** for working with data inside tables.  
They do not define the database structure — they operate *within* that structure.

### 2.1 SELECT  
- **Purpose:** Retrieve data from one or more tables.  
- **Example:**
```sql
SELECT Staff_Name, Salary
FROM Staff
WHERE Salary > 15000;
```

### 2.2 INSERT  
**Purpose:** Add new rows of data into a table.  

**Example:**
```sql
INSERT INTO Staff (Staff_ID, Staff_Name, DeptNumb, Salary)
VALUES (510, 'Harrison', 'D38', 15400);
```

### 2.3 UPDATE  
**Purpose:** Modify existing records.  

**Example:**
```sql
UPDATE Staff
SET Salary = Salary * 1.15
WHERE Staff_ID = 510;
```

### 2.4 DELETE  
**Purpose:** Remove existing records from a table.  

**Example:**
```sql
DELETE FROM Staff
WHERE Staff_ID = 540;
```

---

## 3. General SQL Query Structure

A basic SQL query for retrieving data follows a specific structure.  
Each part (called a **clause**) tells the database what to do and in what order.

**General Form:**
```sql
SELECT [DISTINCT] field(s)
FROM table(s)
[WHERE predicate]
[GROUP BY field(s)]
[HAVING predicate]
[ORDER BY field(s) [ASC|DESC]];
```

### Key Points
- **SELECT** — Chooses which columns to display.  
- **DISTINCT** — Removes duplicate rows from the results.  
- **FROM** — Identifies the table(s) to get data from.  
- **WHERE** — Filters rows based on conditions.  
- **GROUP BY** — Groups rows that have the same values in specified columns.  
- **HAVING** — Filters the grouped results (like WHERE but for groups).  
- **ORDER BY** — Sorts the results in ascending (`ASC`) or descending (`DESC`) order.  


### Analogy
Imagine making a list of employees:
1. **SELECT**: Decide what details to list (name, salary).  
2. **FROM**: Decide which department’s employee list to use.  
3. **WHERE**: Only include employees with salary above $50,000.  
4. **GROUP BY**: Organize them by department.  
5. **HAVING**: Only include departments with more than 5 employees.  
6. **ORDER BY**: Sort the list by salary, highest first.

---


## 4. Core SQL Clauses

SQL queries are built from **clauses**, each serving a specific role in retrieving or shaping data.  
Below are the most important ones used in Data Manipulation Language (DML).

---

### 4.1 SELECT / SELECT DISTINCT / FROM
- **SELECT** — Specifies the columns (attributes) to be returned in the query output.  
- **SELECT DISTINCT** — Returns only unique rows, removing duplicates from the results.  
- **FROM** — Specifies the table(s) from which to retrieve the data.

**Example:**
```sql
SELECT DISTINCT DeptNumb
FROM Staff;
```

### 4.2 WHERE  
Filters rows based on specific conditions before grouping or ordering.  

Uses **comparison operators**:  
- `=` (equal to)  
- `>` (greater than)  
- `<` (less than)  
- `>=` (greater than or equal to)  
- `<=` (less than or equal to)  
- `<>` (not equal to)  

Uses **logical operators**:  
- `AND`  
- `OR`  
- `NOT`  

**Example:**
```sql
SELECT Staff_Name, Salary
FROM Staff
WHERE Salary > 18000 AND Job = 'SALES';
```

### 4.3 ORDER BY  
Sorts the result set based on one or more columns.  

- **Ascending order** (`ASC`) is the default.  
- **Descending order** (`DESC`) must be specified explicitly.  
- Can sort by multiple columns, separated by commas.  

**Example:**
```sql
SELECT Staff_Name, Salary
FROM Staff
ORDER BY Salary DESC;
```

### 4.4 GROUP BY  
Groups rows that have the same values in specified columns into summary rows.  

- Commonly used with **aggregate functions** such as `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`.  
- Only grouped columns and aggregate function results can appear in the `SELECT` clause.  

**Example:**
```sql
SELECT DeptNumb, AVG(Salary) AS Avg_Salary
FROM Staff
GROUP BY DeptNumb;

```

### 4.5 HAVING  
Filters the groups formed by the `GROUP BY` clause based on aggregate conditions.  

- Similar to `WHERE`, but applies **after grouping**.  
- Must be used with aggregate functions (e.g., `AVG`, `SUM`, `COUNT`).  

**Example:**
```sql
SELECT DeptNumb, AVG(Salary) AS Avg_Salary
FROM Staff
GROUP BY DeptNumb
HAVING AVG(Salary) > 20000;
```

## 5. Aggregate Functions

Aggregate functions perform calculations on a set of rows and return a single summary value.  
They are most often used with `GROUP BY`, but can also be used without grouping to summarize the entire table.

---

### Common Aggregate Functions

| Function  | Purpose |
|-----------|---------|
| `COUNT()` | Counts the number of rows. |
| `SUM()`   | Adds up the values in a numeric column. |
| `AVG()`   | Calculates the average of numeric values. |
| `MIN()`   | Returns the smallest value. |
| `MAX()`   | Returns the largest value. |

---

### Example 1 — Count rows per department
```sql
SELECT DeptNumb, COUNT(*) AS Employee_Count
FROM Staff
GROUP BY DeptNumb;
```

---


## 6. Joins

Joins allow you to retrieve data from **two or more tables** based on a related column between them.  
They are essential when data is normalized across multiple tables.

**Key idea:**  
A join combines rows from different tables when a related column (usually a **primary key** in one table and a **foreign key** in another) matches.

---

### 6.1 JOIN using WHERE clause
The older, implicit join style uses the `WHERE` clause to match rows between tables.

**Example:**
```sql
SELECT Course_Code, Course_Name, Instructor_Name
FROM Course AS C, Instructor AS I
WHERE C.Instructor_ID = I.Instructor_ID
ORDER BY C.Course_Code;
```


### 6.2 JOIN using FROM + ON clause (explicit join)  
The modern, explicit join style specifies the join directly in the `FROM` clause and uses `ON` to define the matching condition.  

- Improves readability, especially for multiple joins.  
- Makes it clear which columns are used for matching.  

**Example:**
```sql
SELECT Division, DeptName, Staff_Name
FROM Org
JOIN Staff
ON Staff.DeptNumb = Org.DeptNumb
WHERE Job = 'MGR'
ORDER BY Division, DeptName;
```

### 6.3 Types of Joins  

Joins come in different types depending on **which rows** are included in the result.  

| Type | Description | Example Use Case |
|------|-------------|------------------|
| **INNER JOIN** | Returns only rows with matching values in both tables. | Employees assigned to a department. |
| **LEFT OUTER JOIN** | Returns all rows from the left table, plus matching rows from the right table (NULL if no match). | All courses, even those without instructors. |
| **RIGHT OUTER JOIN** | Returns all rows from the right table, plus matching rows from the left table (NULL if no match). | All instructors, even if they teach no courses. |
| **FULL OUTER JOIN** | Returns all rows when there is a match in either table. | All courses and all instructors, even if unrelated. |

---

**Example — LEFT OUTER JOIN:**
```sql
SELECT Course_Code, Instructor_Name
FROM Course
LEFT OUTER JOIN Instructor
ON Course.Instructor_ID = Instructor.Instructor_ID;
```
## 7. UNION Operations  

The `UNION` operator combines the **results of two or more `SELECT` statements** into a single result set.  

**Key differences from JOIN:**  
- **JOIN**: Combines columns from multiple tables based on a relationship.  
- **UNION**: Combines rows from multiple queries into one list.  

---

### 7.1 Rules for UNION
1. Each `SELECT` must have the **same number of columns**.  
2. Columns must have **compatible data types** in the same positions.  
3. `UNION` removes duplicate rows by default.  
4. Use `UNION ALL` to keep duplicates.  

---

### Example:
```sql
SELECT Staff_Name, Years
FROM Staff
WHERE Years < 3

UNION

SELECT Applicant_Name, EdLevel
FROM Applicant
WHERE EdLevel > 14;
```

---


## 8. Correlation Variables  

Correlation variables are **temporary table aliases** used when the **same table** is referenced more than once in a query.  
They help compare rows within the same table.

---

### Key Points:
- You assign an alias to each table occurrence.  
- Commonly used in **self-joins** (joining a table to itself).  
- The aliases make it clear which instance of the table is being referred to.

---

### Example:  
Find employees whose salary is **greater than the salary of the manager** in the same department.

```sql
SELECT X.Staff_Name, X.Salary, Y.Salary AS Manager_Salary
FROM Staff AS X, Staff AS Y
WHERE X.DeptNumb = Y.DeptNumb
  AND Y.Job = 'MGR'
  AND X.Job <> 'MGR'
  AND X.Salary > Y.Salary;
```

---


## 9. ANY and ALL Operators  

These operators are used in **subqueries** to compare a value against a list of values returned by the subquery.

---

### 9.1 ANY  
- Returns `TRUE` if the comparison is true **for at least one** value in the subquery result.  
- Think of it as: *"Matches at least one option from the list."*  

**Example:** List employees who work in the **EASTERN** division.  
```sql
SELECT Staff_Name, Staff_ID
FROM Staff
WHERE DeptNumb = ANY (
    SELECT DeptNumb
    FROM Org
    WHERE Division = 'EASTERN'
);
```

### 9.2 ALL  
- Returns `TRUE` if the comparison is true **for every** value in the subquery result.  
- Think of it as: *"The value must satisfy the condition against all returned values."*  

**Example:** Find the department(s) with the **highest average salary**.  
```sql
SELECT DeptNumb, AVG(Salary) AS Avg_Salary
FROM Staff
GROUP BY DeptNumb
HAVING AVG(Salary) >= ALL (
    SELECT AVG(Salary)
    FROM Staff
    GROUP BY DeptNumb
);
```

## 10. Views in DML Context  

A **view** is a virtual table created from the result of a `SELECT` statement.  
It does not store data itself — it’s a saved query that displays data from one or more underlying tables.

---

### 10.1 Creating a View
**Example:** Create a view listing all salespersons and their income.  
```sql
CREATE VIEW Sales (Division, Department, Staff_Name, Income) AS
SELECT Division, DeptName, Staff_Name, Salary + Comm
FROM Staff, Org
WHERE Staff.DeptNumb = Org.DeptNumb
  AND Job = 'SALES';
```

### 10.2 Querying a View  
Once created, a view can be used **just like a regular table** in your queries.  
Any changes to the underlying tables will automatically be reflected when you query the view.

**Example:**
```sql
SELECT *
FROM Sales
WHERE Income > 20000;
```

### 10.3 Updating Data Through a View  
You can perform `INSERT`, `UPDATE`, or `DELETE` on a view, but only if the view meets certain conditions.

---

#### Rules for an Updatable View:
1. The view must be based on **a single base table**.  
2. The view must not contain **calculated columns** (e.g., `Salary + Comm`).  
3. The view must not use `DISTINCT`, `GROUP BY`, `HAVING`, `UNION`, or aggregate functions.  
4. All columns required by the base table’s constraints (e.g., `NOT NULL` without a default) must be included in the view.  

---

**Example — Updating through a view:**
```sql
UPDATE Sales
SET Income = Income + 1000
WHERE Staff_Name = 'Harrison';
```

### 10.4 Restrictions on View Modifications  

Not all views allow data modifications. Certain design choices make a view **read-only**.

---

#### Common Restrictions:
- **Computed or Derived Columns**  
  - Columns created using expressions (e.g., `Salary + Comm`) cannot be updated.  

- **Missing Required Columns**  
  - If the view omits columns from the base table that are `NOT NULL` without a default value, you cannot insert rows through the view.  

- **Multiple Base Tables**  
  - Views that join multiple tables are usually read-only.  

- **Aggregations or Grouping**  
  - Views that use `GROUP BY`, `HAVING`, `DISTINCT`, or aggregate functions cannot be updated.  

---

**Example of a Non-Updatable View:**
```sql
CREATE VIEW HighIncomeStaff AS
SELECT Staff_Name, Salary + Comm AS Total_Income
FROM Staff
WHERE Salary + Comm > 20000;
```
---



## Final Summary — SQL DML (Data Manipulation Language)

**Core Idea:**  
DML is about working with the **data** inside a database — adding, reading, updating, and deleting it — without changing the database’s structure.

---

### Key Takeaways:
- **DML Commands:**  
  - `SELECT` — Retrieve data.  
  - `INSERT` — Add new rows.  
  - `UPDATE` — Modify existing rows.  
  - `DELETE` — Remove rows.  

- **Query Structure:**  
  - Common clauses: `SELECT`, `FROM`, `WHERE`, `GROUP BY`, `HAVING`, `ORDER BY`.  
  - The order of clauses in SQL matters.

- **Filtering Data:**  
  - Use `WHERE` for row-level conditions.  
  - Use `HAVING` for group-level conditions after aggregation.

- **Sorting Data:**  
  - `ORDER BY` sorts results in ascending (`ASC`) or descending (`DESC`) order.

- **Aggregations:**  
  - Functions like `COUNT`, `SUM`, `AVG`, `MIN`, and `MAX` summarize data.  
  - Often combined with `GROUP BY`.

- **Joins:**  
  - Combine rows from multiple tables based on a related column.  
  - Types: `INNER`, `LEFT OUTER`, `RIGHT OUTER`, `FULL OUTER`.

- **UNION:**  
  - Combines results from multiple `SELECT` statements into one result set.  
  - Requires the same number and type of columns.

- **Correlation Variables:**  
  - Aliases used when joining a table to itself (self-joins).

- **ANY / ALL:**  
  - `ANY` checks if a condition matches at least one subquery result.  
  - `ALL` checks if a condition matches all subquery results.

- **Views in DML:**  
  - Saved queries that act like virtual tables.  
  - Some views allow data modifications; others are read-only.

---

**Mental Model:**  
Think of **DDL** as the **blueprint and construction** of a building (structure), and **DML** as the **everyday activity inside it** — moving in furniture, updating rooms, and cleaning things out.


