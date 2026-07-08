# 50 Most Asked SQL Questions for QA Tester Job Applications

## Table of Contents
1. [Basic SQL Concepts](#basic-sql-concepts) (Q1-Q10)
2. [Advanced SQL Queries](#advanced-sql-queries) (Q11-Q25)
3. [Database Design & Relationships](#database-design--relationships) (Q26-Q35)
4. [Performance & Optimization](#performance--optimization) (Q36-Q45)
5. [Real-World Scenarios for QA Testing](#real-world-scenarios-for-qa-testing) (Q46-Q50)

---

## Basic SQL Concepts

### Q1: What is SQL?

**Answer:**
SQL (Structured Query Language) is a standard programming language used to manage and manipulate relational databases. It allows you to create, read, update, and delete data (CRUD operations).

**Use in QA Testing:**
- Write queries to verify data in databases
- Validate business logic
- Extract test data

**Example:**
```sql
SELECT * FROM users WHERE status = 'active';
```

---

### Q2: What is a Database and why is it important for QA?

**Answer:**
A database is an organized collection of structured data that is stored and accessed electronically. For QA testing, databases are crucial for:
- Verifying data persistence
- Validating data integrity
- Testing data validations
- Extracting test data

**Example Database Structure:**
```
users_db/
├── users (table)
├── orders (table)
├── products (table)
└── payments (table)
```

---

### Q3: What is a Table in SQL?

**Answer:**
A table is a collection of related data entries stored in rows (records) and columns (fields). Each table represents a single entity type.

**Example:**
```sql
CREATE TABLE users (
    user_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    age INT,
    created_at TIMESTAMP
);
```

---

### Q4: What is the difference between SQL and MySQL?

**Answer:**

| SQL | MySQL |
|-----|-------|
| Language used to query databases | Database management system (DBMS) |
| Standard across all relational databases | Specific implementation using SQL |
| Cannot be installed or downloaded | Can be downloaded and installed |
| Used with Oracle, SQL Server, MySQL, PostgreSQL | Open-source relational database |

**For QA:** MySQL is commonly used in web applications for testing purposes.

---

### Q5: What is a Primary Key?

**Answer:**
A primary key is a column (or set of columns) that uniquely identifies each row in a table. It must contain unique, non-null values.

**Characteristics:**
- ✅ Unique values
- ✅ No NULL values allowed
- ✅ Only one per table
- ✅ Automatically indexed

**Example:**
```sql
CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    emp_name VARCHAR(100),
    department VARCHAR(50)
);

-- Inserting data
INSERT INTO employees VALUES (1, 'John Doe', 'IT');
INSERT INTO employees VALUES (2, 'Jane Smith', 'HR');
-- INSERT INTO employees VALUES (1, 'Alice Brown', 'Finance'); -- ERROR! Duplicate primary key
```

**QA Use Case:**
```sql
-- Verify no duplicate employee IDs
SELECT emp_id, COUNT(*) FROM employees GROUP BY emp_id HAVING COUNT(*) > 1;
```

---

### Q6: What is a Foreign Key?

**Answer:**
A foreign key is a column (or set of columns) that creates a link between two tables by referencing the primary key of another table. It enforces referential integrity.

**Example:**
```sql
-- Parent table
CREATE TABLE departments (
    dept_id INT PRIMARY KEY,
    dept_name VARCHAR(100)
);

-- Child table
CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    emp_name VARCHAR(100),
    dept_id INT,
    FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
);

INSERT INTO departments VALUES (1, 'IT');
INSERT INTO departments VALUES (2, 'HR');

INSERT INTO employees VALUES (1, 'John Doe', 1);
INSERT INTO employees VALUES (2, 'Jane Smith', 2);
-- INSERT INTO employees VALUES (3, 'Bob Brown', 5); -- ERROR! Foreign key constraint
```

**QA Use Case:**
```sql
-- Verify referential integrity
SELECT e.emp_name, d.dept_name 
FROM employees e 
LEFT JOIN departments d ON e.dept_id = d.dept_id 
WHERE d.dept_id IS NULL; -- Find orphaned records
```

---

### Q7: What is a Unique Key?

**Answer:**
A unique key ensures that all values in a column are unique, preventing duplicate entries. Unlike primary key, multiple NULL values are allowed.

**Differences from Primary Key:**

| Primary Key | Unique Key |
|-------------|-----------|
| Only one per table | Multiple per table |
| No NULL values | Multiple NULLs allowed |
| Automatically indexed | Not automatically indexed |
| Enforces uniqueness & identity | Enforces uniqueness only |

**Example:**
```sql
CREATE TABLE users (
    user_id INT PRIMARY KEY,
    username VARCHAR(50) UNIQUE,
    email VARCHAR(100) UNIQUE,
    phone VARCHAR(20)
);

INSERT INTO users VALUES (1, 'john_doe', 'john@email.com', '123-456-7890');
-- INSERT INTO users VALUES (2, 'john_doe', 'jane@email.com', NULL); -- ERROR! Duplicate username
```

---

### Q8: What is a JOIN and list its types?

**Answer:**
A JOIN combines rows from two or more tables based on a related column. It's essential for querying related data.

**Types of JOINs:**

1. **INNER JOIN** - Returns only matching rows
2. **LEFT JOIN** - Returns all rows from left table + matching rows from right
3. **RIGHT JOIN** - Returns all rows from right table + matching rows from left
4. **FULL OUTER JOIN** - Returns all rows from both tables
5. **CROSS JOIN** - Returns Cartesian product

**Visual Representation:**
```
Table A          Table B
┌─────┐         ┌─────┐
│ A1  │         │ B1  │
│ A2  │         │ B2  │
│ A3  │         │ B3  │

INNER JOIN: Returns A2, B2 (common)
LEFT JOIN: Returns A1, A2, A3 + B2 (all from left)
RIGHT JOIN: Returns A2 + B1, B2, B3 (all from right)
FULL JOIN: Returns A1, A2, A3 + B1, B2, B3 (all from both)
```

---

### Q9: Explain INNER JOIN with a real example

**Answer:**
INNER JOIN returns only the rows that have matching values in both tables.

**Database Setup:**
```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100),
    city VARCHAR(50)
);

INSERT INTO customers VALUES (1, 'Alice Johnson', 'New York');
INSERT INTO customers VALUES (2, 'Bob Smith', 'Los Angeles');
INSERT INTO customers VALUES (3, 'Charlie Brown', 'Chicago');

INSERT INTO orders VALUES (101, 1, '2024-01-15');
INSERT INTO orders VALUES (102, 2, '2024-01-20');
INSERT INTO orders VALUES (103, 1, '2024-02-10');
-- Note: Customer 3 has no orders
```

**INNER JOIN Query:**
```sql
SELECT o.order_id, c.customer_name, c.city, o.order_date
FROM orders o
INNER JOIN customers c ON o.customer_id = c.customer_id;

/* OUTPUT:
order_id | customer_name    | city        | order_date
---------|-----------------|-------------|------------
101      | Alice Johnson   | New York    | 2024-01-15
102      | Bob Smith       | Los Angeles | 2024-01-20
103      | Alice Johnson   | New York    | 2024-02-10
*/
```

**QA Use Case:**
```sql
-- Verify all orders have valid customers
SELECT COUNT(*) FROM orders o 
LEFT JOIN customers c ON o.customer_id = c.customer_id 
WHERE c.customer_id IS NULL; -- Should be 0
```

---

### Q10: What is the difference between WHERE and HAVING clause?

**Answer:**

| WHERE | HAVING |
|-------|--------|
| Filters rows BEFORE grouping | Filters rows AFTER grouping |
| Works with individual rows | Works with aggregated data |
| Cannot use aggregate functions | Uses aggregate functions (COUNT, SUM, AVG) |
| Applied during query execution | Applied after GROUP BY |

**Example:**
```sql
-- Sample data
CREATE TABLE sales (
    sale_id INT,
    product_name VARCHAR(50),
    amount DECIMAL(10, 2),
    region VARCHAR(50)
);

INSERT INTO sales VALUES 
(1, 'Laptop', 1200, 'North'),
(2, 'Mouse', 25, 'North'),
(3, 'Laptop', 1200, 'South'),
(4, 'Keyboard', 75, 'South'),
(5, 'Monitor', 300, 'North');

-- Using WHERE (filters before grouping)
SELECT region, COUNT(*) as total_sales
FROM sales
WHERE amount > 100
GROUP BY region;

/* OUTPUT:
region | total_sales
-------|------------
North  | 2
South  | 2
*/

-- Using HAVING (filters after grouping)
SELECT region, COUNT(*) as total_sales
FROM sales
GROUP BY region
HAVING COUNT(*) > 2;

/* OUTPUT:
region | total_sales
-------|------------
North  | 3
*/

-- Using both
SELECT region, SUM(amount) as total_revenue
FROM sales
WHERE amount > 50
GROUP BY region
HAVING SUM(amount) > 500;

/* OUTPUT:
region | total_revenue
-------|---------------
North  | 1525
South  | 1500
*/
```

---

## Advanced SQL Queries

### Q11: How do you find duplicate records in a table?

**Answer:**
Finding duplicates is crucial for data quality validation in QA testing.

**Method 1: Using GROUP BY and HAVING**
```sql
-- Find users with duplicate emails
CREATE TABLE users (
    user_id INT,
    email VARCHAR(100),
    name VARCHAR(50)
);

INSERT INTO users VALUES 
(1, 'john@email.com', 'John'),
(2, 'jane@email.com', 'Jane'),
(3, 'john@email.com', 'Johnny'),
(4, 'bob@email.com', 'Bob'),
(5, 'jane@email.com', 'Janet');

-- Find duplicate emails
SELECT email, COUNT(*) as count
FROM users
GROUP BY email
HAVING COUNT(*) > 1;

/* OUTPUT:
email           | count
----------------|------
john@email.com  | 2
jane@email.com  | 2
*/
```

**Method 2: Using ROW_NUMBER() window function**
```sql
SELECT *
FROM (
    SELECT user_id, email, name,
           ROW_NUMBER() OVER (PARTITION BY email ORDER BY user_id) as row_num
    FROM users
) as ranked
WHERE row_num > 1;

/* OUTPUT - Returns duplicate records:
user_id | email          | name   | row_num
--------|----------------|--------|--------
3       | john@email.com | Johnny | 2
5       | jane@email.com | Janet  | 2
*/
```

**QA Use Case:**
```sql
-- Data quality check: Verify no duplicate customer emails
SELECT COUNT(*) FROM (
    SELECT email, COUNT(*) 
    FROM customers 
    GROUP BY email 
    HAVING COUNT(*) > 1
) as duplicates;
-- Expected result: 0 (no duplicates)
```

---

### Q12: Write a query to fetch the second highest salary

**Answer:**
A common interview question to test your understanding of subqueries and ranking.

**Method 1: Using MAX and Subquery**
```sql
CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    emp_name VARCHAR(100),
    salary DECIMAL(10, 2)
);

INSERT INTO employees VALUES 
(1, 'Alice', 100000),
(2, 'Bob', 95000),
(3, 'Charlie', 90000),
(4, 'Diana', 100000),
(5, 'Eve', 85000);

-- Get second highest salary
SELECT MAX(salary) as second_highest_salary
FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);

/* OUTPUT:
second_highest_salary
---------------------
95000
*/
```

**Method 2: Using DISTINCT and ORDER BY with LIMIT**
```sql
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1, 1;  -- Skip 1st result, get 2nd
-- MySQL syntax: LIMIT 1 OFFSET 1
```

**Method 3: Using ROW_NUMBER() window function**
```sql
SELECT salary
FROM (
    SELECT salary,
           ROW_NUMBER() OVER (ORDER BY salary DESC) as rank
    FROM employees
) as ranked
WHERE rank = 2;

/* OUTPUT:
salary
------
95000
*/
```

**Method 4: Using DENSE_RANK() (handles duplicate salaries)**
```sql
SELECT salary
FROM (
    SELECT salary,
           DENSE_RANK() OVER (ORDER BY salary DESC) as dense_rank
    FROM employees
) as ranked
WHERE dense_rank = 2;

/* OUTPUT (if multiple employees have same highest salary):
salary
------
100000  -- Both employees with max salary get rank 1
100000  -- Second highest rank is still 100000
*/
```

**QA Use Case:**
```sql
-- Verify salary data integrity
SELECT emp_id, emp_name, salary
FROM employees
WHERE salary IN (
    SELECT salary FROM employees 
    ORDER BY salary DESC 
    LIMIT 2
)
ORDER BY salary DESC;
```

---

### Q13: What is Normalization and why is it important for QA?

**Answer:**
Normalization is the process of organizing data in a database to reduce redundancy and improve data integrity. It's important for QA because it helps identify data inconsistencies.

**Normal Forms:**

**First Normal Form (1NF):**
- All columns contain atomic (indivisible) values
- No repeating groups

**Before 1NF (NOT normalized):**
```
students table:
student_id | name      | courses
-----------|-----------|------------------------
1          | John      | Math, Science, English
2          | Jane      | Physics, Chemistry
```

**After 1NF (normalized):**
```
students table:
student_id | name
-----------|------
1          | John
2          | Jane

student_courses table:
student_id | course
-----------|----------
1          | Math
1          | Science
1          | English
2          | Physics
2          | Chemistry
```

**Second Normal Form (2NF):**
- Must be in 1NF
- Remove partial dependencies

**Third Normal Form (3NF):**
- Must be in 2NF
- Remove transitive dependencies

**QA Use Case:**
```sql
-- Check for normalized data structure (no repeating groups)
-- Verify each cell contains single value
SELECT * FROM employees WHERE phone LIKE '%,%'; -- Should return 0
```

---

### Q14: What is Denormalization and when would you use it?

**Answer:**
Denormalization is the intentional process of adding redundancy to a database to improve read performance at the cost of write performance.

**When to Denormalize:**
- High-traffic read-heavy applications
- Complex joins affecting performance
- Reporting and analytics databases
- Historical data preservation

**Example:**
```sql
-- Original normalized structure
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

CREATE TABLE order_items (
    item_id INT PRIMARY KEY,
    order_id INT,
    product_id INT,
    quantity INT,
    unit_price DECIMAL(10, 2),
    FOREIGN KEY (order_id) REFERENCES orders(order_id)
);

-- Denormalized structure for faster reporting
CREATE TABLE orders_denormalized (
    order_id INT PRIMARY KEY,
    customer_id INT,
    customer_name VARCHAR(100),
    customer_city VARCHAR(50),
    order_date DATE,
    total_items INT,
    total_amount DECIMAL(10, 2)
);
```

**QA Considerations:**
```sql
-- Verify denormalized data consistency with source data
SELECT COUNT(*) as mismatches
FROM orders_denormalized od
LEFT JOIN orders o ON od.order_id = o.order_id
WHERE od.customer_name != o.customer_name;
-- Expected: 0
```

---

### Q15: What is an Index and why is it important?

**Answer:**
An index is a database object that improves the speed of data retrieval. It works like an index in a book—you can jump directly to relevant pages instead of reading every page.

**Benefits:**
- ✅ Faster SELECT queries and WHERE clauses
- ✅ Faster JOIN operations
- ✅ Faster ORDER BY and GROUP BY

**Drawbacks:**
- ❌ Slows down INSERT, UPDATE, DELETE operations
- ❌ Uses additional disk space
- ❌ Needs maintenance

**Example:**
```sql
-- Without index (slow for large tables)
SELECT * FROM employees WHERE email = 'john@email.com';

-- Create index
CREATE INDEX idx_email ON employees(email);

-- Same query now executes faster
SELECT * FROM employees WHERE email = 'john@email.com';

-- Create composite index (multiple columns)
CREATE INDEX idx_lastname_firstname ON employees(last_name, first_name);

-- Check indexes on table
SHOW INDEXES FROM employees;

-- Remove index
DROP INDEX idx_email ON employees;
```

**QA Use Case:**
```sql
-- Check if critical columns have indexes
SELECT TABLE_NAME, COLUMN_NAME
FROM INFORMATION_SCHEMA.STATISTICS
WHERE TABLE_SCHEMA = 'your_database'
AND TABLE_NAME = 'employees';
```

---

### Q16: Explain different types of indexes

**Answer:**

**1. Primary Index (Clustered Index)**
- Automatically created on primary key
- Only one per table
- Data physically sorted by this index

```sql
-- Primary key automatically creates clustered index
CREATE TABLE users (
    user_id INT PRIMARY KEY,
    username VARCHAR(100)
);
```

**2. Unique Index**
- Ensures all values are unique
- Created on UNIQUE KEY constraint

```sql
CREATE UNIQUE INDEX idx_unique_email ON users(email);
```

**3. Non-Clustered Index (Secondary Index)**
- Multiple per table
- Points to clustered index
- Doesn't affect physical order

```sql
CREATE INDEX idx_username ON users(username);
CREATE INDEX idx_created_date ON users(created_date);
```

**4. Composite Index**
- Index on multiple columns

```sql
CREATE INDEX idx_city_state ON users(city, state);

-- Query that benefits from this index
SELECT * FROM users WHERE city = 'NYC' AND state = 'NY';
```

**5. Full-Text Index**
- Used for text searching

```sql
CREATE FULLTEXT INDEX idx_description ON products(description);

-- Full-text search
SELECT * FROM products WHERE MATCH(description) AGAINST('laptop' IN BOOLEAN MODE);
```

**6. Spatial Index**
- For geographic/geometric data

```sql
CREATE SPATIAL INDEX idx_location ON stores(location);
```

---

### Q17: How do you retrieve unique records?

**Answer:**
Use the DISTINCT keyword to remove duplicate records.

**Example:**
```sql
CREATE TABLE orders (
    order_id INT,
    customer_id INT,
    product VARCHAR(50),
    amount DECIMAL(10, 2)
);

INSERT INTO orders VALUES 
(1, 1, 'Laptop', 1200),
(2, 2, 'Mouse', 25),
(3, 1, 'Keyboard', 75),
(4, 3, 'Monitor', 300),
(5, 1, 'Laptop', 1200);

-- Get unique customers
SELECT DISTINCT customer_id FROM orders;

/* OUTPUT:
customer_id
-----------
1
2
3
*/

-- Get unique products
SELECT DISTINCT product FROM orders;

/* OUTPUT:
product
-------
Laptop
Mouse
Keyboard
Monitor
*/

-- Count unique customers
SELECT COUNT(DISTINCT customer_id) as unique_customers FROM orders;

/* OUTPUT:
unique_customers
----------------
3
*/
```

**QA Use Case:**
```sql
-- Verify no duplicate product SKUs
SELECT COUNT(*) as duplicate_skus
FROM (
    SELECT sku, COUNT(*) 
    FROM products 
    GROUP BY sku 
    HAVING COUNT(*) > 1
) as duplicates;
-- Expected: 0
```

---

### Q18: How can you prevent SQL injection?

**Answer:**
SQL injection is a security vulnerability where attackers insert malicious SQL code. It's important for QA to understand prevention methods.

**Vulnerable Code (UNSAFE):**
```python
# Python example
user_input = "' OR '1'='1"
query = "SELECT * FROM users WHERE username = '" + user_input + "'"
# Results in: SELECT * FROM users WHERE username = '' OR '1'='1'
# This returns ALL records!
```

**Prevention Method 1: Parameterized Queries (Prepared Statements)**
```java
// Java example - SAFE
String username = request.getParameter("username");
String query = "SELECT * FROM users WHERE username = ?";
PreparedStatement stmt = connection.prepareStatement(query);
stmt.setString(1, username);  // Parameter binding
ResultSet rs = stmt.executeQuery();
```

**Prevention Method 2: Parameterized Queries in Python**
```python
# Python with parameterization - SAFE
import mysql.connector

username = request.form['username']
query = "SELECT * FROM users WHERE username = %s"
cursor.execute(query, (username,))  # Parameters separated from query
results = cursor.fetchall()
```

**Prevention Method 3: Stored Procedures**
```sql
-- Create stored procedure
CREATE PROCEDURE GetUserByUsername(IN user_name VARCHAR(100))
BEGIN
    SELECT * FROM users WHERE username = user_name;
END;

-- Call procedure (safe)
CALL GetUserByUsername('john_doe');
```

**Prevention Method 4: Input Validation**
```java
// Validate input before using in query
String username = request.getParameter("username");

// Check for dangerous characters
if (username.contains("'") || username.contains("--") || username.contains(";")) {
    throw new IllegalArgumentException("Invalid username");
}
```

**QA Testing for SQL Injection:**
```sql
-- Test cases to detect SQL injection vulnerabilities
-- Test 1: Single quote
Input: ' OR '1'='1
Expected: Error or no results, not all records

-- Test 2: Comment
Input: admin' --
Expected: Error or handled gracefully

-- Test 3: UNION-based
Input: ' UNION SELECT * FROM passwords --
Expected: Error or handled gracefully

-- Test 4: Time-based blind
Input: ' OR SLEEP(5) --
Expected: Query completes in normal time, not delayed
```

---

### Q19: What is a Subquery and provide an example

**Answer:**
A subquery (inner query or nested query) is a query within another SQL query. The subquery provides data to the outer query.

**Types of Subqueries:**

**1. Scalar Subquery (returns single value)**
```sql
-- Get average salary
SELECT AVG(salary) FROM employees;  -- Returns: 95000

-- Use in WHERE clause
SELECT emp_name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

/* OUTPUT:
emp_name | salary
---------|-------
Alice    | 100000
Diana    | 100000
*/
```

**2. Row Subquery (returns single row with multiple columns)**
```sql
-- Find employee with same name and department as 'John'
SELECT * FROM employees
WHERE (emp_name, dept_id) = (
    SELECT emp_name, dept_id 
    FROM employees 
    WHERE emp_id = 1
);
```

**3. Table Subquery (returns multiple rows and columns)**
```sql
-- Find all employees earning more than average
SELECT emp_name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

**4. Correlated Subquery (subquery references outer query)**
```sql
-- Find employees earning more than their department average
SELECT e1.emp_name, e1.salary, e1.dept_id
FROM employees e1
WHERE e1.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.dept_id = e1.dept_id
);
```

**5. Subquery in FROM clause (Derived Table)**
```sql
SELECT * FROM (
    SELECT emp_name, salary,
           AVG(salary) OVER (PARTITION BY dept_id) as dept_avg
    FROM employees
) as emp_with_avg
WHERE salary > dept_avg;
```

**QA Use Case:**
```sql
-- Verify all orders have at least one order item
SELECT COUNT(*) as orphaned_orders
FROM orders o
WHERE NOT EXISTS (
    SELECT 1 FROM order_items oi 
    WHERE oi.order_id = o.order_id
);
-- Expected: 0
```

---

### Q20: Explain DELETE, TRUNCATE, and DROP

**Answer:**
Three different ways to remove data, each with different implications.

**Comparison Table:**

| Feature | DELETE | TRUNCATE | DROP |
|---------|--------|----------|------|
| **Type** | DML (Data Manipulation Language) | DDL (Data Definition Language) | DDL |
| **Removes** | Data (rows) | Data (rows) | Table structure + data |
| **WHERE clause** | Supported | Not supported | N/A |
| **Identity reset** | Not reset | Resets to seed | N/A |
| **Triggers** | Fires DELETE triggers | Doesn't fire triggers | Doesn't fire triggers |
| **Transaction** | Can rollback in transaction | Cannot rollback (in some DBs) | Cannot rollback |
| **Speed** | Slower | Faster | Fastest |
| **Disk space** | Keeps allocated space | Deallocates space | Deallocates space |
| **Recovery** | Can recover individual rows | Cannot recover rows easily | Cannot recover table |

**DELETE Example:**
```sql
-- Delete specific rows
DELETE FROM employees WHERE dept_id = 5;

-- Delete with WHERE clause
DELETE FROM employees WHERE salary < 50000;

-- Delete all rows (but keeps table structure)
DELETE FROM employees;

-- Rollback possible in transaction
BEGIN TRANSACTION;
DELETE FROM employees;
ROLLBACK;  -- Data restored

-- Check deleted rows
SELECT COUNT(*) FROM employees;  -- Returns 0
```

**TRUNCATE Example:**
```sql
-- Remove all rows quickly
TRUNCATE TABLE employees;

-- Identity/auto-increment resets to seed value
INSERT INTO employees(emp_name, salary) VALUES ('Alice', 100000);
-- emp_id will be 1 (if original was 1000)

-- Cannot rollback in some databases
BEGIN TRANSACTION;
TRUNCATE TABLE employees;
ROLLBACK;  -- May not work depending on DB
```

**DROP Example:**
```sql
-- Remove entire table
DROP TABLE employees;

-- Table structure, indexes, triggers all removed
-- Try to query
SELECT * FROM employees;  -- ERROR: Table doesn't exist
```

**Real-World Scenarios:**
```sql
-- Scenario 1: Remove inactive users (use DELETE)
DELETE FROM users WHERE status = 'inactive' AND last_login < DATE_SUB(NOW(), INTERVAL 2 YEAR);

-- Scenario 2: Clear temporary data table (use TRUNCATE)
TRUNCATE TABLE temp_upload_data;

-- Scenario 3: Remove obsolete table (use DROP)
DROP TABLE legacy_user_logs;

-- Scenario 4: Safe deletion with transaction (use DELETE)
START TRANSACTION;
DELETE FROM orders WHERE order_date < '2020-01-01';
-- Review changes before committing
COMMIT;
```

**QA Use Case:**
```sql
-- Data cleanup test
-- 1. Count records before
SELECT COUNT(*) FROM test_data;  -- Returns 1000

-- 2. Delete test data
DELETE FROM test_data WHERE test_run_id = 'AUTOMATION_RUN_001';

-- 3. Verify deletion
SELECT COUNT(*) FROM test_data;  -- Returns 950

-- 4. Verify count matches expected
SELECT COUNT(*) FROM test_data WHERE test_run_id = 'AUTOMATION_RUN_001';  -- Returns 0
```

---

### Q21: What is a View?

**Answer:**
A view is a virtual table based on SQL queries. It doesn't store data physically; instead, it displays data from underlying tables dynamically.

**Characteristics:**
- ✅ Virtual table (no physical storage)
- ✅ Based on SELECT queries
- ✅ Can simplify complex queries
- ✅ Provides security (hide sensitive columns)
- ✅ Can be updated (limited)

**Example:**
```sql
-- Create a simple view
CREATE VIEW active_employees AS
SELECT emp_id, emp_name, salary, department
FROM employees
WHERE status = 'active';

-- Query the view (same as querying table)
SELECT * FROM active_employees;

-- Create a complex view with JOINs
CREATE VIEW employee_departments AS
SELECT e.emp_id, e.emp_name, e.salary, d.dept_name, d.location
FROM employees e
INNER JOIN departments d ON e.dept_id = d.dept_id
WHERE e.status = 'active';

-- Query complex view
SELECT * FROM employee_departments WHERE dept_name = 'IT';

-- Create a view hiding sensitive data
CREATE VIEW public_employee_info AS
SELECT emp_id, emp_name, department
FROM employees
WHERE status = 'active';  -- Hides salary information

-- Update view (updates underlying table)
UPDATE active_employees SET salary = 105000 WHERE emp_id = 1;

-- Delete view
DROP VIEW active_employees;
```

**QA Use Case:**
```sql
-- Create view for test data validation
CREATE VIEW test_data_summary AS
SELECT 
    COUNT(*) as total_records,
    COUNT(DISTINCT user_id) as unique_users,
    MIN(created_at) as oldest_record,
    MAX(created_at) as newest_record
FROM test_data
WHERE test_run_id = 'AUTOMATION_RUN_001';

-- Run test data quality checks
SELECT * FROM test_data_summary;
```

---

### Q22: What is a Stored Procedure?

**Answer:**
A stored procedure is a pre-written, reusable collection of SQL statements stored in the database that performs a specific task. It can accept parameters and return values.

**Advantages:**
- ✅ Reusable code
- ✅ Reduced network traffic
- ✅ Better security
- ✅ Encapsulation of business logic
- ✅ Performance optimization

**Example:**
```sql
-- Create simple stored procedure
DELIMITER //
CREATE PROCEDURE GetEmployeeByID(IN emp_id INT)
BEGIN
    SELECT * FROM employees WHERE emp_id = emp_id;
END //
DELIMITER ;

-- Call procedure
CALL GetEmployeeByID(1);

-- Procedure with OUTPUT parameter
DELIMITER //
CREATE PROCEDURE GetEmployeeCount(OUT emp_count INT)
BEGIN
    SELECT COUNT(*) INTO emp_count FROM employees;
END //
DELIMITER ;

-- Call with output
CALL GetEmployeeCount(@count);
SELECT @count;  -- Returns employee count

-- Procedure with INPUT and OUTPUT parameters
DELIMITER //
CREATE PROCEDURE GetDepartmentStats(
    IN dept_id INT,
    OUT total_employees INT,
    OUT avg_salary DECIMAL(10, 2)
)
BEGIN
    SELECT COUNT(*), AVG(salary)
    INTO total_employees, avg_salary
    FROM employees
    WHERE dept_id = dept_id;
END //
DELIMITER ;

-- Call procedure
CALL GetDepartmentStats(1, @emp_count, @salary);
SELECT @emp_count as employees, @salary as average_salary;

-- Procedure with flow control
DELIMITER //
CREATE PROCEDURE GiveRaise(IN emp_id INT, IN raise_percent DECIMAL(5, 2))
BEGIN
    DECLARE current_salary DECIMAL(10, 2);
    
    -- Get current salary
    SELECT salary INTO current_salary 
    FROM employees 
    WHERE emp_id = emp_id;
    
    -- Check if salary exists
    IF current_salary IS NULL THEN
        SELECT 'Employee not found' as message;
    ELSE
        -- Update salary
        UPDATE employees 
        SET salary = salary * (1 + raise_percent / 100)
        WHERE emp_id = emp_id;
        SELECT 'Raise applied successfully' as message;
    END IF;
END //
DELIMITER ;

-- Call procedure with conditional logic
CALL GiveRaise(1, 10);  -- 10% raise
```

**QA Testing Stored Procedures:**
```sql
-- Test 1: Verify procedure exists
SELECT ROUTINE_NAME FROM INFORMATION_SCHEMA.ROUTINES 
WHERE ROUTINE_NAME = 'GetEmployeeByID';

-- Test 2: Test with valid input
CALL GetEmployeeByID(1);

-- Test 3: Test with invalid input
CALL GetEmployeeByID(999);  -- Should handle gracefully

-- Test 4: Test output parameters
CALL GetDepartmentStats(1, @count, @avg);
SELECT @count, @avg;

-- Test 5: Verify data consistency after procedure execution
BEGIN;
CALL GiveRaise(1, 5);
-- Verify update
SELECT salary FROM employees WHERE emp_id = 1;
ROLLBACK;  -- Test rollback
```

---

### Q23: What is a Trigger?

**Answer:**
A trigger is a special type of stored procedure that automatically executes (or "fires") in response to specific events on a particular table or view in a database.

**Trigger Types:**
- BEFORE/AFTER
- INSERT/UPDATE/DELETE
- FOR EACH ROW

**Example:**
```sql
-- Create a table to log changes
CREATE TABLE salary_audit (
    audit_id INT AUTO_INCREMENT PRIMARY KEY,
    emp_id INT,
    old_salary DECIMAL(10, 2),
    new_salary DECIMAL(10, 2),
    change_date TIMESTAMP,
    action VARCHAR(10)
);

-- Create trigger to log salary changes
DELIMITER //
CREATE TRIGGER salary_update_trigger
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
    IF NEW.salary != OLD.salary THEN
        INSERT INTO salary_audit(emp_id, old_salary, new_salary, change_date, action)
        VALUES(NEW.emp_id, OLD.salary, NEW.salary, NOW(), 'UPDATE');
    END IF;
END //
DELIMITER ;

-- Create trigger to log deletions
DELIMITER //
CREATE TRIGGER salary_delete_trigger
BEFORE DELETE ON employees
FOR EACH ROW
BEGIN
    INSERT INTO salary_audit(emp_id, old_salary, new_salary, change_date, action)
    VALUES(OLD.emp_id, OLD.salary, NULL, NOW(), 'DELETE');
END //
DELIMITER ;

-- Update employee salary (triggers fire automatically)
UPDATE employees SET salary = 110000 WHERE emp_id = 1;

-- Check audit log
SELECT * FROM salary_audit;

-- Create trigger for validation
DELIMITER //
CREATE TRIGGER validate_salary
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
    IF NEW.salary < 0 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Salary cannot be negative';
    END IF;
END //
DELIMITER ;

-- View triggers
SHOW TRIGGERS FROM your_database;

-- Delete trigger
DROP TRIGGER salary_update_trigger;
```

**QA Use Case:**
```sql
-- Verify triggers are working correctly
-- Test 1: Insert new employee
INSERT INTO employees(emp_name, salary, dept_id) VALUES('Test User', 50000, 1);

-- Test 2: Update salary
UPDATE employees SET salary = 55000 WHERE emp_id = (SELECT MAX(emp_id) FROM employees);

-- Test 3: Check audit log was updated
SELECT * FROM salary_audit ORDER BY audit_id DESC LIMIT 1;
-- Expected: New record in audit table

-- Test 4: Verify invalid data is rejected
INSERT INTO employees(emp_name, salary, dept_id) VALUES('Invalid', -1000, 1);
-- Expected: Error due to trigger validation
```

---

### Q24: What is the difference between UNION and UNION ALL?

**Answer:**

| UNION | UNION ALL |
|-------|-----------|
| Removes duplicate rows | Includes all rows (duplicates) |
| Slower (removes duplicates) | Faster (no duplicate check) |
| Uses more resources | Uses fewer resources |

**Example:**
```sql
CREATE TABLE us_customers (
    customer_id INT,
    customer_name VARCHAR(100),
    country VARCHAR(50)
);

CREATE TABLE international_customers (
    customer_id INT,
    customer_name VARCHAR(100),
    country VARCHAR(50)
);

INSERT INTO us_customers VALUES
(1, 'John Doe', 'USA'),
(2, 'Jane Smith', 'USA'),
(3, 'Bob Johnson', 'USA');

INSERT INTO international_customers VALUES
(4, 'Alice Brown', 'Canada'),
(3, 'Bob Johnson', 'USA'),  -- Duplicate
(5, 'Charlie Wilson', 'UK');

-- UNION (removes duplicates)
SELECT * FROM us_customers
UNION
SELECT * FROM international_customers;

/* OUTPUT:
customer_id | customer_name    | country
------------|-----------------|--------
1           | John Doe        | USA
2           | Jane Smith      | USA
3           | Bob Johnson     | USA
4           | Alice Brown     | Canada
5           | Charlie Wilson  | UK
(5 rows - Bob Johnson appears once)
*/

-- UNION ALL (includes all rows)
SELECT * FROM us_customers
UNION ALL
SELECT * FROM international_customers;

/* OUTPUT:
customer_id | customer_name    | country
------------|-----------------|--------
1           | John Doe        | USA
2           | Jane Smith      | USA
3           | Bob Johnson     | USA
4           | Alice Brown     | Canada
3           | Bob Johnson     | USA
5           | Charlie Wilson  | UK
(6 rows - Bob Johnson appears twice)
*/
```

**Performance Comparison:**
```sql
-- UNION (slower)
SELECT email FROM users
UNION
SELECT email FROM archived_users;

-- UNION ALL (faster if you don't mind duplicates)
SELECT email FROM users
UNION ALL
SELECT email FROM archived_users;
```

---

### Q25: What is ACID Property?

**Answer:**
ACID is an acronym for four essential properties that guarantee reliable database transactions.

**A - Atomicity**
- Transaction is "all or nothing"
- Either all operations complete or none complete

```sql
BEGIN TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;
COMMIT;  -- Both succeed or both fail

-- If error occurs before COMMIT
ROLLBACK;  -- Both operations are undone
```

**C - Consistency**
- Database moves from one consistent state to another
- All rules/constraints are maintained

```sql
-- Before transaction:
accounts: total balance = $10,000

BEGIN TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;
COMMIT;

-- After transaction:
accounts: total balance = $10,000 (still consistent)
```

**I - Isolation**
- Concurrent transactions don't interfere with each other
- Each transaction executes in isolation

```sql
-- Transaction 1 (User A)
BEGIN TRANSACTION;
SELECT balance FROM accounts WHERE account_id = 1;  -- Reads $1000

-- Transaction 2 (User B) happens in between
BEGIN TRANSACTION;
UPDATE accounts SET balance = 500 WHERE account_id = 1;
COMMIT;

-- Transaction 1 continues
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
COMMIT;  -- Uses isolated/original value ($1000), not User B's change
```

**D - Durability**
- Once committed, data persists even after system failures
- Data survives hardware failure, power loss, etc.

```sql
BEGIN TRANSACTION;
INSERT INTO orders VALUES(1001, 'John', 500);
COMMIT;  -- Data written to disk

-- Even if server crashes immediately after, data is preserved
```

**QA Testing ACID Properties:**
```sql
-- Test Atomicity
START TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;
-- Verify both succeed or both fail
SELECT SUM(balance) FROM accounts;  -- Should remain constant
COMMIT;

-- Test Consistency
-- Verify all constraints are maintained
SELECT * FROM accounts WHERE balance < 0;  -- Should be empty

-- Test Isolation
-- Run concurrent transactions and verify no interference
-- Use different database connections/sessions

-- Test Durability
-- Insert data, commit, forcefully shut down server
-- Restart and verify data persists
SELECT * FROM orders WHERE order_id = 1001;
```

---

## Database Design & Relationships

### Q26: What is a Composite Key?

**Answer:**
A composite key is a combination of two or more columns that together form a unique identifier for each row. Individually, columns may not be unique, but their combination is.

**Example:**
```sql
CREATE TABLE student_courses (
    student_id INT NOT NULL,
    course_id INT NOT NULL,
    semester VARCHAR(20),
    grade CHAR(1),
    PRIMARY KEY (student_id, course_id, semester)
);

INSERT INTO student_courses VALUES
(1, 101, 'Fall 2024', 'A'),
(1, 101, 'Spring 2025', 'B'),  -- Same student-course, different semester
(1, 102, 'Fall 2024', 'A'),     -- Same student, different course
(2, 101, 'Fall 2024', 'B');

-- This is valid (same student_id-course_id but different semester)
-- This would be invalid (duplicate combination)
-- INSERT INTO student_courses VALUES(1, 101, 'Fall 2024', 'C'); -- ERROR!
```

**QA Use Case:**
```sql
-- Verify composite key uniqueness
SELECT student_id, course_id, semester, COUNT(*) 
FROM student_courses 
GROUP BY student_id, course_id, semester 
HAVING COUNT(*) > 1;
-- Should return 0 rows
```

---

### Q27: What is a Self Join?

**Answer:**
A self join is a join of a table to itself. It's useful when comparing rows within the same table.

**Example:**
```sql
CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    emp_name VARCHAR(100),
    manager_id INT,
    FOREIGN KEY (manager_id) REFERENCES employees(emp_id)
);

INSERT INTO employees VALUES
(1, 'Alice Johnson', NULL),      -- CEO, no manager
(2, 'Bob Smith', 1),             -- Alice is manager
(3, 'Charlie Brown', 1),         -- Alice is manager
(4, 'Diana Prince', 2),          -- Bob is manager
(5, 'Eve Wilson', 2);            -- Bob is manager

-- Find employee and their manager
SELECT e.emp_name as employee, m.emp_name as manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.emp_id;

/* OUTPUT:
employee        | manager
----------------|-------------
Alice Johnson   | NULL
Bob Smith       | Alice Johnson
Charlie Brown   | Alice Johnson
Diana Prince    | Bob Smith
Eve Wilson      | Bob Smith
*/

-- Find employees at same level (siblings)
SELECT e1.emp_name as employee1, e2.emp_name as employee2
FROM employees e1
JOIN employees e2 ON e1.manager_id = e2.manager_id
WHERE e1.emp_id < e2.emp_id;

/* OUTPUT:
employee1       | employee2
----------------|---------------
Bob Smith       | Charlie Brown
Diana Prince    | Eve Wilson
*/
```

**QA Use Case:**
```sql
-- Verify hierarchical data integrity
SELECT e.emp_name, e.manager_id, m.emp_name as manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.emp_id
WHERE e.manager_id IS NOT NULL AND m.emp_name IS NULL;
-- Should return 0 rows (no orphaned manager references)
```

---

### Q28: What is an Alias in SQL?

**Answer:**
An alias is a temporary name assigned to a table or column for the duration of a query. It improves readability and helps in avoiding ambiguity.

**Column Alias:**
```sql
SELECT emp_name as employee_name, salary as employee_salary
FROM employees;

-- With calculations
SELECT emp_name, salary * 12 as annual_salary
FROM employees;

/* OUTPUT:
employee_name | employee_salary
--------------|----------------
John Doe      | 1200000
Jane Smith    | 1140000
*/
```

**Table Alias:**
```sql
-- Without alias (ambiguous which salary refers to)
SELECT e.emp_name, e.salary, d.dept_name
FROM employees e
INNER JOIN departments d ON e.dept_id = d.dept_id;

-- Improves readability in complex queries
SELECT e.emp_name as employee, d.dept_name as department, e.salary
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.dept_id;
```

**Alias in Subqueries:**
```sql
SELECT avg_salary FROM (
    SELECT AVG(salary) as avg_salary FROM employees
) as salary_stats;
```

---

### Q29: What is the LIKE clause?

**Answer:**
The LIKE clause is used to search for a specified pattern in a column. It uses wildcards for pattern matching.

**Wildcards:**
- `%` - Represents zero or more characters
- `_` - Represents exactly one character

**Examples:**
```sql
CREATE TABLE products (
    product_id INT,
    product_name VARCHAR(100)
);

INSERT INTO products VALUES
(1, 'Apple iPhone 15'),
(2, 'Samsung Galaxy S24'),
(3, 'Apple MacBook Pro'),
(4, 'Google Pixel 9'),
(5, 'OnePlus 12');

-- Find products starting with 'Apple'
SELECT * FROM products WHERE product_name LIKE 'Apple%';

/* OUTPUT:
product_id | product_name
-----------|-------------------
1          | Apple iPhone 15
3          | Apple MacBook Pro
*/

-- Find products containing 'Galaxy'
SELECT * FROM products WHERE product_name LIKE '%Galaxy%';

/* OUTPUT:
product_id | product_name
-----------|----------------------
2          | Samsung Galaxy S24
*/

-- Find products with specific length pattern (iPhone)
SELECT * FROM products WHERE product_name LIKE '%Phone%';

/* OUTPUT:
product_id | product_name
-----------|--------------------
1          | Apple iPhone 15
*/

-- Find products ending with number
SELECT * FROM products WHERE product_name LIKE '%_';

-- Case-insensitive search
SELECT * FROM products WHERE product_name LIKE 'apple%';  -- Also finds 'Apple'

-- NOT LIKE
SELECT * FROM products WHERE product_name NOT LIKE 'Apple%';
```

**QA Use Case:**
```sql
-- Find users with invalid email format
SELECT * FROM users 
WHERE email NOT LIKE '%@%.%';  -- Missing @ or . for domain

-- Find duplicate product codes
SELECT * FROM products 
WHERE product_code LIKE 'DUP_%';
```

---

### Q30: Explain Aggregate Functions

**Answer:**
Aggregate functions perform calculations on a set of values and return a single result.

**Common Aggregate Functions:**

**COUNT()** - Counts rows
```sql
SELECT COUNT(*) as total_employees FROM employees;
SELECT COUNT(emp_id) as count_ids FROM employees;  -- Same as above
SELECT COUNT(DISTINCT department) as unique_departments FROM employees;
```

**SUM()** - Adds values
```sql
SELECT SUM(salary) as total_payroll FROM employees;
SELECT SUM(salary) as it_payroll FROM employees WHERE department = 'IT';
```

**AVG()** - Calculates average
```sql
SELECT AVG(salary) as avg_salary FROM employees;
SELECT AVG(salary) as avg_by_dept, department 
FROM employees 
GROUP BY department;
```

**MIN()** - Finds minimum value
```sql
SELECT MIN(salary) as lowest_salary FROM employees;
SELECT MIN(age) as youngest_employee_age FROM employees;
```

**MAX()** - Finds maximum value
```sql
SELECT MAX(salary) as highest_salary FROM employees;
SELECT MAX(hire_date) as most_recent_hire FROM employees;
```

**Complete Example:**
```sql
CREATE TABLE sales (
    sale_id INT,
    salesperson VARCHAR(50),
    amount DECIMAL(10, 2),
    sale_date DATE
);

INSERT INTO sales VALUES
(1, 'John', 5000, '2024-01-15'),
(2, 'Jane', 7500, '2024-01-20'),
(3, 'John', 6000, '2024-01-25'),
(4, 'Bob', 4500, '2024-02-10'),
(5, 'Jane', 8000, '2024-02-15');

SELECT 
    COUNT(*) as total_sales,
    SUM(amount) as total_revenue,
    AVG(amount) as avg_sale_value,
    MIN(amount) as lowest_sale,
    MAX(amount) as highest_sale
FROM sales;

/* OUTPUT:
total_sales | total_revenue | avg_sale_value | lowest_sale | highest_sale
------------|---------------|----------------|-------------|-------------
5           | 30500         | 6100.00        | 4500        | 8000
*/

-- With GROUP BY
SELECT 
    salesperson,
    COUNT(*) as num_sales,
    SUM(amount) as total_revenue,
    AVG(amount) as avg_sale_value
FROM sales
GROUP BY salesperson;

/* OUTPUT:
salesperson | num_sales | total_revenue | avg_sale_value
------------|-----------|---------------|---------------
John        | 2         | 11000         | 5500.00
Jane        | 2         | 15500         | 7750.00
Bob         | 1         | 4500          | 4500.00
*/
```

**QA Use Case:**
```sql
-- Validate test data distribution
SELECT 
    test_status,
    COUNT(*) as count,
    ROUND(COUNT(*) * 100.0 / (SELECT COUNT(*) FROM test_results), 2) as percentage
FROM test_results
GROUP BY test_status;

/* Expected output for balanced test distribution:
test_status | count | percentage
------------|-------|----------
PASS        | 850   | 85.00
FAIL        | 150   | 15.00
*/
```

---

### Q31-Q35: (Continuing with remaining questions...)

Due to length constraints, here are brief answers for Q31-Q35:

### Q31: What is ORDER BY and GROUP BY?

**ORDER BY** - Sorts result set  
**GROUP BY** - Groups rows sharing same values

```sql
-- ORDER BY (sorts results)
SELECT * FROM employees ORDER BY salary DESC;

-- GROUP BY (groups and aggregates)
SELECT department, COUNT(*) FROM employees GROUP BY department;
```

---

### Q32: What is the BETWEEN operator?

**Answer:**
BETWEEN selects values within a range (inclusive).

```sql
-- Find employees with salary between 50000 and 100000
SELECT * FROM employees 
WHERE salary BETWEEN 50000 AND 100000;

-- Find orders from date range
SELECT * FROM orders 
WHERE order_date BETWEEN '2024-01-01' AND '2024-12-31';
```

---

### Q33: What is LIMIT clause?

**Answer:**
LIMIT restricts the number of rows returned.

```sql
-- Get first 10 records
SELECT * FROM employees LIMIT 10;

-- Get records 11-20 (offset 10, limit 10)
SELECT * FROM employees LIMIT 10 OFFSET 10;

-- MySQL syntax
SELECT * FROM employees LIMIT 10, 10;
```

---

### Q34: What is a Transaction?

**Answer:**
A sequence of operations performed as a single logical unit. Either all complete or none complete.

```sql
BEGIN TRANSACTION;
UPDATE account SET balance = balance - 100 WHERE id = 1;
UPDATE account SET balance = balance + 100 WHERE id = 2;
COMMIT;  -- or ROLLBACK;
```

---

### Q35: How do you handle NULL values?

**Answer:**
Use IS NULL, IS NOT NULL, IFNULL(), COALESCE()

```sql
-- Check for NULL
SELECT * FROM employees WHERE manager_id IS NULL;

-- Handle NULL in calculations
SELECT emp_name, IFNULL(commission, 0) as commission FROM employees;
SELECT emp_name, COALESCE(nickname, first_name, 'Unknown') as name FROM employees;
```

---

## Performance & Optimization

### Q36: How do you retrieve the first N records?

**Answer:**
Use LIMIT clause (MySQL/PostgreSQL) or TOP clause (SQL Server).

```sql
-- MySQL/PostgreSQL
SELECT * FROM employees LIMIT 5;

-- SQL Server
SELECT TOP 5 * FROM employees;

-- Get records 6-15
SELECT * FROM employees LIMIT 5 OFFSET 5;

-- PostgreSQL
SELECT * FROM employees LIMIT 5 OFFSET 5;
```

---

### Q37: How do you get the current date in SQL?

**Answer:**
Different databases use different functions.

```sql
-- MySQL
SELECT NOW();           -- Returns datetime
SELECT CURDATE();       -- Returns date only
SELECT CURTIME();       -- Returns time only

-- SQL Server
SELECT GETDATE();
SELECT CAST(GETDATE() AS DATE);

-- PostgreSQL
SELECT NOW();
SELECT CURRENT_DATE;

-- Oracle
SELECT SYSDATE FROM DUAL;
```

---

### Q38: How to count total rows in a table?

**Answer:**

```sql
-- Count all rows
SELECT COUNT(*) as total_rows FROM employees;

-- Count non-NULL values in specific column
SELECT COUNT(manager_id) as managed_employees FROM employees;

-- Count distinct values
SELECT COUNT(DISTINCT department) as unique_departments FROM employees;
```

---

### Q39: How to add a new column to a table?

**Answer:**

```sql
-- Add single column
ALTER TABLE employees ADD COLUMN phone_number VARCHAR(20);

-- Add multiple columns
ALTER TABLE employees 
ADD COLUMN phone_number VARCHAR(20),
ADD COLUMN email VARCHAR(100);

-- Add with DEFAULT value
ALTER TABLE employees ADD COLUMN status VARCHAR(20) DEFAULT 'active';

-- Add with NOT NULL constraint
ALTER TABLE employees ADD COLUMN hire_date DATE NOT NULL;
```

---

### Q40: How do you rename a table?

**Answer:**

```sql
-- MySQL
ALTER TABLE old_table_name RENAME TO new_table_name;
RENAME TABLE old_table_name TO new_table_name;

-- SQL Server
EXEC sp_rename 'old_table_name', 'new_table_name';

-- PostgreSQL
ALTER TABLE old_table_name RENAME TO new_table_name;

-- Oracle
ALTER TABLE old_table_name RENAME TO new_table_name;
```

---

### Q41: How would you copy data from one table to another?

**Answer:**

```sql
-- Copy specific columns
INSERT INTO employees_backup (emp_id, emp_name, salary)
SELECT emp_id, emp_name, salary FROM employees;

-- Copy all columns
INSERT INTO employees_backup 
SELECT * FROM employees;

-- Copy with conditions
INSERT INTO employees_backup 
SELECT * FROM employees WHERE department = 'IT';

-- Create table and copy (MySQL)
CREATE TABLE employees_backup AS
SELECT * FROM employees;
```

---

### Q42-Q50: Real-World Scenarios for QA Testing

### Q42: How do you test data consistency?

**Answer:**

```sql
-- Verify referential integrity
SELECT COUNT(*) as orphaned_orders
FROM orders o
LEFT JOIN customers c ON o.customer_id = c.customer_id
WHERE c.customer_id IS NULL;
-- Expected: 0

-- Check for NULL values where not allowed
SELECT COUNT(*) as missing_values
FROM orders
WHERE order_date IS NULL OR customer_id IS NULL;
-- Expected: 0

-- Verify calculated fields
SELECT order_id
FROM orders
WHERE total_amount != (unit_price * quantity)
-- Expected: 0
```

---

### Q43: How do you find and fix data quality issues?

**Answer:**

```sql
-- Find duplicate emails
SELECT email, COUNT(*) 
FROM users 
GROUP BY email 
HAVING COUNT(*) > 1;

-- Find invalid data types
SELECT * FROM products WHERE price LIKE '%[^0-9.]%';

-- Find NULL where not expected
SELECT * FROM customers WHERE first_name IS NULL;

-- Remove duplicates
DELETE FROM users u1
WHERE EXISTS (
    SELECT 1 FROM users u2 
    WHERE u1.email = u2.email 
    AND u1.user_id > u2.user_id
);
```

---

### Q44: How do you test stored procedure with edge cases?

**Answer:**

```sql
-- Test 1: Normal case
CALL CalculateDiscount(100, @discount);
-- Expected: @discount = 5

-- Test 2: Boundary case
CALL CalculateDiscount(0, @discount);
-- Expected: @discount = 0

-- Test 3: Invalid input (negative)
CALL CalculateDiscount(-100, @discount);
-- Expected: Error or 0

-- Test 4: NULL input
CALL CalculateDiscount(NULL, @discount);
-- Expected: Error or NULL

-- Test 5: Large number
CALL CalculateDiscount(999999999, @discount);
-- Expected: Handles gracefully
```

---

### Q45: How do you identify and prevent SQL injection attacks?

**Answer:**

```sql
-- Vulnerable query
SELECT * FROM users WHERE username = '" + input + "'

-- Attack: ' OR '1'='1
-- Results in: SELECT * FROM users WHERE username = '' OR '1'='1'

-- SOLUTION 1: Use parameterized queries (Java)
String query = "SELECT * FROM users WHERE username = ?";
PreparedStatement stmt = connection.prepareStatement(query);
stmt.setString(1, username);

-- SOLUTION 2: Input validation
if (username.contains("'") || username.contains("--")) {
    throw new IllegalArgumentException("Invalid input");
}

-- SOLUTION 3: Use stored procedures
CALL GetUserByUsername(?)

-- Testing for vulnerabilities
Test inputs:
' OR '1'='1
'; DROP TABLE users; --
1' UNION SELECT * FROM passwords --
```

---

## Real-World Scenarios for QA Testing

### Q46: Database Testing for E-Commerce Application

**Answer:**

```sql
-- Test 1: Verify order data integrity
SELECT o.order_id, c.customer_id, p.product_id
FROM orders o
LEFT JOIN customers c ON o.customer_id = c.customer_id
LEFT JOIN products p ON o.product_id = p.product_id
WHERE c.customer_id IS NULL OR p.product_id IS NULL;
-- Expected: 0 orphaned records

-- Test 2: Verify inventory updates after order
SELECT p.product_id, p.stock_quantity, COUNT(oi.order_item_id) as ordered
FROM products p
LEFT JOIN order_items oi ON p.product_id = oi.product_id
GROUP BY p.product_id
HAVING p.stock_quantity < 0;
-- Expected: 0 (stock should never be negative)

-- Test 3: Verify payment status consistency
SELECT o.order_id, o.order_status, p.payment_status
FROM orders o
LEFT JOIN payments p ON o.order_id = p.order_id
WHERE (o.order_status = 'completed' AND p.payment_status != 'success')
OR (o.order_status = 'pending' AND p.payment_status = 'success');
-- Expected: 0 inconsistencies

-- Test 4: Find orders without any items
SELECT o.order_id, COUNT(oi.order_item_id) as item_count
FROM orders o
LEFT JOIN order_items oi ON o.order_id = oi.order_id
GROUP BY o.order_id
HAVING COUNT(oi.order_item_id) = 0;
-- Expected: 0 (orders should have at least 1 item)

-- Test 5: Verify pricing consistency
SELECT oi.order_item_id, oi.unit_price, p.current_price
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id
WHERE ABS(oi.unit_price - p.current_price) > 100;
-- May find price discrepancies (expected if price changed)
```

---

### Q47: Database Testing for Financial Application

**Answer:**

```sql
-- Test 1: Verify account balance after transaction
START TRANSACTION;
SELECT @before_balance := balance FROM accounts WHERE account_id = 1;
UPDATE accounts SET balance = balance - 500 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 500 WHERE account_id = 2;
SELECT @after_balance := balance FROM accounts WHERE account_id = 1;
-- Verify: @before_balance - 500 = @after_balance
ROLLBACK;

-- Test 2: Verify total amount in system (ACID - Consistency)
SELECT SUM(balance) as total_before FROM accounts;
-- Make transaction
SELECT SUM(balance) as total_after FROM accounts;
-- Both should be equal

-- Test 3: Find accounts with negative balance (should not exist)
SELECT * FROM accounts WHERE balance < 0;
-- Expected: 0

-- Test 4: Verify transaction logs match account balances
SELECT a.account_id, a.balance, 
       (SELECT SUM(CASE WHEN from_account = a.account_id THEN -amount ELSE amount END)
        FROM transactions WHERE to_account = a.account_id OR from_account = a.account_id) as transaction_sum
FROM accounts a
WHERE a.balance != transaction_sum;
-- Expected: 0 mismatches

-- Test 5: Find transactions without corresponding entries
SELECT t.transaction_id FROM transactions t
LEFT JOIN transaction_log tl ON t.transaction_id = tl.transaction_id
WHERE tl.transaction_id IS NULL;
-- Expected: 0 orphaned transactions
```

---

### Q48: Database Testing for User Management System

**Answer:**

```sql
-- Test 1: Verify no duplicate emails
SELECT email, COUNT(*) 
FROM users 
GROUP BY email 
HAVING COUNT(*) > 1;
-- Expected: 0

-- Test 2: Verify user roles are valid
SELECT DISTINCT role FROM users
WHERE role NOT IN ('admin', 'manager', 'user', 'guest');
-- Expected: 0 invalid roles

-- Test 3: Verify created_date <= updated_date
SELECT user_id 
FROM users 
WHERE created_date > updated_date;
-- Expected: 0

-- Test 4: Verify password is not stored in plain text (if applicable)
SELECT user_id FROM users 
WHERE password_hash IS NULL OR password_hash = '';
-- Expected: 0

-- Test 5: Find inactive users not updated in X days
SELECT user_id, DATEDIFF(NOW(), updated_date) as days_inactive
FROM users
WHERE updated_date < DATE_SUB(NOW(), INTERVAL 90 DAY)
AND status = 'active';
-- Verify these should be marked as inactive

-- Test 6: Verify audit trail
SELECT u.user_id, COUNT(al.log_id) as action_count
FROM users u
LEFT JOIN audit_log al ON u.user_id = al.user_id
GROUP BY u.user_id
ORDER BY action_count ASC;
```

---

### Q49: Query Optimization for Performance Testing

**Answer:**

```sql
-- BEFORE (slow query with multiple lookups)
SELECT * FROM orders o
WHERE EXISTS (
    SELECT 1 FROM customers c WHERE c.customer_id = o.customer_id AND c.status = 'active'
) AND EXISTS (
    SELECT 1 FROM products p WHERE p.product_id = o.product_id AND p.status = 'available'
)

-- AFTER (optimized with JOIN and index)
SELECT o.*
FROM orders o
INNER JOIN customers c ON o.customer_id = c.customer_id AND c.status = 'active'
INNER JOIN products p ON o.product_id = p.product_id AND p.status = 'available'
WHERE o.created_date > DATE_SUB(NOW(), INTERVAL 30 DAY)

-- Add indexes for better performance
CREATE INDEX idx_customer_status ON customers(customer_id, status);
CREATE INDEX idx_product_status ON products(product_id, status);
CREATE INDEX idx_order_created ON orders(created_date);

-- Check query execution plan
EXPLAIN SELECT o.* FROM orders o
INNER JOIN customers c ON o.customer_id = c.customer_id
WHERE c.status = 'active';
```

---

### Q50: Comprehensive Database Health Check for QA

**Answer:**

```sql
-- COMPREHENSIVE DATABASE HEALTH CHECK SCRIPT

-- 1. Table Structure Validation
SELECT 
    TABLE_NAME,
    COLUMN_COUNT,
    INDEX_COUNT
FROM (
    SELECT 
        t.TABLE_NAME,
        COUNT(DISTINCT c.COLUMN_NAME) as COLUMN_COUNT,
        COUNT(DISTINCT s.INDEX_NAME) as INDEX_COUNT
    FROM INFORMATION_SCHEMA.TABLES t
    LEFT JOIN INFORMATION_SCHEMA.COLUMNS c ON t.TABLE_NAME = c.TABLE_NAME
    LEFT JOIN INFORMATION_SCHEMA.STATISTICS s ON t.TABLE_NAME = s.TABLE_NAME
    WHERE t.TABLE_SCHEMA = 'your_database'
    GROUP BY t.TABLE_NAME
) stats;

-- 2. Data Quality Metrics
SELECT 
    'Total Users' as metric, COUNT(*) as count FROM users
UNION ALL
SELECT 'Active Users', COUNT(*) FROM users WHERE status = 'active'
UNION ALL
SELECT 'Inactive Users', COUNT(*) FROM users WHERE status = 'inactive'
UNION ALL
SELECT 'Users with NULL email', COUNT(*) FROM users WHERE email IS NULL
UNION ALL
SELECT 'Total Orders', COUNT(*) FROM orders
UNION ALL
SELECT 'Completed Orders', COUNT(*) FROM orders WHERE status = 'completed';

-- 3. Referential Integrity Check
SELECT 
    'Orphaned Orders' as issue,
    COUNT(*) as count
FROM orders o
LEFT JOIN customers c ON o.customer_id = c.customer_id
WHERE c.customer_id IS NULL
UNION ALL
SELECT 'Orphaned Order Items',
    COUNT(*)
FROM order_items oi
LEFT JOIN orders o ON oi.order_id = o.order_id
WHERE o.order_id IS NULL;

-- 4. Performance Metrics
SELECT 
    TABLE_NAME,
    ROUND(((data_length + index_length) / 1024 / 1024), 2) as table_size_mb,
    TABLE_ROWS
FROM INFORMATION_SCHEMA.TABLES
WHERE TABLE_SCHEMA = 'your_database'
ORDER BY table_size_mb DESC;

-- 5. Anomaly Detection
SELECT 
    'Records with future dates' as anomaly,
    COUNT(*) as count
FROM orders
WHERE created_date > NOW()
UNION ALL
SELECT 'Negative prices', COUNT(*) 
FROM products 
WHERE price < 0
UNION ALL
SELECT 'Invalid quantities', COUNT(*) 
FROM order_items 
WHERE quantity <= 0;

-- QA Health Check Report
SELECT 
    'Database Health Check' as report_name,
    NOW() as check_time,
    'PASSED' as status
WHERE NOT EXISTS (
    SELECT 1 FROM orders o
    LEFT JOIN customers c ON o.customer_id = c.customer_id
    WHERE c.customer_id IS NULL
);
```

---

## Key Takeaways for QA Professionals

### ✅ Do's:
- Learn to write efficient queries
- Understand data relationships
- Always validate data integrity
- Use parameterized queries to prevent SQL injection
- Test both normal and edge cases
- Keep audit trails for data changes
- Verify referential integrity
- Document database schema and relationships

### ❌ Don'ts:
- Never run production queries without testing
- Don't trust user input (prevent SQL injection)
- Don't ignore NULL values in queries
- Don't assume data is always valid
- Don't mix business logic in queries
- Don't use SELECT * in production (specify columns)
- Don't ignore indexes in large tables
- Don't perform calculations in WHERE clause unnecessarily

---

## SQL Cheat Sheet for Quick Reference

### Basic Operations
```sql
SELECT * FROM table;                    -- Retrieve data
INSERT INTO table VALUES (...);         -- Add data
UPDATE table SET col = value;           -- Modify data
DELETE FROM table WHERE condition;      -- Remove data
```

### Joins
```sql
INNER JOIN    -- Only matching rows
LEFT JOIN     -- All left + matching right
RIGHT JOIN    -- All right + matching left
FULL JOIN     -- All rows from both tables
CROSS JOIN    -- Cartesian product
```

### Functions
```sql
COUNT()       -- Count rows
SUM()         -- Add values
AVG()         -- Average
MIN()         -- Minimum
MAX()         -- Maximum
LIKE          -- Pattern matching
BETWEEN       -- Range selection
IN            -- Multiple values
```

---

## Conclusion

These 50 SQL questions cover the essentials for QA testing roles. Focus on:

1. **Understanding relationships** between tables
2. **Ensuring data integrity** through proper validation
3. **Preventing security issues** like SQL injection
4. **Optimizing queries** for performance
5. **Writing test scripts** to validate database behavior

Master these concepts, and you'll be well-equipped for SQL-related interview questions and real-world QA database testing! 🎯
