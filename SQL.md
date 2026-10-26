# SQL — Structured Query Language

## 1. Introduction to SQL

SQL stands for **Structured Query Language**. It is used to store, retrieve, update, and delete data in relational databases.

Popular relational databases include MySQL, PostgreSQL, Oracle Database, and SQL Server.

### Basic Database Terms

- **Database:** An organized collection of data.
- **Table:** Data organized into rows and columns.
- **Row:** A single record in a table.
- **Column:** A property of the data.
- **Schema:** The structure of database objects, such as tables and their relationships.
- **Data Type:** Defines the kind of value a column can store.

## 2. SQL Command Categories

### DDL — Data Definition Language

Used to define or modify database structures.

- `CREATE` — Creates database objects.
- `ALTER` — Modifies an existing structure.
- `DROP` — Removes a database object.
- `TRUNCATE` — Removes all rows from a table.

```sql
CREATE TABLE employees (
    id INT,
    name VARCHAR(100),
    salary DECIMAL(10, 2)
);

ALTER TABLE employees ADD department VARCHAR(50);
```

### DML — Data Manipulation Language

Used to insert, update, and delete records.

- `INSERT` — Adds new records.
- `UPDATE` — Modifies existing records.
- `DELETE` — Removes records.

```sql
INSERT INTO employees (id, name, salary)
VALUES (1, 'Amit', 60000);

UPDATE employees
SET salary = 65000
WHERE id = 1;

DELETE FROM employees
WHERE id = 1;
```

**Important:** Always use a suitable `WHERE` condition when updating or deleting specific records.

### DQL — Data Query Language

Used to retrieve data.

- `SELECT` — Retrieves records from a table.

```sql
SELECT * FROM employees;

SELECT name, salary
FROM employees
WHERE salary > 50000;
```

### DCL — Data Control Language

Used to control database permissions.

- `GRANT` — Gives privileges.
- `REVOKE` — Removes privileges.

```sql
GRANT SELECT ON company.employees TO analyst;

REVOKE SELECT ON company.employees FROM analyst;
```

The exact permission syntax depends on the database system.

### TCL — Transaction Control Language

Used to manage transactions.

- `COMMIT` — Saves transaction changes.
- `ROLLBACK` — Reverts uncommitted changes.
- `SAVEPOINT` — Creates a point for partial rollback.

```sql
START TRANSACTION;

UPDATE employees
SET salary = 70000
WHERE id = 1;

COMMIT;
```

## 3. SQL Data Types

Data types determine what values a column can store.

| Data Type | Purpose | Example |
|---|---|---|
| `INT` | Whole numbers | `25` |
| `VARCHAR(n)` | Variable-length text | `'Amit'` |
| `CHAR(n)` | Fixed-length text | `'IN'` |
| `DECIMAL(p,s)` | Exact decimal values | `45000.50` |
| `DATE` | Calendar dates | `'2026-10-26'` |
| `TIME` | Time values | `'10:30:00'` |
| `BOOLEAN` | True/false values where supported | `TRUE` |

## 4. SQL Constraints

Constraints enforce rules on table data.

- `PRIMARY KEY` — Uniquely identifies each row and cannot contain NULL.
- `FOREIGN KEY` — Enforces a relationship with a referenced key.
- `UNIQUE` — Prevents duplicate values according to database rules.
- `NOT NULL` — Prevents NULL values.
- `CHECK` — Ensures a condition is satisfied.
- `DEFAULT` — Supplies a default value when one is not provided.

```sql
CREATE TABLE departments (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(100) UNIQUE
);

CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    salary DECIMAL(10, 2) CHECK (salary >= 0),
    department_id INT,
    status VARCHAR(20) DEFAULT 'ACTIVE',
    FOREIGN KEY (department_id)
        REFERENCES departments(department_id)
);
```

## 5. Basic SQL Queries

### SELECT

Retrieves selected columns.

```sql
SELECT name, salary FROM employees;
```

### DISTINCT

Returns unique combinations of selected values.

```sql
SELECT DISTINCT department_id FROM employees;
```

### WHERE

Filters records based on a condition.

```sql
SELECT * FROM employees
WHERE salary > 50000;
```

### ORDER BY

Sorts the result.

```sql
SELECT * FROM employees
ORDER BY salary DESC;
```

`ASC` means ascending order; `DESC` means descending order.

### LIMIT

Restricts the number of returned rows in MySQL and PostgreSQL.

```sql
SELECT * FROM employees
LIMIT 5;
```

### LIKE

Matches text patterns. `%` represents zero or more characters; `_` represents one character.

```sql
SELECT * FROM employees
WHERE name LIKE 'A%';
```

### IN

Checks whether a value matches one of several values.

```sql
SELECT * FROM employees
WHERE department_id IN (1, 2, 3);
```

### BETWEEN

Checks whether a value lies within an inclusive range.

