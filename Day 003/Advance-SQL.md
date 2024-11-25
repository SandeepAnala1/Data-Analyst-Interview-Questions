### 1) Explain the use of the IN and NOT IN operators in correlated subqueries. Provide an example.
- **Answer**:
  - **IN** and **NOT IN** operators are used in correlated subqueries to filter rows based on the results of another query.
  - **IN**: Checks if a value exists within a list of values returned by the subquery.
  - **NOT IN**: Checks if a value does not exist within a list of values returned by the subquery.
  - **Example**:
    ```sql
    SELECT employee_name
    FROM employees
    WHERE department_id IN (
        SELECT department_id
        FROM departments
        WHERE location = 'New York'
    );
    ```
    - This query retrieves the names of employees who work in departments located in New York.

### 2) How do ANY and ALL operators work in correlated subqueries?
- **Answer**:
  - **ANY**: Compares a value to any value in a list or subquery and returns true if any comparison is true.
  - **ALL**: Compares a value to all values in a list or subquery and returns true if all comparisons are true.
  - **Example with ANY**:
    ```sql
    SELECT employee_name
    FROM employees
    WHERE salary > ANY (
        SELECT salary
        FROM employees
        WHERE department_id = 10
    );
    ```
    - This query retrieves employees whose salary is greater than any salary in department 10.
  - **Example with ALL**:
    ```sql
    SELECT employee_name
    FROM employees
    WHERE salary > ALL (
        SELECT salary
        FROM employees
        WHERE department_id = 10
    );
    ```
    - This query retrieves employees whose salary is greater than all salaries in department 10.

### 3) Describe the EXISTS and NOT EXISTS operators. When would you use them?
- **Answer**:
  - **EXISTS**: Returns true if the subquery returns one or more rows.
  - **NOT EXISTS**: Returns true if the subquery returns no rows.
  - Used to check for the existence or non-existence of rows that satisfy certain conditions.
  - **Example**:
    ```sql
    SELECT employee_name
    FROM employees
    WHERE EXISTS (
        SELECT 1
        FROM projects
        WHERE projects.employee_id = employees.employee_id
    );
    ```
    - This query retrieves the names of employees who are assigned to at least one project.

### 4) How do correlated subqueries affect query execution plans?
- **Answer**:
  - Correlated subqueries can significantly impact query execution plans and performance.
  - The subquery is executed once for each row processed by the outer query, potentially leading to multiple executions and increased processing time.
  - Execution plans for correlated subqueries often show nested loop joins, which can be less efficient compared to other join methods.

### 5) When should you avoid using correlated subqueries?
- **Answer**:
  - Avoid using correlated subqueries when they can be replaced with more efficient joins or non-correlated subqueries.
  - If the subquery runs multiple times due to the correlation, it can lead to performance issues, especially with large datasets.
  - Consider rewriting the query using joins or using indexed columns to improve performance.
  - **Example**:
    ```sql
    -- Correlated subquery
    SELECT employee_name
    FROM employees e
    WHERE e.salary > (
        SELECT AVG(salary)
        FROM employees
        WHERE department_id = e.department_id
    );

    -- Using JOIN for better performance
    SELECT e.employee_name
    FROM employees e
    JOIN (
        SELECT department_id, AVG(salary) AS avg_salary
        FROM employees
        GROUP BY department_id
    ) d ON e.department_id = d.department_id
    WHERE e.salary > d.avg_salary;
    ```

### 6) Provide an example of using the ROW_NUMBER window function.
- **Answer**:
  ```sql
  SELECT employee_name, department_id, salary,
         ROW_NUMBER() OVER (PARTITION BY department_id ORDER BY salary DESC) AS row_num
  FROM employees;
  ```
  - This query assigns a unique row number to each employee within their department based on descending salary order.

### 7) Describe the RANK and DENSE_RANK window functions with examples.
- **Answer**:
  - **RANK**: Assigns a unique rank to each row within the partition of the result set, with gaps in the ranking sequence if there are ties.
    ```sql
    SELECT employee_name, department_id, salary,
           RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) AS rank
    FROM employees;
    ```
    - This query ranks employees within their department based on descending salary, leaving gaps for ties.
  - **DENSE_RANK**: Similar to RANK, but without gaps in the ranking sequence for ties.
    ```sql
    SELECT employee_name, department_id, salary,
           DENSE_RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) AS dense_rank
    FROM employees;
    ```
    - This query ranks employees within their department based on descending salary, without gaps for ties.

