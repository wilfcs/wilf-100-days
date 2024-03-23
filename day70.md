Indexing in DBMS
- Indexing is used to optimise the performance of a database by minimising the number of disk accesses required when a query is processed.
- Inside a disk the information is stored in blocks like this ->
- 
- The white box is the db stored in a disk and the yellow boxes represent blocks. Suppose there are 100000 records in the disk. And each block has 10 records. 
- Suppose we have to find a student by its roll number. We will search the box inside the disk linearly. This will take a lot of time. Thats why we use the concept of indexing that works magic using these blocks.
- The index is a type of data structure. It is used to locate and access the data in a database table quickly.
- Index will have all the roll numbers along with the block number that have the roll number. Now using that, instead of searching the entire disk of 100000 records you just have to search 10. The roll number in the index though will always be stored in the sorted order so it can be found easily.
- Search Key: Contains copy of primary key or candidate key of the table (in the above case it was roll number).
- Data Reference: Pointer holding the address of disk block where the value of the corresponding key is stored.
- Types of indexing: 
- Advantages of Indexing
	- 1. Faster access and retrieval of data.
	- 2. IO is less.
- Limitations of Indexing
	- 1. Additional space to store index table
	- 2. Indexing Decrease performance in INSERT, DELETE, and UPDATE query.

NoSQL:
NoSQL databases (aka "not only SQL") are non-tabular databases and store data differently than relational tables. NoSQL databases
come in a variety of types based on their data model. The main types are document, key-value, wide-column, and graph. They
provide flexible schemas and scale easily with large amounts of data and high user loads.
	1. They are schema free.
	2. Data structures used are not tabular, they are more flexible, has the ability to adjust dynamically.
	3. Can handle huge amount of data (big data).
	4. Most of the NoSQL are open sources and has the capability of horizontal scaling.
	5. It just stores data in some format other than relational.
	
NoSQL Database Advantages:
- Flexible Schema
	- RDBMS has pre-defined schema, which become an issue when we do not have all the data with us or we need to change the schema. It's a huge task to change schema on the go.
- Horizontal Scaling
	- Horizontal scaling, also known as scale-out, refers to bringing on additional nodes to share the load. This is difficult with relational databases due to the difficulty in spreading out related data across nodes. With non-relational databases, this is made simpler since collections are self-contained and not coupled relationally. This allows them to be distributed across nodes more simply, as queries do not have to “join” them together across nodes.
	- Scaling horizontally is achieved through Sharding OR Replica-sets.
- High Availability
	- NoSQL databases are highly available due to its auto replication feature i.e. whenever any kind of failure happens data replicates itself to the preceding consistent state.
	- If a server fails, we can access that data from another server as well, as in NoSQL database data is stored at multiple servers.
- Easy insert and read operations.
	- Queries in NoSQL databases can be faster than SQL databases. Why? Data in SQL databases is typically normalised, so queries for a single object or entity require you to join data from multiple tables. As your tables grow in size, the joins can become expensive. However, data in NoSQL databases is typically stored in a way that is optimised for queries. The rule of thumb when you use MongoDB is data that is accessed together should be stored together. Queries typically do not require joins, so the queries are very fast.
	- But difficult delete or update operations.
- Caching mechanism.
- NoSQL use case is more for Cloud applications.

When to use NoSQL?
	1. Fast-paced Agile development
	2. Storage of structured and semi-structured data
	3. Huge volumes of data
	4. Requirements for scale-out architecture
	5. Modern application paradigms like micro-services and real-time streaming.

Types of NoSQL Data Models:
- Key-Value Stores: The simplest type of NoSQL database is a key-value store. Every data element in the database is stored as a key value pair consisting of an attribute name (or "key") and a value.
	- e.g., Oracle NoSQL, Amazon DynamoDB, MongoDB also supports Key-Value store, Redis.
	- There are several use-cases where choosing a key value store approach is an optimal solution:
		- a) Real time random data access, e.g., user session attributes in an online application such as gaming or finance.
		- b) Caching mechanism for frequently accessed data or configuration based on keys.
		- c) Application is designed on simple key-based queries.
- Column-Oriented: While a relational database stores data in rows and reads data row by row, a column store is organised as a set of columns. This means that when you want to run analytics on a small number of columns, you can read those columns directly without consuming memory with the unwanted data. Columns are often of the same type and benefit from more efficient compression, making reads even faster. e.g., Cassandra, RedShift, Snowflake.
- Document Based Stores: This DB store data in documents similar to JSON (JavaScript Object Notation) objects. Each document contains pairs of fields and values.
- Graph Based Stores: A graph database focuses on the relationship between data elements. Each element is stored as a node (such as a person in a social media graph). Use cases include fraud detection, social networks, and knowledge graphs.

NoSQL Databases Dis-advantages:
- Data Redundancy
- Update & Delete operations are costly.
- All type of NoSQL Data model doesn’t fulfil all of your application needs, For example, graph databases are excellent for analysing relationships in your data but may not provide what you need for everyday retrieval of the data such as range queries.
- Doesn’t support ACID properties in general.
- Doesn’t support data entry with consistency constraints.

Types of DB:
- Relational Databases
- NoSQL Database
- Object Oriented Database
- Hierarchical Databases
- Network Databases