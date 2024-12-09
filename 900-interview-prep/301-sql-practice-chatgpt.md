---
title: SQL practice Questions
---
The following questions are generated using chatGPT. If I thoroughly practice these questions, covering the broad range of topics, it should be sufficient for building a strong foundation for SQL interviews. However, to ensure I am fully prepared for a variety of question styles, it would still be beneficial to practice more problems from sites like leetcode, stratascratch and data lemur.

### 1. **Basic Querying (CRUD Operations)**

**Tables:**
- `Employees`: (id, name, age, department_id, salary)
- `Departments`: (id, department_name)
#### Easy:
1. Retrieve the names and ages of all employees from the `Employees` table.
```sql
select name, age from Employees
```

2. Find the department names where employees have a salary greater than 50,000.
```sql
select distinct d.department_name
from Employees e join Departments d
on e.department_id = d.id
where e.salary > 50000
```

#### Medium:
1. Find the number of employees in each department.
```sql
   select d.department_name, count(distinct e.id) as cnt
   from Employees e left join Departments d
   on e.department_id = d.id
   group by d.department_name
```
2. Update the salary of employees in the "Finance" department by increasing it by 10%.
```sql
   update Employees e
   set e.salary = e.salary + 0.1 * e.salary
   from Departments d
   where e.department_id = d.id
   and d.department_name = 'Finance'
```
3. Delete all employees from the `Employees` table who are younger than 25.
```sql
   delete from Employees
   where age < 25
```

#### Hard:
1. Write a query to retrieve the top 5 highest-paid employees in each department.
```sql
   select department_id, name
   from (select department_id, name, 
   rank() over (partition by department_id order by salary desc) as rnk
   from Employees) as ranked_employees
   where rnk <6;
```

2. Insert a new employee into the `Employees` table, ensuring the department exists in the `Departments` table. Use transactions to ensure data integrity.
```sql
   begin;
   
   if exists (select 1 from Departments where id = :dep_id:)
   then
   insert into Employees (name, age, salary, department_id)
   values (:name:, :age:, :salary:, :dep_id:);
   
   commit;
   
   else
   rollback;
   raise notice 'department id % does not exist, employee not added', :dep_id:;
   
   end if;
```

3. Find the name of the department with the highest total employee salary.
```sql
   select dep.name
   from (
	   select department_id, sum(salary) as total_salary,
	   rank() over (order by sum(salary) desc) as rnk
	   from Employees
	   group by department_id
   ) as dept_salaries
   join Departments dep
   on dept_salaries.department_id = dep.id
   where rnk = 1
```

----
### 2. **Joins**

**Tables:**

- `Orders`: (order_id, customer_id, order_date, amount)
- `Customers`: (customer_id, customer_name, customer_address)

#### Easy:

1. Retrieve all orders along with the customer names.
```sql
select c.customer_name, o.order_id
from Orders o
left join Customers c
on o.customer_id = c.customer_id
```
2. Find the total amount of all orders placed by each customer.
```sql
select c.customer_name, sum(o.amount) as total_amount
from Orders o
left join Customers c
on o.customer_id = c.customer_id
group by c.customer_name;
```

#### Medium:

1. List all customers who have not placed any orders.
```sql
select distinct c.customer_name
from Orders o
right join Customers c
on o.customer_id = c.customer_id
where order_id is null
```
2. Write a query to find customers who placed more than 5 orders in the last 6 months.
```sql
select c.customer_name
from Orders o
left join Customers c
on o.customer_id = c.customer_id
where order_date >= now() - interval '6 months'
group by o.customer_id
having count(order_id) > 5
```
3. Retrieve the customer names and their order amounts for orders that exceed $500.
```sql
select c.customer_name, o.order_id, o.amount
from Orders o
join Customers c
on o.customer_id = c.customer_id
where o.amount > 500
```
 note: using left join - VALID - if you want to include customers with no orders; however, it will still return NULL values for amount in those cases. use INNER JOIN to only retrieve customers who have placed orders exceeding $500
#### Hard:

