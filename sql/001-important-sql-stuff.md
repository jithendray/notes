---
tags: sql
---
# SQL stuff to remember
### `count(*)` vs `count(id)`
- **Use `COUNT(*)`**: When you want to count **all rows**, regardless of whether columns contain `NULL` values. Since `emp_id` is typically a primary key (or not `NULL`), `COUNT(*)` and `COUNT(emp_id)` will generally give the same result in this scenario.

- **Use `COUNT(emp_id)`**: If you specifically want to count non-`NULL` values in the `emp_id` column. This could be useful if `emp_id` is nullable (which is rare) or if you’re counting a different nullable column.

---
### where vs having
- use `HAVING` to filter after aggregation

- use `GROUP BY` to filter before aggregation

---
### Tuple Distinct Counting in PostgreSQL

- when there is a need to concatenate values across multiple columns - instead of type casting into VARCHAR and using string concatenation - use tuple distinct counting.
- this is more efficient and avoids overhead of type casting and concatenation.
- for example 
	- if u want to calculate distinct count of shipments - and the unique key for one shipment is a combination of shipment_id and sub_id.
	- use `count(distinct (shipment_id, sub_id))`  instead of `count(distinct shipment_id::varchar || sub_id::varchar)`.
---
### LIKE ANY(ARRAY[]) and SIMILAR TO
- if you want to match multiple strings 

```sql
when lower(business_name) like '%cafe%' or lower(business_name) like '%café%' 
```

```sql
when lower(business_name) like any(array['%cafe%','%café%'])
```

```sql
when lower(business_name) similar to '%cafe|café%'
```

---
### Using `CASE WHEN` Inside `COUNT()` and `SUM()`

#### Using `CASE WHEN` Inside `COUNT()`
**Purpose**: Count rows based on specific conditions.

**Syntax**:
```sql
COUNT(CASE WHEN condition THEN 1 END)
```

**Example**:
```sql
SELECT 
    COUNT(CASE WHEN status = 'completed' THEN 1 END) AS completed_sales_count
FROM 
    sales;
```

#### Using `CASE WHEN` Inside `SUM()`

**Purpose**: Sum values conditionally based on specific criteria.

**Syntax**:
```sql
SUM(CASE WHEN condition THEN value_column ELSE 0 END)
```

**Example**:
```sql
SELECT 
    SUM(CASE WHEN status = 'completed' THEN quantity ELSE 0 END) AS total_completed_quantity
FROM 
    sales;
```

#### Using `CASE WHEN` in Other Functions

You can use `CASE WHEN` with various aggregate and non-aggregate functions in SQL, including:

1. **AVG()**: Calculate the average based on conditions.
```sql
AVG(CASE WHEN condition THEN value_column END)
```

2. **MAX()**: Get the maximum value based on conditions.
```sql
MAX(CASE WHEN condition THEN value_column END)
```

3. **MIN()**: Get the minimum value based on conditions.
```sql
MIN(CASE WHEN condition THEN value_column END)
```

4. **STRING_AGG()**: Concatenate values conditionally (in databases that support it).
```sql
STRING_AGG(CASE WHEN condition THEN value_column END, ', ')
```

---
### Regular Expression `description ~* '\y(plum|cherry|rose|hazelnut)\y'`

This is the regular expression pattern that is being matched against the `description`. Let's break down this pattern:
- The `~` operator in PostgreSQL (and some other databases) is used for case-sensitive regular expression matching.
- If you want case-insensitive matching with regular expressions, you would typically use `~*`. However, if `lower()` is applied, `~` works effectively as the case is already normalized.
- **`\y`**: This is a word boundary anchor in PostgreSQL regex syntax. It asserts a position at the beginning or end of a word. In this context:
	- `\y` at the beginning indicates that the match must start at a word boundary.
	- `\y` at the end indicates that the match must end at a word boundary.
- **`(plum|cherry|rose|hazelnut)`**: This is a group of alternatives, also known as a capturing group:
	- The `|` operator acts as a logical "OR". So this part of the regex matches any of the four specified words: "plum", "cherry", "rose", or "hazelnut".
- **Combined**: The whole expression `'\y(plum|cherry|rose|hazelnut)\y'` means "match any occurrence of the words plum, cherry, rose, or hazelnut, ensuring they are complete words (not substrings within other words)".

Example Matches
- It would match:
	- "This wine has aromas of plum."
    - "Cherry flavor is prominent in this blend."
    - "Notes of rose and hazelnut are present."
- It would **not** match:
    - "The plumcake was delicious." (because "plum" is part of "plumcake").
    - "Cherryblossom trees bloom in spring." (because "cherry" is part of "cherryblossom").
    - "I love hazelnuts." (because "hazelnuts" is not the exact word "hazelnut").

---
### THIS
- this 
- dsjklfhjkldsjf
- sdlkfjslkdfjjdfs
- zdlsfjlgnzdkjfgnvdjkzlfbv
- kjdfnjkdzfghkudf 
	-  lkxjvkjldfnvkldjsfjgnldfs
	- dfldkfnblkdfjnldfgk
	- dflkbndfgbfgklbn
- dlfknvdklbvndlkfbnd
	- dlfmkdfklbvndf
	- dfbdfbdf
- dfbvlkcfblvkdnblkdnfb
	- dbvkd;lfbvmdf
	- dfgbbdfbdfgb
	- dfbdfbdfgbnfgbhrt
- ;dmbflkfxdgnblkfxnkl 
- lo;dfjb;djb;odfc
- lkxfmvlkcbmnvlkdfbjnmlkdbfjnldf
- 