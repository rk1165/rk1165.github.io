+++
title = 'General SQL'
date = 2024-02-11

+++

### Chap1

- The process of refining a database design to ensure that each independent piece of information is in only one place (except for foreign keys) is known as _normalization_.
- `mysql -u user -p db_name` for connecting to a db.
- `mysql -h host -u user -p db_name`
- `SELECT DATABASE()` to see which db is currently in use
- `LOAD DATA LOCAL INFILE '/path/pet.txt' INTO TABLE pet;`
- You can force a case-sensitive sort for a column by using `BINARY` like so: `ORDER BY BINARY col_name`.
- To determine how many years old each of your pets is, use the `TIMESTAMPDIFF()` function. Its arguments are the unit in which you want the result expressed, and the two dates for which to take the difference. `TIMESTAMPDIFF(YEAR, birth, CURDATE())`
- MySQL provides several functions for extracting parts of dates, such as `YEAR()`, `MONTH()`, and `DAYOFMONTH()`
- `DATE_ADD()` enables you to add a time interval to a given date : `DATE_ADD(CURDATE(),INTERVAL 1 MONTH)`
- Two `NULL` values are regarded as equal in a `GROUP BY`.
- When doing an `ORDER BY`, `NULL` values are presented first if you do `ORDER BY ... ASC` and last if you do `ORDER BY ... DESC`.
- It is entirely possible to insert a zero or empty string into a `NOT NULL` column, as these are in fact `NOT NULL`
- `SELECT * FROM pet WHERE REGEXP_LIKE(name, '^b');`
- To force a regular expression comparison to be case-sensitive, use a case-sensitive collation, or use the `BINARY` keyword to make one of the strings a binary string, or specify the c match-control character.
- `SELECT * FROM pet WHERE REGEXP_LIKE(name, '^.{5}$');`
- `SHOW CREATE TABLE table_name` : displays the statement used to create the table
- `SHOW INDEX FROM tbl_name` : produces information about indexes of a table
- `SELECT @min_price:=MIN(price),@max_price:=MAX(price) FROM shop;`
- When searching on two different keys combined with `OR` use `UNION` instead. as searching for a single key can be optimized.
- You can retrieve the most recent automatically generated AUTO_INCREMENT value with the `LAST_INSERT_ID()`
- For MyISAM tables, you can specify AUTO_INCREMENT on a secondary column in a multiple-column index. In this case, the generated value for the AUTO_INCREMENT column is calculated as MAX(auto_increment_column) + 1 WHERE prefix=given-prefix. This is useful when you want to put data into ordered groups.

### Chap 2

- `show character set;`
- maxlen for `char` 255 bytes whereas for `varchar` it is 64KB.
- To store data that might exceed 64KB we need to use one of the text types:

| Text type    | Maximum number of bytes |
| :----------- | :---------------------- |
| `Tinytext`   | 255                     |
| `Text`       | 65535                   |
| `Mediumtext` | 16,777,215              |
| `Longtext`   | 4,294,967,295           |

- Integer types : `Tinyint`, `Smallint`, `Mediumint`, `Int`, `BigInt`
- Floating-point types : `Float(p, s)`, `Double(p, s)` p is for _precision_ total digits allowed and s is for _scale_ digits allowed to the right of the decimal poin.
- Temporal types

| Type        | Default format        | Allowable values                           |
| :---------- | :-------------------- | :----------------------------------------- |
| `Date`      | `YYYY-MM-DD`          | 1000-01-01 to 9999-1                       |
| `Datetime`  | `YYYY-MM-DD HH:MM:SS` | 1000-01-01 00:00:00 to 9999-12-31 23:59:59 |
| `Timestamp` | `YYYY-MM-DD HH:MM:SS` | 1970-01-01 00:00:00 to 2037-21-31 23:59:59 |
| `Year`      | `YYYY`                | 1901 to 2155                               |
| `Time`      | `HHH:MM:SS`           | -838:59:59 to 838:59:59                    |

- use the `describe` command as `DESC table_name` to look at table info
- to specify if a column cannot be null we use the keyword `not null` after the type definition.
- in MySQL turn on the _auto-increment_ feature to generate primary key automatically. This is done generally during table creation.
- `set foreign_key_checks=0;` to temporarily disable foreign key
- `mysql -u user_name -p --xml db_name` formats the output automatically to XML
- `show tables`
- `show databases`

### Chap 3

