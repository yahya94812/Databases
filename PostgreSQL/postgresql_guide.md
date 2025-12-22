# PostgreSQL Essential Concepts Guide

## 1. Introduction to Databases

A **database** is an organized collection of structured data. PostgreSQL is a powerful, open-source relational database management system (RDBMS) that uses SQL (Structured Query Language).

## 2. Basic Database Concepts

### Tables
Tables store data in rows and columns, similar to spreadsheets. Each column has a specific data type.

### Primary Key
A unique identifier for each row in a table. No two rows can have the same primary key value.

### Foreign Key
A column that references the primary key of another table, creating relationships between tables.

## 3. Getting Started

### Creating a Database
```sql
CREATE DATABASE company_db;
```

### Connecting to a Database
```sql
\c company_db
```

## 4. Data Types

PostgreSQL supports various data types:

- **INTEGER**: Whole numbers (e.g., 42)
- **SERIAL**: Auto-incrementing integer
- **VARCHAR(n)**: Variable-length text up to n characters
- **TEXT**: Unlimited length text
- **DATE**: Date values (YYYY-MM-DD)
- **TIMESTAMP**: Date and time
- **BOOLEAN**: TRUE or FALSE
- **NUMERIC(p,s)**: Precise decimal numbers

## 5. Creating Tables

### Example: Employees Table
```sql
CREATE TABLE employees (
    employee_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    hire_date DATE,
    salary NUMERIC(10, 2),
    department_id INTEGER
);
```

### Example: Departments Table
```sql
CREATE TABLE departments (
    department_id SERIAL PRIMARY KEY,
    department_name VARCHAR(100) NOT NULL,
    location VARCHAR(100)
);
```

### Adding Foreign Key Constraint
```sql
ALTER TABLE employees
ADD CONSTRAINT fk_department
FOREIGN KEY (department_id) REFERENCES departments(department_id);
```

## 6. Inserting Data (INSERT)

### Insert Single Row
```sql
INSERT INTO departments (department_name, location)
VALUES ('Engineering', 'San Francisco');
```

### Insert Multiple Rows
```sql
INSERT INTO departments (department_name, location)
VALUES 
    ('Sales', 'New York'),
    ('Marketing', 'Chicago'),
    ('HR', 'Boston');
```

### Insert Employee Data
```sql
INSERT INTO employees (first_name, last_name, email, hire_date, salary, department_id)
VALUES 
    ('John', 'Doe', 'john.doe@company.com', '2023-01-15', 75000.00, 1),
    ('Jane', 'Smith', 'jane.smith@company.com', '2023-02-20', 82000.00, 1),
    ('Mike', 'Johnson', 'mike.j@company.com', '2023-03-10', 68000.00, 2);
```

## 7. Querying Data (SELECT)

### Select All Columns
```sql
SELECT * FROM employees;
```

### Select Specific Columns
```sql
SELECT first_name, last_name, salary FROM employees;
```

### Using WHERE Clause
```sql
SELECT * FROM employees
WHERE salary > 70000;
```

### Multiple Conditions
```sql
SELECT * FROM employees
WHERE salary > 70000 AND department_id = 1;
```

### Using LIKE for Pattern Matching
```sql
SELECT * FROM employees
WHERE email LIKE '%@company.com';
```

## 8. Sorting Data (ORDER BY)

```sql
-- Ascending order (default)
SELECT * FROM employees
ORDER BY salary;

-- Descending order
SELECT * FROM employees
ORDER BY salary DESC;

-- Multiple columns
SELECT * FROM employees
ORDER BY department_id, salary DESC;
```

## 9. Filtering with Aggregate Functions

### COUNT
```sql
SELECT COUNT(*) FROM employees;
```

### AVG, MIN, MAX, SUM
```sql
SELECT 
    AVG(salary) as avg_salary,
    MIN(salary) as min_salary,
    MAX(salary) as max_salary,
    SUM(salary) as total_salary
FROM employees;
```

