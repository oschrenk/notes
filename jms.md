# JMS

Java Message Service (JMS) is a message oriented middleware (MOM), providing applications with a way to create, send, receive and read messages. It allows two components within in a distributed application to be loosely coupled and asynchronous.

## Models

A message system is classified into three different messages models

- point-to-point messaging
- publish-subscribe messaging
- request-reply messaging

JMS only supports the first two.

### Point-to-Point model

A _sender_ posts messages to a particular _queue_. A _receiver_ reads the messages from the same queue. The sender knows the destination of the message and posts the message directly to the queue of the receiver.

- only one consumer gets the message
- the producer does not have to be running, when the consumer consumes the message or vice versa
- each successfully processed message is acknowledged

### Publish-Subscribe model

_Subscribers_ my register interest in receiving messages on a particular message topic. The _publisher_ doesn't know of the subscriber nor the subscriber of the publisher. A good analogy is an anonymous bulletin board.

- multiple consumers can get the message
- subscriber has to remain active to receive messages, unless a durable subscription has been established which could trigger a redistribution of unread messages

## Provider

[Apache ActiveMQ][apache.activemq]

[apache.activemq]: http://activemq.apache.org/
