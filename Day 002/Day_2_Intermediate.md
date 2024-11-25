### 1) What is the difference between a primary key and a unique key?
- **Primary Key**:
  - Uniquely identifies each record in a table.
  - Cannot contain NULL values.
  - Only one primary key allowed per table.
  - Creates a clustered index by default.

- **Unique Key**:
  - Ensures all values in a column or set of columns are unique.
  - Can contain NULL values (one per column combination).
  - Multiple unique keys allowed per table.
  - Creates a non-clustered index by default.

### 2) Explain the concept and purpose of a foreign key in a database.
- **Foreign Key**:
  - A column or set of columns in one table that references the primary key or unique key in another table.
  - Ensures referential integrity between tables.
  - Helps maintain consistent and valid relationships between data in different tables.

### 3) How do composite keys work, and in what scenarios would you use them?
- **Composite Key**:
  - A primary key consisting of two or more columns.
  - Used when a single column is not sufficient to uniquely identify a record.
  - Example Scenario: In a table `OrderDetails`, a composite key could be (`OrderID`, `ProductID`) since an individual order can contain multiple products.

### 4) What are the constraints associated with primary keys and foreign keys?
- **Primary Key Constraints**:
  - Ensures unique identification of each record.
  - Cannot contain NULL values.
  - Implicitly creates a unique index.

- **Foreign Key Constraints**:
  - Ensures that the value in the foreign key column(s) exists in the referenced primary/unique key column(s).
  - Prevents actions that would destroy links between tables (e.g., cannot delete a record in the parent table if related records exist in the child table).

### 5) Can a table have multiple primary keys? Explain your answer.
- A table **cannot** have multiple primary keys.
- **Reason**:
  - A primary key uniquely identifies each record in a table. Allowing multiple primary keys would violate this principle and create ambiguity in uniquely identifying records. Instead, a table can have a composite primary key if multiple columns are needed to ensure uniqueness.

### 6) How can you filter records in a table to retrieve rows where the 'name' column starts with the letter 'A'?
- **SQL Query**:
  ```sql
  SELECT * FROM table_name WHERE name LIKE 'A%';
  ```

### 7) Write a SQL query to find all products in the 'products' table that were added to the database in the last 30 days.
- **SQL Query**:
  ```sql
  SELECT * FROM products 
  WHERE add_date >= CURRENT_DATE - INTERVAL '30' DAY;
  ```

### 8) Explain the use of the BETWEEN operator in the WHERE clause with an example.
- **Explanation**:
  - The `BETWEEN` operator is used to filter the result set within a certain range. The range includes the start and end values.
  - **Example**:
    ```sql
    SELECT * FROM orders 
    WHERE order_date BETWEEN '2023-01-01' AND '2023-12-31';
    ```
  - This query retrieves all orders with `order_date` between January 1, 2023, and December 31, 2023, inclusive.

### 9) What is the purpose of the ORDER BY clause, and how does it work with multiple columns?
- **Purpose**:
  - The `ORDER BY` clause is used to sort the result set by one or more columns, either in ascending (`ASC`) or descending (`DESC`) order.
  - **Multiple Columns**:
    - When sorting by multiple columns, the result set is first sorted by the first specified column. If there are ties (same values in the first column), the second column is used to sort those ties, and so on.
  - **Example**:
    ```sql
    SELECT * FROM employees 
    ORDER BY department ASC, salary DESC;
    ```
  - This query sorts employees by department in ascending order and, within each department, by salary in descending order.

### 10) Write a SQL query to retrieve all rows from the 'sales' table and sort the results by 'sale_date' in descending order.
- **SQL Query**:
  ```sql
  SELECT * FROM sales 
  ORDER BY sale_date DESC;
  ```

### 11) How can you sort the results of a query by two columns, one in ascending and one in descending order?
- **SQL Query**:
  ```sql
  SELECT * FROM table_name 
  ORDER BY column1 ASC, column2 DESC;
  ```
  - This query sorts the results by `column1` in ascending order and then by `column2` in descending order if there are ties in `column1`.

### 12) Explain how the NULL values are treated when using the ORDER BY clause.
- **NULL Values**:
  - When using `ORDER BY`, NULL values are treated as the lowest possible values.
  - In ascending order (`ASC`), NULLs appear first.
  - In descending order (`DESC`), NULLs appear last.
  - Some databases allow customizing this behavior using `NULLS FIRST` or `NULLS LAST`.

### 13) What is the purpose of the GROUP BY clause, and how does it work with aggregate functions?
- **Purpose**:
  - The `GROUP BY` clause groups rows that have the same values in specified columns into summary rows.
  - Often used with aggregate functions (`COUNT`, `SUM`, `AVG`, `MAX`, `MIN`) to perform calculations on each group.
  - **Example**:
    ```sql
    SELECT department, COUNT(employee_id) AS employee_count 
    FROM employees 
    GROUP BY department;
    ```
  - This query counts the number of employees in each department.

### 14) Explain the difference between the WHERE and HAVING clauses. Provide an example.
- **WHERE Clause**:
  - Used to filter rows before any grouping or aggregation takes place.
  - Cannot be used with aggregate functions.
  - **Example**:
    ```sql
    SELECT * FROM orders 
    WHERE order_date >= '2024-01-01';
    ```

- **HAVING Clause**:
  - Used to filter groups after grouping has taken place.
  - Can be used with aggregate functions.
  - **Example**:
    ```sql
    SELECT department, COUNT(employee_id) AS employee_count 
    FROM employees 
    GROUP BY department 
    HAVING COUNT(employee_id) > 5;
    ```
  - This query filters departments to only those with more than 5 employees.

