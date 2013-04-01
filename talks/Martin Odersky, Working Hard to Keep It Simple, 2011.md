# Martin Odersky, Working Hard to Keep It Simple #

Talk given at O'Reilly OSCON Java 2011. [Watch it](http://www.youtube.com/watch?v=3jg1AheF4n0).

## Concurrency and Parallelism ##

These are two different concepts

- _Parallel_ programming. Execute programs faster on parallel hardware. Programs could just as well run sequentially.
- _Concurrent_ programming. Manage concurrent execution threads explicitly. We have something that is inherenlty concurrent.

Both are very hard problems.

## Fundamental problem ##

- _Non-determinism_ caused by _concurrent threads_ accesing _shared mutable_ state
- it helps to encapsulate state in actors or transactions, but the fundamental problem stays the same
- to get deterministic processing, avoid the mutable state!
- avoiding mutable state means programming _functionally_

## Space vs Time ##

The mode of approaching a problem functionally is quite different from the imperative mode. The **imperative mode is based on time, while the functional approach is based in space**.

If you built something in space its easy to build independent blocks. In imperative programming we create locks and and regions that basically control time access (control execution path). This is the problem, we are optimistic animals, we want to create something, are goal oriented and do not want to erect walls.

## Scala is a Unifier ##

Scala combines the Object-Oriented and the Functional way of thinking. The combination is the strong suite of Scala.

Scala is a both language that is very fast to first product and very scalable afterwards.

## Different Tools for Different Purposes ##

**Parallelism**. Collections are an excellent way to express parallel computations.

**Concurrency**. The actor model.

## Going parallel ##

Martin describes an example on how Scala is terse. I don't show the Java code here but only the Scala code

	val people: Array[Person]
	val(minors, adults) = people partition (_.age < 18_)

That's nice and pleasant. But the real advantage is that it is easily parallelized.

	val people: Array[Person]
	val(minors, adults) = people.par partition (_.age < 18_)

You just add `.par`. It converts the array to a parallel array, which is worked on in parallel.


Sometimes you need more control.

## Actors for Concurrent Programming ##

- simple message-oriented programming model for multi-threading
- serializes access to shared resources using queues and function passing
- easier for programmers to create reliable concurrent processing
- many sources of contention, races, locking and dead-locks removed

Example

	class Person(val name: String, val age: Int)

	actor {
		receive {
			case people: Set[Person] => val (minors, adults) =
			  people partition (_.age < 18_)
			  Facebook ! minors
			  LinkedIn ! adults
		}
	}
