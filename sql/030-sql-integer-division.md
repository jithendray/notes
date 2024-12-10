---
tags:
  - sql
---
# Integer Division PostgreSQL

In PostgreSQL (as in many other programming and query languages), when you perform division between two integers, the result is also an integer. This is known as integer division. For example, `5 / 2` would result in `2` rather than `2.5`.

To ensure that the division operation results in a floating-point number (i.e., retains any fractional part), you can multiply the numerator (or the denominator) by `1.0`. This effectively converts the numerator (or denominator) to a floating-point number, which in turn forces the division to be a floating-point division.

Here’s a detailed explanation with examples:

1. **Integer Division:**
    
    ```sql
    SELECT 5 / 2 AS result;  -- Result is 2
    ```
    
2. **Floating-Point Division:** By multiplying the numerator by `1.0`, you convert it to a floating-point number, thus forcing floating-point division.
    
    ```sql
    SELECT 5 * 1.0 / 2 AS result;  -- Result is 2.5
    ```
    
    Alternatively, you can cast one of the operands to `double precision`:
    
    ```sql
    SELECT 5 / 2::double precision AS result;  -- Result is 2.5
    ```
    
    Or:
    
    ```sql
    SELECT CAST(5 AS double precision) / 2 AS result;  -- Result is 2.5
    ```
    

**Why this approach is used:**

- **Type Promotion:** Multiplying by `1.0` is a simple and common way to promote an integer to a floating-point number.
- **Ensuring Precision:** When you need precise calculations involving division that should result in fractional values, promoting one of the operands to a floating-point type ensures the division operation does not lose the fractional part.