### 8) How can window functions be used to calculate running totals or moving averages?
- **Answer**:
  - **Running Totals**:
    ```sql
    SELECT employee_name, salary,
           SUM(salary) OVER (ORDER BY employee_name ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total
    FROM employees;
    ```
    - This query calculates the running total of salaries.
  - **Moving Averages**:
    ```sql
    SELECT employee_name, salary,
           AVG(salary) OVER (ORDER BY employee_name ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS moving_avg
    FROM employees;
    ```
    - This query calculates the moving average of salaries over the current row and the two preceding rows.

### 9) Explain the PARTITION BY clause in window functions with an example.
- **Answer**:
  - **PARTITION BY**: Divides the result set into partitions and applies the window function to each partition independently.
  - **Example**:
    ```sql
    SELECT employee_name, department_id, salary,
           SUM(salary) OVER (PARTITION BY department_id ORDER BY salary DESC) AS department_total_salary
    FROM employees;
    ```
    - This query calculates the total salary for each department, ordered by descending salary.

### 10) What is the difference between the LAG and LEAD functions? Provide examples.
- **Answer**:
  - **LAG**: Retrieves the value of a column from a previous row within the same result set.
    ```sql
    SELECT employee_name, salary,
           LAG(salary, 1, 0) OVER (ORDER BY employee_name) AS previous_salary
    FROM employees;
    ```
    - This query retrieves the salary from the previous row, defaulting to 0 if there is no previous row.
  - **LEAD**: Retrieves the value of a column from a subsequent row within the same result set.
    ```sql
    SELECT employee_name, salary,
           LEAD(salary, 1, 0) OVER (ORDER BY employee_name) AS next_salary
    FROM employees;
    ```
    - This query retrieves the salary from the next row, defaulting to 0 if there is no next row.

### 11) Explain the difference between RANGE and ROWS in the frame clause.
- **RANGE**:
  - Operates on the logical range of values.
  - Considers all rows with the same value as the current row within the specified range.
  - Useful for calculating metrics over ranges of values.
- **ROWS**:
  - Operates on physical rows.
  - Considers a specific number of rows before or after the current row.
  - Useful for calculating metrics over a fixed number of rows.

### 12) Provide an example of using the frame clause to define a window frame for a window function.
- **Example**:
  ```sql
  SELECT employee_name, salary,
         SUM(salary) OVER (ORDER BY salary 
                           ROWS BETWEEN 2 PRECEDING AND 1 FOLLOWING) AS sum_salary
  FROM employees;
  ```
  - This query calculates the sum of salaries for the current row, the two preceding rows, and the one following row.

### 13) Explain the syntax and use cases of a non-recursive CTE with an example.
- **Answer**:
  - **Syntax**:
    ```sql
    WITH cte_name AS (
        SELECT column1, column2
        FROM table_name
        WHERE condition
    )
    SELECT * FROM cte_name;
    ```
  - **Use Cases**:
    - Simplify complex queries by breaking them into more manageable parts.
    - Improve readability and maintainability.
    - Reuse the result of the CTE in the main query.
  - **Example**:
    ```sql
    WITH SalesCTE AS (
        SELECT product_id, SUM(quantity) AS total_sales
        FROM sales
        GROUP BY product_id
    )
    SELECT product_id, total_sales
    FROM SalesCTE
    WHERE total_sales > 100;
    ```

### 14) Describe recursive CTEs and their use cases. Provide an example.
- **Answer**:
  - **Recursive CTEs**:
    - Used to query hierarchical or tree-structured data.
    - Consists of an anchor member and a recursive member.
    - Repeats until the recursion stops (usually defined by a termination condition).
  - **Use Cases**:
    - Traversing hierarchical data such as organizational charts, file systems, or bill of materials.
    - Performing operations on hierarchical data like finding descendants or ancestors.
  - **Example**:
    ```sql
    WITH RECURSIVE EmployeeHierarchy AS (
        -- Anchor member
        SELECT employee_id, manager_id, employee_name, 1 AS level
        FROM employees
        WHERE manager_id IS NULL
        UNION ALL
        -- Recursive member
        SELECT e.employee_id, e.manager_id, e.employee_name, eh.level + 1
        FROM employees e
        INNER JOIN EmployeeHierarchy eh ON e.manager_id = eh.employee_id
    )
    SELECT employee_id, manager_id, employee_name, level
    FROM EmployeeHierarchy;
    ```

### 15) Explain the concept of anchor members and recursive members in recursive CTEs.
- **Answer**:
  - **Anchor Members**:
    - The initial query in a recursive CTE.
    - Provides the base result set that the recursion starts with.
    - Executes only once.
  - **Recursive Members**:
    - The subsequent query that references the CTE itself.
    - Runs repeatedly, using the results from the previous iteration to produce the next set of results.
    - Continues until no more rows are returned by the recursive query or a termination condition is met.
  - **Example**:
    ```sql
    WITH RECURSIVE NumberSeries AS (
        -- Anchor member
        SELECT 1 AS number
        UNION ALL
        -- Recursive member
        SELECT number + 1
        FROM NumberSeries
        WHERE number < 10
    )
    SELECT number
    FROM NumberSeries;
    ```
    - The anchor member starts with `1`, and the recursive member increments the number until it reaches `10`.

