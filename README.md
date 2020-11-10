# MongoDB Interview Questions

*Click <img src="assets/star.png" width="18" height="18" align="absmiddle" title="Star" /> if you like the project. Pull Request are highly appreciated.*

## Table of Contents

* *[MongoDB Commands](mongodb-commands.md)*

<br/>

## Q. ***What is MongoDB?***

**MongoDB** is a document-oriented NoSQL database used for high volume data storage. Instead of using tables and rows as in the traditional relational databases, MongoDB makes use of collections and documents. Documents consist of key-value pairs which are the basic unit of data in MongoDB. Collections contain sets of documents and function which is the equivalent of relational database tables.

**Key Components**

**1. _id**: The `_id` field represents a unique value in the MongoDB document. The `_id` field is like the document\'s primary key. If you create a new document without an `_id` field, MongoDB will automatically create the field.

**2. Collection**:  This is a grouping of MongoDB documents. A collection is the equivalent of a table which is created in any other RDMS such as Oracle.

**3. Cursor**: This is a pointer to the result set of a query. Clients can iterate through a cursor to retrieve results.

**4. Database**: This is a container for collections like in RDMS wherein it is a container for tables. Each database gets its own set of files on the file system. A MongoDB server can store multiple databases.

**5. Document**: A record in a MongoDB collection is basically called a document. The document, in turn, will consist of field name and values.

**6. Field**: A name-value pair in a document. A document has zero or more fields. Fields are analogous to columns in relational databases.

**Example:** Connecting MongoDB Cloud using MongoDB Compass

<p align="center">
   <img src="assets/mongodb-compass.png" alt="MongoDB Compass" width="800px" title="MongoDB Compass" />
</p>

