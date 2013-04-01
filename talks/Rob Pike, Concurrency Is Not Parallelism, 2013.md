# Rob Pike, Concurrency Is Not Parallelism, 2013 #

Talk given at Waza 2012. [Watch it](http://vimeo.com/groups/waza2012/videos/49718712)

- the world is not object-oriented
- the programming tools are nout supporting the world we live in

## Concurrency Is Not Parallelism ##

_Concurrency_ is a way to build to things. It's the composition of independently executing things, typically functions, or processes (not in the Linux way, but in a more abstract way)

_Parallelism_ on the other hand is the simultaneous execution of multiple things, possibly related, possibly not.

Concurrency is about dealing with a lot of things at once, and parallelism is about doing a lot of things at once. Obviously related but separate ideas. Concurrency is about structure, parallelism is about execution.

Concurrency is a way to structure a thing, so that I can maybe that I can use parallelism to do a better job, but parallelism is not the goal, the goal is the structure.

Concurrency gives you way to structure of independent pieces but then you have to coordinate these pieces and that means communication. Tony Hoare wrote a paper called "Communicating sequential processes" in 1978, one of the greatest paper in Computer Science. Based on this tools and languages followed.

Problem:

Take books from one pile and move it to the incinerator, another worker doesn't help. Each worker needs the correct tools like a cart. You have to coordinate the workers.

Double everything, that is parallel, right? Instantiating a new worker is concurrent, but not necessarily parallel. A worker does not have to work.

Three workers, three piles, once incinerator. Each worker has a different job. This is a finder grained concurrency. We may to more work but can make it run faster.

We improve the performance of the program by adding a concurrent procedure to an existing design. We added something and made it faster. That is kind of weird.

- One worker the loads things into the cart
- One worker that takes the cart to the incinerator
- One worker that unloads into the incinerator
- One worker that bring the empty cart back

Concurrent Design does not imply parallelism. We do not have to worry about parallelism. If we get the concurrency right, parallelism can be applied freely.

Another design

- one worker carries books to the dump
- one worker carries the books from the dump to the incinerator

The point is: **Break the problem down into small problems and compose it**.

## Background about Go ##

Here Rob Pike goes and explains some stuff about Go and implementation.