+++
title = 'PostgreSQL'
date = 2024-02-11

+++

### Postgresql

- The PostgreSQL server can handle multiple concurrent connections from clients. To achieve this it starts ("forks") a new process for each connection.
- `sudo -u postgres -i` then `psql`
- `sudo -u postgres createuser user_name` : creates a role with name user_name.
- `createdb mydb`
- `dropdb mydb`
- `psql` internal commands start with `\`
- To get a list of internal commands type `\?`
- Tables are grouped into databases, and a collection of database constitutes a database _cluster_
- PostgreSQL supports the standard SQL types `int`, `smallint`, `real`, `double precision`, `char(N)`, `varchar(N)`, `date`, `time`, `timestamp`, and `interval`, as well as other types of general utility and a rich set of geometric types.
- The `point` type is an example of a PostgreSQL-specific data type.
- `DROP TABLE tablename` to remove the table
- You could also have used `COPY` to load large amounts of data from flat-text files.
- `COPY weather FROM '/home/user/weather.txt';`
- A query that accesses multiple rows of the same or different tables at one time is called a _join_ query
- `select * from weather left outer join cities on (weather.city = cities.name)` This query is called a _left outer join_ because the table mentioned on the left of the join operator will have each of its rows in the output at least once, whereas the table on the right will only have those rows output that match some row of the left table. When outputting a left-table row for which there is no right-table match, empty (null) values are substituted for the right-table columns.
- there are aggregates to compute the `count`, `sum`, `avg`, `max` and `min` over a set of rows.
- `LIKE` does pattern matching
- `WHERE` selects input rows before groups and aggregates are computed (thus, it controls which rows go into the aggregate computation), whereas `HAVING` selects group rows after groups and aggregates are computed
- You can update existing rows using the `UPDATE` command
- Without a qualification `DELETE` will remove all rows from the given table, leaving it empty. So be wary of the form `DELETE FROM tablename`
- You can create a `view` over the query, which gives a name to the query that you can refer to like an ordinary table
- Views can be used in almost any place a real table can be used. Building views upon other views is not uncommon
- The essential point of a transaction is that it bundles multiple steps into a single, all-or-nothing operation.
- Another important property of transactional databases is closely related to the notion of atomic updates: when multiple transactions are running concurrently, each one should not be able to see the incomplete changes made by others
- In PostgreSQL, a transaction is set up by surrounding the SQL commands of the transaction with `BEGIN` and `COMMIT` commands.
- Savepoints allow you to selectively discard parts of the transaction, while committing the rest.
- After defining a savepoint with `SAVEPOINT`, one can if needed roll back to the savepoint with `ROLLBACK TO`. All the transaction's database changes between defining the savepoint and rolling back to it are discarded, but changes earlier than the savepoint are kept.
- If you are sure you won't need to roll back to a particular savepoint again, it can be released, so the system can free some resources
- A _window function_ performs a calculation across a set of table rows that are somehow related to the current row.
- A window function call always contains an `OVER` clause directly following the window function's name and argument(s)
- To control the order in which rows are processed by window functions we can use `ORDER BY` within `OVER`.
- another important concept associated with window functions: for each row, there is a set of rows within its partition called its _window frame_
- Window functions are permitted only in `SELECT` list and the `ORDER BY` clause of the query.
- Window functions execute after regular aggregate functions.
- When a query involves multiple window functions, each windowing behaviour can be named in a `WINDOW` clause and then referenced in `OVER`
- `\l` list the databases
- `\c db_name` connect to the given db_name
- `\dt` list the tables in the database
- `text` type is for variable length character strings.
- In PostgreSQL, a table can inherit from zero or more other tables.
- `SELECT`, `UPDATE` and `DELETE` supports `ONLY` notation
- The `DISTINCT` clause can be used on one or more columns of a table
- If you specify multiple columns, the DISTINCT clause will evaluate the duplicate based on the combination of values of theses columns
- PostgreSQL also provides the `DISTINCT ON (expression)` to keep the "first" row of each group of duplicates
- The `DISTINCT ON` expression must match the leftmost expression in the ORDER BY clause
- In case you want to skip a number of rows before returning the `n` rows, you use `OFFSET` clause placed after the `LIMIT` clause
- We often use the `LIMIT` clause to get the number of highest or lowest items in a table.
- PostgreSQL evaluates `ORDER BY` clause after the SELECT clause.
- Other caluses evaluated before the SELECT clause such as WHERE, GROUP BY, and HAVING
- A natural join is a join that creates an implicit join based on the same column names in the joined tables.
- The `GROUP BY` clause must appear right after the FROM or WHERE clause.
- Apply aggregate function for each group
- To filter groups use the `HAVING` clause instead of `WHERE` clause.
- The `HAVING` clause sets the condition for group rows created by the `GROUP BY` clause after the `GROUP BY` clause applies while the WHERE clause sets the condition for individual rows before `GROUP BY` clause applies. This is the main difference between the `HAVING` and `WHERE` clauses.
- The `UNION` operator combines result sets of two or more SELECT statements into a single result set.
- Syntax: `select col_1, col_2 from tbl_1 UNION select col_1, col_2 from tbl_2;`
- Rules applied to the queries:
  - Both queries must return the same number of columns.
  - The corresponding columns in the queries must have compatible data types.
- The `UNION` operator removes all duplicate rows unless the `UNION ALL` is used.
- The `EXCEPT` operator returns distinct rows from the first (left) query that are not in the output of the second (right) query.
- Syntax: `select column_list from A where condition_a EXCEPT select column_list from B where condition_b;`
- A grouping set is a set of columns by which you group.
- The `GROUPING SETS` allows to define multiple grouping sets in the same query
- Syntax: `select c1, c2, aggregate_function(C3) from table_name group by grouping sets ( (c1, c2), (c1), (c2), ());`
- The `GROUPING` function accepts a name of a column and returns bit 0 if the column is the member of the current grouping set and 1 otherwise.
- The `CUBE` allows to generate multiple grouping sets.
- Syntax: `select c1, c2, c3, aggregate(c4) from table_name group by cube(c1, c2, c3);`
- The `CUBE` subclause is a short way to define multiple grouping sets. The query generates all possible grouping sets based on the dimension columns specified in the CUBE.
- The `ROLLUP` assumes a hierarchy among the input columns and generates all grouping sets that make sense considering the hierarchy.
- ROLLUP(c1, c2, c3) generates only four grouping sets, assuming the hierarchy c1 > c2 > c3 as follows:
- (C1, c2, c3), (C1, c2), (C1), ()

### Using postgres and pgadmin in docker

- Start the containers for pg and pgadmin4 with the following command
- `sudo docker run -p 5432:5432 --name pg -e POSTGRES_PASSWORD=${password} postgres`
- `sudo docker run -p 5555:80 --name pgadmin -e PGADMIN_DEFAULT_EMAIL="abc" -e PGADMIN_DEFAULT_PASSWORD="password" dpage/pgadmin4`
- visit the link localhost:555 to start pgadmin4 web page
- both containers are part of a custom [bridge network](https://docs.docker.com/network/bridge/) for automatic DNS resolution
- Execute `docker network inspect bridge/name_default` and find the IPV4 address.
- Use this ipv4 address in the server name field in pgadmin4
- the default username is postgres

### PostgreSQL for Everybody

- `\dt` : list all the tables in the database
- `\d+ $table_name` : describes table with a given name
- `\i lesson1.sql` : run commands from `lesson1.sql` (file)
- `select * from users order by email offset 1 limit 2` : skip ahead by 1 and give 2.
- `CHAR(n)` : allocates the entire space (faster for small strings where length is known) like GUID, or Hash (64 characters)
- `VARCHAR(n)` : allocates a variable amount of space depending on the data length (less space)
- `TEXT` varying length. Generally not used with indexing and sorting. Blog, post or comments or whole web pages
- `BYTEA(n)` up to 255 bytes. Not indexed or sorted
- `SMALLINT` (-32768, 32768)
- `INTEGER` (2 Billion)
- `BIGINT` (10\*\*18 ish)
- `REAL` (32 bit) 10\*\*38 with seven digits of accuracy
- `DOUBLE PRECISION` (64 bit) 10\*\*308 with 14 digits of accuracy
- `NUMERIC(accuracy, decimal)` specified digits of accuracy and digits after the decimal point
- `TIMESTAMP` (4713 BC , 294276 AD)
- `DATE` 'YYYY-MM-DD'
- `TIME` 'HH:MM:SS'
- `TIMESTAMPTZ` TIMESTAMP with TIME ZONE
- `NOW()` built in postgres function
- `\copy track_raw(title, artist, album, count, rating, len) FROM 'library.csv' with DELIMITER ',' CSV;`
- Never use your _logical key_ as the **primary key**
- Relationships that are based on matching string fields are less efficient than integers.
- On Delete choices
  - `Default/Restrict` - Don't allow changes that break the constraint
  - `CASCADE` : adjust child rows by removing or updating to maintain consistency
  - `SET NULL` : set the foreign key columns in the child rows to null `INTEGER NULL` allowing nulls
- We use `repeat()` to generate long strings (horizontal)
- We use `generate_series()` to generate lots of rows (vertical)
- We use `random()` to make rows unique
- PostgreSQL Index Types:
  - B-Tree : Maintains order - usually preferred
    - Helps on exact lookup, prefix lookup, <, >, range, sort
  - HASH
    - Smaller - helps only on exact lookup
- `WHERE` Clause operators for REGEX
  - `~` Matches
  - `~*` Matches case insensitive
  - `!~` Does not match
  - `!~*` Does not match case insensitive
  - Different than `LIKE` - Match anywhere
    - `tweet ~ 'UMSI`'
    - `tweet LIKE '%UMSI%'`
