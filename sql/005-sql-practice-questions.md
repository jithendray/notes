---
tags:
  - sql
---
# SQL practice Questions

Note: These questions are generated through ChatGPT - which should cover all areas to focus on and get well versed with.

### Should you focus solely on these questions?

If one thoroughly practice these questions, covering the broad range of topics, it should be sufficient for building a strong foundation for SQL interviews. However, to ensure you are fully prepared for a variety of question styles, it would still be beneficial to practice more problems from sites

### 1. **Basic Querying (CRUD Operations)**

**Tables:**
- `Employees`: (id, name, age, department_id, salary)
- `Departments`: (id, department_name)

#### Easy:
1. Retrieve the names and ages of all employees from the `Employees` table.
2. Find the department names where employees have a salary greater than 50,000.

#### Medium:
1. Find the number of employees in each department.
2. Update the salary of employees in the "Finance" department by increasing it by 10%.
3. Delete all employees from the `Employees` table who are younger than 25.

#### Hard:
1. Write a query to retrieve the top 5 highest-paid employees in each department.
2. Insert a new employee into the `Employees` table, ensuring the department exists in the `Departments` table. Use transactions to ensure data integrity.
3. Find the name of the department with the highest total employee salary.

---

### 2. **Joins**

**Tables:**
- `Orders`: (order_id, customer_id, order_date, amount)
- `Customers`: (customer_id, customer_name, customer_address)

#### Easy:
1. Retrieve all orders along with the customer names.
2. Find the total amount of all orders placed by each customer.

#### Medium:
1. List all customers who have not placed any orders.
2. Write a query to find customers who placed more than 5 orders in the last 6 months.
3. Retrieve the customer names and their order amounts for orders that exceed $500.

#### Hard:
1. Find the top 3 customers based on the total value of their orders, but only for orders placed in 2023.
2. Write a query to find customers who placed their first order in 2023 and then placed an additional order within the next 30 days.
3. Write a self-join query to retrieve all customers who have the same order amount for at least two different orders.

---

### 3. **Subqueries**

**Tables:**
- `Products`: (product_id, product_name, price)
- `Sales`: (sale_id, product_id, sale_date, quantity)

#### Easy:
1. Find the products that have never been sold.
2. Retrieve the names of products that have been sold more than 100 times.

#### Medium:
1. Write a query to find the average price of all products that have been sold at least once.
2. Retrieve the names of the products whose price is above the average price.
3. Find the products that have the second highest total sales.

#### Hard:
1. Write a query to find products that have been sold exactly once in each month of 2023.
2. Using a correlated subquery, retrieve products where the total quantity sold is greater than the average quantity sold per product.
3. Find products that have a price greater than the average price of products sold in the same month as their first sale.

---

### 4. **Aggregation Functions**

**Tables:**
- `Sales`: (sale_id, product_id, sale_date, quantity, price)
- `Stores`: (store_id, store_name)

#### Easy:
1. Find the total quantity sold across all stores.
2. Retrieve the average price of products sold in 2023.

#### Medium:
1. Write a query to find the total sales per store.
2. Retrieve the top 3 stores by total sales.
3. Find the number of sales that happened in each month of 2023.

#### Hard:
1. Write a query to find the product with the highest total revenue (quantity * price).
2. Find the store with the most unique products sold.
3. Retrieve the store where the average sales per day exceed the average of all stores.

---

### 5. **Window Functions**

**Tables:**
- `Employees`: (employee_id, department_id, salary, hire_date)
- `Departments`: (department_id, department_name)

#### Easy:
1. Retrieve the rank of employees based on their salary within their department.
2. List all employees along with their cumulative salary over time, based on their hire date.

#### Medium:
1. Find the employee in each department who has the highest salary using a window function.
2. Write a query to find the average salary of employees, along with a moving average over their last 3 hires.
3. Find employees whose salary is above the average salary of their department, ranked by salary.