```sql
SELECT * FROM employees
WHERE salary BETWEEN 30000 AND 60000;
```

## 6. Aggregate Functions

Aggregate functions calculate a result from multiple rows.

| Function | Purpose |
|---|---|
| `COUNT()` | Counts rows or non-NULL values |
| `SUM()` | Calculates a total |
| `AVG()` | Calculates an average |
| `MIN()` | Finds the minimum |
| `MAX()` | Finds the maximum |

```sql
SELECT COUNT(*) FROM employees;

SELECT SUM(salary) FROM employees;

SELECT AVG(salary) FROM employees;

SELECT MIN(salary), MAX(salary)
FROM employees;
```

## 7. GROUP BY and HAVING

### GROUP BY

Groups rows that share the same value so aggregate functions can calculate results for each group.

```sql
SELECT department_id, AVG(salary)
FROM employees
GROUP BY department_id;
```

### HAVING

Filters groups after aggregation.

```sql
SELECT department_id, AVG(salary) AS avg_salary
FROM employees
GROUP BY department_id
HAVING AVG(salary) > 50000;
```

**Difference:**

- `WHERE` filters individual rows before grouping.
- `HAVING` filters groups after grouping.

## 8. SQL Joins

Joins combine related data from multiple tables.

Assume `employees.department_id` references `departments.department_id`.

### INNER JOIN

Returns only matching rows from both tables.

```sql
SELECT e.name, d.department_name
FROM employees e
INNER JOIN departments d
ON e.department_id = d.department_id;
```

### LEFT JOIN

Returns every row from the left table and matching rows from the right table. Missing matches produce NULL values for right-side columns.

```sql
SELECT e.name, d.department_name
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id;
```

### RIGHT JOIN

Returns every row from the right table and matching rows from the left table.

```sql
SELECT e.name, d.department_name
FROM employees e
RIGHT JOIN departments d
ON e.department_id = d.department_id;
```

### CROSS JOIN

Returns every possible combination of rows from both tables.

```sql
SELECT e.name, d.department_name
FROM employees e
CROSS JOIN departments d;
```

### SELF JOIN

Joins a table with itself, useful for relationships such as employees and managers.

```sql
SELECT e.name AS employee,
       m.name AS manager
FROM employees e
LEFT JOIN employees m
ON e.manager_id = m.id;
```

## 9. Subqueries

A subquery is a query nested inside another SQL query.

```sql
SELECT name, salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

This retrieves employees whose salary is greater than the overall average salary.

Subqueries can be used with `SELECT`, `INSERT`, `UPDATE`, and `DELETE`, depending on the query and database.

## 10. UNION and UNION ALL

Combine the results of multiple queries with compatible column counts and types.

### UNION

Combines results and removes duplicate rows.

```sql
SELECT name FROM employees
UNION
SELECT name FROM contractors;
```

### UNION ALL

Combines results while retaining duplicates.

```sql
SELECT name FROM employees
UNION ALL
SELECT name FROM contractors;
```

## 11. CASE Expression

Provides conditional logic inside a query.

```sql
SELECT name, salary,
       CASE
           WHEN salary >= 70000 THEN 'High'
           WHEN salary >= 40000 THEN 'Medium'
           ELSE 'Low'
       END AS salary_level
FROM employees;
```

## 12. Common Table Expressions (CTEs)

A CTE defines a temporary named result that can be referenced by the main query.

```sql
WITH high_salary AS (
    SELECT *
    FROM employees
    WHERE salary > 50000
)
SELECT name, salary
FROM high_salary;
```

CTEs improve readability when queries become complex. Recursive CTEs can also work with hierarchical data.

## 13. Window Functions

Window functions calculate values across related rows without combining them into a single row per group.

### ROW_NUMBER()

Assigns a unique sequential number to each row within the specified ordering.

```sql
SELECT name, department_id, salary,
       ROW_NUMBER() OVER (
           PARTITION BY department_id
           ORDER BY salary DESC
       ) AS row_num
FROM employees;
```

Other important window functions include:

- `RANK()` — Assigns ranks, leaving gaps after ties.
- `DENSE_RANK()` — Assigns ranks without gaps after ties.
- `LAG()` — Accesses a value from a previous row.
- `LEAD()` — Accesses a value from a following row.

## 14. Views

A view is a named query that can be used like a virtual table.

```sql
CREATE VIEW high_salary_employees AS
SELECT name, salary
FROM employees
WHERE salary > 50000;

