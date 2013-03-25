# Concurrency #

The predominant approach to concurrency today is that of shared mutable state. A number of objects whose state can be change by multiple parts of your application, each running in their own thread. Typically, the code is interspersed with read and write locks, to make sure that the state can only be changed in a controlled way.

Low-level synchronization constructs are very hard to reason about.

## Reactor pattern ##

Reactor pattern has been introduced in [Schmidt95](http://www.cse.wustl.edu/~schmidt/PDF/reactor-siemens.pdf) as a general architecture for event-driven systems.

All reactor systems are single threaded by definition, but can exist in a multithreaded environment.

The purpose of the Reactor design pattern is to avoid the common problem of creating a thread for each incoming message/request/connection. It receives events from a set of handles and distributes them sequentially to the corresponding event handlers

You assign a single thread that is responsible to monitor all the connections.  When all the IO operations are completed for a connection, that thread can fire up an event so that another thread starts processing the data coming from the connection. This approach works well when you have to handle a lot of connections, because you are not force to dedicate a thread for each connection, which might consume lot of resources if the number of connections is high.

## Actor model ##

Idea is that your application consists of lots of light-weight entities, each responsible for only a very small task. Business logic arises out of the interaction between several actors, delegating tasks to each other or passing messages to collaborators.

An actor is a computational entity that, in response to a message it receives, can concurrently:
- send a finite number of messages to other actors;
- create a finite number of new actors;
- designate the behavior to be used for the next message it receives.

## Sources ##

- [Daniel Westheide, The Actor Approach to Concurrency](http://danielwestheide.com/blog/2013/02/27/the-neophytes-guide-to-scala-part-14-the-actor-approach-to-concurrency.html)