#### Hard:
1. Write a query to rank employees within their department, but reset the ranking whenever a new department is encountered.
2. Retrieve employees who are in the top 10% of salaries across all departments using a percentile window function.
3. Write a query to find the difference between the current employee's salary and the next highest salary, partitioned by department.

---

### 6. **Set Operations**

**Tables:**
- `Employees`: (employee_id, name, department_id)
- `Old_Employees`: (employee_id, name, department_id)

#### Easy:
1. Find all employees who are currently in the `Employees` table but not in the `Old_Employees` table.
2. Retrieve employees who are in both the `Employees` and `Old_Employees` tables.

#### Medium:
1. Write a query to find employees who are either in the `Employees` table or the `Old_Employees` table, but not both.
2. Retrieve the intersection of employees who were in both the `Employees` and `Old_Employees` tables.
3. Find employees who were in the `Old_Employees` table but have now moved to a different department.

#### Hard:
1. Write a query to find employees who were in the `Old_Employees` table, but are no longer in the current `Employees` table, and whose department has changed.
2. Combine the `Employees` and `Old_Employees` tables to create a union of both datasets, and then filter out any duplicates.
3. Retrieve employees who were promoted from one department to another, and have a salary increase greater than 20%.

---

### 7. **String Functions**

**Tables:**
- `Employees`: (employee_id, first_name, last_name, email)
- `Departments`: (department_id, department_name)

#### Easy:
1. Retrieve the first and last names of all employees in uppercase.
2. Find employees whose email contains "admin" in it.

#### Medium:
1. Write a query to retrieve employees whose last name starts with "A" and ends with "Z".
2. Find employees whose full name is longer than 20 characters.
3. Replace all occurrences of "Manager" in the `department_name` with "Lead".

#### Hard:
1. Write a query to find employees whose email domain (e.g., @company.com) is different from "example.com".
2. Retrieve employees whose names are palindromes.
3. Find employees whose full names contain alternating vowels and consonants.

---

### 8. **Date and Time Functions**

**Tables:**
- `Orders`: (order_id, customer_id, order_date, amount)
- `Customers`: (customer_id, customer_name)

#### Easy:
1. Retrieve all orders placed in the last 30 days.
2. Find customers who placed an order on their birthday (assume `customer_birthday` exists).

#### Medium:
1. Write a query to find the difference between the earliest and latest order date for each customer.
2. Retrieve customers who placed an order in every month of 2023.
3. Find customers whose first and last order happened on the same day of the week (e.g., both on Mondays).

#### Hard:
1. Write a query to find customers who placed an order in every quarter of the year for 2 consecutive years.
2. Retrieve customers whose average time between orders is less than 15 days.
3. Find orders where the time between placement and delivery is more than twice the average time.

---

### 9. **Indexes and Query Optimization**

**Tables:**
- `Products`: (product_id, product_name, price)
- `Sales`: (sale_id, product_id, sale_date, quantity)

#### Easy:
1. Retrieve the `product_name` for the top-selling product in terms of quantity.
2. Write a query to return the total sales amount, ensuring that an index on `product_id` is used.

#### Medium:
1. Find products that contribute to the top 10% of total revenue. Use `EXPLAIN` to ensure efficient query execution.
2. Write a query to optimize a report that generates the total sales per product, ensuring that indexes are used.
3. Retrieve the top 5 products in terms of quantity sold, but only for products that have a price above the average. Optimize the query using indexing.

#### Hard:
1. Write a query to retrieve products where the ratio of sales to product price is above average. Ensure the query is optimized using indexing and `EXPLAIN ANALYZE`.
2. Retrieve the products whose sales have increased by more than 20% compared to the previous year. Optimize the query with appropriate indexing.
3. Write a query that finds products sold in stores with the longest sales history (earliest to latest sale dates), ensuring the use of indexes for performance.

---

You're right! Let me create questions for the remaining 11 subtopics. Each subtopic will have 2 easy, 3 medium, and 3 hard/advanced questions with table column details, just like before.

