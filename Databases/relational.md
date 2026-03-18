# Relational Databases

Relational databases are databases that organize data based on the [relational model](#relational-model) of
data. Some popular examples of relational databases include MySQL, PostgreSQL, SQLite, Oracle, and Microsoft SQL Server.

## Relational Model

The relational model organizes data into tables (called relations), where each table consists of rows (tuples)
and columns (attributes). Relationships between tables are established through shared keys, such as primary keys
and foreign keys, allowing data to be linked across multiple tables without duplication. This model provides a
strong mathematical foundation based on set theory and predicate logic, enabling powerful querying through
languages like SQL.

## ACID Properties in DBMS

ACID Properties explain how database transactions keep its integrity.

### Atomicity

Atomicity means a transsaction is all-or-nothing. It either all succeeds or nothng succeeds. If any part of the transaction fails, the entire transaction is rolled back to keep the database consistent.

### Consistency

Consistency means that the database must remain in a valid state before and after a transaction. All data will be valid according to all defined rules.

### Isolation

Guarentees that all transactions will occur in isolation. No transaction will be affected by any other transaction. So a transaction cannot read data from any other transaction that has not yet completed yet. Changes are also not visible until committed.

### Durability

Durability ensures that once a transaction is commited, its changes are permanently saved, even if the system fails. The data is stored in non-volatile memory, so the database can recover to its last committed state without losing data.
## References

- Wikipedia contributors. (2026). Database. In *Wikipedia, The Free Encyclopedia*. Retrieved from
  [Relational database on Wikipedia](https://en.wikipedia.org/wiki/Relational_database)
