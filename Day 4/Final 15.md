### 1) What is normalization and why is it important?
Normalization is the process of organizing the fields and tables of a relational database to minimize redundancy and dependency. The main objectives are to:
- Eliminate redundant data (for example, storing the same data in multiple tables).
- Ensure data dependencies make sense (only storing related data in a table).

Normalization involves dividing large tables into smaller ones and defining relationships between them. The benefits of normalization include:
- Improved data integrity: Reduces the chances of data anomalies and inconsistencies.
- Reduced redundancy: Saves storage space and avoids duplication.
- Enhanced performance: Efficiently updates and manages the data.
- Simplified queries: Makes complex queries easier to write and understand.

### 2) Explain the concept of ACID properties in SQL.
ACID is an acronym that stands for Atomicity, Consistency, Isolation, and Durability. These properties ensure reliable processing of database transactions.
- **Atomicity**: Ensures that a transaction is all-or-nothing. If any part of the transaction fails, the entire transaction is rolled back, leaving the database unchanged.
- **Consistency**: Ensures that a transaction brings the database from one valid state to another, maintaining database invariants.
- **Isolation**: Ensures that transactions occur independently without interference. Intermediate results of a transaction are not visible to other transactions until the transaction is completed.
- **Durability**: Ensures that once a transaction is committed, it remains committed even in the case of a system failure. Changes are permanently stored in the database.

### 3) What are indexes and how do they improve query performance?
Indexes are special data structures associated with tables or views that improve the speed of data retrieval operations. They work similarly to the index in a book, allowing the database to find rows more quickly.
- **Improved Query Performance**: By reducing the amount of data the database needs to scan to find results, indexes speed up SELECT queries and WHERE clause searches.
- **Efficient Data Retrieval**: Indexes are particularly useful for large tables where full table scans would be time-consuming.
- **Types of Indexes**: Common types include B-tree indexes, hash indexes, and bitmap indexes.

### 4) What is a view in SQL and what are its uses?
A view is a virtual table based on the result set of an SQL query. It contains rows and columns, just like a real table, but does not store the data itself. Instead, it dynamically retrieves data from the underlying tables.
- **Uses of Views**:
  - Simplify complex queries: Provide a simpler interface to complex queries.
  - Security: Restrict access to specific rows or columns.
  - Data Abstraction: Present data in a different format without changing the underlying tables.
  - Consistency: Ensure that users see a consistent view of the data, even if the underlying data changes.

Example:
```sql
CREATE VIEW SalesView AS
SELECT order_id, customer_id, total_amount
FROM orders
WHERE status = 'Completed';
```

### 5) Explain the difference between DELETE and TRUNCATE commands.
- **DELETE**: Removes rows from a table based on a condition in the WHERE clause. It logs each row deletion and can be rolled back if needed. It also allows the use of WHERE clause to specify which rows to delete.
  ```sql
  DELETE FROM employees WHERE department = 'Sales';
  ```
- **TRUNCATE**: Removes all rows from a table, but does not log individual row deletions. It is faster than DELETE and cannot be rolled back once executed. TRUNCATE resets any auto-increment counters on the table.
  ```sql
  TRUNCATE TABLE employees;
  ```

Key Differences:
- **Speed**: TRUNCATE is faster because it does not log individual row deletions.
- **Rollback**: DELETE can be rolled back (if within a transaction), but TRUNCATE cannot.
- **Condition**: DELETE can specify which rows to remove with a WHERE clause, while TRUNCATE removes all rows.
- **Triggers**: DELETE activates triggers, while TRUNCATE does not activate triggers.

These concepts are fundamental to understanding and effectively using SQL in database management and operations.

### 6) What is a subquery and how is it different from a JOIN?
A **subquery** is a query nested within another SQL query. It is used to return data that will be used in the main query as a condition to further restrict the data to be retrieved. Subqueries can be used in SELECT, INSERT, UPDATE, or DELETE statements.

A **JOIN**, on the other hand, is used to combine rows from two or more tables based on a related column between them. JOINS are used to retrieve data from multiple tables and present it as a single set of results.

**Key Differences:**
- **Subquery**: Can return a single value or a set of values and is typically used to filter results or perform calculations.
- **JOIN**: Combines rows from two or more tables based on a related column and returns a single result set that includes columns from all joined tables.

