+++
title = 'JDBC'
date = 2024-02-11

+++

### What are the differences between ResultSet and RowSet?

| ResultSet                                                       | RowSet                                                             |
| --------------------------------------------------------------- | ------------------------------------------------------------------ |
| A `ResultSet` always maintains connection with the database.    | A `RowSet` can be connected, disconnected from the database.       |
| It cannot be serialized.                                        | A `RowSet` object can be serialized.                               |
| `ResultSet` object cannot be passed other over network.         | You can pass a `RowSet` object over the network.                   |
| By default, `ResultSet` object is not scrollable or, updatable. | By default, `RowSet` object is scrollable and updatable.           |
| ResultSet object is not a JavaBean object You can create/get a  | `RowSet` Object is a JavaBean object. You can get a `RowSet` using |
| `ResultSet` using the executeQuery() method.                    | the `RowSetProvider.newFactory().createJdbcRowSet()` method.       |

- `Statement` interface is used for DDL statements like `CREATE`, `ALTER`, `DROP` etc.

### What are the different types of locking in JDBC?

1. **Row and Key Locks**: Useful when updating the rows (update, insert or delete operations), as they increase concurrency.
2. **Page Locks**: Locks the page when the transaction updates, inserts or deletes rows or keys. The database server locks the entire page that contains the row. The lock is made only once by database server, even more rows are updated. This lock is suggested in the situation where large number of rows is to be changed at once.
3. **Table Locks**: Utilizing table locks is efficient when a query accesses most of the tables of a table. These are of two types:
   - **Shared lock**: One shared lock is placed by the database server, which prevents other to perform any update operations.
   - **Exclusive lock**: One exclusive lock is placed by the database server, irrespective of the number of the rows that are updated.
4. **Database Lock**: In order to prevent the read or update access from other transactions when the database is open, the database lock is used.

### What are the differences between Stored Procedure and functions?

| Functions                                                              | Procedures                                                                   |
| ---------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| has a return type and returns a value.                                 | does not have a return type. But it returns values using the OUT parameters. |
| don't allow output parameters                                          | allow both input and output parameters.                                      |
| cannot manage transactions inside a function.                          | can manage transactions inside a function.                                   |
| cannot call stored procedures from a function                          | can call a function from a stored procedure.                                 |
| can call a function using a select statement.                          | cannot call a procedure using select statements.                             |
| can't use a function with DML queries. Only Select queries are allowed | can use DML queries such as insert, update, select etc… with procedures.     |


- **Optimistic Locking** is a strategy where you read a record, take note of a version number (other methods to do this involve dates, timestamps or checksums/hashes) and check that the version hasn't changed before you write the record back. When you write the record back, you filter the update on the version to make sure it's atomic. (i.e. hasn't been updated between when you check the version and write the record to the disk) and update the version in one hit. If the record is dirty (i.e. different version to yours) you abort the transaction and the user can re-start it.
- This strategy is mostly applicable to high-volume systems and three-tier architectures where you do not necessarily maintain a connection to the database for your session. In this situation the client cannot actually maintain database locks as the connections are taken from a pool and you may not be using the same connection from one access to the next.
- **Pessimistic Locking** is when you lock the record for your exclusive use until you have finished with it. It has much better integrity than optimistic locking but requires you to be careful with your application design to avoid Deadlocks. To use pessimistic locking you need either a direct connection to the database (as would typically be the case in a two tier client server application) or an externally available transaction ID that can be used independently of the connection.
- In the latter case you open the transaction with the TxID and then reconnect using that ID. The DBMS maintains the locks and allows you to pick the session back up through the TxID. This is how distributed transactions using two-phase commit protocols (such as XA or COM+ Transactions) work.