**[[Getting Started](https://docs.mongodb.com/guides/)]**

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***How many indexes does MongoDB create by default for a new collection?***

By default MongoDB creates a unique index on the `_id` field during the creation of a collection. The `_id` index prevents clients from inserting two documents with the same value for the `_id` field.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***What are Indexes in MongoDB?***

Indexes support the efficient execution of queries in MongoDB. Without indexes, MongoDB must perform a collection scan, i.e. scan every document in a collection, to select those documents that match the query statement. If an appropriate index exists for a query, MongoDB can use the index to limit the number of documents it must inspect.

Indexes are special data structures that store a small portion of the collection\'s data set in an easy to traverse form. The index stores the value of a specific field or set of fields, ordered by the value of the field. The ordering of the index entries supports efficient equality matches and range-based query operations. In addition, MongoDB can return sorted results by using the ordering in the index.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***Why does Profiler use in MongoDB?***

The database profiler captures data information about read and write operations, cursor operations, and database commands. The database profiler writes data in the `system.profile` collection, which is a capped collection.

The database profiler collects detailed information about Database Commands executed against a running mongod instance. This includes CRUD operations as well as configuration and administration commands.

Profiler has 3 profiling levels.

* **Level 0** - Profiler will not log any data
* **Level 1** - Profiler will log only slow operations above some threshold
* **Level 2** - Profiler will log all the operations

**1. To get current profiling level.**

```bash
db.getProfilingLevel()  

// Output
0
```

**2. To check current profiling status**

```bash
db.getProfilingStatus()


// Output
{ "was" : 0, "slowms" : 100 }
```

**3. To set profiling level**

```bash
db.setProfilingLevel(1, 40)

// Output
{ "was" : 0, "slowms" : 100, "ok" : 1 }
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***How to remove attribute from MongoDb Object?***

**$unset**

The `$unset` operator deletes a particular field. If the field does not exist, then `$unset` does nothing. When used with `$` to match an array element, `$unset` replaces the matching element with `null` rather than removing the matching element from the array. This behavior keeps consistent the array size and element positions.

syntax:

```js
{ $unset: { <field1>: "", ... } }
```

**Example**: delete the `properties.service` attribute from all records on this collection.

```js
db.collection.update(
    {},
    {
        $unset : {
            "properties.service" : 1
        }
    },
    {
        multi: true
    }
);
```

**To verify they have been deleted you can use:**

```js
db.collection.find(
    {
        "properties.service" : {
            $exists : true
         }
    }
).count(true);
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***What is "Namespace" in MongoDB?***

MongoDB stores BSON (Binary Interchange and Structure Object Notation) objects in the collection. The concatenation of the collection name and database name is called a namespace

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***Mention the command to insert a document in a database called school and collection called persons?*** 

```js
use school;
db.persons.insert( { name: "Alex", age: "28" } )
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***What is a replica set?***

It is a group of mongo processes that maintain same data set. Replica sets provide redundancy and high availability, and are the basis for all production deployments. A replica set contains a primary node and multiple secondary nodes.

The primary node receives all write operations. A replica set can have only one primary capable of confirming writes with `{ w: "majority" }` write concern; although in some circumstances, another mongod instance may transiently believe itself to also be primary.

The secondaries replicate the primary\'s oplog and apply the operations to their data sets such that the secondaries\' data sets reflect the primary\'s data set. If the primary is unavailable, an eligible secondary will hold an election to elect itself the new primary.

<p align="center">
  <img src="assets/replica-set.png" alt="Replica Set" width="500px" />
</p>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***When should we embed one document within another in MongoDB?*** 

You should consider embedding documents for:

* *contains* relationships between entities
* One-to-many relationships
* Performance reasons

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***How is data stored in MongoDB?***

Data in MongoDB is stored in BSON documents – JSON-style data structures. Documents contain one or more fields, and each field contains a value of a specific data type, including arrays, binary data and sub-documents. Documents that tend to share a similar structure are organized as collections.

It may be helpful to think of documents as analogous to rows in a relational database, fields as similar to columns, and collections as similar to tables.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***What is Replication in Mongodb?***

Replication is the process of synchronizing data across multiple servers. Replication provides redundancy and increases data availability. With multiple copies of data on different database servers, replication protects a database from the loss of a single server. Replication also allows you to recover from hardware failure and service interruptions.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***What are the differences between MongoDB and SQL SERVER?***

* The MongoDB store the data in documents with JSON format but SQL store the data in Table format.
* The MongoDB provides high performance, high availability, easy scalability etc.  rather than SQL Server.
* In the MongoDB, we can change the structure simply by adding, removing column from the existing documents.

<p align="center">
  <img src="assets/RDBMS_MongoDB_Mapping.jpg" alt="MongoDB & SQL Server" width="400px" />
</p>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***Compare SQL databases and MongoDB at a high level?***

SQL databases store data in form of tables, rows, columns and records. This data is stored in a pre-defined data model which is not very much flexible for today's real-world highly growing applications. MongoDB in contrast uses a flexible structure which can be easily modified and extended.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***Can you create an index on an array field in MongoDB?***

Yes. An array field can be indexed in MongoDB. In this case, MongoDB would index each value of the array.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

#### Q. ***How can you achieve transaction and locking in MongoDB?*** 
#### Q. ***What are NoSQL databases? What are the different types of NoSQL databases?*** 
#### Q. ***How is MongoDB better than other SQL databases?*** 
#### Q. ***Does MongoDB support ACID transaction management and locking functionalities?*** 
#### Q. ***How can I combine data from multiple collections into one collection?*** 
#### Q. ***What does MongoDB not being ACID compliant really mean?*** 
#### Q. ***Find objects between two dates MongoDB?*** 
#### Q. ***How to query MongoDB with "like"?*** 
#### Q. ***Should I normalize my data before storing it in MongoDB?*** 
#### Q. ***Is there an "upsert" option in the mongodb insert command?*** 
#### Q. ***What is oplog?*** 
#### Q. ***How can you achieve primary key - foreign key relationships in MongoDB?*** 
#### Q. ***Does MongoDB pushes the writes to disk immediately or lazily?*** 
#### Q. ***If you remove a document from database, does MongoDB remove it from disk?*** 
#### Q. ***Can one MongoDB operation lock more than one databases?***  
#### Q. ***Explain the structure of ObjectID in MongoDB?*** 
#### Q. ***What is the difference b/w MongoDB and CouchDB?*** 
#### Q. ***What is the difference between MongoDB and MySQL?*** 
#### Q. ***What is a covered query in MongoDB?*** 
#### Q. ***Mention the command to check whether you are on the master server or not?***
#### Q. ***Why MongoDB is not preferred over a 32-bit system?*** 
#### Q. ***What do you understand by NoSQL databases?***  
#### Q. ***Why are MongoDB data files large in size?*** 
#### Q. ***What is Sharding in MongoDB?*** 
#### Q. ***What is Aggregation in MongoDB?*** 
#### Q. ***How can you isolate your cursors from intervening with the write operations?*** 
#### Q. ***At what interval does MongoDB write updates to the disk?*** 
#### Q. ***By default, MongoDB writes and reads data from both primary and secondary replica sets. True or False.?***
#### Q. ***Mention the command to list all the indexes on a particular collection?***
#### Q. ***What happens if an index does not fit into RAM?*** 
#### Q. ***Does MongoDB provide a facility to do text searches?*** 
#### Q. ***Where can I run MongoDB?*** 
#### Q. ***How to remove a field completely from a MongoDB document?*** 
#### Q. ***How does Journaling work in MongoDB?*** 
#### Q. ***Why is a covered query important?*** 
#### Q. ***How does MongoDB provide concurrency?*** 
#### Q. ***What are Primary and Secondary Replica sets?*** 
#### Q. ***How replication works in MongoDB?*** 
#### Q. ***What are alternatives to MongoDB?*** 
#### Q. ***Update MongoDB field using value of another field?*** 
#### Q. ***How does MongoDB ensure high availability?*** 
#### Q. ***Is MongoDB schema-less?*** 
#### Q. ***What is the advantage of the backup features in Ops Manager versus traditional backup strategies?*** 
#### Q. ***What is splitting in mongodb?*** 
#### Q. ***What is a Storage Engine in MongoDB?***
#### Q. ***How to condense large volumes of data in Mongo?*** 
#### Q. ***What is horizontal scalability?***
#### Q. ***Is it possible to update MongoDB field using value of another field?***
#### Q. ***How to check if a field contains a substring?***
#### Q. ***How to find document with array that contains a specific value?***
#### Q. ***How to find MongoDB records where array field is not empty?***
#### Q. ***How to get the last N records from find?***
#### Q. ***Explain relationships in mongodb?***
#### Q. ***What is use of capped collection in MongoDB?***

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>
