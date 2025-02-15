#### Databases
In MongoDB, databases hold one or more collections of documents.
Select using `use myDB`
Create using 
```mongodb
use myNewDB
db.myNewCollection1.insertOne( { x: 1 } )
```

here, insertOne both creates the database and add the collection and the document
#### Collections
MongoDB stores documents in collections. Collections are analogous to tables in relational databases.
If a collection does not exist, MongoDB creates the collection when you first store data for that collection. However, you can use `db.createCollection()` with various options such as max doc size etc.
```mongodb
db.myNewCollection2.insertOne( { x: 1 } )
db.myNewCollection3.createIndex( { y: 1 } )
```

*Collections are assigned an immutable UUID. The collection UUID remains the same across all members of a replica set and shards in a sharded cluster.* Get UUID information using `db.getCollectionInfos()` 


#### Documents
MongoDB stores data records as BSON documents. BSON is a binary representation of [JSON](https://www.mongodb.com/docs/manual/reference/glossary/#std-term-JSON) documents, though it contains more data types than JSON.
- MongoDB documents are composed of field-and-value pairs
- The value of a field can be any of the BSON [data types](https://www.mongodb.com/docs/manual/reference/bson-types/#std-label-bson-types), including other documents, arrays, and arrays of documents.
- Field names can't be _id_ / null
- The maximum BSON document size is 16 megabytes.
- Unlike JavaScript objects, the fields in a BSON document are ordered.
- each document stored in a collection requires a unique `_id` field that acts as a [primary key](https://www.mongodb.com/docs/manual/reference/glossary/#std-term-primary-key). If an inserted document omits the `_id` field, the MongoDB driver automatically generates an [ObjectId](https://www.mongodb.com/docs/manual/reference/bson-types/#std-label-objectid) for the `_id` field.

*The MongoDB Query API is the mechanism that you use to interact with your data.* The 2 ways of doing so are: 
- CRUD Operations
- Aggregation Pipelines

### CRUD Operations

#### Create
Create or insert operations add new documents to a collection. If the collection does not currently exist, insert operations will create the collection. 
```js
db.collection.insertOne()
db.collection.insertMany()
```

All write operations in MongoDB are [atomic](https://www.mongodb.com/docs/manual/core/write-operations-atomicity/) on the level of a single document.

#### Read
Read operations retrieve documents  from a collection ; i.e. query a collection for documents. 
```js
db.collection.find()
```

Using the sql where:
```js
const cursor = db.collection('inventory').find({ status: 'D' });
```

##### Condition Specifications
- AND operation: 
```js
const cursor = db.collection('inventory').find({
  status: 'A',
  qty: { $lt: 30 }
});
```

- OR operation:
```js
const cursor = db.collection('inventory').find({
  $or: [{ status: 'A' }, { qty: { $lt: 30 } }]
});
```

[Read more about Operators](https://www.mongodb.com/docs/manual/reference/operator/query/)

#### Update
```js
Collection.updateOne() 
Collection.updateMany()
Collection.replaceOne()
```

Using updateOne: first doc where item = 'paper'
```js
await db.collection('inventory').updateOne(
  { item: 'paper' },
  {
    $set: { 'size.uom': 'cm', status: 'P' },
    $currentDate: { lastModified: true }
  }
);
```

#### Delete 
```js
Collection.deleteMany() 
Collection.deleteOne()
```

Using deleteOne
```js
await db.collection('inventory').deleteOne({ status: 'D' });
```


### Aggregation Operations/Pipeline
Aggregation operations process multiple documents and return computed results.

An aggregation pipeline consists of one or more [stages](https://www.mongodb.com/docs/manual/reference/operator/aggregation-pipeline/#std-label-aggregation-pipeline-operator-reference) that process documents:
- Each stage performs an operation on the input documents. For example, a stage can filter documents, group documents, and calculate values.
- The documents that are output from a stage are passed to the next stage.
- An aggregation pipeline can return results for groups of documents. For example, return the total, average, maximum, and minimum values.

Example aggregation:
```js
db.orders.aggregate( [
   {
      $match: { size: "medium" }
	},
   {
      $group: { _id: "$name", totalQuantity: { $sum: "$quantity" } }
   }
] )
```

[List of Aggregation Stages](https://www.mongodb.com/docs/manual/reference/operator/aggregation-pipeline/#std-label-aggregation-pipeline-operator-reference)

An aggregation pipeline consists of one or more [stages](https://www.mongodb.com/docs/manual/reference/operator/aggregation-pipeline/#std-label-aggregation-pipeline-operator-reference) that process documents:
- A stage does not have to output one document for every input document. For example, some stages may produce new documents or filter out documents.
- The same stage can appear multiple times in the pipeline with these stage exceptions: [`$out`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/out/#mongodb-pipeline-pipe.-out), [`$merge`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/merge/#mongodb-pipeline-pipe.-merge), and [`$geoNear`.](https://www.mongodb.com/docs/manual/reference/operator/aggregation/geoNear/#mongodb-pipeline-pipe.-geoNear)
- To calculate averages and perform other calculations in a stage, use [aggregation expressions](https://www.mongodb.com/docs/manual/reference/operator/aggregation/#std-label-aggregation-expressions) that specify [aggregation operators](https://www.mongodb.com/docs/manual/reference/operator/aggregation/#std-label-aggregation-expression-operators). 

##### Optimization Tips
- When you use a [`$project`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#mongodb-pipeline-pipe.-project) stage it should typically be the last stage in your pipeline, used to specify which fields to return to the client.
- When you have a sequence with [`$sort`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/sort/#mongodb-pipeline-pipe.-sort) followed by a [`$match`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/match/#mongodb-pipeline-pipe.-match), the [`$match`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/match/#mongodb-pipeline-pipe.-match) moves before the [`$sort`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/sort/#mongodb-pipeline-pipe.-sort) to minimize the number of objects to sort.
- [Read this](https://www.mongodb.com/docs/manual/core/aggregation-pipeline-optimization/)

### Atomicity and Transactions

#### Atomicity
In MongoDB, a write operation is atomic(write operation that either completes entirely or doesn't complete at all.) on the level of a single document, even if the operation modifies multiple embedded documents _within_ a single document.

When a single write operation (e.g. [`db.collection.updateMany()`](https://www.mongodb.com/docs/manual/reference/method/db.collection.updateMany/#mongodb-method-db.collection.updateMany)) modifies multiple documents, the modification of each document is atomic, but the operation as a whole is not atomic.

#### Concurrency Control
Concurrency control allows multiple applications to run concurrently without causing data inconsistency or conflicts. 

A [`findAndModify`](https://www.mongodb.com/docs/manual/reference/command/findAndModify/#mongodb-dbcommand-dbcmd.findAndModify) operation on a document is [atomic](https://www.mongodb.com/docs/manual/reference/glossary/#std-term-atomic-operation): if the find condition matches a document, the update is performed on that document. Concurrent queries and additional updates on that document are not affected until the current update is complete.

### Indexes
Indexes support efficient execution of queries in MongoDB. Without indexes, MongoDB must scan every document in a collection to return query results. 
Although indexes improve query performance, adding an index has negative performance impact for write operations. *For collections with a high write-to-read ratio, indexes are expensive because each insert must also update any indexes.*

If your application is repeatedly running queries on the same fields, you can create an index on those fields to improve performance.
MongoDB indexes use a [B-tree](https://en.wikipedia.org/wiki/B-tree) data structure.

### Data Modelling
Data modeling refers to the organization of data within a database and the links between related entities. Data in MongoDB has a **flexible schema model**, which means:

- [Documents](https://www.mongodb.com/docs/manual/reference/glossary/#std-term-document) within a single [collection](https://www.mongodb.com/docs/manual/reference/glossary/#std-term-collection) are not required to have the same set of fields.
- A field's data type can differ between documents within a collection.

##### Linking Data
While we use normalization in sql and then use joins, here:
To link related data, you can either:
- Embed related data within a single document.
- Store related data in a separate collection and access it with a [reference.](https://www.mongodb.com/docs/manual/data-modeling/#std-label-data-modeling-reference)

### Sharding
[Sharding](https://www.mongodb.com/docs/manual/reference/glossary/#std-term-sharding) is a method for distributing data across multiple machines. MongoDB uses sharding to support deployments with very large data sets and high throughput operations."

There are two methods for addressing system growth: vertical and horizontal scaling.

#### Vertical Scaling
_Vertical Scaling_ involves increasing the capacity of a single server, such as using a more powerful CPU, adding more RAM, or increasing the amount of storage space. Limitations in technology and the hard ceilings of cloud providers show the practical maximum of vertical scaling

#### Horizontal Scaling
_Horizontal Scaling_ involves dividing the system dataset and load over multiple servers, adding additional servers to increase capacity as required. While the overall speed or capacity of a single machine may not be high, each machine handles a subset of the overall workload, potentially providing better efficiency than a single high-speed high-capacity server

TBR Later



