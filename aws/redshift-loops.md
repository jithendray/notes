---
tags: aws
---
# Redshift: Loops and Conditional Expressions

When we need to iterate through the data or implement logic conditionally, SQL's looping and conditional expressions come in particularly handy. In this post, I'll explain how to use them in Redshift SQL.

### Redshift SQL - PL/pgsql

Redshift implements stored procedures using the PostgreSQL procedural language (PL/pgsql). Control structures in pgsqlpl can be utilised to build vital business logics for flexible data manipulation. Redshift can also use majority of plpgsql statements, but not all of them. For future reference, I'm listing the Redshift supported control structures in this post.

  
### Control Structures in Redshift

Redshift's control structures are usually divided into two categories:

- Conditional
- Iterative


### Conditional Control Structures

A task can be carried out using conditional control structures under specific circumstances. In conditional control structures, IF statements are commonly utilised. These operate similarly to general-purpose programming languages. If a value in a table needs to be updated based on a conditional flow, for instance, IF statements can be utilised in this situation. In Redshift's plpgsql, there are three types of conditional control structures available:

- IF - THEN - END IF

- IF - THEN - ELSE - END IF

- IF - THEN - ELSIF - THEN - ELSE - END IF

#### CASE vs IF in SQL
  
CASE expressions and IF statements are similar. CASE can be substituted with IF, but not the other way around. While  CASE is an expression that is used to return data values, IF is a statement that is frequently used for logic flow. Although it has several limitations, CASE is essentially like a function with built-in IF logic. CASE is limited in comparison to what an IF statement may accomplish.

#### Syntax for IF statements

Note: The following syntaxes are taken from the [Redshift documentation](https://docs.aws.amazon.com/redshift/latest/dg/c_PLpgSQL-statements.html#r_PLpgSQL-conditionals-if).


**IF-THEN-END IF**

```sql
IF <boolean-expression> THEN

  statements

END IF;      
```


**IF-THEN-ELSE-END IF**
```sql
IF <boolean-expression> THEN

  statements

ELSE

  statements

END IF;
```

**IF-THEN-ELSIF-THEN-ELSE-END IF**
```sql
IF <boolean-expression> THEN

  statements

[ ELSIF <boolean-expression> THEN

  statements

[ ELSIF <boolean-expression> THEN

  statements

    ...] ]

[ ELSE

  statements ]

END IF;
```


#### Example

Let's say your teammate inserted a few rows into "open_jobs" table with wrong "profession"s as either 'Data Analyst' or 'Data Scientist' instead of 'Data Engineer'. You have to update the "profession" of those rows to 'Data Engineer'. If no wrong rows are there, then you have to re-insert the rows of 'Data Engineer' profession. If rows are there, raise a notice.
  

```sql
CREATE OR REPLACE PROCEDURE data_cleaning()
LANGUAGE plpgsql
AS $$
BEGIN
    IF EXISTS (SELECT FROM open_jobs WHERE profession='Data Analyst') THEN
    UPDATE open_jobs SET profession='Data Engineer' WHERE profession='Data Analyst';
    ELSIF EXISTS (SELECT FROM open_jobs WHERE profession='Data Scientist') THEN
    UPDATE open_jobs SET profession='Data Engineer' WHERE profession='Data Scientist';
    ELSIF NOT EXISTS  (SELECT FROM open_jobs WHERE profession='Data Engineer') THEN
    INSERT INTO open_jobs SELECT * FROM stage_jobs WHERE profession='Data Engineer';
    ELSE
    RAISE INFO 'The Data Engineer open job rows were correctly inserted into the table'
END;
$$;
```


### Iterative Control Structures
The execution flow of plpgsql commands can be managed with the use of iterative control statements. The following iterative control structures are supported by Redshift:

- LOOP
- EXIT
- FOR
- WHILE
- CONTINUE

LOOP and EXIT are keywords used in plpgsql procedures for starting and ending the loop respectively. WHILE and FOR loops operate similarly to general-purpose programming languages.

Lets see simple examples using LOOPS in Redshift SQL.

#### LOOP - EXIT - WHEN

```sql
CREATE OR REPLACE PROCEDURE simple_loop()
LANGUAGE plpgsql
AS $$
DECLARE
  count INTEGER := 0;
BEGIN
  <<print_waiting>>
  LOOP
  RAISE INFO 'WAITING: %', count;
  cnt = cnt + 1;
    EXIT print_waiting WHEN (cnt > 10);
  END LOOP;
END;
$$;
```

output:

```
CALL redshift_simple_loop();

INFO:  WAITING: 0
INFO:  WAITING: 1
INFO:  WAITING: 2
INFO:  WAITING: 3
INFO:  WAITING: 4
INFO:  WAITING: 5
INFO:  WAITING: 6
INFO:  WAITING: 7
INFO:  WAITING: 8
INFO:  WAITING: 9
INFO:  WAITING: 10
```


#### WHILE LOOP

```sql
WHILE revenue > 0 AND cost > 0 LOOP
  -- some computations here
END LOOP;
```

#### FOR LOOP

```sql
CREATE OR REPLACE PROCEDURE records_info_message()
LANGUAGE plpgsql
AS $$
DECLARE
  r record;
BEGIN
    for f in select title, year
            from film_details
            where director='RAJAMOULI'
  LOOP
  RAISE INFO '%(year: %)', f.title, f.director;;
  END LOOP;
END;
$$;
```

output:
```
INFO:  RRR(year: 2022)
INFO:  Bahubali 2(year: 2017)
INFO:  Bahubali(year: 2015)
```

  
### Fibonacci using LOOPs in UDFs

I explained about using SQL UDFs with Redshift in my [previous blog](https://jithendray.github.io/redshift-udf/). We may execute sophisticated logics like computing the Fibonacci number for each value in a table column in SQL by using LOOPs in UDFs.
  

```sql
CREATE FUNCTION f_fibonacci(int)
returns float stable AS $$
DECLARE n INTEGER:= $1;
fib       integer := 0;
counter   integer := 0 ;
i         integer := 0 ;
j         integer := 1 ;

BEGIN
  IF (n < 1) THEN
    fib := 0 ;
  END IF;
  LOOP
    EXIT WHEN counter = n ;
    counter := counter + 1 ;
    SELECT j,i + j INTO   i,j ;
  END LOOP;

  fib := i;
  raise notice '%', fib;
END;

$$ language sql;
```
  
**Further Information:**

- [Supported PL/pgSQL statements](https://docs.aws.amazon.com/redshift/latest/dg/c_PLpgSQL-statements.html)