# MongoDB Coding Practice

## Q. ***Mention the command to insert a document in a database called company and collection called employee?***

```js
use company;
db.employee.insert( { name: "John", email: "john.k@gmail.com" } )
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***Mention the command to check whether you are on the master server or not?***

```js
db.isMaster()
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***How to combine data from multiple collections into one collection?***

**$lookup**

Performs a left outer join to an unsharded collection in the same database to filter in documents from the “joined” collection for processing. To each input document, the `$lookup` stage adds a new array field whose elements are the matching documents from the “joined” collection. The `$lookup` stage passes these reshaped documents to the next stage.

**Syntax**

```js
{
   $lookup:
     {
       from: <collection to join>,
       localField: <field from the input documents>,
       foreignField: <field from the documents of the "from" collection>,
       as: <output array field>
     }
}
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***How do I perform the SQL JOIN equivalent in MongoDB?***

```js
// Sample Records

comments
  { uid:12345, pid:444, comment="blah" }
  { uid:12345, pid:888, comment="asdf" }
  { uid:99999, pid:444, comment="qwer" }

users
  { uid:12345, name:"john" }
  { uid:99999, name:"mia"  }
```

**Answers**

```js
{
   $lookup:
     {
       from: <collection to join>,
       localField: <field from the input documents>,
       foreignField: <field from the documents of the "from" collection>,
       as: <output array field>
     }
}
```

## Q. ***How to query MongoDB with "like"?***

```js
db.users.insert({name: 'paulo'})
db.users.insert({name: 'patric'})
db.users.insert({name: 'pedro'})

db.users.find({name: /a/})  //like '%a%'
db.users.find({name: /^pa/}) //like 'pa%'
db.users.find({name: /ro$/}) //like '%ro'
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***Find objects between two dates MongoDB?***

Operator `$gte` and `$lt` is used to find objects between two dates in MongoDB.

**Example**: Creating a collection

```js
>db.order.insert({"OrderId":1,"OrderAddrees":"US","OrderDateTime":ISODate("2020-02-19")};
WriteResult({ "nInserted" : 1 })

>db.order.insert({"OrderId":2,"OrderAddrees":"UK","OrderDateTime":ISODate("2020-02-26")};
WriteResult({ "nInserted" : 1 })
```

Display all documents from the collection using `find()` method.

```js
> db.order.find().pretty();

// Output
{
   "_id" : ObjectId("5c6c072068174aae23f5ef57"),
   "OrderId" : 1,
   "OrderAddrees" : "US",
   "OrderDateTime" : ISODate("2020-02-19T00:00:00Z")
}
{
   "_id" : ObjectId("5c6c073568174aae23f5ef58"),
   "OrderId" : 2,
   "OrderAddrees" : "UK",
   "OrderDateTime" : ISODate("2020-02-26T00:00:00Z")
}
```

Here is the query to find objects between two dates:

```js
> db.order.find({"OrderDateTime":{ $gte:ISODate("2020-02-10"), $lt:ISODate("2020-02-21") }
}).pretty();


// Output
{
   "_id" : ObjectId("5c6c072068174aae23f5ef57"),
   "OrderId" : 1,
   "OrderAddrees" : "US",
   "OrderDateTime" : ISODate("2020-02-19T00:00:00Z")
}
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***Is it possible to update MongoDB field using value of another field?***

The aggregate function can be used to update MongoDB field using the value of another field.

**Example**

```js
db.collection.<update method>(
    {},
    [
        {"$set": {"name": { "$concat": ["$firstName", " ", "$lastName"]}}}
    ]
)
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***How to check if a field contains a substring?***

The `$regex` operator can be used to check if a field contains a string in MongoDB.

```js
db.users.findOne({"username" : {$regex : ".*some_string.*"}});
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***How to get the last N records from find?***

```js
// Syntax
db.<CollectionName>.find().sort({$natural:-1}).limit(value)


// Example
db.employee.find().sort({$natural:-1}).limit(100)
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***How to remove a field completely from a MongoDB document?***

How do I remove words completely from all the documents in this collection?

```js
{ 
    name: 'book',
    tags: {
        words: ['abc','123'], // <-- remove it comletely
        lat: 33,
        long: 22
    }
}
```

**Answer**

```js
db.example.update({}, {$unset: {words: 1}}, false, true);
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***How to find MongoDB records where array field is not empty?***

```js
db.inventory.find({ pictures: { $exists: true, $ne: [] } })
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***How to find document with array that contains a specific value?***

Populate the inventory collection

```js
db.inventory.insertMany([
   { item: "journal", qty: 25, tags: ["blank", "red"], dim_cm: [ 14, 21 ] },
   { item: "notebook", qty: 50, tags: ["red", "blank"], dim_cm: [ 14, 21 ] },
   { item: "paper", qty: 100, tags: ["red", "blank", "plain"], dim_cm: [ 14, 21 ] },
   { item: "planner", qty: 75, tags: ["blank", "red"], dim_cm: [ 22.85, 30 ] },
   { item: "postcard", qty: 45, tags: ["blue"], dim_cm: [ 10, 15.25 ] }
]);
```

To query if the array field contains at least one element with the specified value, use the filter { `<field>`: `<value>` } where `<value>` is the element value.

```js
db.inventory.find( { tags: "red" } )
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>