### 15) Explain the GROUP_CONCAT function and provide an example of how to use it.
- **GROUP_CONCAT**:
  - Concatenates values from multiple rows into a single string, with an optional separator.
  - Commonly used with `GROUP BY`.
  - **Example**:
    ```sql
    SELECT department, GROUP_CONCAT(employee_name ORDER BY employee_name ASC SEPARATOR ', ') AS employee_list 
    FROM employees 
    GROUP BY department;
    ```
  - This query lists all employee names in each department as a comma-separated string, sorted by employee names.

### 16) Write a SQL query to concatenate all employee names in the 'employees' table grouped by their 'department_id'.
- **SQL Query**:
  ```sql
  SELECT department_id, 
         GROUP_CONCAT(employee_name ORDER BY employee_name ASC SEPARATOR ', ') AS employee_list
  FROM employees 
  GROUP BY department_id;
  ```

### 17) What is the purpose of the ROLLUP operator in SQL, and how does it work?
- **Purpose**:
  - The `ROLLUP` operator is used to generate subtotals and a grand total for a set of columns.
  - It extends the functionality of the `GROUP BY` clause by creating a hierarchical grouping.
  - Useful for generating reports that require multiple levels of aggregations.

### 18) Write a SQL query to generate a report of total sales per product, including a grand total using the ROLLUP operator.
- **SQL Query**:
  ```sql
  SELECT product_id, 
         SUM(sales_amount) AS total_sales 
  FROM sales 
  GROUP BY ROLLUP(product_id);
  ```
  - This query calculates total sales per product and includes a grand total for all products.

### 19) How can you use the COUNT, SUM, AVG, MIN, and MAX functions with the GROUP BY clause? Provide an example.
- **Example**:
  ```sql
  SELECT department_id, 
         COUNT(employee_id) AS employee_count,
         SUM(salary) AS total_salary,
         AVG(salary) AS average_salary,
         MIN(salary) AS minimum_salary,
         MAX(salary) AS maximum_salary
  FROM employees 
  GROUP BY department_id;
  ```
  - This query provides various aggregate statistics (count, sum, average, minimum, and maximum salary) for each department.

### 20) Write a SQL query to categorize employees into 'High', 'Medium', and 'Low' salary groups using the CASE statement.
- **SQL Query**:
  ```sql
  SELECT employee_name, salary,
         CASE 
             WHEN salary > 80000 THEN 'High'
             WHEN salary BETWEEN 50000 AND 80000 THEN 'Medium'
             ELSE 'Low'
         END AS salary_category
  FROM employees;
  ```
  - This query categorizes employees into 'High', 'Medium', and 'Low' salary groups based on their salary values.

### 21) How can you use the CASE statement to replace NULL values with a default value? Provide an example.
- **Example**:
  ```sql
  SELECT employee_name, 
         CASE 
             WHEN salary IS NULL THEN 0
             ELSE salary
         END AS salary
  FROM employees;
  ```
  - This query replaces NULL values in the `salary` column with `0`.

### 22) Explain how nested CASE statements work with an example.
- **Explanation**:
  - Nested `CASE` statements allow for more complex conditional logic within a single `CASE` statement.
  - **Example**:
    ```sql
    SELECT employee_name,
           CASE 
               WHEN salary IS NULL THEN 'No Salary'
               ELSE 
                   CASE 
                       WHEN salary > 80000 THEN 'High'
                       WHEN salary BETWEEN 50000 AND 80000 THEN 'Medium'
                       ELSE 'Low'
                   END
           END AS salary_category
    FROM employees;
    ```
  - This query categorizes employees into salary groups and handles NULL salaries by labeling them 'No Salary'.

### 23) What is the difference between INNER JOIN and LEFT JOIN? Provide examples for each.
- **INNER JOIN**:
  - Returns only the rows that have matching values in both tables.
  - **Example**:
    ```sql
    SELECT orders.order_id, customers.customer_name
    FROM orders
    INNER JOIN customers ON orders.customer_id = customers.customer_id;
    ```

- **LEFT JOIN**:
  - Returns all rows from the left table and the matched rows from the right table. If there is no match, the result is NULL on the side of the right table.
  - **Example**:
    ```sql
    SELECT orders.order_id, customers.customer_name
    FROM orders
    LEFT JOIN customers ON orders.customer_id = customers.customer_id;
    ```

### 24) Write a SQL query to retrieve all records from the 'orders' table and matching records from the 'customers' table using a RIGHT JOIN.
- **SQL Query**:
  ```sql
  SELECT orders.order_id, customers.customer_name
  FROM orders
  RIGHT JOIN customers ON orders.customer_id = customers.customer_id;
  ```

### 25) Explain the FULL JOIN and how it differs from INNER JOIN and LEFT JOIN. Provide an example.
- **FULL JOIN**:
  - Returns all rows when there is a match in either left or right table. If there is no match, the result is NULL from the side that does not have a match.
  - **Example**:
    ```sql
    SELECT orders.order_id, customers.customer_name
    FROM orders
    FULL JOIN customers ON orders.customer_id = customers.customer_id;
    ```
  - **Difference**:
    - **INNER JOIN**: Returns only the rows with matching values in both tables.
    - **LEFT JOIN**: Returns all rows from the left table and the matched rows from the right table, with NULLs for non-matching rows from the right table.
    - **FULL JOIN**: Returns all rows when there is a match in either left or right table, with NULLs for non-matching rows from both tables.
