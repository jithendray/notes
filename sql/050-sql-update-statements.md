---
tags:
  - sql
---
# UPDATE statements in SQL

The `UPDATE` statement in SQL is used to modify the existing data in a table. It allows you to change one or more rows based on certain conditions. 

#### Basic Syntax:
```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```
- **table_name**: The name of the table in which you want to update the data.
- **SET**: Specifies the columns to update and their new values.
- **WHERE**: Defines the condition(s) that determine which rows are updated. If no `WHERE` clause is provided, **all rows in the table will be updated**.

> **Important**: Always use the `WHERE` clause to avoid accidentally updating all rows in a table.

### Examples and Scenarios

#### 1. **Updating a Single Column**
You can update a single column for rows that meet a specific condition.

**Scenario:** Increase the salary of employees with a salary less than 40,000.
```sql
UPDATE Employees
SET salary = salary + 5000
WHERE salary < 40000;
```

#### 2. **Updating Multiple Columns**
You can update multiple columns in a single query.

**Scenario:** Update both the salary and age of an employee with a specific `id`.
```sql
UPDATE Employees
SET salary = 60000, age = 35
WHERE id = 101;
```

#### 3. **Using `UPDATE` with Subqueries**
You can use a subquery in the `SET` or `WHERE` clause to update rows based on data from another table or a calculation.

**Scenario:** Increase the salary of employees who earn less than the average salary in their department.
```sql
UPDATE Employees
SET salary = salary * 1.10
WHERE salary < (SELECT AVG(salary) FROM Employees);
```

#### 4. **Updating Rows Using a Join**
You can update rows in one table based on conditions from another table by using `JOIN` in the `UPDATE` statement.

**Scenario:** Increase the salary of all employees in the "Finance" department by 10%.
```sql
UPDATE Employees e
SET e.salary = e.salary * 1.10
FROM Departments d
WHERE e.department_id = d.id
AND d.department_name = 'Finance';
```

#### 5. **Conditional Updates with `CASE`**
You can use `CASE` within an `UPDATE` statement to apply different updates based on certain conditions.

**Scenario:** Provide a 10% raise to employees in the "IT" department and a 5% raise to those in the "HR" department.
```sql
UPDATE Employees e
SET e.salary = CASE 
                 WHEN d.department_name = 'IT' THEN e.salary * 1.10
                 WHEN d.department_name = 'HR' THEN e.salary * 1.05
              END
FROM Departments d
WHERE e.department_id = d.id
AND d.department_name IN ('IT', 'HR');

```
#### 6. **Updating All Rows in a Table**
If no `WHERE` clause is specified, **all rows** in the table will be updated.

**Scenario:** Reset the performance bonus of all employees to zero.
```sql
UPDATE Employees
SET bonus = 0;
```

#### 7. **Updating with a `LIMIT` (Vendor-specific)**
Some database systems (like MySQL) allow you to update a limited number of rows using the `LIMIT` clause.

**Scenario:** Update the salaries of the first 5 employees based on their hire date.
```sql
UPDATE Employees
SET salary = salary * 1.05
ORDER BY hire_date
LIMIT 5;
```

(Note: `LIMIT` is not standard SQL and may not work in some databases like PostgreSQL, but MySQL and some others support it. In PostgreSQL, there is no direct `LIMIT` support in an `UPDATE` statement.)

```sql
WITH cte AS (
    SELECT id
    FROM Employees
    ORDER BY hire_date
    LIMIT 5
)
UPDATE Employees
SET salary = salary * 1.05
WHERE id IN (SELECT id FROM cte);

```


#### 8. **Updating Data Using a Calculated Value**
You can use calculations in the `SET` clause.

**Scenario:** Update the `age` column to reflect the actual age of employees, calculated from their birth year.
```sql
UPDATE Employees
SET age = YEAR(CURDATE()) - birth_year;
```

#### 9. **Updating Rows with NULL Values**
You can target rows with `NULL` values and update them.

**Scenario:** Update the `department_id` to a default value (e.g., 999) for all employees who do not have a department assigned (`NULL` value).
```sql
UPDATE Employees
SET department_id = 999
WHERE department_id IS NULL;
```

#### 10. **Updating Based on Aggregates**
You can update rows based on aggregate values calculated over a set of rows.

**Scenario:** Increase the salary of employees in departments with more than 10 employees by 5%.
```sql
UPDATE Employees
SET salary = salary * 1.05
WHERE department_id IN (
    SELECT department_id 
    FROM Employees
    GROUP BY department_id
    HAVING COUNT(*) > 10
);
```

### Important Considerations for `UPDATE` Queries

1. **Transaction Management**: For critical updates, always consider wrapping your `UPDATE` statement in a transaction. This allows you to rollback in case something goes wrong.

2. **Performance**: Updating a large number of rows can be performance-intensive. If you need to update a massive dataset, consider breaking the update into smaller batches.

3. **Locking**: The `UPDATE` statement will typically lock the rows being updated. Make sure that this won’t lead to deadlocks or other locking issues in a concurrent environment.

4. **Testing Before Running**: It’s always a good idea to run a `SELECT` query with the same `WHERE` clause first to check which rows are affected before running the actual `UPDATE`.

5. **Backing Up Data**: Especially when making significant updates, it is crucial to have a backup of the data in case something goes wrong.