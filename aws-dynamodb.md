# Amazon DynamoDB


<!-- vim-markdown-toc GFM -->

* [Limits](#limits)
  * [API Limits](#api-limits)
* [Basics](#basics)
  * [Working with Items](#working-with-items)
  * [Query](#query)
  * [Scan](#scan)
  * [Error Retries and Exponential Backoff](#error-retries-and-exponential-backoff)
* [Advanced](#advanced)
  * [Using Secondary Indexes](#using-secondary-indexes)
    * [Local Secondary Index](#local-secondary-index)
  * [Streams](#streams)
* [Capacity Planning](#capacity-planning)
  * [Provisioned](#provisioned)
  * [Burst Capacity](#burst-capacity)
  * [Adaptive Capacity](#adaptive-capacity)
* [Metrics](#metrics)
* [Design](#design)
* [Glossary](#glossary)
* [Resources](#resources)

<!-- vim-markdown-toc -->

## Limits

See [2] for details but for most regions including EU (Frankfurt) and EU (Ireland)

*Read Capacity*
> One read capacity unit = one strongly consistent read per second, or two eventually consistent reads per second, for items up to 4 KB in size.
> Transactional read requests require two read capacity units to perform one read per second for items up to 4 KB.

*Single Partition Key Value access*
If your access pattern  exceeds 3000 RCU or 1000 WCU for a single partition key value, your requests might be throttled with a `ProvisionedThroughputExceededException` error. See https://aws.amazon.com/blogs/database/choosing-the-right-dynamodb-partition-key/

*Write capacity*
> One write capacity unit = one write per second, for items up to 1 KB in size.
> Transactional write requests require two write capacity units to perform one write per second for items up to 1 KB.

*Capacity Increase*
You can increase Read Capacityor Write Capacity as often as necessary. But you are not permitted to increase /to rapidly/. "rapidly" is not better specified in [2].

*Capacity Decrease*
A decrease is allowed up to four times any time per day.  A day is defined according to the GMT time zone. Additionally, if there was no decrease in the past hour, an additional decrease is allowed

*Throughput*
> Per table – 40,000 read capacity units and 40,000 write capacity units
> Per account – 80,000 read capacity units and 80,000 write capacity units

*Table Size*
There is no practical limit on a table's size. Tables are unconstrained in terms of the number of items or the number of bytes.

*Table Count*
Initial limit of 256 but you can ask support to increase it

*Table Names*
Must be at least 3 characters long, but no greater than 255 characters long. Allowed characters are: `A-Z a-z 0-9 _ - .`

*Attribute Names*
Must be at least 1 character long, but no greater than 64 KB long. The exceptions are secondary index partition and index sort keys, these must be encoded using UTF-8, and the total size of each name (after encoding) cannot exceed 255 bytes.

*Item Size*
Maximum item size in DynamoDB is 400 KB, which includes both attribute name binary length (UTF-8 length) and attribute value lengths (again binary length). The attribute name counts towards the size limit.

*Adapative Capacity and Throttling*
Throttling can occur if a single partition receives more than 3,000 RCUs or 1,000 WCUs. From [Understanding DynamoDB Adaptive Capacity](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-partition-key-design.html#bp-partition-key-partitions-adaptive)

*Burst Capacity Throttling*
DynamoDB currently retains up to five minutes (300 seconds) of unused read and write capacity.  From (Using Burst Capacity Effectively)[https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-partition-key-design.html#bp-partition-key-throughput-bursting]

*Index Count*
Each table in DynamoDB has a limit of 20 global secondary indexes (default limit) and 5 local secondary indexes per table.

### API Limits

*CreateTable/UpdateTable/DeleteTable*
In general, you can have up to 50 `CreateTable`, `UpdateTable`, and `DeleteTable` requests running simultaneously.

*BatchGetItem*
Single `BatchGetItem` operation can retrieve a maximum of 100 items. The total size of all the items retrieved cannot exceed 16 MB.

`BatchWriteItem`
Single BatchWriteItem operation can contain up to 25 PutItem or DeleteItem requests. The total size of all the items written cannot exceed 16 MB.

*Query*
Result set from a Query is limited to 1 MB per call. You can use the `LastEvaluatedKey` from the query response to retrieve more results. This limit applies before the filter expression is evaluated.

*Scan*
The result set from a Scan is limited to 1 MB per call. You can use the `LastEvaluatedKey` from the scan response to retrieve more results.

## Basics

Dynamo DB is a schemaless NoSQL solution. Each item has one ore more attributes. Each item has one attribute that uniquely identifies the item. The Primary Key is made of up to two parts. The Partition Key and optional Sort Key. Dynamo Db applies a hash function on the partition key to determine the target partition for the data distribution across multiple servers.

You will soon realize that querying data with DynamoDB is hard. You don't have joins, views, `distinct` and other things you might be used to from the relational world. The tools at your disposal are denormalization and secondary indexes.

### Working with Items

DynamoDB provides four operations for basic create/read/update/delete (CRUD) functionality:
* `PutItem` – create an item.
* `GetItem` – read an item.
* `UpdateItem` – update an item.
* `DeleteItem` – delete an item.

In addition to the four basic CRUD operations, DynamoDB also provides the following:
* `BatchGetItem` – read up to 100 items from one or more tables.
* `BatchWriteItem` – create or delete up to 25 items in one or more tables.

### Query
The Query operation finds items based on primary key values. You can query any table or secondary index that has a composite primary key (a partition key and a sort key).

You can optionally provide a filter expression. A filter expression determines which items within the Query results should be returned to you. All of the other results are discarded.

DynamoDB paginates the results from Query operations. A single Query will only return a result set that fits within the 1 MB size limit. To determine whether there are more results, determine if the result contains a `LastEvaluatedKey` element. If so execute new Query and take the `LastEvaluatedKey` value from step 1 and use it as the `ExclusiveStartKey` parameter in the new Query request.

In addition to the items that match your criteria, the Query response contains the following elements:

* *ScannedCount* — the number of items that matched the key condition expression, before a filter expression (if present) was applied.
* *Count* — the number of items that remain, after a filter expression (if present) was applied.

You can specify the `ReturnConsumedCapacity` parameter in a Query request to obtain this information. The following are the valid settings for `ReturnConsumedCapacity`:

* `NONE` — no consumed capacity data is returned. (This is the default.)
* `TOTAL` — the response includes the aggregate number of read capacity units consumed.
* `INDEXES` — the response shows the aggregate number of read capacity units consumed, together with the consumed capacity for each table and index that was accessed.

### Scan
A Scan operation reads every item in a table or a secondary index. By default, a Scan operation returns all of the data attributes for every item in the table or index. You can use the `ProjectionExpression` parameter so that Scan only returns some of the attributes, rather than all of them.

### Error Retries and Exponential Backoff
The SDKs have built in retries with exponential backoff.

## Advanced

### Using Secondary Indexes

See https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/LSI.html#LSI.ThroughputConsiderations

#### Local Secondary Index

> For index queries that read attributes that are not projected into the local secondary index, DynamoDB will need to fetch those attributes from the base table, in addition to reading the projected attributes from the index. These fetches occur when you include any non-projected attributes in the Select or ProjectionExpression parameters of the Query operation. Fetching causes additional latency in query responses, and it also incurs a higher provisioned throughput cost: In addition to the reads from the local secondary index described above, you are charged for read capacity units for every base table item fetched. This charge is for reading each entire item from the table, not just the requested attributes.

### Streams
A DynamoDB stream is an ordered flow of information about changes to items in an Amazon DynamoDB table. When you enable a stream on a table, DynamoDB captures information about every modification to data items in the table.

DynamoDB Streams guarantees the following:
* Each stream record appears exactly once in the stream.
* For each item that is modified in a DynamoDB table, the stream records appear in the same sequence as the actual modifications to the item.

If you perform a `PutItem` or `UpdateItem` operation that does not change any data in an item, then DynamoDB Streams will not write a stream record for that operation.

You van choose the structure of the stream records:
* Keys only—only the key attributes of the modified item.
* New image—the entire item, as it appears after it was modified.
* Old image—the entire item, as it appeared before it was modified.
* New and old images—both the new and the old images of the item.

Stream records are organized into groups, or shards. Each shard acts as a container for multiple stream records, and contains information required for accessing and iterating through these records. The stream records within a shard are removed automatically after 24 hours.

This especially means that all data in DynamoDB Streams is subject to a 24 hour lifetime.

## Capacity Planning

*Provisioned*. Select provisioned to save on throughput costs if you can reliably estimate your application’s throughput requirements.
*On-demand* Amazon does capacity planning for you

### Provisioned

If you choose provisioned mode, you specify the number of reads and writes per second that you require for your application.

When a request is throttled, it fails with an HTTP 400 code (`Bad Request`) and a `ProvisionedThroughputExceededException`. Your application has to take care of that. The AWS SDKs have built-in support for retrying throttled requests, so you do not need to write this logic yourself.

### Burst Capacity
Whenever you are not fully using a partition's throughput, DynamoDB reserves a portion of that unused capacity for later bursts of throughput to handle usage spikes. [3]

DynamoDB currently retains up to five minutes (300 seconds) of unused read and write capacity. During an occasional burst of read or write activity, these extra capacity units can be consumed quickly—even faster than the per-second provisioned throughput capacity that you've defined for your table. DynamoDB can also consume burst capacity for background maintenance and other tasks without prior notice.

### Adaptive Capacity
Throughput is evenly distributed across all partitions. But sometimes certain partitions are "hot", more active than others. To better accommodate uneven access patterns, adaptive capacity works by automatically increasing throughput capacity for partitions that receive more traffic.

There is typically a 5-minute to 30-minute interval between the time throttling of a hot partition begins and the time that adaptive capacity activates.

## Metrics

DynamoDB console displays Amazon CloudWatch metrics for your tables, so you can monitor throttled read requests and write requests. If you encounter excessive throttling, you should consider increasing your table's provisioned throughput settings.

## Design

For DynamoDB you shouldn't start designing your schema until you know the questions it will need to answer. Understanding the business problems and the application use cases up front is essential.

You should maintain as few tables as possible in a DynamoDB application. Most well designed applications require only one table.

> The first step in designing your DynamoDB application is to identify the specific query patterns that the system must satisfy.

*In particular, it is important to understand three fundamental properties of your application's access patterns before you begin:*

* *Data size*: Knowing how much data will be stored and requested at one time will help determine the most effective way to partition the data.
* *Data shape*: Instead of reshaping data when a query is processed (as an RDBMS system does), a NoSQL database organizes data so that its shape in the database corresponds with what will be queried. This is a key factor in increasing speed and scalability.
* *Data velocity*: DynamoDB scales by increasing the number of physical partitions that are available to process queries, and by efficiently distributing data across those partitions. Knowing in advance what the peak query loads might be helps determine how to partition data to best use I/O capacity.

After you identify specific query requirements, you can organize data according to general principles that govern performance:

* *Keep related data together*.   Research on routing-table optimization 20 years ago found that "locality of reference" was the single most important factor in speeding up response time: keeping related data together in one place. This is equally true in NoSQL systems today, where keeping related data in close proximity has a major impact on cost and performance. Instead of distributing related data items across multiple tables, you should keep related items in your NoSQL system as close together as possible. As a general rule, you should maintain as few tables as possible in a DynamoDB application. As emphasized earlier, most well designed applications require only one table, unless there is a specific reason for using multiple tables. Exceptions are cases where high-volume time series data are involved, or datasets that have very different access patterns—but these are exceptions. A single table with inverted indexes can usually enable simple queries to create and retrieve the complex hierarchical data structures required by your application.
* *Use sort order*. Related items can be grouped together and queried efficiently if their key design causes them to sort together. This is an important NoSQL design strategy.
* *Distribute queries*. It is also important that a high volume of queries not be focused on one part of the database, where they can exceed I/O capacity. Instead, you should design data keys to distribute traffic evenly across partitions as much as possible, avoiding "hot spots."
* *Use global secondary indexes*. By creating specific global secondary indexes, you can enable different queries than your main table can support, and that are still fast and relatively inexpensive.

## Glossary

*Document Types*
A document type can represent a complex structure with nested attributes—such as you would find in a JSON document. The document types are list and map.

*Eventually Consistent Reads* When you read data from a DynamoDB table, the response might not reflect the results of a recently completed write operation. The response might include some stale data. If you repeat your read request after a short time, the response should return the latest data.

*Eventually Consistent Reads* When you request a strongly consistent read, DynamoDB returns a response with the most up-to-date data, reflecting the updates from all prior write operations that were successful. A strongly consistent read might not be available if there is a network delay or outage.

*Global Secondary Index*
An index with a partition key and a sort key that can be different from those on the base table. A global secondary index is considered "global" because queries on the index can span all of the data in the base table, across all partitions.

*Local secondary index* — an index that as the same partition key as the base table, but a different sort key. A local secondary index is "local" in the sense that every partition of a local secondary index is scoped to a base table partition that has the same partition key value.

*Item*
An item is a collection of attributes. Each attribute has a name and a value. An attribute value can be a scalar, a set, or a document type.

*Partition Key*
DynamoDB uses the partition key value as input to an internal hash function. The output from the hash function determines the partition (physical storage internal to DynamoDB) in which the item will be stored. All items with the same partition key value are stored together, in sorted order by sort key value.

*Primary Key*
The primary key uniquely identifies each item in the table, so that no two items can have the same key.  DynamoDB supports two different kinds of primary keys:
1. Partition Key. A simple primary key, composed of one attribute known as the partition key.
2. Partition key and sort key – Referred to as a composite primary key, this type of key is composed of two attributes. The first attribute is the partition key, and the second attribute is the sort key.

*Provisioned throughput*: Is the maximum amount of capacity that an application can consume from a table or index. If your application exceeds your provisioned throughput capacity on a table or index, it is subject to request throttling.

*Read Capacity Unit*: one strongly consistent read per second, or two eventually consistent reads per second, for items up to 4 KB in size. If item that larger than 4 KB, DynamoDB will consume additional read capacity units.

*Scalar Types*
A scalar type can represent exactly one value. The scalar types are number, string, binary, Boolean, and null.

*Set Types*
A set type can represent multiple scalar values. The set types are string set, number set, and binary set. All of the elements within a set must be of the same type.

*Write Capacity Unit*: one write per second for items up to 1 KB in size. If item larger than 1 KB, DynamoDB will consume additional write capacity units.

*Secondary Index*
You can create one or more secondary indexes on a table. A secondary index lets you query the data in the table using an alternate key, in addition to queries against the primary key. DynamoDB supports two kinds of indexes:
1. Global secondary index – An index with a partition key and sort key that can be different from those on the table.
2. Local secondary index – An index that has the same partition key as the table, but a different sort key.

## Resources

[1]: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ReadWriteCapacityMode.html
[2]: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Limits.html
[3]: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-partition-key-design.html#bp-partition-key-partitions-adaptive
