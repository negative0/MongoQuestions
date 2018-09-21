## Questions
### Data 1: 
```javascript
db.posts.insertMany([
  {
   
   title: 'MongoDB Overview', 
   description: 'MongoDB is no sql database',
   by_user: 'abc',
   url: 'http://www.abc.com',
   tags: ['mongodb', 'database', 'NoSQL'],
   likes: 100
},
{

   title: 'NoSQL Overview', 
   description: 'No sql database is very fast',
   by_user: 'abc',
   url: 'http://www.abc.com',
   tags: ['mongodb', 'database', 'NoSQL'],
   likes: 10
},
{

   title: 'Neo4j Overview', 
   description: 'Neo4j is no sql database',
   by_user: 'Neo4j',
   url: 'http://www.neo4j.com',
   tags: ['neo4j', 'database', 'NoSQL'],
   likes: 750
}
]);
```
### Question 1

- Display a list stating how many tutorials are written by each user, then you will use the following aggregate() method −


#### Answer
```javascript
db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : 1}}}])
```
### Data 2:
Sales Collection
```javascript
db.sales.insertMany([ 
  { "_id" : 1, "item" : "abc", "price" : 10, "quantity" : 2, "date" : ISODate("2014-03-01T08:00:00Z") },
  { "_id" : 2, "item" : "jkl", "price" : 20, "quantity" : 1, "date" : ISODate("2014-03-01T09:00:00Z") },
  { "_id" : 3, "item" : "xyz", "price" : 5, "quantity" : 10, "date" : ISODate("2014-03-15T09:00:00Z") },
  { "_id" : 4, "item" : "xyz", "price" : 5, "quantity" : 20, "date" : ISODate("2014-04-04T11:21:39.736Z") },
  { "_id" : 5, "item" : "abc", "price" : 10, "quantity" : 10, "date" : ISODate("2014-04-04T21:23:13.331Z") }
]);
```

### Question 2

- Write an aggregate query to calculate the Average cost per item (price * quantity)

- :bulb: Use $multiply: \[ a , b \] and $avg.



#### Answer:
```javascript
db.sales.aggregate([{
    $group: { 
        "_id":  "$item" ,
        "avg_cost":{ $avg: {$multiply: ["$price", "$quantity"]}}
    }
}]);
```

### Question 3

- Calculate the total price and the average quantity as well as counts for all documents in the collection

##### Expected Output:

```
{ "_id" : null, "totalPrice" : 290, "averageQuantity" : 8.6, "count" : 5 }

```

#### Answer:
```
db.sales.aggregate(
   [
      {
        $group : {
           _id : null,
           totalPrice: { $sum: { $multiply: [ "$price", "$quantity" ] } },
           averageQuantity: { $avg: "$quantity" },
           count: { $sum: 1 }
        }
      }
   ]
)
```
