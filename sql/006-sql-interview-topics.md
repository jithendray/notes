---
tags: sql
---
# SQL interview topics

Here are key SQL question patterns and topics you should focus on for cracking data engineering interviews at FAANG and unicorn startups:

### 1. **Basic Querying (CRUD Operations)**

- **SELECT**: Filtering data using `WHERE`, `DISTINCT`, and `GROUP BY`.
- **INSERT**, **UPDATE**, **DELETE**: Manipulating data in tables.
- **Sorting**: Using `ORDER BY`, sorting by multiple columns.
- **Alias**: Using `AS` for table and column aliases.

### 2. **Joins (Mastery is Key)**

- **INNER JOIN**: Fetching matching rows across tables.
- **LEFT JOIN**, **RIGHT JOIN**, **FULL JOIN**: Working with non-matching rows.
- **Self Join**: Joining a table with itself.
- **Cross Join**: Cartesian product of two tables.
- **Anti-Joins**: Using `NOT EXISTS`, `LEFT JOIN WHERE NULL` to filter missing data.

### 3. **Subqueries**

- **Correlated Subqueries**: Subqueries dependent on the outer query.
- **Non-correlated Subqueries**: Simple subqueries within `WHERE`, `SELECT`, and `FROM` clauses.
- **Exists/Not Exists**: Filtering results based on existence of subquery results.

### 4. **Aggregation Functions**

- **SUM**, **COUNT**, **AVG**, **MIN**, **MAX**: Calculating aggregate values.
- **GROUP BY**: Grouping data by columns and applying aggregate functions.
- **HAVING**: Filtering groups after aggregation.

### 5. **Window Functions (Advanced Analytics)**

- **ROW_NUMBER**, **RANK**, **DENSE_RANK**: Ranking rows within partitions.
- **LEAD/LAG**: Accessing subsequent or previous row data.
- **NTILE**: Dividing rows into a specified number of roughly equal parts.
- **PARTITION BY**: Grouping rows for windowed calculations.
- **Moving Averages**, **Running Totals**: Cumulative or moving calculations.

### 6. **Set Operations**

- **UNION/UNION ALL**: Combining results from multiple queries.
- **INTERSECT**: Fetching common rows across queries.
- **EXCEPT** (or **MINUS** in some databases): Fetching rows present in one result set but not another.

### 7. **String Functions**

- **CONCAT**, **SUBSTRING**, **LENGTH**, **REPLACE**: Manipulating strings.
- **LIKE**: Pattern matching using `%` and `_`.

### 8. **Date and Time Functions**

- **DATEADD**, **DATEDIFF**, **DATEPART**: Calculating differences or manipulating dates.
- **FORMAT**: Formatting date and time for display.
- **Extracting**: Extracting day, month, year, hour, etc., from a date.

### 9. **Indexes and Query Optimization**

- **Indexes**: Understanding how indexes impact performance.
- **EXPLAIN/EXPLAIN ANALYZE**: Analyzing query execution plans.
- **Optimizing**: Strategies for reducing query time and improving performance.
- **Query Caching**, **Materialized Views**: Understanding caching mechanisms.

### 10. **Transactions and ACID Properties**

- **BEGIN**, **COMMIT**, **ROLLBACK**: Handling transactions.
- **ACID Properties**: Understanding Atomicity, Consistency, Isolation, and Durability.
- **Isolation Levels**: `READ COMMITTED`, `READ UNCOMMITTED`, `REPEATABLE READ`, `SERIALIZABLE`.

### 11. **Normalization and Denormalization**

- **Normalization Forms**: Understanding 1NF, 2NF, 3NF, and BCNF.
- **Denormalization**: When to use and its impact on performance.

### 12. **Data Modeling**

- **ER Diagrams**: Understanding relationships between tables.
- **Star and Snowflake Schema**: Understanding dimensional modeling.
- **Fact and Dimension Tables**: Optimizing data warehouses.

### 13. **Common Table Expressions (CTEs) and Recursive Queries**

- **WITH**: Simplifying complex queries.
- **Recursive CTEs**: Handling hierarchical or tree-structured data.

### 14. **Handling NULLs**

- **COALESCE**, **IFNULL**, **IS NULL**: Handling and replacing `NULL` values.
- **NULL Safety**: Ensuring `NULL`aware calculations and conditions.

### 15. **Pivoting and Unpivoting Data**

- **PIVOT**: Converting row data into columns.
- **UNPIVOT**: Converting columns back into rows.

### 16. **Working with JSON**

- **JSON Functions**: `JSON_EXTRACT`, `JSONB` (in PostgreSQL), parsing and querying JSON data.

### 17. **Constraints and Relationships**

- **PRIMARY KEY**, **FOREIGN KEY**, **UNIQUE**, **NOT NULL**: Enforcing constraints in table design.
- **ON DELETE/UPDATE CASCADE**: Handling referential integrity.

### 18. **Advanced SQL Topics**

- **Dynamic SQL**: Constructing and executing SQL dynamically.
- **Stored Procedures/Functions**: Writing and using reusable code blocks.
- **Triggers**: Automatically invoking SQL logic on data changes.

### 19. **ETL-related Queries**

- **Bulk Operations**: Handling batch insertions, updates, or deletes.
- **Data Migration**: Writing SQL for data movement between systems.
- **Upserts**: Efficiently inserting or updating data.

### 20. **Big Data and Partitioning**

- **Table Partitioning**: Breaking large tables into smaller, more manageable pieces.
- **Distributed SQL**: Understanding SQL in distributed environments (like Presto or Spark SQL).

---

### FAANG Interview Focus:

1. **Joins** and **Subqueries**: Often combined in complex scenarios.
2. **Window Functions**: Essential for handling analytical queries.
3. **Query Optimization**: Writing efficient queries is critical.
4. **Data Modeling**: Align with understanding of data warehouses and real-world scenarios.
5. **Complex Aggregations**: Queries involving multi-level aggregations and filters.

Work on mastering these topics through hands-on practice on platforms like LeetCode, StrataScratch, or SQLZoo. Let me know which topics you want to dive deeper into!