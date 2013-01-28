# Asynchronous Processing #

- certain actions may take too long to answer in normal http request/response cycle
	- process images
	- transcode video
	- log analysis
- reliable communication with other services in system architecture

## Using databases for asynchronous processing ##

Polling requires very fast and frequent queries to the table to be most effective, which adds a significant load to the database even at a medium volume. Even worse, a given database table is typically prepared to be fast at adding data, updating data or querying data, but almost never all three on the same table. If you are constantly inserting, updating and querying, race conditions and deadlocks become inevitable.

Sharing a database between applications (or services) is a bad thing. It's just too tempting to put amorphous shared state in there and before you know it you'll have a hugely coupled monster.

It is important to mention that a database can be used as a queue. In fact, every database can be made to work at a low or medium volume as a queue given enough time and effort of a clever developer. Moreover, there are actually databases such as PostgreSQL that have additional support for queuing.

## What is a message queue? ##

At the simplest level, a message queue is a way for applications and discrete components to send messages between one another in order to reliably communicate. Message queues are typically (but not always) ‘brokers’ that facilitate message passing by providing a protocol or interface which other services can access. This interface connects producers which create messages and the consumers which then process them.

## Features ##

On top of the fundamental ability to pass messages quickly and reliably, most message queues offer additional complementary features. For instance, multiple separate queues can allow different messages to be passed to different queues. This allows the messages of different types to be consumed by different services.

Message delivery behavior will often vary depending on your need. Certain messages will go from one producer to a single consumer (direct) while other times the message is sent to multiple different listening consumers (fanout).

Another critical feature of message queues is robustness and reliability through persistence strategies. In order to keep reliability of the messages high, most message queues offer the ability to persist all messages to disk until they have been received and completed by the consumer(s). Even if the applications or the message queue itself happens to crash, the messages are safe and will be accessible to consumers as soon as the system is operational. In contrast, transient tasks performed synchronously in a web app or on a thread in memory will be lost if anything goes awry.

[#Miso:2013]: Asynchronous Processing in Web Applications [Part 1](http://blog.gomiso.com/2012/11/15/asynchronous-processing-in-web-applications-part-1-a-database-is-not-a-queue/), [Part 2](http://blog.gomiso.com/2013/01/06/asynchronous-processing-in-web-applications-part-2-developers-need-to-understand-message-queues/)

