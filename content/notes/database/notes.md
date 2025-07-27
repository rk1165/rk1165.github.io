+++
title = 'Database Notes'
date = 2024-02-11

+++

### General

- `SERIAL` will automatically increase itself.
- Constraints : `NOT NULL`, `UNIQUE`, `PRIMARY KEY`, `DEFAULT`, `CHECK`
- `update tablename set some_col to sth where colname = sth and colname2 = sth2;`
- `delete from tableName where colname = sth;`
- `select colname, count(*) from tableName GROUP BY someColName;` will group the rows by given colName and show their counts
- `select colname, count(*) from tableName GROUP BY someColName having someproprty;` will group the rows by given colName and show only the rows which satisfy given property
- `select origin, destination, name FROM flights JOIN passengers ON passengers.flight_id = flights.id;` last part shows how the two tables are related. This says that if you take the flight_id col from passengers this matches with the id col. of flights. Only get the things that match.
- `select origin, destination, name FROM flights LEFT JOIN passengers ON passengers.flight_id = flights.id;` take the table on the left (flights) and make sure that all the rows in the flights table is included in the final result even if they don't have a match
- SQL Transaction: `BEGIN COMMIT`

### Parent child relationship

- Primary Key is the parent, the foreign key is the child
- FK points back to the parent
- commenter is the parent and the comment is the child.
- if given a comment we can figure out who posted the comment.
- The children inherit the traits from the parent. Such as the foreign key
- When designing relationship we need to understand which table does the foreign key goes to. That is decided by the child.
- foreign key goes on the child primary key goes on the Parent

#### Lookup table

- What does a key do:
  - protects our integrity
  - protects our uniqueness
  - Improves functionality of database
  - less work needs to be done
  - allows for added complexity
- Lookup table helps add complexity. eg Membership table example. Gold, Silver, etc can have other column mapping their price to it.

### Superkey and Candidate Key

- Superkey is any number of columns that forces every single row to be unique
- Consider adding some kind of column to make each row unique
- Superkeys are not defined in the database
- A Candidate key is the least number of columns that forces every single row to be unique.
- Since there can be many candidate keys, we choose one of the candidate key to be as our primary key
- the candidate keys we didn't select are called alternate keys
- We can create an index on the alternate key (if good alternate key) it will help run the database faster

### Category of Primary keys:

- Surrogate key: made up keys. Doesn't have real world meaning
- Natural key: it's natural such as user_id for a user. Has real world significance. That is already in the database
- Surrogate keys are column added to the database

### Pros and Cons of Surrogate and Natural Keys

| Surrogate                               | Natural                            |
| --------------------------------------- | ---------------------------------- |
| P: Typically numbers                    | P: Real world value                |
| C: We have to add a column to our table | C: Not always easy to come up with |

### Foreign Keys

- Foreign key is a reference, it references a primary key
- PK could be in the same table or other table
- NOT NULL : have to give the column a value for every row
- We could have foreign key references change.
- FK Constraints
  - ON DELETE and ON UPDATE refers to the parents. When we delete or update the parent we want the change to take effects on the child
- RESTRICT, CASCADE, SET NULL refers to the child i.e what happens to the child when we update or delete the parent.
- RESTRICT ON DELETE stops the parent from being delete
- RESTRICT ON UPDATE changes the updated value back to the original
- CASCADE : whatever happens to the parent we want it to happen to the child too.
- SET NULL: Basically set the children as NULL for that specific value.
- With SET NULL the children can't have NOT NULL characteristics.

### Simple, Composite and Compound Primary Key

- Simple key consists of one column.
- Composite key are multiple columns e.g (fn + ln + email)
- Compound is a key that has multiple columns and they are all keys themselves. Most common example is of intermediary table.
- EER, ERD, ER methods to draw the database tables

### Cardinality

- The relationship type between row of one table and rows of another table.
- Cross Bow notation
- |--------| : one to One
- |-------<- : One to many
- ->-------| : many to one
- ->------<- : many to many

### Modality

- Whether or not the relationship is required?
- Does the FK column has the NOT NULL characteristics?
- So we denote that by `0` or `|`. A `0` means it accepts null values.
- A `|` means NOT NULL

### Normalization

- Normal forms are a checklist that you have to follow.
- Three normal forms: 1NF, 2NF, 3NF
- 1NF : Deals with data being atomic. Multiple values shouldn't be in a column
- There must be a 1NF in order to put things in order in 2NF.
- 2NF deals with what's known as a **partial dependency**. That's when a column only depends on part of the primary key. So the primary key has to be multiple columns
- For 2NF we remove all the partial dependency
- 3NF has to do with something called **transitive dependency** which is when a column depends upon a column which depends upon a primary key.
- Move transitive dependency to a new table and create a FK that references that table.

#### Indexes

- Non-clustered indexes points to the data. These are like the indexes at the end of the book.
- Clustered indexes organizes the data. We can only have one of these.
- We index a column to make query faster.
- Indexes speed the joins.
- Composite index : indexes on two columns.
- **Data Types** Strings, Date, Numeric, Text
- Date, Time, DateTime, TimeStamp,