### 10. **Conditional Aggregates**

**Tables:**
- `Orders`: (order_id, customer_id, order_date, amount)
- `Products`: (product_id, product_name, category, price)

#### Easy:
1. Find the total sales amount for all orders where the order date is in 2023.
2. Retrieve the count of orders where the amount exceeds $100.

#### Medium:
1. Calculate the total sales per product category, but only for products priced above $50.
2. Write a query to find the number of orders for each customer that had an amount greater than the average order amount.
3. Retrieve the number of products sold in each category where the total revenue is greater than $500.

#### Hard:
1. Find the product category with the highest total sales, but only include products that had more than 5 sales.
2. Calculate the percentage of orders per product category where the amount exceeds $200, grouped by year.
3. Retrieve the customers who placed the highest number of orders in 2023 where the order amount exceeded the average.

---

### 11. **Group By and Having Clauses**

**Tables:**
- `Sales`: (sale_id, product_id, sale_date, quantity, store_id)
- `Stores`: (store_id, store_name, location)

#### Easy:
1. Find the total quantity sold for each product.
2. Retrieve the total number of sales per store.

#### Medium:
1. Write a query to find the total quantity sold per store for products with more than 100 sales.
2. Retrieve the stores that have sold more than $500 in total revenue.
3. Find the products sold in more than 3 stores, with total revenue exceeding $1000.

#### Hard:
1. Write a query to find the stores that have the highest total sales for each product category.
2. Retrieve the stores with more than 100 sales, but exclude those where the average sale amount is below the store's median sales value.
3. Find the products that contribute to at least 25% of total sales for each store.

---

### 12. **Case Statements**

**Tables:**
- `Employees`: (employee_id, name, department_id, salary, hire_date)
- `Departments`: (department_id, department_name)

#### Easy:
1. Write a query to classify employees based on their salary into "Low", "Medium", and "High".
2. Retrieve the names of employees and mark whether they have been hired in the last 5 years as "Recent" or "Experienced".

#### Medium:
1. Find employees who belong to the "Engineering" department and categorize their salaries as "Below Avg" or "Above Avg" based on the department's average salary.
2. Write a query to assign each employee a bonus category: 5%, 10%, or 15% based on their salary range.
3. Retrieve the number of employees in each department, labeling departments with more than 10 employees as "Large" and the rest as "Small".

#### Hard:
1. Write a query to dynamically assign employees to salary bands (e.g., below 50K = "Entry Level", 50K-100K = "Mid Level", above 100K = "Senior Level"), and display the count for each band.
2. Retrieve the department with the highest average salary, using a `CASE` statement to label departments as "High-paying" or "Low-paying".
3. Find employees who have been with the company for more than 10 years and categorize them based on their salary increase over the years: "High Growth" if > 20%, "Moderate Growth" if between 10-20%, and "Low Growth" otherwise.

---

### 13. **Common Table Expressions (CTEs)**

**Tables:**
- `Transactions`: (transaction_id, user_id, transaction_date, amount)
- `Users`: (user_id, user_name, signup_date)

#### Easy:
1. Write a CTE to retrieve users who made transactions in the last 30 days.
2. Create a CTE to find the total transaction amount per user.

#### Medium:
1. Using a CTE, find users who have made more than 5 transactions, but only consider transactions in the last year.
2. Create a CTE to find the average transaction amount per user, then retrieve users whose average transaction exceeds $100.
3. Write a query using CTEs to find the top 3 users with the highest total transaction amount in 2023.

#### Hard:
1. Create a recursive CTE to find users who made at least one transaction each month in 2023.
2. Write a CTE to calculate the difference between each user's first and last transaction, and retrieve users with the highest transaction growth.
3. Use a CTE to retrieve users who have spent more than the average for all users, but only in the months where their transaction total exceeded $500.

---

### 14. **Recursive Queries**

