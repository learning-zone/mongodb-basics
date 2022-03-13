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

**$lookup:**

Performs a left outer join to an unsharded collection in the same database to filter in documents from the “joined” collection for processing. To each input document, the `$lookup` stage adds a new array field whose elements are the matching documents from the “joined” collection. The `$lookup` stage passes these reshaped documents to the next stage.

**Syntax:**

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

## Q. ***Mention the command to remove indexes and list all the indexes on a particular collection?***

**List all Indexes on a Collection**

```js
// To view all indexes on the people collection

db.people.getIndexes()
```

**List all Indexes for a Database**

```js
// To list all the collection indexes in a database

db.getCollectionNames().forEach(function(collection) {
   indexes = db[collection].getIndexes();
   print("Indexes for " + collection + ":");
   printjson(indexes);
});
```

**Remove Indexes**

MongoDB provides two methods for removing indexes from a collection:

* `db.collection.dropIndex()`
* `db.collection.dropIndexes()`

**1. Remove Specific Index**

```js
db.accounts.dropIndex( { "tax-id": 1 } )


// Output
{ "nIndexesWas" : 3, "ok" : 1 }
```

**2. Remove All Indexes**

```js
// The following command removes all indexes from the accounts collection

db.accounts.dropIndexes()
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***Select only selected elements from given collection?***

Consider a ```books``` collection with the following document:

```js
{
  "_id" : 1,
  title: "abc123",
  isbn: "0001122223334",
  author: { last: "zzz", first: "aaa" },
  copies: 5
}
```

The following ```$project``` stage adds the new fields isbn, lastName, and copiesSold:

```js
db.books.aggregate([
{
    $project : {
        title:1,
        isbn: {
               prefix: { $substr: [ "$isbn", 0, 3 ] },
               group: { $substr: [ "$isbn", 3, 2 ] },
               publisher: { $substr: [ "$isbn", 5, 4 ] },
               title: { $substr: [ "$isbn", 9, 3 ] },
               checkDigit: { $substr: [ "$isbn", 12, 1] }
            },
         lastName: "$author.last",
        copiesSold: "$copies"
    }
}
])
```

The operation results in the following document:

```js
{
   "_id" : 1,
   "title" : "abc123",
   "isbn" : {
      "prefix" : "000",
      "group" : "11",
      "publisher" : "2222",
      "title" : "333",
      "checkDigit" : "4"
   },
   "lastName" : "zzz",
   "copiesSold" : 5
}
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***How to join 3 or more collections in MongoDB?***

Let's say we have 3 hypothetical collections in MongoDB: customers, orders, and orderItems.

Each customer has multiple orders, and each order has multiple order items.

**Example:**

```js
// customers
[
    {
        customer_id: 1,
        name: "Jim Smith",
        email: "jim.smith@example.com"
    },
    {
        customer_id: 2,
        name: "Bob Jones",
        email: "bob.jones@example.com"
    }
]


// orders
[
    {
        order_id: 1,
        customer_id: 1
    },
    {
        order_id: 2,
        customer_id: 1
    }
]


// orderItems
[
    {
        order_item_id: 1,
        name: "Foo",
        price: 4.99,
        order_id: 1
    },
    {
        order_item_id: 2,
        name: "Bar",
        price: 17.99,
        order_id: 1
    },
    {
        order_item_id: 3,
        name: "baz",
        price: 24.99,
        order_id: 2
    }
]
```

**Desired Result:**

```js
[
    {
        customer_id: 1,
        name: "Jim Smith",
        email: "jim.smith@example.com"
        orders: [
            {
                order_id: 1,
                items: [
                    {
                        name: "Foo",
                        price: 4.99
                    },
                    {
                        name: "Bar",
                        price: 17.99
                    }
                ]
            },
            {
                order_id: 2,
                items: [
                    {
                        name: "baz",
                        price: 24.99
                    }
                ]
            }
        ]
    },
    {
        customer_id: 2,
        name: "Bob Jones",
        email: "bob.jones@example.com"
        orders: []
    }
]
```

### Answer

Do nested lookup using lookup with pipeline,

1. ```$lookup``` with orders collection.
2. ```let```, define variable customer_id that is from main collection, to access this reference variable inside pipeline using ```$$``` like ```$$customer_id```.
3. ```pipeline``` can add pipeline stages same as we do in root level pipeline
4. ```$expr``` whenever we match internal fields it requires expression match condition, so ```$$customer_id``` is parent collection field that declared in let and $customer_id is child collection's/current collection's field
5. ```$lookup``` with orderitems collection

```js
db.customers.aggregate([
  {
    $lookup: {
      from: "orders",
      let: { customer_id: "$customer_id" },
      pipeline: [
        { $match: { $expr: { $eq: ["$$customer_id", "$customer_id"] } } },
        {
          $lookup: {
            from: "orderitems",
            localField: "order_id",
            foreignField: "order_id",
            as: "items"
          }
        }
      ],
      as: "orders"
    }
  }
])
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***How to validate data in mongodb?***

`db.collection.validate(<documents>)` validates a collection. The method scans a collection data and indexes for correctness and returns the result.

**Syntax:**

```js
db.collection.validate( {
   full: <boolean>,          // Optional
   repair: <boolean>         // Optional, added in MongoDB 5.0
} )
```

**Example:**

```js
// validate a collection using the default validation setting
db.myCollection.validate({ })

// perform a full validation of collection
db.myCollection.validate( { full: true } )

// repair collection
db.myCollection.validate( { repair: true } )
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>