### 16) Query Using a Correlated Subquery to Find Employees Who Earn More Than the Average Salary in Their Department
- **Query**:
  ```sql
  SELECT employee_name, department_id, salary
  FROM employees e1
  WHERE salary > (
      SELECT AVG(salary)
      FROM employees e2
      WHERE e2.department_id = e1.department_id
  );
  ```

### 17) Use a Window Function to Rank Employees Based on Their Salaries Within Each Department
- **Query**:
  ```sql
  SELECT employee_name, department_id, salary,
         RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) AS salary_rank
  FROM employees;
  ```

### 18) Create a Query Using the Frame Clause to Calculate a 7-Day Moving Average of Sales
- **Query**:
  ```sql
  SELECT sale_date, sales_amount,
         AVG(sales_amount) OVER (
             ORDER BY sale_date
             ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
         ) AS moving_avg
  FROM sales;
  ```

### 19) Write a CTE to Find the Hierarchical Structure of Employees in an Organization (Manager-Employee Relationships)
- **Query**:
  ```sql
  WITH EmployeeHierarchy AS (
      SELECT employee_id, manager_id, employee_name, 1 AS level
      FROM employees
      WHERE manager_id IS NULL
      UNION ALL
      SELECT e.employee_id, e.manager_id, e.employee_name, eh.level + 1
      FROM employees e
      INNER JOIN EmployeeHierarchy eh ON e.manager_id = eh.employee_id
  )
  SELECT employee_id, manager_id, employee_name, level
  FROM EmployeeHierarchy;
  ```

### 20) Develop a Query Using Recursive CTE to Generate a Sequence of Dates Between Two Given Dates
- **Query**:
  ```sql
  WITH RECURSIVE DateSequence AS (
      SELECT '2024-01-01'::DATE AS date
      UNION ALL
      SELECT date + INTERVAL '1 day'
      FROM DateSequence
      WHERE date < '2024-01-31'::DATE
  )
  SELECT date
  FROM DateSequence;
  ```
  - This query generates a sequence of dates from January 1, 2024, to January 31, 2024. Adjust the start and end dates as needed.

### 21) Create a Query Using the LAG Function to Compare Sales of the Current Month with the Previous Month
- **Query**:
  ```sql
  SELECT month, sales_amount,
         LAG(sales_amount, 1, 0) OVER (ORDER BY month) AS previous_month_sales,
         sales_amount - LAG(sales_amount, 1, 0) OVER (ORDER BY month) AS sales_difference
  FROM monthly_sales;
  ```

### 22) Create a Query Using ANY and ALL Operators in a Correlated Subquery to Compare Sales Data
- **Query with ANY**:
  ```sql
  SELECT product_id, sales_amount
  FROM sales s
  WHERE sales_amount > ANY (
      SELECT sales_amount
      FROM sales
      WHERE product_id = s.product_id
  );
  ```
  - This query retrieves products whose sales amount is greater than any sales amount for the same product in the subquery.

- **Query with ALL**:
  ```sql
  SELECT product_id, sales_amount
  FROM sales s
  WHERE sales_amount > ALL (
      SELECT sales_amount
      FROM sales
      WHERE product_id = s.product_id
  );
  ```
  - This query retrieves products whose sales amount is greater than all sales amounts for the same product in the subquery.

### 23) Write a Query to Identify the Top 3 Highest-Paid Employees in Each Department Using Window Functions
- **Query**:
  ```sql
  SELECT employee_name, department_id, salary
  FROM (
      SELECT employee_name, department_id, salary,
             RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) AS salary_rank
      FROM employees
  ) ranked_employees
  WHERE salary_rank <= 3;
  ```

### 24) Use the Frame Clause in a Window Function to Calculate a Rolling Average of Sales Over the Last 30 Days
- **Query**:
  ```sql
  SELECT sale_date, sales_amount,
         AVG(sales_amount) OVER (
             ORDER BY sale_date
             ROWS BETWEEN 29 PRECEDING AND CURRENT ROW
         ) AS rolling_avg
  FROM sales;
  ```

### 25) Write a Query to Find the Second Highest Salary in Each Department Using Window Functions
- **Query**:
  ```sql
  SELECT department_id, employee_name, salary
  FROM (
      SELECT department_id, employee_name, salary,
             DENSE_RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) AS salary_rank
      FROM employees
  ) ranked_salaries
  WHERE salary_rank = 2;
  ```