SELECT * FROM high_salary_employees;
```

Views can simplify complex queries and help control access to data. Their update capabilities depend on the view and database.

## 15. Indexes

An index helps the database locate records more efficiently, often improving search and join performance.

```sql
CREATE INDEX idx_employee_name
ON employees(name);
```

Indexes can improve reads but require storage and may slow down inserts, updates, and deletes.

## 16. Stored Procedures and Triggers

### Stored Procedure

A stored procedure is a named set of SQL statements stored in the database and executed when called.

Stored procedure syntax differs between database systems.

### Trigger

A trigger runs automatically when a configured database event occurs, such as an insert, update, or delete.

Common uses include auditing changes and enforcing certain data rules.

## 17. Database Keys

- **Primary Key:** Uniquely identifies a record.
- **Foreign Key:** References a key in another or the same table.
- **Candidate Key:** A minimal set of columns capable of uniquely identifying a row.
- **Composite Key:** A key consisting of multiple columns.
- **Alternate Key:** A candidate key not selected as the primary key.
- **Superkey:** Any set of columns that uniquely identifies a row.

## 18. Database Normalization

Normalization organizes relational data to reduce unnecessary duplication and avoid data anomalies.

- **1NF:** Each column contains atomic values, without repeating groups.
- **2NF:** The table is in 1NF, and every non-prime attribute depends on the whole candidate key.
- **3NF:** The table is in 2NF, and non-prime attributes have no improper transitive dependency on candidate keys.
- **BCNF:** Every determinant of a non-trivial functional dependency is a superkey.

Normalization often involves splitting data into related tables and connecting them using keys.

## 19. Relationships Between Tables

- **One-to-One:** One person has one passport record.
- **One-to-Many:** One department has many employees.
- **Many-to-Many:** Many students enroll in many courses, usually through a junction table.

## 20. Transactions and ACID Properties

A transaction is a group of database operations treated as one logical unit.

ACID stands for:

- **Atomicity:** All operations succeed together or the transaction is rolled back.
- **Consistency:** A transaction preserves defined database rules.
- **Isolation:** Concurrent transactions are controlled to limit interference.
- **Durability:** Committed changes persist according to the database's guarantees.

```sql
START TRANSACTION;

UPDATE accounts
SET balance = balance - 1000
WHERE account_id = 1;

UPDATE accounts
SET balance = balance + 1000
WHERE account_id = 2;

COMMIT;
```

This illustrates transferring money between accounts. In a real application, validate balances and ensure both updates succeed before committing.

## 21. Concurrency and Isolation

Concurrency occurs when multiple transactions access or modify data at the same time.

Important concepts:

- **Dirty Read:** Reading another transaction's uncommitted changes.
- **Non-repeatable Read:** Reading the same row twice and seeing different committed values.
- **Phantom Read:** Repeating a query and finding additional or missing rows that match its condition.
- **Lost Update:** One transaction overwrites another transaction's change.
- **Isolation Levels:** Read Uncommitted, Read Committed, Repeatable Read, and Serializable.

Exact behavior depends on the database engine and its implementation.

## 22. Query Optimization

Query optimization aims to execute SQL queries efficiently.

Important practices:

- Create indexes for suitable search and join columns.
- Retrieve only the columns you need instead of always using `SELECT *`.
- Filter records appropriately.
- Avoid unnecessary joins and repeated subqueries.
- Use `EXPLAIN` to inspect the query execution plan.
- Measure performance with realistic data before changing indexes or queries.

## 23. SQL with Java Using JDBC

JDBC (Java Database Connectivity) allows Java applications to interact with relational databases.

Important JDBC concepts:

- `DriverManager` and `DataSource`
- `Connection`
- `Statement`
- `PreparedStatement`
- `CallableStatement`
- `ResultSet`
- Exception handling with `SQLException`
- Transactions using `commit()` and `rollback()`
- Connection pooling

Example:

```java
String sql = "SELECT name, salary FROM employees WHERE id = ?";

try (PreparedStatement ps = connection.prepareStatement(sql)) {
    ps.setInt(1, 1);

    try (ResultSet rs = ps.executeQuery()) {
        while (rs.next()) {
            String name = rs.getString("name");
            double salary = rs.getDouble("salary");
        }
    }
}
```

`PreparedStatement` uses parameter binding and is generally preferred for queries with user-supplied values. It helps prevent SQL injection when used correctly.

## 24. Recommended Learning Order

1. SQL fundamentals and data types
2. DDL, DML, and SELECT queries
3. Constraints and keys
4. WHERE, ORDER BY, LIKE, IN, and BETWEEN
5. Aggregate functions, GROUP BY, and HAVING
6. Joins and subqueries
7. UNION, CASE, and CTEs
8. Window functions
9. Views and indexes
10. Normalization and relationships
11. Transactions, ACID, and isolation
12. Query optimization
13. JDBC integration with Java
14. SQL interview questions and practice problems

## 25. Learning Goals

- Build strong SQL fundamentals.
- Write queries to retrieve and manipulate data.
- Understand relationships and database design.
- Solve common SQL interview problems.
- Understand transactions, indexes, and query performance.
- Integrate SQL databases with Java backend applications.

**Goal:** Develop practical SQL skills for Java Backend Development and technical interviews.
