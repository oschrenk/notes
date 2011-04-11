# NoSQL #

Offers BASE (Basically Available, Soft state, Eventual consistency) as a consistency model as opposed to the reltational database conecpt of ACID (Atomicity, Consistency, Isolation, Durability), which means that given a sufficiently long period of time over which no updates are sent, we can expect that during this period, all updates will eventually propagate through the system and all the replicas will be consistent.

## Data models ##

### Columnar ###

Content is stored in columns rather than rows.Instead of storing 

	1,Smith,Joe,40000;
	2,Jones,Mary,50000;
	3,Johnson,Cathy,44000;

in RAM, a columnar representation stores (in a simplified way)

	1,2,3;
	Smith,Jones,Johnson;
	Joe,Mary,Cathy;
	40000,50000,44000;

Column oriented databases when data needs to be aggregated

- [Google Bigtable](http://labs.google.com/papers/bigtable.html) Bigtable is a distributed storage system for managing structured data that is designed to scale to a very large size
- [HBase](http://hbase.apache.org/)  HBase is the Hadoop database
- [Apache Cassandra](http://cassandra.apache.org/) Cassandra is an open source distributed database management system. 
- [MonetDB](http://www.monetdb.nl/) is a database system for high-performance applications in data mining, OLAP, GIS, XML Query, text and multimedia retrieval.

### Key/Value ###

- schema-less
- data is stored as datatypes of used programming language or object
- no fixed data model

- [Memcached](http://memcached.org/) open source, high-performance, distributed memory object caching system
- [Riak](https://github.com/basho/riak) decentralized datastore
- [Redis](http://redis.io/)  is an open source, advanced key-value store

### Document Oriented ###

- allows empty fields without needing storage
- indexes can be created on the fields 

- [Apache CouchDB](http://couchdb.apache.org/) is a document-oriented database that can be queried and indexed in a MapReduce fashion using JavaScript.
- [MongoDB](http://www.mongodb.org/) is a scalable, high-performance, open source, document-oriented database
- [Jackrabbit](http://jackrabbit.apache.org/) is a fully conforming implementation of the Content Repository for Java Technology API (JCR, specified in JSR 170 and 283)

### Graph Oriented ###

- [Neo4j](http://neo4j.org/) is a graph database implemented in Java. 

## Notes ##

- persistence vs non-persistence, i.e., some NoSQL technologies are entirely in-memory data stores
- Be ready to deal with no standard interface like JDBC/ODBC or standarized query language like SQL; every NoSQL tool has a different interface
- web-scale/large-scale NoSQL systems are distributed across dozens to hundreds of servers and networks as opposed to a shared database system
- Get used to the possibly uncomfortable realization that you won’t know where data lives (most of the time)
- Get used to the fact that data may not always be consistent; ‘eventually consistent’ is one of the key elements of the BASE model
- Get used to the fact that data may not always be available
- Understand that some solutions are partition-tolerant and some are not

[source](http://nosql.mypopescu.com/post/3599841629/9-things-to-acknowledge-about-nosql-databases)