- `having` clause filters group data in the same way the `where` clause filters raw data.
- **MySQL** includes a `limit` clause that allows to sort data and then discard all but the first X rows
- `RIGHT(fed_id, 3)` extract the last three characters of the `fed_id` column

### Chap 4

- use `BETWEEN` operator when filtering for upper and lower limit for range.
- upper and lower limits are inclusive.
- `LEFT(lname, 1)` strip off the first letter of the `lname` column.
- `where lname REGEXP ^[FG]`
- Two nulls are never equal to each other
- To test whether an expression is `null` you need to use the `is null` operator

### Chap 5

- If the names of the columns used to join the two tables are identical, use the `USING` subclause instead of the `ON` subclause
- _equi-joins_ values from the two tables must match for the join to succeed.
- tables can also be joined via range of values, which are referred to as _non-equi joins_.
- join conditions belong in the `on` subclause while filter conditions belong in the `where` clause.

### Chap 6

- To perform a set operation we place a _set operator_ between two `SELECT` statements.
- `UNION` and `UNION ALL` opreators combine multiple data sets. The difference b/w the two is that `UNION` sorts the combnied set and removes duplicates.
- `INTERSECT` operator is for performing intersections. (Not in MySQL)
- `EXCEPT` opreator is for perfoming the except operator. (Not in MySQL) returns the first table minus the any overlap with the second table.

### Chap 7

#### Working with Strings

| Database   | Type     | Range |
| :--------- | :------- | :---- |
| MySQL      | CHAR     | 255   |
| Oracle     | CHAR     | 2000  |
| SQL Server | CHAR     | 8000  |
| MySQL      | VARCHAR  | 65535 |
| Oracle     | VARCHAR2 | 4000  |
| SQL Server | VARCHAR  | 8000  |
| MySQL      | text     | 4GB   |
| Oracle     | CLOB     | 128TB |
| SQL Server | TEXT     | 2GB   |

- MySQL throws error if the given string data size either exceeds the maximum designated length set for column or maximum allowed for the data type.
- `SELECT @@session.sql_mode` to check which mode MySQL is in.
- `SET sql_mode=ansi`
- in ANSI mode, a warning is thrown and the given string data is truncated.
- `SHOW WARNINGS` for showing the warnings.
- escape a single quote by adding another single quote directly before it.
- `QUOTE` places quotes around the entire string _and_ adds escapes to any single quotes/apostrophes within the string. Useful when retrieving data for data export.
- `CHAR` to build strings from any of the 255 characters in ASCII. Oracle uses `CHR`
- `CONCAT` to contacatenate individual strings. (`||` operator in Oracle), (`+` operator in SQL Server)
- `LENGTH` function returns the number of characters in the string. (SQL Server - `LEN`)
- `POSITION` function for finding the location of a substring within a string.
- `LOCATE` same like position but with given search start parameter.
- SQL Server `CHARINDX` , Oracle - `INSTR` : with 2 or 3 parameter
- `STRCMP` -1 if first string comes before, 1 if after, 0 if identical. It is case insensitive
- `LIKE` and `REGEXP` returns 0 or 1 depending on the match
- `INSERT` takes four arguments: the original string, the position at which to start, the number of characters to replace, and the replacement string.
- `SUBSTRING` to _extract_ a specified number of characters starting at a specified position.

#### Working with Numeric data

- The `ceil()` and `floor()` functions are used to round either up or down to the closest integer
- `round(x)` or `round(x, n)`
- `truncate(x, n)`
- Both `truncate()` and `round()` also allow a negative value for the second argument, making numbers to the left of the decimal place truncated or rounded

#### Working with Temporal data

- `utc_timestamp()` returns the current UTC timestamp.
- `SELECT @@global.time_zone, @session.time_zone;`
- `SET time_zone = 'Europe/Zurich';`
- `str_to_date` takes a format string along with the date string.
- `date_add()` allows to add any kind of interval (e.g., days, months, years) to a specified date to generate another date.
- `last_day()` looks up the number of days in that month. returns `date`
- `convert_tz()`
- `dayname()` to determine which day of the week a certain date falls on.
- `datediff()` returns the number of full days between two dates.
- to use `cast()` provide a value or expression, the `as` keyword, and the type to which the value needs to be converted

### Chap 8

- `with cube` option generates summary rows for _all_ possible combinations of the grouping columns.
- When grouping data, apply filter conditions _after_ the groups have been generated. The `having` clause is used to place these types of filter conditions.

### Chap 9

