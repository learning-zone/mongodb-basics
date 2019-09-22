## MongoDB Interview Questions and Answers

#### Q1: Explain what is MongoDB? 
Mongo-DB is a document database which provides high performance,
high availability and easy scalability.

#### Q2: How many indexes does MongoDB create by default for a new collection? 
By default, MongoDB created the _id collection for every collection.

#### Q3: Does Mongodb Support Foreign Key Constraints? 
No. MongoDB does not support such relationships.

#### Q4: Which are the most important features of MongoDB? 
* Flexible data model in form of documents
* Agile and highly scalable database
* Faster than traditional databases
* Expressive query language

#### Q5: What are Indexes in MongoDB? 
Indexes support the efficient execution of queries in MongoDB. Without indexes, MongoDB must perform a collection scan, i.e. scan every document in a collection, to select those documents that match the query statement. If an appropriate index exists for a query, MongoDB can use the index to limit the number of documents it must inspect.

#### Q6: Why does Profiler use in MongoDB? 
MongoDB uses a database profiler to perform characteristics of each operation against the database. You can use a profiler to find queries and write operations

#### Q7: If you remove an object attribute, is it deleted from the database? 
Yes, it be. Remove the attribute and then re-save () the object.

#### Q8: Does MongoDB need a lot space of Random Access Memory (RAM)? 
No. MongoDB can be run on small free space of RAM.

#### Q9: What is “Namespace” in MongoDB? 
MongoDB stores BSON (Binary Interchange and Structure Object Notation) objects in the collection. The concatenation of the collection name and database name is called a namespace

#### Q10: Mention the command to insert a document in a database called school and collection called persons. 
```js
use school;
db.persons.insert( { name: "Pradeep", dept: "CSE" } )
```

#### Q11: What is a replica set? 
It is a group of mongo instances that maintain same data set. Replica sets provide redundancy and high availability, and are the basis for all production deployments.

#### Q12: When should we embed one document within another in MongoDB? 
You should consider embedding documents for:

* *contains* relationships between entities
* One-to-many relationships
* Performance reasons

#### Q13: How is data stored in MongoDB? 
Data in MongoDB is stored in BSON documents – JSON-style data structures. Documents contain one or more fields, and each field contains a value of a specific data type, including arrays, binary data and sub-documents. Documents that tend to share a similar structure are organized as collections.

It may be helpful to think of documents as analogous to rows in a relational database, fields as similar to columns, and collections as similar to tables.

#### Q14: What Is Replication In Mongodb? 
Replication is the process of synchronizing data across multiple servers. Replication provides redundancy and increases data availability. With multiple copies of data on different database servers, replication protects a database from the loss of a single server. Replication also allows you to recover from hardware failure and service interruptions.

#### Q15: What are the differences between MongoDB and MySQL/SQL SERVER? 
* The MongoDB store the data in documents with JSON format but SQL store the data in Table format.
* The MongoDB provides high performance, high availability, easy scalability etc.  rather than SQL Server.
* In the MongoDB, we can change the structure simply by adding, removing column from the existing documents.

#### Q16: Compare SQL databases and MongoDB at a high level. 
SQL databases store data in form of tables, rows, columns and records. This data is stored in a pre-defined data model which is not very much flexible for today's real-world highly growing applications. MongoDB in contrast uses a flexible structure which can be easily modified and extended.

#### Q17: Can you create an index on an array field in MongoDB? If yes, what happens in this case? 
Yes. An array field can be indexed in MongoDB. In this case, MongoDB would index each value of the array.

#### Q18: How can you achieve transaction and locking in MongoDB? 
*TODO*

#### Q19: What are NoSQL databases? What are the different types of NoSQL databases? 
*TODO*

#### Q20: How is MongoDB better than other SQL databases? 
*TODO*

#### Q21: Does MongoDB support ACID transaction management and locking functionalities? 
*TODO*

#### Q22: How can I combine data from multiple collections into one collection? 
*TODO*

#### Q23: What does MongoDB not being ACID compliant really mean? 
*TODO*

#### Q24: Find objects between two dates MongoDB 
*TODO*

#### Q25: How to query MongoDB with “like”? 
*TODO*

#### Q26: Should I normalize my data before storing it in MongoDB? 
*TODO*

#### Q27: Is there an “upsert” option in the mongodb insert command? 
*TODO*

#### Q28: What is oplog? 
*TODO*

#### Q29: How can you achieve primary key - foreign key relationships in MongoDB? 
*TODO*

#### Q30: Does MongoDB pushes the writes to disk immediately or lazily? 
*TODO*

#### Q31: If you remove a document from database, does MongoDB remove it from disk? 
*TODO*

#### Q32: Can one MongoDB operation lock more than one databases? If yes, how? 
*TODO*

#### Q33: Explain the structure of ObjectID in MongoDB. 
*TODO*

#### Q34: What is the difference b/w MongoDB and CouchDB? 
*TODO*

#### Q35: What is the difference between MongoDB and MySQL? 
*TODO*

#### Q36: What is a covered query in MongoDB? 
*TODO*

#### Q37: Mention the command to check whether you are on the master server or not. 
*TODO*

#### Q38: Why MongoDB is not preferred over a 32-bit system? 
*TODO*

#### Q39: What do you understand by NoSQL databases? Explain. 
*TODO*

#### Q40: Why are MongoDB data files large in size? 
*TODO*

#### Q41: What is Sharding in MongoDB? Explain. 
*TODO*

#### Q42: What is sharding? 
*TODO*

#### Q43: What is Aggregation in MongoDB? 
*TODO*

#### Q44: How can you isolate your cursors from intervening with the write operations? 
*TODO*

#### Q45: At what interval does MongoDB write updates to the disk? 
*TODO*

#### Q46: By default, MongoDB writes and reads data from both primary and secondary replica sets. True or False. 
*TODO*

#### Q47: Mention the command to list all the indexes on a particular collection. 
*TODO*

#### Q48: What happens if an index does not fit into RAM? 
*TODO*

#### Q49: Does MongoDB provide a facility to do text searches? How? 
*TODO*

#### Q50: Where can I run MongoDB? 
*TODO*

#### Q51: How to remove a field completely from a MongoDB document? 
*TODO*

#### Q52: How does Journaling work in MongoDB? 
*TODO*

#### Q53: Why is a covered query important? 
*TODO*

#### Q54: How does MongoDB provide concurrency? 
*TODO*

#### Q55: What are Primary and Secondary Replica sets? 
*TODO*

#### Q56: How replication works in MongoDB? 
*TODO*

#### Q57: What are alternatives to MongoDB? 
*TODO*

#### Q58: Update MongoDB field using value of another field 
*TODO*

#### Q59: How does MongoDB ensure high availability? 
*TODO*

#### Q60: Is MongoDB schema-less? 
*TODO*

#### Q61: What's the advantage of the backup features in Ops Manager versus traditional backup strategies? 
*TODO*

#### Q62: What is splitting in mongodb? 
*TODO*

#### Q63: Which are the two storage engines used by MongoDB? 
*TODO*

#### Q64: What is a Storage Engine in MongoDB 
*TODO*

#### Q65: How to condense large volumes of data in Mongo? 
*TODO*

