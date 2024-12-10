---
tags: aws
---
# Redshift: User-Defined Functions

Today I learned, I can create functions in SQL and use them over and over for repetitive tasks while building or developing pipelines. I've been working with AWS Redshift for the past few months. Redshift is a very powerful and cost-effective cloud data warehousing solution provided by Amazon. It has its own Redshift SQL dialect which is a variant of PostgreSQL.

 1. TOC

{:toc}


### What are User-defined functions?


Redshift offers various inbuilt functions like `sysdate` or `coalesce` which can be executed whenever required. As the name suggestes, User-defined functions are the functions defined by a user to extend the inbuilt capabilities of RedshiftSQL. The defined functions are stored inside the database and can be used by any user with sufficient permissions and credentials.

### Language support for UDFs

UDFs in general can be created using SQL `select` statements. Redshift UDFs can also be created using **Python** functions. While UDFs written in SQL are better at performance and less prone to errors, Python UDF support comes with advantage of inbuilt libraries. In addition to Python STL, modules like pandas, numpy, scipy and dateutil are part of RedshiftSQL.

Redshift also supports AWS Lambda UDFs which are custom functions defined in Lambda. Sometimes, there might be some limitations for SQL and python UDFs. This can be handled using Lambda UDFs since these can be created in any language that is supported by AWS Lambda. This includes Go, Java, C#, Node.js etc. So, most of the languages can be integrated into SQL statemnts in Redshift and can be utilized over and over by all the users.

### SQL UDF

SQL UDF integrates SQL `select` clause that runs whenever it is called and returns  a scalar. A function can be created using `create function` statement. It has the following parameters - input arguments along with data types, one return argument, a SQL `select` clause. A weird point to note here is - the arguments cannot be named inside the UDF `select` clause. They must be refered as $1, $2 and so on based on the order of the arguments mentioned in function creation. The `select` clause should not include any other clauses like `from`, `where`, `limit` etc. instead these should be used in the SQL statament that calls the UDF.
#### Structure of SQL UDF

```sql
create function f_sample_udf(<arg1_datatype>, <arg2_datatype>,...)
    returns <return_datatype>
stable
as $$

    select ...

$$ language sql;
```

#### Example - SQL UDF

**Example 1:**

```sql
create function f_clean_string_number(varchar)
    returns float
stable
as $$
    select cast(regex_replace($1, ',', '') as float)
$$ language sql;
```

**Example 2:**

```sql
create function f_percentage(float, float)
    returns float
stable
as $$
    select case when $2 > $1 then ($1/$2)*100
        else ($2/$1)*100
    end
$$ language sql;
```

Now the above UDF can be used as following:

```sql
select f_percentage(acheived, monthly_target) from finance;
```


### Python UDF
Python UDFs are also similar to SQL UDFs but python code can be used inside a function directly. The language definition for a UDF for using python inside it is **PLPYTHONU**. Python UDF requires argument names inside the `select` clause. Modules like pandas and numpy can also be imported apart from the python standard library. The input and return data types suppoorted by Python UDFs are SMALLINT, INTEGER, BIGINT, DECIMAL, REAL, DOUBLE PRECISION, BOOLEAN, CHAR, VARCHAR, DATE, and TIMESTAMP.  For a mapping of Amazon Redshift data types to Python data types, refer to [Python UDF datatypes](https://docs.aws.amazon.com/redshift/latest/dg/udf-data-types.html).


#### Structure of Python UDF

```sql
create function f_sample_udf(arg1 <arg1_datatype>, arg2 <arg2_datatype>, ...))
returns <return_datatype>
stable
as $$

    <PYTHON CODE>

$$ language plpythonu;
```

#### Example - Python UDF

**Example 1:**

```sql
create function f_clean_string_number(txt varchar)
    returns varchar
stable
as $$
    return float(txt.replace(',',''))
$$ language plpythonu;
```

**Example 2:**
  
```sql
create function f_percentage(x float, y float)
    returns float
stable
as $$
    if y > x:
        return (x/y)*100
    else:
        return (y/x)*100
$$ language plpythonu;
```

Now the above UDF can be used as following:

```sql
select f_percentage(acheived, monthly_target) from finance;
```

### UDF Security and Permissions
- To create a UDF, the user must have permission for sql or plpythonu language usage. By default, USAGE ON LANGUAGE SQL is granted to PUBLIC. However, you must explicitly grant USAGE ON LANGUAGE PLPYTHONU to specific users or groups.
- The permissions can be revoked using general SQL `grant` and `revoke` clauses.

```sql
grant usage on language plpythonu to group <group_name>;

revoke usage on language sql from group <group_name>;
```

- The user also needs to have **execute** permission to run function created by others. By default, the access is granted to **PUBLIC**. To restrict usage, the access should be revoked and then the previlige should be granted to the particular users or user group.

```sql
revoke execute on function f_sample_function(a float, b float) from PUBLIC;

grant execute on function f_sample_function(a float, b float) to group udf_devs;
```
  
### Note
- AWS recommends to name all UDFs starting with "**f_**". This is not mandatory but it is a good practice. Redshift reserves the f_ prefix exclusively for UDFs and by prefixing the UDF names with f_, the user can ensure that his UDF name will not with any other existing or future functions.
- UDFs can also be created in a particular schema. The schema name can be added while defining UDF from `create function` clause.
- UDFs with same name can exist in different schemas.
- UDFs with same name can also exist in same schema if they are provided with different number of arguments and different datatypes.
- For example, two functions `f_average(float, float, float)` and `f_average(float, float)` and `f_average(decimal, decimal)` can co-exist in same schema. Redshift query engine determines which function to call based on the number of arguments provided and the data types of the arguments.
- UDF can be dropped using `drop function f_sample_udf(<arguments>)`

### Further Information

- [AWS documentation](https://docs.aws.amazon.com/redshift/latest/dg/user-defined-functions.html)