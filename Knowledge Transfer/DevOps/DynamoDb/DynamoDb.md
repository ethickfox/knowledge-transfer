---
Interview graded: false
Last edited time: 2023-08-21T09:23
Needs Rework: false
Status: Not started
Topic:
  - "[[AWS]]"
---
## **Tables, Items and Attributes**

support for ACID transactions

DynamoDB is serverless. You don't have to provision, patch, or manage servers, and you don't have to install, maintain, or operate software. DynamoDB automatically scales tables up and down to adjust for capacity and maintain performance.

![[/Untitled 98.png|Untitled 98.png]]

The primary key is used to uniquely identify each item in an Amazon DynamoDB table. A primary key is of two types:

1. **Simple Primary Key**: This is composed of one attribute called the Partition Key. If we wanted to store a customer record, then we could have used `customerID` or `email` as a partition key to uniquely identify the customer in the DynamoDB table.
2. **Composite Primary Key**: This is composed of two attributes - a partition key and a sort keys. In our example above, each order is uniquely identified by a composite primary key with `customerID` as the partition key and `orderID` as the sort key.

When we write an item to the table, DynamoDB uses the value of the partition key as input to an internal hash function. The output of the hash function determines the partition in which the item will be stored.

  

We can use a secondary index to query the data in the table using an alternate key, in addition to queries against the primary key. Secondary Indexes are of two types:

- **Global Secondary Index (GSI)**: An index with a partition key and sort key that are different from the partition key and sort key of the table.
- **Local Secondary Index (LSI)**: An index that has the same partition key as the table, but a different sort key.

  

DynamoDB is a web service, and interactions with it are stateless. So we can interact with DynamoDB via REST API calls over HTTP(S). Unlike connection protocols like JDBC, applications do not need to maintain a persistent network connections.

  

![[/Untitled 1 27.png|Untitled 1 27.png]]

![[/Untitled 2 19.png|Untitled 2 19.png]]

Data types

- Scalar
    - Numbers
    - Strings
    - Binary
    - Boolean.
- Collections
    - String Set Number Set
    - Heterogeneous Binary Set
    - Differentiated Map
    - DynamoDB accepts Null values.

Operation types

- GET/PUT operations that use the user-defined unique identifier are supported.
- By enabling querying of a non-primary key characteristic using both local and global secondary indexes, it offers flexible querying.
- Data for an item linked to a single attribute separation primary key can be read and written quickly.
- It enables you to access all the items for a single aggregate partition-sort key along a number of sort keys using the Query API.

"global Secondary index" refers to an index with a partition and divided key that differs from those on the table. Per table, a maximum of 5 global associated indexes are permitted.

• Local secondary index: An index with a different sort key than the table's partition key. Since every index partition is bound to a table partition with the same partition key, it is regarded as being "local."There can be a maximum of five local associated indexes per table.Currently, once local secondary indexes are created, Amazon DynamoDB is unable to remove them from the table; however, the entire table can be deleted.

**Projection**

Represents attributes that are copied (projected) from the table into an index. These are in addition to the primary key attributes and index key attributes, which are automatically projected.

The set of attributes that are projected into the index:

- `KEYS_ONLY` - Only the index and primary keys are projected into the index.
- `INCLUDE` - In addition to the attributes described in `KEYS_ONLY`, the secondary index will include other non-key attributes that you specify.
- `ALL` - All of the table attributes are projected into the index.

### **What sort of query functionality is supported by DynamoDB?**

It supports GET/PUT operations using the user-specified primary key. The ability to query a non-primary key attribute using both local and global secondary indexes promotes flexible querying.

**DynamoDB Atomic**

Atomic counters are a feature of DynamoDB that let you change the value of an established attribute without affecting other write requests by using the update method. It increases the value of this characteristic by one each time the programme is executed.

Amazon DynamoDB Accelerator (DAX) is a fully managed, highly available caching service built for [Amazon DynamoDB](https://aws.amazon.com/dynamodb/). DAX delivers up to a 10 times performance improvement—from milliseconds to microseconds—even at millions of requests per second.

DAX does all the heavy lifting required to add in-memory acceleration to your DynamoDB tables, without requiring developers to manage cache invalidation, data population, or cluster management.

DynamoDB's provisioned throughput feature enables users to specify the read and write capacity needs for their table. The user can then make sure that one‘s table can accommodate the volume of traffic they anticipate.