### SQLITE

- `sqlite3 db_name_to_connect_to`
- `.databases` : to see which database session is connected
- `.tables` : list tables in database
- `.headers on` : turn on column for printing

### 2PC Commit

- In Relational DB transactions become difficult under heavy load. When we try to horizontally scale a RDB, making it distributed, we must now account for _distributed transactions_.
- In order to account for successful completion across multiple hosts, the idea of a two-phase commit (2PC) is introduced.
- The 2PC is a commonly used algorithm for achieving consensus in distributed systems, involving two sets of interactions between hosts known as the prepare phase and commit phase.
- 2PC _blocks_, i.e clients must wait for a prior transaction to finish before they can access the blocked resource.
- Other issue is schema. Not all schemas map well to the relational model. One type of system is _complex event processing system_ or _stream processing system_

### Random

- A **wire protocol** is the format for interactions between a database server and its clients. It encompasses authentication, sending queries, receiving responses, and so on. It is a description of the exact bytes sent and received by servers and clients. It does NOT encompass the actual query language itself, let alone database semantics.

### DataStores and when to use them

- It’s possible that your data doesn’t conform nicely to a relational schema or your schema is changing so frequently that the rigid structure of a relational database is slowing down your development. In this case, you can consider using a non-relational database instead.

#### NoSQL

- NoSQL databases can be categorized into a few types, but there are two primary types which come to mind when we think of NoSQL databases: **document stores** and **wide column stores**.
- A document store is basically a fancy key-value store where the key is often omitted and never used (although one does get assigned under the hood — we just don’t typically care about it). The values are blobs of semi-structured data, such as JSON or XML, and we treat the data store like it’s just a big array of these blobs. The query language of the document store will then allow you to filter or sort based on the content inside of those document blobs.
- A popular document store is **MongoDB**
- A wide column store is somewhere in between a document store and a relational DB. It still uses tables, rows, and columns like a relational DB, but the names and formats of the columns can be different for various rows in the same table.
- Popular wide column stores are **Cassandra** and **Bigtable**
- Non-relational databases are most suited to handling large volumes of data and/or unstructured data. Extremely popular in the world of big data because writes are fast

#### KEY VALUE store

- A key-value store is basically a production-scale hashmap: a map from keys to values. There are no fancy schemas or relationships between data. No tables or other logical groups of data of the same type. Just keys and values, that’s it.
- At Shopify, we use two key-value stores: **Redis** and **Memcached**.
- Both Redis and Memcached are in-memory key-value stores, so their performance is top-notch.
- One major difference between Redis and Memcached is that Redis supports some data structures as values. You can declare that a value in Redis is a list, set, queue, hash map, or even a HyperLogLog, and then perform operations on those structures. With Memcached, everything is just a blob and if you want to perform any operations on those blobs, you have to do it yourself and then write it back to the key again.
- Redis can also be configured to persist to disk, which Memcached cannot.
- Key-value stores are good for simple applications that need to store simple objects temporarily. An obvious example is a cache. A less obvious example is to use Redis lists to queue units of work with simple input parameters.

#### FULL TEXT SEARCH ENGINE

- Search engines are a special type of data store designed for a very specific use case: searching text-based documents.
- At Shopify we use **Elasticsearch** for our full-text search. Elasticsearch is replicated and distributed out of the box, which makes it easy to scale.
- The most important feature of any search engine, though, is that it performs exceptionally well for text searches.
- The real value add of a full-text search engine is when you need to look for particular words or substrings within longer text fields.

#### MESSAGE QUEUE

- At Shopify, we use **Kafka** for all our streaming needs. Payloads called “messages” are inserted into Kafka “topics” by “producers.” On the other end, Kafka “consumers” can read messages from a topic in the same order they were inserted in.
- Under the hood, Kafka is implemented as a distributed, append-only log. It’s just files! Although not human-readable files.
- Kafka is often used to ship data between services. The producer-consumer model has a big advantage over other solutions: because Kafka itself acts as the message broker, you can simply ship your data into Kafka and then the receiving service can poll for updates

### Log Structured storage

- There are 2 basic ways to do database storage
  - log-structured storage engines (like "log-structured merge trees") : You write new stuff to the end of the log, and then query for the latest update to read. The idea here is that this way you don't need to make random disk writes. LevelDB, RocksDB, and Cassandra all use log-structured storage
  - page-oriented storate engines (like b-trees) : when we need this, we need to search for the right page (4k of data or so) that contains your data and update it. PostgreSQL, MySQL, etcd, and sqlite all use page-oriented storage engines
- Writing to disk is expensive. Because of that, databases will often just do writes to memory, and then save their state to disk later. Write-ahead log (WAL): To avoid losing data, you can just log all your writes to disk by appending to a file.


- Isolation is a property of a database where simultaneous operations on the same data never see each other’s in-progress work.