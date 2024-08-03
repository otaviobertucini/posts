



- Non-relational databases can use data structures like graphs, documents, key-value pairs and others
- SQL databases can suffer from impedance mismatch, that is when database rows and columns cannot be easily translated into objects understandable by the programming language
	- In JSON representation, used in the document model, all of the data is in one place
	- The problem this approach is that multiples objects can have the same value. For example, two students can have the property “School” repeated.
		- This means that if the school name needs to be updated, every occurence need to be updated
		- On the other hand, instead of storing the school, one could store a reference to a school and only update the referenced row
	- Another thing is that joins need to be made by the application and requires multiple queries to get related data
	- Documents databases are often schemaless, meaning that the application needs to check if the stored/received data is consistent. Actually, the schema exists, but outside of the database
		- One advantage of the “schemaless” approach is the new schemas can be stored without the database needing to know it
		- Also, schemaless are good for applications with heterogeneous data
	- Documents database also have the advantage of data locality, since all data is in a single place, without the need of multiple joins and queries to retrieve the relations
- Denormalization = loss of data consistency
	- Normalized data models can require more round trips to the server.
- A good options is to use hybrid databases, meaning we can store heterogeneous data and use the advantage of relations on it.
- Non-relational databases are generally more scalable and can handle large volumes of unstructured data.
	- Non-relational databases are more scalable because they are designed to distribute data across multiple servers easily.
	- Relational databases are hard to scale horizontally because they rely on complex relationships and transactions that require consistency and integrity across multiple tables. This makes it challenging to distribute the data across multiple servers without running into issues with synchronization and data consistency.
- SQL databases provide ACID (Atomicity, Consistency, Isolation, Durability) compliance, which ensures reliable transactions.
- Non-relational databases might offer eventual consistency, which can be a consideration depending on the application’s requirements.





For more information on relational and non-relational databases, consider the following articles and books:


### Articles:

- "SQL vs NoSQL: The Differences" by IBM

[bookmark](https://hzhou.scholar.harvard.edu/blog/mysql-vs-mongodb)

- "Understanding the Differences Between SQL and NoSQL Databases" by MongoDB

[bookmark](https://learn.microsoft.com/en-us/azure/cosmos-db/nosql/modeling-data)


[bookmark](https://learn.microsoft.com/en-us/azure/cosmos-db/)


### Books:

- "Designing Data-Intensive Applications" by Martin Kleppmann
- "SQL and NoSQL Databases: Models, Languages, Consistency Options and Architectures for Big Data Management" by Andreas Meier and Michael Kaufmann
- "NoSQL Distilled: A Brief Guide to the Emerging World of Polyglot Persistence" by Pramod J. Sadalage and Martin Fowler