**Example of a Subquery:**
```sql
SELECT employee_id, first_name
FROM employees
WHERE department_id = (SELECT department_id FROM departments WHERE department_name = 'Sales');
```

**Example of a JOIN:**
```sql
SELECT e.employee_id, e.first_name, d.department_name
FROM employees e
JOIN departments d ON e.department_id = d.department_id
WHERE d.department_name = 'Sales';
```

### 7) Describe the use of GROUP BY and HAVING clauses.
- **GROUP BY**: The `GROUP BY` clause is used to group rows that have the same values in specified columns into summary rows, like "total salary by department."
  ```sql
  SELECT department, SUM(salary)
  FROM employees
  GROUP BY department;
  ```

- **HAVING**: The `HAVING` clause is used to filter groups based on a condition, similar to how the `WHERE` clause is used to filter rows.
  ```sql
  SELECT department, SUM(salary)
  FROM employees
  GROUP BY department
  HAVING SUM(salary) > 50000;
  ```

### 8) What is the difference between a correlated subquery and a non-correlated subquery?
- **Non-Correlated Subquery**: A subquery that can be executed independently of the outer query. The subquery runs once and its result is used by the outer query.
  ```sql
  SELECT employee_id, first_name
  FROM employees
  WHERE department_id = (SELECT department_id FROM departments WHERE department_name = 'Sales');
  ```

- **Correlated Subquery**: A subquery that uses values from the outer query. The subquery is executed once for each row processed by the outer query.
  ```sql
  SELECT e1.employee_id, e1.first_name
  FROM employees e1
  WHERE e1.salary > (SELECT AVG(e2.salary) FROM employees e2 WHERE e2.department_id = e1.department_id);
  ```

### 9) How would you retrieve duplicate records from a table?
To retrieve duplicate records from a table, you can use the `GROUP BY` clause along with the `HAVING` clause.
```sql
SELECT column1, column2, COUNT(*)
FROM table_name
GROUP BY column1, column2
HAVING COUNT(*) > 1;
```
Example:
```sql
SELECT first_name, last_name, COUNT(*)
FROM employees
GROUP BY first_name, last_name
HAVING COUNT(*) > 1;
```

### 10) How do you handle NULL values in SQL? Provide an example query.
- **IS NULL**: Used to check for NULL values.
  ```sql
  SELECT * FROM employees WHERE department IS NULL;
  ```

- **IS NOT NULL**: Used to check for non-NULL values.
  ```sql
  SELECT * FROM employees WHERE department IS NOT NULL;
  ```

- **COALESCE()**: Returns the first non-NULL value in a list.
  ```sql
  SELECT COALESCE(department, 'Unknown') FROM employees;
  ```

- **IFNULL()**: Returns a specified value if the expression is NULL (MySQL).
  ```sql
  SELECT IFNULL(department, 'Unknown') FROM employees;
  ```

- **NULLIF()**: Returns NULL if the two expressions are equal.
  ```sql
  SELECT NULLIF(department, 'Unknown') FROM employees;
  ```

**Example Query Handling NULL Values:**
```sql
SELECT first_name, last_name, COALESCE(department, 'No Department') AS Department
FROM employees;
```

These concepts and techniques are essential for effectively working with SQL and managing database operations.

### 11) Explain the different wildcard characters used with the LIKE operator.

The `LIKE` operator is used in a `WHERE` clause to search for a specified pattern in a column. Wildcard characters are used to define the pattern.

- **%**: Represents zero, one, or multiple characters.
  - Example: `SELECT * FROM employees WHERE first_name LIKE 'J%';` (Finds any first name starting with 'J').

- **_**: Represents a single character.
  - Example: `SELECT * FROM employees WHERE first_name LIKE 'J_n';` (Finds any first name with 'J' as the first letter, any character as the second letter, and 'n' as the third letter).

- **[]**: Represents any single character within the brackets.
  - Example: `SELECT * FROM employees WHERE first_name LIKE 'J[oa]n';` (Finds 'Jan' or 'Jon').