**Tables:**
- `Employees`: (employee_id, name, manager_id, salary, hire_date)

#### Easy:
1. Write a recursive query to find the hierarchy of managers and their direct reports.
2. Retrieve all employees who report directly or indirectly to a specific manager.

#### Medium:
1. Using a recursive query, find the total number of employees under each manager, including indirect reports.
2. Write a query to find all employees who report to managers in the "Engineering" department.
3. Retrieve employees who are at least 3 levels deep in the hierarchy, starting from the CEO.

#### Hard:
1. Write a recursive query to find the path from a specific employee to the CEO in the reporting hierarchy.
2. Create a recursive query to calculate the total salary of all employees reporting to each manager, including indirect reports.
3. Use a recursive query to find the shortest reporting chain between two employees in the organization.

---

### 15. **Performance Tuning (Indexes, Query Optimization)**

**Tables:**
- `Customers`: (customer_id, name, location)
- `Orders`: (order_id, customer_id, order_date, total_amount)

#### Easy:
1. Write an optimized query to find the total number of orders placed by each customer, using indexing on `customer_id`.
2. Retrieve the total order amount for customers in 2023, ensuring the query uses an index on `order_date`.

#### Medium:
1. Write a query to retrieve the top 5 customers by order amount, but ensure that an index on `total_amount` is used.
2. Optimize a query that retrieves the total sales per month, by creating an index on `order_date`.
3. Write a query to retrieve the customers who placed more than 5 orders, optimizing it using the appropriate indexes on `customer_id`.

#### Hard:
1. Create an optimized query that retrieves customers with the largest year-over-year growth in order amounts, ensuring it uses indexes on `order_date` and `customer_id`.
2. Write an optimized query to find the average order amount for customers in each location, using partitioning and indexing to improve performance.
3. Write a query that retrieves customers with the highest order frequency, ensuring the query is optimized by indexing `order_date` and `customer_id`.

---

### 16. **Handling NULLs**

**Tables:**
- `Products`: (product_id, product_name, price)
- `Sales`: (sale_id, product_id, sale_date, quantity, discount)

#### Easy:
1. Write a query to find all products with a `NULL` price.
2. Retrieve all sales where the `discount` is not `NULL`.

#### Medium:
1. Write a query to replace `NULL` values in the `discount` column with 0 and calculate the total revenue.
2. Retrieve the average price of products, treating `NULL` prices as 0.
3. Write a query to count how many products have a `NULL` price and how many do not.

#### Hard:
1. Write a query to retrieve products where the `price` is `NULL`, but their total sales (quantity * price) are not, using a conditional calculation.
2. Retrieve all sales with a `NULL` discount, and calculate the total revenue as if the discount was 0, ensuring no rows with `NULL` values are excluded.
3. Write a query to replace all `NULL` prices with the average price of the product category.

---

### 17. **Pivot and Unpivot Operations**

**Tables:**
- `Sales`: (sale_id, product_id, sale_date, quantity, store_id)
- `Stores`: (store_id, store_name, location)

#### Easy:
1. Write a query to pivot the total sales per store by month.
2. Retrieve the total quantity sold per store and product category using a pivot operation.

#### Medium:
1. Write a query to pivot the total sales per product, grouped by store and quarter.
2. Using a pivot operation, calculate the total sales per store for each quarter in 2023.
3. Create a query to unpivot the sales data, showing the total sales per store for each month in a single row.

#### Hard:
1. Write a query to pivot the sales data by year, product category, and store location, and calculate the total revenue in each combination.
2. Using a pivot operation, find the stores with the highest total sales in each quarter, and pivot the data to show the results by year and store.
3. Write a query to unpivot sales data across multiple years, showing total sales for each product, store, and year in one row.

---

### 18. **Data Types and Conversions**

**Tables:**
- `Transactions`: (transaction_id,

 user_id, transaction_date, amount, status)

#### Easy:
1. Write a query to convert the `transaction_date` column from a string to a `DATE` type.
2. Convert the `amount` column from a string to a numeric value in a query and calculate the total.

