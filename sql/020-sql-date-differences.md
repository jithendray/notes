---
tags:
  - sql
---
# Data differences in SQL

In PostgreSQL, there is no direct `DATEDIFF` function as found in some other SQL databases like SQL Server. However, you can achieve the same result using various date and time functions provided by PostgreSQL.

To calculate the difference between two dates, you can simply subtract them, which will return the difference in days. Here are a few examples:

1. **Calculating the difference in days:**
    
    ```sql
    SELECT (date1 - date2) AS date_difference
    FROM your_table;
    ```
    
2. **Calculating the difference in months:**
    
    ```sql
    SELECT EXTRACT(MONTH FROM age(date1, date2)) AS month_difference
    FROM your_table;
    ```
    
3. **Calculating the difference in years:**
    
    ```sql
    SELECT EXTRACT(YEAR FROM age(date1, date2)) AS year_difference
    FROM your_table;
    ```
    
4. **Using the `AGE` function:**
    
    The `AGE` function can provide a more detailed interval between two dates, including years, months, and days.
    
    ```sql
    SELECT age(date1, date2) AS age_difference
    FROM your_table;
    ```
    
5. **Calculating the difference in hours, minutes, or seconds:**
    
    You can use the `EXTRACT` function to get specific parts of the interval.
    
    ```sql
    SELECT EXTRACT(EPOCH FROM (timestamp1 - timestamp2)) / 3600 AS hours_difference,
           EXTRACT(EPOCH FROM (timestamp1 - timestamp2)) / 60 AS minutes_difference,
           EXTRACT(EPOCH FROM (timestamp1 - timestamp2)) AS seconds_difference
    FROM your_table;
    ```
    
6. **Using `INTERVAL` function:**
    
    ```sql
    SELECT (CURRENT_DATE - INTERVAL '29 days') AS current_date_30days
    FROM your_table
    ```
    
    Using `INTERVAL '29 days'` covers a 30-day period because it includes both the start and end dates. Here's a detailed breakdown of why `29 days` is used instead of `30 days`:
    
    **Period Calculation:**
    
    - If you want a period ending on `2019-07-27` and inclusive of this date, you subtract `29 days` from `2019-07-27`.
    - This gives you a starting date of `2019-06-28`, making the period from `2019-06-28` to `2019-07-27`, inclusive.
    
    Let's see this with an example:
    
    - End date: `2019-07-27`
    - Start date: `2019-06-28`
    
    The period from `2019-06-28` to `2019-07-27` includes exactly 30 days.
    
    **Inclusive Period:**
    
    Here's the mathematical reasoning:
    
    - When you subtract `29 days` from `2019-07-27`, you are counting backward, including the end date.
    - Therefore, `2019-07-27` minus `29 days` covers `2019-07-27` itself and the previous `29` days, which totals `30` days.