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

**Example:**

Connecting MongoDB Cloud using MongoDB Compass

<p align="center">
   <img src="assets/mongodb-compass.png" alt="MongoDB Compass" width="800px" title="MongoDB Compass" />
</p>

**[[Getting Started](https://docs.mongodb.com/guides/)]**

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***What are Indexes in MongoDB?***

Indexes support the efficient execution of queries in MongoDB. Without indexes, MongoDB must perform a collection scan, i.e. scan every document in a collection, to select those documents that match the query statement. If an appropriate index exists for a query, MongoDB can use the index to limit the number of documents it must inspect.

Indexes are special data structures that store a small portion of the collection\'s data set in an easy to traverse form. The index stores the value of a specific field or set of fields, ordered by the value of the field. The ordering of the index entries supports efficient equality matches and range-based query operations. In addition, MongoDB can return sorted results by using the ordering in the index.

**Example**

The `createIndex()` method only creates an index if an index of the same specification does not already exist. The following example ( using Node.js ) creates a single key descending index on the name field:

```js
collection.createIndex( { name : -1 }, function(err, result) {
   console.log(result);
   callback(result);
}
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***What are the types of Indexes available in MongoDB?***

MongoDB supports the following types of the index for running a query.

**1. Single Field Index**

MongoDB supports user-defined indexes like single field index. A single field index is used to create an index on the single field of a document. With single field index, MongoDB can traverse in ascending and descending order. By default, each collection has a single field index automatically created on the `_id` field, the primary key.

**Example**

```js
{
  "_id": 1,
  "person": { name: "Alex", surname: "K" },
  "age": 29,
  "city": "New York"
}
```

We can define, a single field index on the age field.

```js
db.people.createIndex( {age : 1} ) // creates an ascending index

db.people.createIndex( {age : -1} ) // creates a descending index
```

With this kind of index we can improve all the queries that find documents with a condition and the age field, like the following:

```js
db.people.find( { age : 20 } )
db.people.find( { name : "Alex", age : 30 } )
db.people.find( { age : { $gt : 25} } )
```

**2. Compound Index**

A compound index is an index on multiple fields. Using the same people collection we can create a compound index combining the city and age field.

```js
db.people.createIndex( {city: 1, age: 1, person.surname: 1  } )
```

In this case, we have created a compound index where the first entry is the value of the city field, the second is the value of the age field, and the third is the person.name. All the fields here are defined in ascending order.

Queries such as the following can benefit from the index:

```js
db.people.find( { city: "Miami", age: { $gt: 50 } } )
db.people.find( { city: "Boston" } )
db.people.find( { city: "Atlanta", age: {$lt: 25}, "person.surname": "Green" } )
```

**3. Multikey Index**

This is the index type for arrays. When creating an index on an array, MongoDB will create an index entry for every element.

**Example**

```js
{
   "_id": 1,
   "person": { name: "John", surname: "Brown" },
   "age": 34,
   "city": "New York",
   "hobbies": [ "music", "gardening", "skiing" ]
 }
```

The multikey index can be created as:

```js
db.people.createIndex( { hobbies: 1} )
```

Queries such as these next examples will use the index:

```js
db.people.find( { hobbies: "music" } )
db.people.find( { hobbies: "music", hobbies: "gardening" } )
```

**4. Geospatial Index**

GeoIndexes are a special index type that allows a search based on location, distance from a point and many other different features. To query geospatial data, MongoDB supports two types of indexes – `2d indexes` and `2d sphere indexes`. 2d indexes use planar geometry when returning results and 2dsphere indexes use spherical geometry to return results.

**5. Text Index**

It is another type of index that is supported by MongoDB. Text index supports searching for string content in a collection. These index types do not store language-specific stop words (e.g. “the”, “a”, “or”). Text indexes restrict the words in a collection to only store root words.

**Example**

Let\'s insert some sample documents.

```js
var entries = db.people("blogs").entries;
entries.insert( {
  title : "my blog post",
  text : "i am writing a blog. yay",
  site: "home",
  language: "english" });
entries.insert( {
  title : "my 2nd post",
  text : "this is a new blog i am typing. yay",
  site: "work",
  language: "english" });
entries.insert( {
  title : "knives are Fun",
  text : "this is a new blog i am writing. yay",
  site: "home",
  language: "english" });
```

Let\'s define create the text index.

```js
var entries = db.people("blogs").entries;
entries.ensureIndex({title: "text", text: "text"}, { weights: {
    title: 10,
    text: 5
  },
  name: "TextIndex",
  default_language: "english",
  language_override: "language" });
```

Queries such as these next examples will use the index:

```js
var entries = db.people("blogs").entries;
entries.find({$text: {$search: "blog"}, site: "home"})
```

**6. Hashed Index**

MongoDB supports hash-based sharding and provides hashed indexes. These indexes are the hashes of the field value. Shards use hashed indexes and create a hash according to the field value to spread the writes across the sharded instances.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***Explain Index Properties in MongoDB?***

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***How many indexes does MongoDB create by default for a new collection?***

By default MongoDB creates a unique index on the `_id` field during the creation of a collection. The `_id` index prevents clients from inserting two documents with the same value for the `_id` field.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***Can you create an index on an array field in MongoDB?***

Yes. An array field can be indexed in MongoDB. In this case, MongoDB would index each value of the array.

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

## Q. ***How to remove attribute from MongoDB Object?***

**$unset**

The `$unset` operator deletes a particular field. If the field does not exist, then `$unset` does nothing. When used with `$` to match an array element, `$unset` replaces the matching element with `null` rather than removing the matching element from the array. This behavior keeps consistent the array size and element positions.

syntax:

```js
{ $unset: { <field1>: "", ... } }
```

**Example:**

delete the `properties.service` attribute from all records on this collection.

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

## Q. ***What is Replication in Mongodb?***

Replication exists primarily to offer data redundancy and high availability. It maintain the durability of data by keeping multiple copies or replicas of that data on physically isolated servers. Replication allows to increase data availability by creating multiple copies of data across servers. This is especially useful if a server crashes or hardware failure.

With MongoDB, replication is achieved through a **Replica Set**. Writer operations are sent to the primary server (node), which applies the operations across secondary servers, replicating the data. If the primary server fails (through a crash or system failure), one of the secondary servers takes over and becomes the new primary node via election. If that server comes back online, it becomes a secondary once it fully recovers, aiding the new primary node.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***What is Replica Set in MongoDB?***

It is a group of mongo processes that maintain same data set. Replica sets provide redundancy and high availability, and are the basis for all production deployments. A replica set contains a primary node and multiple secondary nodes.

The primary node receives all write operations. A replica set can have only one primary capable of confirming writes with `{ w: "majority" }` write concern; although in some circumstances, another mongod instance may transiently believe itself to also be primary.

The secondaries replicate the primary\'s oplog and apply the operations to their data sets such that the secondaries\' data sets reflect the primary\'s data set. If the primary is unavailable, an eligible secondary will hold an election to elect itself the new primary.

<p align="center">
  <img src="assets/replica-set.png" alt="Replica Set" width="400px" />
</p>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***What is an Embedded MongoDB Document?***

An embedded, or nested, MongoDB Document is a normal document that is nested inside another document within a MongoDB collection. Embedding connected data in a single document can reduce the number of read operations required to obtain data. In general, we should structure our schema so that application receives all of its required information in a single read operation.

**Example:**

In the normalized data model, the address documents contain a reference to the patron document.

```js
// patron document
{
   _id: "joe",
   name: "Joe Bookreader"
}

// address documents
{
   patron_id: "joe", // reference to patron document
   street: "123 Fake Street",
   city: "Faketon",
   state: "MA",
   zip: "12345"
}

{
   patron_id: "joe",
   street: "1 Some Other Street",
   city: "Boston",
   state: "MA",
   zip: "12345"
}
```

Embedded documents are particularly useful when a **one-to-many** relationship exists between documents. In the example shown above, we see that a single customer has multiple addresses associated with him. The nested document structure makes it easy to retrieve complete address information about this customer with just a single query.

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

In MongoDB, Data is stored in BSON documents (short for `Bin­ary JSON`). These documents are stored in MongoDB in JSON (JavaScript Object Notation) format. JSON documents support embedded fields, so related data and lists of data can be stored with the document instead of an external table. Documents contain one or more fields, and each field contains a value of a specific data type, including arrays, binary data and sub-documents. Documents that tend to share a similar structure are organized as collections.

JSON is formatted as name/value pairs. In JSON documents, field names and values are separated by a colon, field name and value pairs are separated by commas, and sets of fields are encapsulated in “curly braces” ({}).

**Example:**

```js
{
  "name": "notebook",
  "qty": 50,
  "rating": [ { "score": 8 }, { "score": 9 } ],
  "size": { "height": 11, "width": 8.5, "unit": "in" },
  "status": "A",
  "tags": [ "college-ruled", "perforated"]
}
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***What are the differences between MongoDB and SQL-SERVER?***

* The MongoDB store the data in documents with JSON format but SQL store the data in Table format.
* The MongoDB provides high performance, high availability, easy scalability etc.  rather than SQL Server.
* In the MongoDB, we can change the structure simply by adding, removing column from the existing documents.

**MongoDB and SQL Server Comparision Table**

|Base of Comparison  | MS SQL Server      | MongoDB             |
|--------------------|--------------------|---------------------|
|Storage Model       |RDBMS               |Document-Oriented    |
|Joins               |Yes                 |No                   |
|Transaction         |ACID                |Multi-document ACID Transactions with snapshot isolation |
|Agile practices     |No                  |Yes                  |
|Data Schema         |Fixed               |Dynamic              |
|Scalability         |Vertical            |Horizontal           |
|Map Reduce          |No                  |Yes                  |
|Language            |SQL query language  |JSON Query Language  |
|Secondary index     |Yes                 |Yes                  |
|Triggers            |Yes                 |Yes                  |
|Foreign Keys        |Yes                 |No                   |
|Concurrency         |Yes                 |yes                  |
|XML Support         |Yes                 |No                   |

<p align="center">
  <img src="assets/RDBMS_MongoDB_Mapping.jpg" alt="MongoDB & SQL Server" width="500px" />
</p>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

#### Q. ***How can you achieve transaction and locking in MongoDB?*** 
#### Q. ***What are NoSQL databases? What are the different types of NoSQL databases?*** 
#### Q. ***How is MongoDB better than other SQL databases?*** 
#### Q. ***Does MongoDB support ACID transaction management and locking functionalities?*** 
#### Q. ***How can I combine data from multiple collections into one collection?*** 
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