1. Find the top 3 customers based on the total value of their orders, but only for orders placed in 2023.
```sql
with total_orders as (select c.customer_name, sum(o.amount) as total_amount
from Orders o
left join Customers c
on o.customer_id = c.customer_id
where extract(year from o.order_date) = 2023
group by c.customer_name
),
ranked_customers as (select customer_name, total_amount,
	rank() over (order by total_amount desc) as rnk
)
select customer_name
from ranked_customers
where rnk <= 3;
```
2. Write a query to find customers who placed their first order in 2023 and then placed an additional order within the next 30 days.
```sql
with first_orders as (
	select customer_id, min(order_date) as first_order_date
	from Orders
	group by customer_id
	having min(order_date) >= '2023-01-01' and min(order_date) < '2024-01-01'
),
additional_orders as (
	select o.customer_id
	from Orders o
	inner join first_orders f
	on o.customer_id = f.customer_id
	where o.order_date > f.first_order_date
	and o.order_date <= f.first_order_date + interval '30 days'
)
select distinct c.customer_name
from additional_orders ao
inner join Customers c
on ao.customer_id = c.customer_id
```
3. Write a self-join query to retrieve all customers who have the same order amount for at least two different orders.
```sql
select o1.customer_id, o1.order_id, o1.amount
from Orders o1
join Orders o2
on o1.customer_id = o2.customer_id
and o1.order_id <> o2.order_id
where o1.amount = o2.amount
```

---

### 3. **Subqueries**

**Tables:**

- `Products`: (product_id, product_name, price)
- `Sales`: (sale_id, product_id, sale_date, quantity)

#### Easy:

1. Find the products that have never been sold.
```sql
select p.product_name 
from Sales s 
right join Products p 
on s.product_id = p.product_id 
where sale_id is null
```
or
```sql
select product_name 
from Products 
where product_id not in ( 
	select product_id from Sales 
)
```
2. Retrieve the names of products that have been sold more than 100 times.
```sql
select p.product_name 
from Products p join Sales s 
on p.product_id = s.product_id 
group by p.product_name 
having count(sale_id) > 100;
```
or
```sql
select product_name 
from Products 
where product_id in ( 
	select product_id 
	from Sales 
	group by product_id 
	having count(sale_id) > 100 
);
```
#### Medium:

1. Write a query to find the average price of all products that have been sold at least once.
```sql
SELECT AVG(p.price) AS average_price 
FROM Products p 
WHERE p.product_id IN (SELECT s.product_id FROM Sales s);
```
2. Retrieve the names of the products whose price is above the average price.
```sql
select product_name 
from Products 
where price > ( select avg(price) from Products );
```
3. Find the products that have the second highest total sales.
```sql
with total_sales as ( 
	select p.product_id, p.product_name, sum(s.quantity * p.price) as total_sales
	from Products p join Sales s 
	on p.product_id = s.product_id 
	group by p.product_id, p.product_name 
), 
ranked_sales as ( 
	select product_id, product_name, total_sales, 
	dense_rank() over (order by total_sales desc) as rnk 
	from total_sales 
) 
select product_name from ranked_sales where rnk = 2;
```

#### Hard:

1. Write a query to find products that have been sold exactly once in each month of 2023.
```sql
with monthly_sales as ( 
	select product_id, count(sale_id) as sales, 
	to_char(sale_date,'YYYY-MM') as year_month 
	from Sales 
	where sale_date >= '2023-01-01' and sale_date < '2024-01-01' 
	group by product_id, to_char(sale_date,'YYYY-MM') 
	having count(sale_id) = 1 
) 
select p.product_name 
from Products p join monthly_sales ms 
on p.product_id = ms.product_id;
```
2. Using a correlated subquery, retrieve products where the total quantity sold is greater than the average quantity sold per product.
3. Find products that have a price greater than the average price of products sold in the same month as their first sale.
```sql
with first_sales as ( 
	select product_id, min(sale_date) as first_sale_date 
	from Sales 
	group by product_id 
), 
average_monthly as ( 
	select 
	extract(year from s.sale_date) as sale_year, 
	extract(month from s.sale_date) as sale_month, 
	avg(p.price) as monthly_average_price 
	from Sales s join Products p 
	on s.product_id = p.product_id 
	group by extract(year from s.sale_date), 
	extract(month from s.sale_date) 
) 
select p.product_name 
from Products p 
join first_sales fs on p.product_id = fs.product_id 
join average_monthly am on extract(year from fs.first_sale_date) = am.sale_year
and extract(month from fs.first_sale_date) = am.sale_month 
where p.price > am.monthly_average_price;
```

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

- `Transactions`: (transaction_id, user_id, transaction_date, amount, status)

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