- Some subqueries are completeley self-contained (_noncorrelated subqueries_), while others reference columns from the containing statement (_correlated subqueries_).
- the `all` operator makes comparisons between a single value and every value in a set. To build such a condition use one of the comparison operators in conjunction with the `all` operator.
- A condition using `any` operator evaluates to `true` as soon as a single comparison is favourable.
- a correlated subquery is executed once for each candidate row.
- Use the `exists` operator to identify that a relationship exists without regard for the quantity.
- Subqueries may be used to construct custom tables, to build conditions, and to generate column values in result sets.

### Chap 10

- natural join allows one to name the tables to be joined but lets the database server determine what the join conditions need to be. It relies on identical column names across multiple tables to infer the proper join conditions.

### Chap 11

- All the expressions returned by the various `when` clauses must evaluate to the same type.
- Case expressions may return any type of expression, including subqueries.
- Simple Case Expressions :

```sql
CASE V0
  WHEN V1 THEN E1
  WHEN V2 THEN E2
  ...
  WHEN VN THEN EN
  [ELSE ED]
END
```

- V0 represents a value, and the symbol V1, V2,...,VN represent values that are to be compared to V0.

### Random

- The wildcard `_` will match a single character so `first_name LIKE '_roy'` will only match ‘Troy’.
- `first_name LIKE '%roy'` will return true for rows with first names ‘roy’, ‘Troy’, and ‘Deroy’ but not ‘royman’.`
- Backticks (ie \`) can be used to denote column and table names. This is useful when the column or table name is the same as a SQL keyword and when they have a space in them
- `WHERE 0` guarantees that no rows will be returned
- `WHERE ex_age` only rows with non-zero ages will be returned.
- `COUNT(<column>)` returns the number of non-null rows in the column.
- To check if an entry is NULL, use `IS` and `IS NOT`
- apply a `CASE WHEN` block which acts as a big if-else statement
- `GROUP BY` block allows us to split up the dataset and apply aggregate functions within each group, resulting in one row per group
- Its most basic form is `GROUP BY <column>, <column>, ...` and comes after the `WHERE` block.
- if we want to filter on the result of the grouping and aggregation : to solve this problem, we use `HAVING`

### SQL queries order

- In a non-image format, the order is:
  - FROM/JOIN and all the ON conditions
  - WHERE
  - GROUP BY
  - HAVING
  - SELECT (including window functions)
  - ORDER BY
  - LIMIT

### Udemy course

- how to give checks so that databases are not dropped by anyone
- SHOW COLUMNS FROM tablename;
- `create table cats3 ( name varchar(100) DEFAULT 'unnamed', age INT DEFAULT 99);`

```sql
CREATE TABLE unique_cats2 (
    cat_id INT NOT NULL AUTO_INCREMENT,
    name VARCHAR(100),
    age INT,
    PRIMARY KEY (cat_id)
);
```

- concat_ws : concat with separator, `concat_ws('-', title, author_fname, author_lname)`

```sql
select substring('Hello World', 1, 4);
select substring('Hello World', 3);
select substring('Hello World', -3);
select replace('Hello World', 'Hell', '%$#@');
select reverse('Hello World');
SELECT CHAR_LENGTH('Hello World');
SELECT UPPER('Hello World');
SELECT LOWER('Hello World');
select distinct author_fname, author_lname from books;
select title, stock_quantity from books where stock_quantity like '____';
```

- `CHAR` is faster for fixed length text. State Abbreviations, Yes/No flags, M/F
- `decimal(5,2)` : 5 total number of digits, 2 digits after decimal
- `DATE` : Values with a date but no time, 'YYYY-MM-DD' format
- `TIME` : Values with a Time but no date, 'HH:MM:SS' format
- `DATETIME` : Values with a Date AND Time. 'YYYY-MM-DD HH:MM:SS' format
- `select curdate(), curtime(), now();`

```sql
CREATE TABLE comments2 (
    content VARCHAR(100),
    changed_at TIMESTAMP DEFAULT NOW() ON UPDATE CURRENT_TIMESTAMP/NOW()
);
```

```sql
-- case statements
SELECT title, stock_quantity,
    CASE
        WHEN stock_quantity BETWEEN 0 AND 50 THEN '*'
        WHEN stock_quantity BETWEEN 51 AND 100 THEN '**'
        ELSE '***'
    END AS STOCK
FROM books;
```

- `show triggers;`
- `drop trigger $trigger_name`