#### Medium:
1. Write a query to retrieve all transactions, converting `transaction_date` to a `TIMESTAMP` and sorting by time.
2. Convert the `amount` column to a numeric value, rounding it to 2 decimal places, and calculate the total.
3. Write a query to change the `status` column from an integer to a string, mapping 0 to "Pending", 1 to "Completed", and 2 to "Failed".

#### Hard:
1. Write a query to convert the `amount` column to a different currency, applying a conversion factor based on the user's location, and calculate the total.
2. Retrieve transactions where the `amount` was entered as a string, convert it to numeric, and flag any entries that couldn't be converted.
3. Write a query to convert the `transaction_date` to UTC and calculate the total transaction amount by day, using time zone conversions for different users.

---

### 19. **String Functions**

**Tables:**
- `Customers`: (customer_id, name, email, phone_number)
- `Orders`: (order_id, customer_id, order_date, total_amount)

#### Easy:
1. Write a query to retrieve all customers whose email ends with "gmail.com".
2. Find customers whose phone number contains the area code "123".

#### Medium:
1. Write a query to extract the domain from each customer's email address and group by domain.
2. Retrieve the total order amount for customers with names starting with "A".
3. Write a query to extract the first name and last name from the `name` column, assuming names are separated by a space.

#### Hard:
1. Write a query to find customers who have placed orders using emails with non-standard characters, using string pattern matching to detect invalid characters.
2. Write a query to calculate the total sales for customers, but only include those whose names contain exactly 3 vowels.
3. Retrieve all customers who have placed orders in 2023, but only if their email domain contains a numeric value (e.g., "abc123.com").

---

### 20. **Joins (Inner, Outer, Cross, Self)**

**Tables:**
- `Employees`: (employee_id, department_id, name, salary)
- `Departments`: (department_id, department_name)

#### Easy:
1. Write an inner join query to retrieve the names and salaries of all employees along with their department names.
2. Write a query to retrieve all employees, along with their department names, using a left outer join.

#### Medium:
1. Write a query to find employees whose salary is above the average salary of their department using a self-join.
2. Write a query to retrieve all departments with more than 10 employees using an outer join.
3. Use a cross join to retrieve all possible department-employee pairings and filter for employees making above the department average salary.

#### Hard:
1. Write a query using a cross join to find all possible department-employee combinations and retrieve the department with the most overlapping employees.
2. Write a query using a self-join to find the employees who have higher salaries than their managers.
3. Write a query to retrieve the department with the highest total salary expenditure, using an inner join and filtering by departments with more than 5 employees.

---

### 21. **Working with JSON**

**Easy:**

1. Query the names of all customers from a table `customer_info` where the JSON field `address` contains the city as 'New York'.  
    **Table:**
    
    - `customer_info`: `id`, `name`, `address (JSON)`
2. Extract the value of the `phone_number` key from a JSON field `contact_details` for all employees in `employee_data`. **Table:**
    
    - `employee_data`: `id`, `name`, `contact_details (JSON)`

**Medium:**

1. In the `orders` table, filter all orders where the `shipping_details` JSON field contains a key `status` with a value of 'delivered'. **Table:**
    
    - `orders`: `order_id`, `customer_id`, `order_date`, `shipping_details (JSON)`
2. From the `user_preferences` table, retrieve the users who have 'email' set to 'subscribed' in their `preferences` JSON column. **Table:**
    
    - `user_preferences`: `user_id`, `name`, `preferences (JSON)`
3. Aggregate data from a table `products` where the `specs` JSON field contains a `category` key. Group the data based on the `category` value. **Table:**
    
    - `products`: `product_id`, `name`, `specs (JSON)`

**Hard/Advanced:**

1. Find the top 3 cities where customers live by extracting and grouping the `city` from the `address` JSON column in the `customer_info` table. **Table:**
    
    - `customer_info`: `id`, `name`, `address (JSON)`
