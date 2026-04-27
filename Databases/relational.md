# Relational Databases

Relational databases are databases that organize data based on the [relational model](#relational-model) of
data. Some popular examples of relational databases include MySQL, PostgreSQL, SQLite, Oracle, and Microsoft SQL Server.

## Relational Model

The relational model organizes data into tables (called relations), where each table consists of rows (tuples)
and columns (attributes). Relationships between tables are established through shared keys, such as primary keys
and foreign keys, allowing data to be linked across multiple tables without duplication. This model provides a
strong mathematical foundation based on set theory and predicate logic, enabling powerful querying through
languages like SQL.

## Normalization

Normalization is an important process in database design that helps improve the database's efficiency, consistency, and
accuracy. It makes it easier to manage and maintain data and ensures that the database is adaptable to changing business needs.

- Normalization is the process of organizing database attributes to reduce/eliminate redundancy(having same data in different places), which increases the size of the database.
- Data redundancy leads to inconsistency problems during *insert, delete, or update* operations, making managing data difficult.
- Involves splitting data into multiple tables, which must be linked each time a query requires data from the split tables

### Problems in the Employee_Department Relation
This section highlights common anomalies and redundancy issues that occur due to improper database design.

1. Insertion Anomaly: If a new department is created but no employee is assigned to it yet, we cannot store its location because we need an employee record to insert.
2. Update Anomaly: If the location of the HR department changes, we must update it in multiple rows (for both Nick Wise and Lily Case). If one row is missed, the data becomes inconsistent.
3. Deletion Anomaly: If all employees in the IT department leave, we lose the department information, including its location.
4. Data Redundancy: The department location is repeated for every employee in the same department.

![Before Normalization](https://media.geeksforgeeks.org/wp-content/uploads/20260121113045221000/before_normalization-.webp)

![After Normalization](https://media.geeksforgeeks.org/wp-content/uploads/20260211112535440628/after_normalization.webp)

### Need of Normalization
The primary objective for normalizing the relations is to eliminate the below anomalies. Failure to reduce anomalies results in data redundancy, which may threaten data integrity and cause additional issues as the database increases. A seperate article on anomalies can be found at [Anomalies in Relational Model](https://www.geeksforgeeks.org/dbms/anomalies-in-relational-model/)

- Insertion Anomalies: Insertion anomalies occur when it is not possible to insert data into a database because the required fields are missing or because the data is incomplete. For example, if a database requires that every record has a primary key, but no value is provided for a particular record, it cannot be inserted into the database.
- Deletion anomalies: Deletion anomalies occur when deleting a record from a database and can result in the unintentional loss of data. For example, if a database contains information about customers and orders, deleting a customer record may also delete all the orders associated with that customer.
- Updation anomalies: Updation anomalies occur when modifying data in a database and can result in inconsistencies or errors. For example, if a database contains information about employees and their salaries, updating an employee’s salary in one record but not in all related records could lead to incorrect calculations and reporting.

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

- Introduction to Database Normalization. (2015, July 2). *GeeksforGeeks*. Retreived April 27, 2026, from
  [Introduction to Database Normalization](https://www.geeksforgeeks.org/dbms/introduction-of-database-normalization/)
