---
Interview graded: true
Last edited time: 2023-07-25T15:21
Needs Rework: false
Status: Not started
Topic:
  - "[[NoSql]]"
---
Document oriented database, using BSON(Binary JSON), well known for data consistency

Build up on CAP concept, which is for disturbed database systems

![[Untitled 45.png|Untitled 45.png]]

Advantages

- Data structure is flexible - easy to add, change, delete
- Entries for an artifact repository can differ

Disadvantages

- Does not automatically catch referential integrity errors
- Application responsible for managing data integrity

![[Untitled 1 10.png|Untitled 1 10.png]]

  

DOCUMENT  
The basic unit of data in MongoDB, may have different structure  

COLLECTION  
A grouping of documents  

DATABAS

Container for collections

Datatypes

- All JSON data types
- Dates
- Numbers
- Object ID's - Every document requires an _id field, which acts as a primary key. If an inserted document doesn't include the _id field, MongoDB automatically generates an ObjectId for the _id field.

  

### Flexible schema

Polymorphic documents, optional schema validation

You can control the shape and contents of documents in a collection by defining a [schema](https://www.mongodb.com/docs/atlas/app-services/schemas/#std-label-schemas). Schemas let you require specific fields, control the type of a field's value, and validate changes before committing write operations.

A root-level collection schema can contain additional schemas that describe the type's properties. Each root-level schema is an `object` schema that has the following form:

```Bash
{
  "bsonType": "object",
  "title": "<Type Name>",
  "required": ["<Required Field Name>", ...],
  "properties": {
    "<Field Name>": <Schema>
  }
}
```

### Connection string

```JavaScript
mongodb+srv://<username>:<password>@clusterO.usqsf.mongodb.net:27017/ 
?retryWrites=true&w=majority
```

ACID Transactions from version 4

  

### Query Language

```JavaScript
//create collection
db.createCollection('customers");

//see inside the collection 
db.customers.find();

// insert a new document into the customers collection
//(inserting a row into a table)
db.customers.insertOne({
id:'qwe',
FirstName: 'Amata'
});

//read statement, select, find statement 
db.customers.find({FirstName: 'Amata'});

//update statement
db.customers.updateMany({City:'Philadelphia'},{$set:{City:'Miami'})

//delete statement 
db.customers.remove({FirstName: 'Amata'})
```

### MongoDb Shell

The MongoDB Shell uses a Node REPL environment. This means that we are able to use JavaScript variable declaration, function declaration, and loops.

```JavaScript

show dbs //list all dbs
use dbName //switch to db
```

### CRUD

If collection in insert query doesn’t exist, it would be created

Insert

```JavaScript
db.users.insertOne({"Name":"TestUser", "password":"123"})
db.users.insertMany([
	{"Name":"TestUser1", "password":"123"},
	{"Name":"TestUser2", "password":"123"}
])
```

**Find**

```JavaScript
db.users.find()
db.users.find({city: {$eq: "KAU"}})
db.users.find({city: {$in: ["SPB", "MSK","KAU"]}})
db.users.find({age: {$gt: 50}})
db.users.find({age: {$gte: 50}})
db.users.find({age: {$lt: 50}})
db.users.find({age: {$lte: 50}})
```

Projection

```JavaScript
db.users.find({age: {$lte: 50}},{name: 1}) //projection, include only name
db.users.find({age: {$lte: 50}},{name: 0}) //projection, exclude name
```

**Find** In Arrays

```JavaScript
db.users.find({topics: {$elemMatch: {$eq: "Java" }}}) //returns only arrays with this value
db.users.find({topics: {$eq: "Java" }}) //returns all matches
```

```JavaScript
db.routes.find({
	$and: [{ "airline": "Southwest Airlines" }, { "stops": { $gte: "1"}}]
})
db.routes.find({
	$or: [{ "airline": "Southwest Airlines" }, { "stops": { $gte: "1"}}]
})
```

**Replace**

```JavaScript
db.books.replaceOne(
	{_id: ObjectId("62c5e671541e2c6bcb528308")},
	{//all data excluding  _id, or existing fields are deleted}
)
```

**UpdateOne**

```JavaScript
db.podcasts.updateOne(
	{_id: ObjectId("62c5e671541e2c6bcb528308")},
	{$set: { subscribers: 98562 }}
)
```

**Upsert**

```JavaScript
db.podcasts.updateOne(
	{_id: ObjectId("62c5e671541e2c6bcb528308")},
	{$set: { subscribers: 98562 }},
	{upsert: true }
)
```

**Push**

Add to array

```JavaScript
db.podcasts.updateOne(
	{_id: ObjectId("62c5e671541e2c6bcb528308")},
	{$push: { hosts: "Some guy" }}
)
```

**Find and Modify**

Guarantees that correct version would be returned without other thread changing it

```JavaScript
db.podcasts.findAndModify({
query: { _id: Objectid("626736114612017812200de")},
update: {$inc: {downloads: 1}},
new: true //return updated document when true, old version when false
})
```

**UpdateMany**

In case of failure will not rollback, so only some documents might be updated

```JavaScript
db.podcasts.updateMany(
	{year: 2022},
	{$set: { subscribers: 98562 }}
)
```

**DeleteOne**

```JavaScript
db.podcasts.deleteOne(
	{_id: ObjectId("62c5e671541e2c6bcb528308")}
)
```

**DeleteMany**

```JavaScript
db.podcasts.deleteMany({ category: "crime" ))
```

**Count**

```JavaScript
db.trips.countDocuments()
db.trips.countDocuments({tripduration:{$gt:120}})
```

### Cursor

Pointer to the result set of a query

```JavaScript
db.companies.find({category_code:"music"}).sort({name:1})// 1 - asc, -1 - desc
db.companies.find({category_code:"music"}).limit(3)// show only 3 items
```

### Aggregation

An analysis and summary of data

**Aggregation Pipeline  
  
**A series of stages completed one at a time, in order

- Filtered
- Sorted
- Grouped
- Transformed

## **Stage**

An aggregation operation performed on the data

### **$match**

Filters for data that matches criteria, Reduces the number of documents. Place as early as possible in the pipeline so it can use indexes, so Lessens processing required.

```JavaScript
db.zips.aggregate([{
	$match:{"state":"CA"}
}])
```

### **$group**

Groups documents by a group key. Output is one document for each unique value of the group key

```JavaScript
db.zips.aggregate([
{
	$match: {"state":"CA"}
},
{
	$group: {
		_id: "$city",
		totalZips: {$count: {}}
	}
}
])

//result
[
	{_id: 'ANNAPOLIS', totalZips: 1 },
	...
]
```

### $sort

Sorts all input documents and passes them through pipeline in sorted order

```JavaScript
db.zips.aggregate([{
	$sort: {
		population: -1 //desc
	}
}])
```

### $limit

Limits the number of documents that are passed on to the next aggregation stage

```JavaScript
db.zips.aggregate([{
	$limit: 3
}])
```

### $project

Determines output shape. Projection similar to find0 operations. Should be the last stage to format the output

```JavaScript
db.zips.aggregate([{
	$project: {
		_id: 0,
		state: 1,
		zip: 0,
		pop: "$population"
	}
}])
```

### $set

Adds or modifies fields in the pipeline. Useful when we want to change existing fields in pipeline or add new ones to be used in upcoming pipeline stages. $set is used to create or change values of new or existing fields, $project can be used to create or change fields, but it also could specify which fields to show.

```JavaScript
db.zips.aggregate([{
	$set: {
		pop_2022: {$round: {$multiply: [1.0031, "$pop"]}}
	}
}])
```

### $count

```JavaScript
db.zips.aggregate([{
	$count: "total_zips"
}])
```

### $skip

Skips over the specified number of [documents](https://www.mongodb.com/docs/manual/reference/glossary/#std-term-document) that pass into the stage and passes the remaining documents to the next stage in the [pipeline.](https://www.mongodb.com/docs/manual/reference/glossary/#std-term-pipeline)

```JavaScript
db.zips.aggregate([
{ $skip: 5 }
])
```

### $redact

Restricts entire documents or content within documents from being outputted based on information stored in the documents themselves.

|   |   |
|---|---|
|**System Variable**|**Description**|
|$$DESCEND|[`$redact`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/redact/#mongodb-pipeline-pipe.-redact) returns the fields at the current document level, excluding embedded documents. To include embedded documents and embedded documents within arrays, apply the [`$cond`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/cond/#mongodb-expression-exp.-cond) expression to the embedded documents to determine access for these embedded documents.|
|$$PRUNE|[`$redact`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/redact/#mongodb-pipeline-pipe.-redact) excludes all fields at this current document/embedded document level, **without** further inspection of any of the excluded fields. This applies even if the excluded field contains embedded documents that may have different access levels.|
|$$KEEP|[`$redact`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/redact/#mongodb-pipeline-pipe.-redact) returns or keeps all fields at this current document/embedded document level, **without** further inspection of the fields at this level. This applies even if the included field contains embedded documents that may have different access levels.|

```JavaScript
db.forecasts.aggregate(
   [
     { $match: { year: 2014 } },
     { $redact: {
        $cond: {
           if: { $gt: [ { $size: { $setIntersection: [ "$tags", userAccess ] } }, 0 ] },
           then: "$$DESCEND",
           else: "$$PRUNE"
         }
       }
     }
   ]
);
```

### **$replaceWith**

Replaces the input document with the specified document. The operation replaces all existing fields in the input document, including the `_id` field. With [`$replaceWith`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/replaceWith/#mongodb-pipeline-pipe.-replaceWith), you can promote an embedded document to the top-level. You can also specify a new document as the replacement.

The [`$replaceWith`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/replaceWith/#mongodb-pipeline-pipe.-replaceWith) stage performs the same action as the [`$replaceRoot`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/replaceRoot/#mongodb-pipeline-pipe.-replaceRoot) stage, but the stages have different forms.

```JavaScript
db.collection.aggregate([
   { $replaceWith: { $mergeObjects: [ { _id: "$_id", first: "", last: "" }, "$name" ] } }
])
```

### $out

Writes the documents that are returned by an aggregation pipeline into a collection. Must be the last stage. Creates a new collection if it does not already exist. If collection exists, $out replaces the existing collection with new data(existing data erased, then insert result of pipeline)

```JavaScript
db.zips.aggregate([{
	$out: "small_states" //writes to collection small_states, also could specify db
}])
```

### $lookup

Performs a left outer join to a collection in the _same_ database to filter in documents from the "joined" collection for processing. The [`$lookup`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/lookup/#mongodb-pipeline-pipe.-lookup) stage adds a new array field to each input document. The new array field contains the matching documents from the "joined" collection. The [`$lookup`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/lookup/#mongodb-pipeline-pipe.-lookup)stage passes these reshaped documents to the next stage.

```JavaScript
{
   $lookup:
     {
       from: <collection to join>,
       localField: <field from the input documents>,
       foreignField: <field from the documents of the "from" collection>,
       as: <output array field>
     }
}

//The operation would correspond to the following pseudo-SQL statement:
SELECT *, <output array field>
FROM collection
WHERE <output array field> IN (
   SELECT *
   FROM <collection to join>
   WHERE <foreignField> = <collection.localField>
);
```

## Indexes

Special data structures, that store small portion of the data in ordered and easy to search efficiently way

![[Untitled 2 7.png|Untitled 2 7.png]]

**Indexes improve query performance:**

- Speed up queries
- Reduce disk I/0
- Reduce resources required
- Support equality matches and range-based operations and return sorted results

**With indexes**  
MongoDB only fetches the documents identified by the index based on the query  

**Without indexes**  
MongoDB reads all documents (collection scan)  
Sorts results in memory  

- There is one default index per collection, which includes only the _id field.
- Every query should use an index.
- If we insert or update documents, we need to update the index data structure
- Index could be deleted or hided
    
    ```JavaScript
    db.customers.dropIndexes()// deletes all except _id
    db.customers.dropIndex({active:1,birthdate:-1, name: 1 })
    db.customers.hideIndex('active_1_birthdate_-1_name_1')
    ```
    

**Most common index types:**

- Single field
- Compound
- Multikey indexes operate on an array field

### Single Field index

```JavaScript
db.customers.createIndex({birthdate:1))
db.customers.createIndex({email:1), (unique: true)) // prevents duplication for emaildb.customers.getIndexes() 
//returns query execution plan
db.customers.explain().find({birthdate: {Sgt:ISODate(*1995-08-01")}})
//IXSCAN - if index is used
//COLSCAN - if not
```

### MultiKey Index

Index on an array field. Limitation - One array field per index

```JavaScript
db.customers.createIndex({accounts:1))
```

### Compound index

Index on several fields. The order of the fields in a compound the index matters, Follow this order: Equality, Sort, Range. The sort order of the field values in the index matters

```JavaScript
db.customers.createIndex({
active: 1, //active is query prefix, used for equality
birthdate: -1, // used for search desc
name: 1}) // used for search asc
//can use index because of activ
db.customers.find({active:true}).sort({birthdate:-1})
db.customers.find({birthdate: {$lt: ISODate ("1995-08-01")], active: true})
//can not use index, because other fields are not prefix
db.customers.find({birthdate: {$lt: ISODate ("1995-08-01")}})
db.customers.find({}).sort ({birthdate: 1})
```

**Equality**

- Test exact matches on single field
- Should be placed first in a compound index
- Reduces query processing time
- Retrieves fewer documents

**Sort**

- Determines the order of results
- Index sort eliminates the need for in-memory sorts
- Sort order is important if query results are sorted by more than 1 field and they mix sort orders

## Data Modeling

What your data looks like  
The relationships among the data  
The tooling you plan to have  
The access patterns that might emerge  

Data that is accessed together should be stored together.

### One-to-one

A relationship where a data entity in one set is connected to exactly one data entity in another set

![[Untitled 3 7.png|Untitled 3 7.png]]

### One-to-Many

A relationship where a data entity in one set is connected to any number of data entities in another set

![[Untitled 4 5.png|Untitled 4 5.png]]

### Many-to-Many

A relationship where any number of data entities in one set are connected to any number of data entities in another set

**Embedding**  
We take related data and insert it into our document  

Store related data in a single document  
Simplify queries and improves overall query performance  
Ideal for one-to-many and many-to-many relationships among data  
Has pitfalls like large documents and unbounded documents  

![[Untitled 5 5.png|Untitled 5 5.png]]

**Referencing**  
We refer to documents in another collection in our document  

References:  
Save the id field of one document in another document as a link between the two  
Simple and sufficient for most use cases  

No duplication of data  
Smaller documents  
Querying from multiple documents costs extra resources and impacts read performance  

![[Untitled 6 5.png|Untitled 6 5.png]]

## ACID

An ACID transaction is a group of database operations that must happen together or not at all, ensuring database safety and consistency

### Single-document

All single document transactions are atomic

```JavaScript
//this transaction is atomic, nether all fields will be updated, nor none of them
db.products. updateOne ( 
{ _id: 100 },
{	$set: {
		quantity: 500,
		details: {model: "2600" make: "Fashionaires"},
		tags: ["coats","outerwear",	"clothing"]
}}
```

### Multi-document

- ACID Transaction
- MongoDB "locks" resources involved in a transaction
- Incurs performance cost and affects latency
- Use multi-document transactions as a precise tool
- A transaction has a maximum runtime of less than one minute after the first write

**Session**  
Used to group database operations that are related to each other and should be run together  

```JavaScript
const session = db.getMongo().startSession();
session.startTransaction()
const account = session. getDatabase('bank") - getCollection('accounts")
account.updateone( {account_id: "MD8740836066"}, {$inc: {balance: -30 }})
account.updateone( {account_id: "MD4560836123"}, {$inc: {balance: 30 }})
session.commitTransaction()
session.abortTransaction()
```