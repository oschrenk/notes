# NoSQL #

1. Understand how ACID compares with BASE (Basically Available, Soft-state, Eventually Consistent)
1. Understand persistence vs non-persistence, i.e., some NoSQL technologies are entirely in-memory data stores
1. Recognize there are entirely different data models from traditional normalized tabular formats: Columnar (Cassandra) vs key/value (Memcached) vs document-oriented (CouchDB) vs graph oriented (Neo4j)
1. Be ready to deal with no standard interface like JDBC/ODBC or standarized query language like SQL; every NoSQL tool has a different interface
1. Architects: rewire your brain to the fact that web-scale/large-scale NoSQL systems are distributed across dozens to hundreds of servers and networks as opposed to a shared database system
1. Get used to the possibly uncomfortable realization that you won’t know where data lives (most of the time)
1. Get used to the fact that data may not always be consistent; ‘eventually consistent’ is one of the key elements of the BASE model
1. Get used to the fact that data may not always be available
1. Understand that some solutions are partition-tolerant and some are not

[source](http://nosql.mypopescu.com/post/3599841629/9-things-to-acknowledge-about-nosql-databases)