## 10. Grouping Data (GROUP BY)
* GROUP BY reduces rows, ORDER BY only sorts rows
```sql
SELECT department_id, COUNT(*) as employee_count, AVG(salary) as avg_salary
FROM employees
GROUP BY department_id;
```

### HAVING Clause (Filter Groups)
```sql
SELECT department_id, AVG(salary) as avg_salary
FROM employees
GROUP BY department_id
HAVING AVG(salary) > 70000;
```

## 11. Joining Tables

### INNER JOIN
Returns rows when there's a match in both tables.

```sql
SELECT e.first_name, e.last_name, e.salary, d.department_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.department_id;
```

### LEFT JOIN
Returns all rows from the left table and matching rows from the right.

```sql
SELECT e.first_name, e.last_name, d.department_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.department_id;
```

### RIGHT JOIN
Returns all rows from the right table and matching rows from the left.

```sql
SELECT e.first_name, e.last_name, d.department_name
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.department_id;
```

## 12. Updating Data (UPDATE)

```sql
-- Update single row
UPDATE employees
SET salary = 80000.00
WHERE employee_id = 1;

-- Update multiple rows
UPDATE employees
SET salary = salary * 1.10
WHERE department_id = 1;
```

## 13. Deleting Data (DELETE)

```sql
-- Delete specific row
DELETE FROM employees
WHERE employee_id = 5;

-- Delete with condition
DELETE FROM employees
WHERE hire_date < '2023-01-01';
```

## 14. Indexes

Indexes speed up data retrieval but slow down writes.

```sql
-- Create index
CREATE INDEX idx_employee_email ON employees(email);

-- Create unique index
CREATE UNIQUE INDEX idx_employee_email_unique ON employees(email);

-- Drop index
DROP INDEX idx_employee_email;
```

## 15. Transactions

Transactions ensure data integrity by grouping operations.

```sql
BEGIN;

UPDATE employees SET salary = 85000 WHERE employee_id = 1;
UPDATE departments SET location = 'Austin' WHERE department_id = 1;

COMMIT;  -- Save changes

-- Or use ROLLBACK to undo changes
```

## 16. Views

Views are virtual tables based on queries.

```sql
CREATE VIEW employee_details AS
SELECT e.first_name, e.last_name, e.salary, d.department_name
FROM employees e
JOIN departments d ON e.department_id = d.department_id;

-- Query the view
SELECT * FROM employee_details;
```

## 17. Common Table Expressions (CTEs)

```sql
WITH high_earners AS (
    SELECT * FROM employees WHERE salary > 75000
)
SELECT first_name, last_name, salary
FROM high_earners
ORDER BY salary DESC;
```

## 18. Subqueries

```sql
-- Subquery in WHERE clause
SELECT first_name, last_name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- Subquery in FROM clause
SELECT dept_stats.department_id, dept_stats.avg_salary
FROM (
    SELECT department_id, AVG(salary) as avg_salary
    FROM employees
    GROUP BY department_id
) AS dept_stats
WHERE dept_stats.avg_salary > 70000;
```

## 19. Useful Commands

```sql
-- Show all tables
\dt

-- Describe table structure
\d employees

-- Show database list
\l

-- Quit psql
\q
```

## 20. Best Practices

1. **Always use PRIMARY KEY** for unique identification
2. **Use constraints** (NOT NULL, UNIQUE, CHECK) to maintain data integrity
3. **Index frequently queried columns** but don't over-index
4. **Use transactions** for related operations
5. **Normalize your database** to reduce redundancy
6. **Use meaningful names** for tables and columns
7. **Backup regularly** using pg_dump
8. **Use prepared statements** to prevent SQL injection

## Practice Exercise

Try creating a simple blog database:

```sql
-- Posts table
CREATE TABLE posts (
    post_id SERIAL PRIMARY KEY,
    title VARCHAR(200) NOT NULL,
    content TEXT,
    author_id INTEGER,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Authors table
CREATE TABLE authors (
    author_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE
);

-- Add foreign key
ALTER TABLE posts
ADD CONSTRAINT fk_author
FOREIGN KEY (author_id) REFERENCES authors(author_id);
```

Now practice inserting data, querying with joins, and updating records!