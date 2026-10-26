# SQL (Structured Query Language)

## 📌 About SQL

SQL (Structured Query Language) is used to communicate with relational databases. It allows us to store, retrieve, update, and delete data efficiently.

SQL is widely used with databases such as MySQL, PostgreSQL, Oracle Database, and SQL Server.

## 📚 Topics Covered

### 1. SQL Fundamentals
- Introduction to SQL and databases
- Tables, rows, and columns
- Data types
- SQL syntax and commands

### 2. SQL Commands
- **DDL:** `CREATE`, `ALTER`, `DROP`, `TRUNCATE`
- **DML:** `INSERT`, `UPDATE`, `DELETE`
- **DQL:** `SELECT`
- **DCL:** `GRANT`, `REVOKE`
- **TCL:** `COMMIT`, `ROLLBACK`, `SAVEPOINT`

### 3. Constraints
- `PRIMARY KEY`
- `FOREIGN KEY`
- `UNIQUE`
- `NOT NULL`
- `CHECK`
- `DEFAULT`

### 4. Querying Data
- `SELECT` and `DISTINCT`
- `WHERE` clause
- `ORDER BY`
- `LIMIT`
- `LIKE`, `IN`, `BETWEEN`
- Aggregate functions: `COUNT()`, `SUM()`, `AVG()`, `MIN()`, `MAX()`

### 5. Joins
- INNER JOIN
- LEFT JOIN
- RIGHT JOIN
- CROSS JOIN
- SELF JOIN

### 6. Advanced SQL
- GROUP BY and HAVING
- Subqueries
- Nested queries
- UNION and UNION ALL
- CASE expressions
- Common Table Expressions (CTEs)
- Window functions
- Views
- Indexes
- Stored procedures and triggers

### 7. Database Concepts
- Normalization
- Relationships between tables
- Transactions and ACID properties
- Database keys
- Query optimization
- Transactions and concurrency

## 💻 Example Query

Retrieve employees whose salary is greater than 50,000.

```sql
SELECT name, salary
FROM employees
WHERE salary > 50000
ORDER BY salary DESC;
```

## 🎯 Learning Goals

- Understand relational databases and SQL fundamentals.
- Write efficient queries to retrieve and manipulate data.
- Solve SQL interview questions.
- Understand joins, subqueries, indexes, and transactions.
- Learn how SQL integrates with Java applications using JDBC.

## 🛠️ Tools

- MySQL
- MySQL Workbench
- IntelliJ IDEA / Eclipse
- Git and GitHub

## 🚀 Practice

Practice SQL queries regularly using real-world datasets and interview problems. Focus on understanding how queries work rather than memorizing syntax.

---

**Goal:** Build strong SQL fundamentals for Java Backend Development and technical interviews.