- **[^]** or **[!]**: Represents any single character not within the brackets.
  - Example: `SELECT * FROM employees WHERE first_name LIKE 'J[^ao]n';` (Finds 'Jen', 'Jin', etc., but not 'Jan' or 'Jon').

### 12) Write a query to find the employee with the highest salary in each department.

```sql
SELECT department_id, employee_id, salary
FROM employees e
WHERE salary = (
  SELECT MAX(salary)
  FROM employees
  WHERE department_id = e.department_id
);
```

### 13) Given a sales table, write a query to calculate the total sales for each month.

Assuming the sales table has columns `sale_date` and `amount`:

```sql
SELECT 
  YEAR(sale_date) AS sale_year, 
  MONTH(sale_date) AS sale_month, 
  SUM(amount) AS total_sales
FROM sales
GROUP BY YEAR(sale_date), MONTH(sale_date)
ORDER BY sale_year, sale_month;
```

### 14) What is the difference between NOW() and CURRENT_DATE()?

- **NOW()**: Returns the current date and time.
  - Example: `SELECT NOW();` might return `2024-07-02 15:23:45`.

- **CURRENT_DATE()**: Returns the current date only (without time).
  - Example: `SELECT CURRENT_DATE();` might return `2024-07-02`.

### 15) What is BLOB and TEXT in MySQL?

- **BLOB** (Binary Large Object): A data type that can store large amounts of binary data, such as images, audio, or multimedia files.
  - **TINYBLOB**: Holds up to 255 bytes.
  - **BLOB**: Holds up to 65,535 bytes (64 KB).
  - **MEDIUMBLOB**: Holds up to 16,777,215 bytes (16 MB).
  - **LONGBLOB**: Holds up to 4,294,967,295 bytes (4 GB).

- **TEXT**: A data type used to store large amounts of text data.
  - **TINYTEXT**: Holds up to 255 characters.
  - **TEXT**: Holds up to 65,535 characters (64 KB).
  - **MEDIUMTEXT**: Holds up to 16,777,215 characters (16 MB).
  - **LONGTEXT**: Holds up to 4,294,967,295 characters (4 GB).

Both BLOB and TEXT types are used for storing large amounts of data, but BLOB is meant for binary data, and TEXT is meant for text data.

### 16) What is the difference between clustered and non-clustered index in SQL?

- **Clustered Index**:
  - **Definition**: A clustered index determines the physical order of data in a table. There can be only one clustered index per table because the data rows themselves can only be sorted in one order.
  - **Usage**: Used when you frequently need to retrieve data in a specific order.
  - **Performance**: Generally faster for range queries as the data is physically sorted.
  - **Example**: Primary keys often create clustered indexes by default.
  ```sql
  CREATE CLUSTERED INDEX idx_name ON table_name (column_name);
  ```

- **Non-Clustered Index**:
  - **Definition**: A non-clustered index does not alter the physical order of the table and maintains a separate structure from the data rows. There can be multiple non-clustered indexes on a table.
  - **Usage**: Useful for queries that search data by non-primary key columns.
  - **Performance**: Slower for range queries but faster for exact matches.
  - **Example**:
  ```sql
  CREATE NONCLUSTERED INDEX idx_name ON table_name (column_name);
  ```

### 17) How many Aggregate functions are available in SQL?

SQL provides several aggregate functions to perform calculations on a set of values and return a single value. The most common aggregate functions are:
1. **COUNT()**: Returns the number of rows.
2. **SUM()**: Returns the sum of values.
3. **AVG()**: Returns the average value.
4. **MIN()**: Returns the minimum value.
5. **MAX()**: Returns the maximum value.
6. **VARIANCE()**: Returns the variance of values.
7. **STDDEV()**: Returns the standard deviation of values.

Different SQL implementations may provide additional aggregate functions.

### 18) What are the syntax and use of the COALESCE function?

The `COALESCE` function returns the first non-NULL value from a list of expressions. If all expressions evaluate to NULL, it returns NULL.

**Syntax**:
```sql
COALESCE(expression1, expression2, ..., expressionN)
```

**Use**:
- To handle NULL values and provide a default value.
- To combine columns and provide a fallback when one column is NULL.

**Example**:
```sql
SELECT COALESCE(middle_name, first_name, 'No Name') AS name
FROM employees;
```

