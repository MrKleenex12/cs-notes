# Document Databases

A **document-oriented database** is one of the main categories of NoSQL databases. It stores data in documents, also
known as semi-structured data. Document-oriented databases are conceptually an extension of the key–value store, another
type of NoSQL database. Some popular examples of document databases include MongoDB, CouchDB, Amazon DynamoDB, and
Firebase Firestore.

Traditional **relational databases** (RDBs) store data in predefined tables, often requiring data to be split across multiple
tables. In contrast, document databases store all information for a given object in a single document, with each document
potentially having a unique structure. This design eliminates the need for object-relational mapping when loading data
into the database.

## Documents

Documents can be described as a complex data structure. A document can be in the form of text, arrays, strings, JSON,
XML, or any such format. You could think of them as similar to objects. They do not have to follow a fixed schema.
Documents within the same collection can contain different fields or structures. A document could look like:

```JSON
{
    "firstName": "Bob", 
    "lastName": "Smith",
    "address": {
        "type": "Home",
        "street1":"5 Oak St.",
        "city": "Boys",
        "state": "AR",
        "zip": "32225",
        "country": "US"
    },
    "hobby": "sailing",
    "phone": {
        "type": "Cell",
        "number": "(555)-123-4567"
    }
}
```

## Schema Flexibility

One of the key differentiators of document databases is their flexible schema. In a relational database, every row in a
table must follow the same predefined structure. Document databases do not enforce a rigid schema. Each document in a
collection can have its own unique set of fields, data types, and nested structures.

This flexibility makes document databases ideal for scenarios where data structures evolve over time. New fields can be
added to documents without needing to alter a table or migrate existing data. For example, one user document might have
a "phone" field while another does not, and both can coexist in the same collection without issue.

However, this flexibility comes with trade-offs. Without a strict schema, data validation and consistency must be handled
at the application level rather than being enforced by the database itself. This means developers need to be more
careful about maintaining data quality.

## Advantages and Disadvantages

### Advantages

- **Flexible schema**: Documents do not need to follow a fixed structure, making it easy to evolve data models over time
  without migrations.
- **Natural data modeling**: Data is stored in a way that closely maps to objects in application code, reducing the need
  for complex joins or object-relational mapping.
- **Horizontal scalability**: Document databases are designed to scale out across multiple servers, making them
  well-suited for handling large volumes of data and high traffic.
- **Performance for read-heavy workloads**: Since all related data is stored within a single document, reads can be
  faster because they avoid costly join operations.
- **Developer productivity**: The document model is intuitive for developers, especially when working with JSON-based
  APIs and modern web applications.

### Disadvantages

- **Data duplication**: Without joins, related data is often embedded within documents, which can lead to duplicated
  data across multiple documents. Updating shared data requires changes in multiple places.
- **Lack of strict consistency**: Many document databases favor eventual consistency over strong consistency, which may
  not be suitable for applications that require strict transactional guarantees.
- **Complex queries across documents**: While querying within a single document is straightforward, queries that span
  multiple collections or require relationships between documents can be more difficult and less efficient.
- **No standardized query language**: Unlike relational databases that use SQL, document databases often have their own
  query languages or APIs, which vary between implementations.

## References

- Wikipedia contributors. (2026). Document-oriented database. In Wikipedia, The Free Encyclopedia*. Retrieved from
  [Document-oriented database on Wikipedia](https://en.wikipedia.org/wiki/Document-oriented_database/)
- GeeksforGeeks. (2026). How Do Document Databases Work? Retrieved from
  [How Do Document Databases Work? on GeeksforGeeks](https://www.geeksforgeeks.org/dbms/how-do-document-databases-work/)