2. In a table `user_data` with a JSON column `activity_log`, extract users who logged in at least 5 times in the last month. **Table:**
    
    - `user_data`: `user_id`, `name`, `activity_log (JSON)`
3. Retrieve all orders where the nested JSON field `order_details` contains products with a `price` greater than 100. **Table:**
    
    - `orders`: `order_id`, `order_details (JSON)`

### 22. **Constraints and Relationships**

**Easy:**

1. Find all rows in the `orders` table where a `foreign key` constraint links `customer_id` to the `customers` table and the customer’s `status` is 'active'. **Table:**
    
    - `orders`: `order_id`, `customer_id`, `order_date`
    - `customers`: `customer_id`, `status`
2. List all the `foreign key` constraints in the `order_items` table that reference the `products` table. **Table:**
    
    - `order_items`: `item_id`, `order_id`, `product_id`
    - `products`: `product_id`, `name`

**Medium:**

1. Display all rows from the `employees` table that violate a `CHECK` constraint ensuring that the `salary` is above 30000. **Table:**
    
    - `employees`: `id`, `name`, `salary`
2. For the `orders` table, select all rows where there is a `foreign key` constraint on `customer_id` that links to the `customers` table, and the customer has made more than 5 orders. **Table:**
    
    - `orders`: `order_id`, `customer_id`, `order_date`
    - `customers`: `customer_id`, `name`
3. Find all orders in the `orders` table that do not have a corresponding entry in the `order_items` table. **Table:**
    
    - `orders`: `order_id`, `customer_id`
    - `order_items`: `order_id`, `product_id`

**Hard/Advanced:**

1. Create a query that checks for `ON DELETE CASCADE` behavior in the `order_items` table when a row in the `orders` table is deleted. Ensure that all dependent rows are removed. **Table:**
    
    - `orders`: `order_id`
    - `order_items`: `order_id`, `product_id`
2. Write a query that identifies all employees in the `employees` table whose `department_id` does not match any department in the `departments` table (i.e., violating a foreign key). **Table:**
    
    - `employees`: `employee_id`, `department_id`
    - `departments`: `department_id`, `name`
3. Detect and return any rows in the `order_items` table where a foreign key constraint referencing `products.product_id` is missing or broken. **Table:**
    
    - `order_items`: `item_id`, `product_id`
    - `products`: `product_id`

### 23. **Indexing and Query Optimization**

**Easy:**

1. Identify all indexes in the `products` table and determine their effectiveness in improving query performance. **Table:**
    
    - `products`: `product_id`, `name`
2. Retrieve all records from the `users` table ordered by `created_at`, ensuring the query uses the index on `created_at` for optimization. **Table:**
    
    - `users`: `user_id`, `created_at`

**Medium:**

1. Explain the query execution plan for fetching `order_id` and `customer_id` from the `orders` table, and suggest optimizations using indexes. **Table:**
    
    - `orders`: `order_id`, `customer_id`
2. Analyze the performance difference between a query with and without indexing on the `email` column in the `customers` table. **Table:**
    
    - `customers`: `customer_id`, `email`
3. In a table `sales`, create an index on `sale_date` and `region` columns, and retrieve data within the last year to compare performance before and after the index. **Table:**
    
    - `sales`: `sale_id`, `sale_date`, `region`

**Hard/Advanced:**

1. Write a query that leverages a composite index on `customer_id` and `order_date` in the `orders` table to improve performance for queries retrieving orders made by customers in the last 30 days. **Table:**
    
    - `orders`: `order_id`, `customer_id`, `order_date`
2. Identify queries in the `products` table that are not using the available indexes and optimize them. Justify your changes with query performance statistics. **Table:**
    
    - `products`: `product_id`, `name`
3. Perform a full-text search on the `description` column in the `articles` table and optimize the search using indexing strategies. **Table:**
    
    - `articles`: `article_id`, `title`, `description`