### 19) Are NULL values the same as zero or a blank space?

No, NULL values are different from zero and blank spaces:
- **NULL**: Represents the absence of any value, unknown or undefined.
- **Zero**: A numerical value representing nothing or zero quantity.
- **Blank Space**: A character value representing an empty string or space character.

**Example**:
- NULL: `SELECT * FROM employees WHERE salary IS NULL;`
- Zero: `SELECT * FROM employees WHERE salary = 0;`
- Blank Space: `SELECT * FROM employees WHERE first_name = '';`

### 20) What is the difference between the ‘HAVING’ clause and the ‘WHERE’ clause?

- **WHERE Clause**:
  - Used to filter rows before any groupings are made.
  - Cannot be used with aggregate functions.
  - Applied to individual rows in the table.
  - **Example**:
  ```sql
  SELECT department, COUNT(*)
  FROM employees
  WHERE salary > 50000
  GROUP BY department;
  ```

- **HAVING Clause**:
  - Used to filter groups after the `GROUP BY` clause has been applied.
  - Can be used with aggregate functions.
  - Applied to grouped rows.
  - **Example**:
  ```sql
  SELECT department, COUNT(*)
  FROM employees
  GROUP BY department
  HAVING COUNT(*) > 10;
  ```

In summary, use `WHERE` to filter rows before grouping, and `HAVING` to filter groups after aggregation.

### 21) What is Auto Increment in SQL?

**Auto Increment** is a feature in SQL that automatically generates a unique value for a primary key column each time a new row is inserted into a table. It is often used for generating unique identifiers, such as primary key values.

**Example in MySQL**:
```sql
CREATE TABLE employees (
    id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50)
);
```
In this example, the `id` column will automatically increment by 1 for each new row.

### 22) How do we avoid getting duplicate entries in a query without using the DISTINCT keyword?

To avoid duplicate entries without using the `DISTINCT` keyword, you can use the `GROUP BY` clause to group rows by the columns that you want to be unique.

**Example**:
```sql
SELECT first_name, last_name
FROM employees
GROUP BY first_name, last_name;
```

### 23) The difference between NVL and NVL2 functions?

**NVL** and **NVL2** are functions used in SQL to handle NULL values, primarily in Oracle.

- **NVL**:
  - Replaces NULL with a specified value.
  - **Syntax**: `NVL(expression, replacement)`
  - **Example**:
    ```sql
    SELECT NVL(commission_pct, 0) AS commission
    FROM employees;
    ```
    If `commission_pct` is NULL, it will be replaced with 0.

- **NVL2**:
  - Evaluates two different expressions based on whether the first expression is NULL or not.
  - **Syntax**: `NVL2(expression, value_if_not_null, value_if_null)`
  - **Example**:
    ```sql
    SELECT NVL2(commission_pct, commission_pct, 0) AS commission
    FROM employees;
    ```
    If `commission_pct` is not NULL, it returns `commission_pct`, otherwise it returns 0.

### 24) What is an ALIAS command?

**Alias** is used to give a temporary name to a table or column for the duration of a query. It helps to make query results more readable and manageable.

**Column Alias**:
```sql
SELECT first_name AS fname, last_name AS lname
FROM employees;
```
The result will display the columns as `fname` and `lname`.

**Table Alias**:
```sql
SELECT e.first_name, e.last_name
FROM employees e;
```
The table `employees` is referenced as `e` within the query.

### 25) How to create empty tables with the same structure as another table?

To create an empty table with the same structure as another table, you can use the `CREATE TABLE ... AS` statement with a `WHERE` clause that always evaluates to false, or use the `CREATE TABLE ... LIKE` statement (in MySQL).

**Using `CREATE TABLE ... AS`**:
```sql
CREATE TABLE new_table AS
SELECT * FROM existing_table WHERE 1 = 0;
```
This copies the structure but not the data.

**Using `CREATE TABLE ... LIKE` (MySQL)**:
```sql
CREATE TABLE new_table LIKE existing_table;
```
This copies both the structure and indexes of the existing table without the data.

These methods help you create an empty table that matches the schema of an existing table, which can be useful for various operations, including testing and data migrations.
