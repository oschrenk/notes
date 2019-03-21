# Amazon DynamoDB

## Limits

See [2] for details but for most regions including EU (Frankfurt) and EU (Ireland)

*Read Capacity*
> One read capacity unit = one strongly consistent read per second, or two eventually consistent reads per second, for items up to 4 KB in size.
> Transactional read requests require two read capacity units to perform one read per second for items up to 4 KB.

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

### API Limits

*CreateTable/UpdateTable/DeleteTable*
In general, you can have up to 50 `CreateTable`, `UpdateTable`, and `DeleteTable` requests running simultaneously.

*BatchGetItem*
Single `BatchGetItem` operation can retrieve a maximum of 100 items. The total size of all the items retrieved cannot exceed 16 MB.

`BatchWriteItem`
Single BatchWriteItem operation can contain up to 25 PutItem or DeleteItem requests. The total size of all the items written cannot exceed 16 MB.

*Query*
Result set from a Query is limited to 1 MB per call. You can use the `LastEvaluatedKey` from the query response to retrieve more results.

*Scan*
The result set from a Scan is limited to 1 MB per call. You can use the `LastEvaluatedKey` from the scan response to retrieve more results.


## Concepts

### Get/Put
### Query
### Scan

### Partitioning

If you anticipate your table scaling beyond a single partition, you should architect your application so that it can use more of the table's full provisioned throughput.

### Primary
Partition Key + Sort Key

#### Partition Key
The partition key portion of a table's primary key determines the logical partitions in which a table's data is stored. You should strive for good uniformity.

### Transactions
### Streams

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

### Error Retries and Exponential Backof
???

## Reactions

??? DynamoDB console displays Amazon CloudWatch metrics for your tables, so you can monitor throttled read requests and write requests. If you encounter excessive throttling, you should consider increasing your table's provisioned throughput settings.

# Glossary

*Eventually Consistent Reads* When you read data from a DynamoDB table, the response might not reflect the results of a recently completed write operation. The response might include some stale data. If you repeat your read request after a short time, the response should return the latest data.

*Eventually Consistent Reads* When you request a strongly consistent read, DynamoDB returns a response with the most up-to-date data, reflecting the updates from all prior write operations that were successful. A strongly consistent read might not be available if there is a network delay or outage.

*Read Capacity Unit*: one strongly consistent read per second, or two eventually consistent reads per second, for items up to 4 KB in size. If item that larger than 4 KB, DynamoDB will consume additional read capacity units.

*Write Capacity Unit*: one write per second for items up to 1 KB in size. If item larger than 1 KB, DynamoDB will consume additional write capacity units.

*Provisioned throughput*: Is the maximum amount of capacity that an application can consume from a table or index. If your application exceeds your provisioned throughput capacity on a table or index, it is subject to request throttling.

# Resources

[1]: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ReadWriteCapacityMode.html
[2]: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Limits.html
[3]: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-partition-key-design.html#bp-partition-key-partitions-adaptive
