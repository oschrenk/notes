# Software Architecture #

## Messaging ##

### Message Queue ###

Also known as _point-to-point messaging_

Messages are sent to a queue. Normally they will be persisted for later consumption.

The queue can have many consumers, but a particular message message will only be consumed by one. Senders (also known as producers) are completely decoupled from the receivers (also known as consumers).

The consumer consumes the message and acknowledges the message. Once acknowledged the message is removed from the queue. If the system crashes before it receives the acknowledgment the message will still be available and will be delivered again.

[Messaging concepts][#jboss.messaging]

### Publisher/Subscriber ###

_Publish/subscribe_ (or _pub/sub_) is a messaging pattern in which the senders are sending their messages to an entity on the server, often called a topic or channel. 

A topic can be subscribed to by different consumers. Each subscriber receives a copy of each message sent to the topic. This is different from the message queue pattern where each message is only consumed by a single consumer.

An example of publish-subscribe messaging would be a news feed. As news articles are created by different editors around the world they are sent to a news feed topic. There are many subscribers around the world who are interested in receiving news items - each one creates a subscription and the messaging system ensures that a copy of each news message is delivered to each subscription.

[Messaging concepts][#jboss.messaging]

## Enterprise Service Bus ##

A bus is a system that forwards data to other parts. An Enterprise Service Bus (ESB) is an architectural concept for distributed systems following SOA.

## Resources ##

[#jboss.messaging]: [Messaging concepts](http://docs.jboss.org/jbossmessaging/docs/usermanual-2.0.0.beta1/html/messaging-concepts.html) 