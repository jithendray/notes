---
tags: sql
---
# to_char in PostgreSQL

The `to_char` function in PostgreSQL is used to format various data types, such as dates and numbers, into a string according to a specified format. It is particularly useful when you need to display data in a human-readable format or when you need specific parts of a date or number.

**Basic Syntax**

```sql
sqlCopy code
to_char(value, format)
```

- **value**: The value you want to format (can be a date, timestamp, or number).
- **format**: The format string that specifies how the value should be formatted.

**Commonly Used Format Patterns**

**For Dates and Timestamps**

- `YYYY`: Year in 4 digits (e.g., 2024)
- `YY`: Year in 2 digits (e.g., 24)
- `MM`: Month number (01-12)
- `Mon`: Abbreviated month name (e.g., Jan, Feb)
- `Month`: Full month name (e.g., January)
- `DD`: Day of the month (01-31)
- `Day`: Full name of the day of the week (e.g., Sunday)
- `HH24`: Hour in 24-hour format (00-23)
- `HH12`: Hour in 12-hour format (01-12)
- `MI`: Minutes (00-59)
- `SS`: Seconds (00-59)

**For Numbers**

- `9`: Digit placeholder (can be multiple digits, e.g., `999`)
- `0`: Digit placeholder that pads with zeros
- `.`: Decimal point
- `,`: Group (thousands) separator
- `S`: Sign (`+` or ``)

**Examples**

**Formatting Dates**

1. **Extract Year and Month**
    
    ```sql
    sqlCopy code
    SELECT to_char(order_date, 'YYYY-MM') AS year_month
    FROM orders;
    ```
    
2. **Full Date with Month Name**
    
    ```sql
    sqlCopy code
    SELECT to_char(order_date, 'Month DD, YYYY') AS formatted_date
    FROM orders;
    ```
    
3. **Day of the Week**
    
    ```sql
    sqlCopy code
    SELECT to_char(order_date, 'Day') AS day_of_week
    FROM orders;
    ```
    

**Formatting Numbers**

1. **Integer with Leading Zeros**
    
    ```sql
    sqlCopy code
    SELECT to_char(7, '000') AS formatted_number;
    -- Output: '007'
    ```
    
2. **Decimal Number with Thousands Separator**
    
    ```sql
    sqlCopy code
    SELECT to_char(12345.67, '9,999.99') AS formatted_number;
    -- Output: '12,345.67'
    ```
    
3. **Currency Format**
    
    ```sql
    sqlCopy code
    SELECT to_char(12345.67, 'L9,999.99') AS formatted_number;
    -- Output depends on locale setting, e.g., '$12,345.67'
    ```