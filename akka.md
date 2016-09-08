# Akka #

## Recveiving messages

```
sealed trait CoffeeRequest
case object CappuccinoRequest extends CoffeeRequest
case object EspressoRequest extends CoffeeRequest

import akka.actor.Actor
class Barista extends Actor {
  def receive = {
    case CappuccinoRequest => println("I have to prepare a cappuccino!")
    case EspressoRequest => println("Let's prepare an espresso.")
  }
}
```

First, we define the types of messages sent between actors. Typically, case classes are used to pass along any parameters. If all the actors needs is an unparameterized message, the message is typically represented as a case object.

In any case, it is crucial that your message is immutable.

## Processing messages ##

So what’s the meaning of this `receive` method? The return type, `PartialFunction[Any, Unit]` may seem strange to you in more than one respect.

The partial function is responsible for processing your message. Whenever another part of your software, actor or not, sends your actor a message, Alla will _eventually_ let it process this message by calling the partial function returned by your actors's `receive` method, passing it the message as an argument.

## Side effecting ##

When processing a message, an actor can do whatever you want it to, apart from returning a value.

As the return type `Unit` suggests, your partial function is side-effecting. This may surprise, but makes for a concurrent programming model. Actors are where your state is located. As each message is received it is processed in isolation in the actor, so ther is no need to reasin about synchronization or locks.

## Untyped ##

The partial function is not only side-effecting, it's also untyped of type `Any`. This is a design choice in Akka and is usually not a problem with the messages themselves strongly typed.

If you need the strong type system, you may want to look at Akka's experimental [Type Channel](http://doc.akka.io/docs/akka/snapshot/scala/typed-channels.html) feature.

## Asynchronous and non-blocking ##

Sending and processing a message is done in an asynchronous and non-blocking fashion. The sender can immediately continue with his work, even if he waits for an answer.

A message is delivered to the actors _mailbox_, basically a queue. The _dispatcher_ will notice the arrival of a new message in an actor's mailbox. If the actor is not already processing a message, it is now allocated one of the threads from the execution context.

The actor blocks the thread for as long as it takes to process the message. Lengthy operations therefore degrade overall performance, as all the other actors message processing have to be scheduled on one of the remaining threads.

A core principle is therefore to spend as little time in your `Receive` function as possible and to avoid blocking code.
