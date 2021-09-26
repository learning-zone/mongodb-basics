## Join 3 or more collections

Let's say we have 3 hypothetical collections in MongoDB: customers, orders, and orderItems.

Each customer has multiple orders, and each order has multiple order items.


### customers

```js
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
```
### orders
```js
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
```

### orderItems
```js
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

### Desired Result
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


# Answer

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
