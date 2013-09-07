# Storm #

> Storm is a free and open source distributed real-time computation system. Storm makes it easy to reliably process unbounded streams of data.

## Concepts ##

### Tuple ###

Storm uses tuples as its data model. A tuple is a named list of values, and a field in a tuple can be an object of any type Storm is able to serialize. Storm uses Kryo for more efficient serialization and supports primitive types, strings, byte arrays, ArrayList, HashMap, HashSet, and the Clojure collection types and defaults to Java serialization. If Storm doesn't find a Kryo Serializer and if no Java serialization is possible, Storm will throw an error. It is highly recommended to write custom Kryo serializer and to only rely on Java serialization for fast prototyping.

### Stream ###

The core abstraction in Storm is the _stream_. A stream is an unbounded sequence of tuples. Streams are defined with a schema, by naming the fields of a tuple.

Every stream has an id, as a convenience streams can be created without defining one, but will then be given the `default` stream id.

The basic components Storm provides for doing stream transformations are _spouts_ and _bolts_.

### Spout ###

A spout is a source of one or more streams. Spouts normally read data from external sources such as databases and files and emit it as a stream of tuples.

Can either be reliable or unreliable. An unreliable spout does not track the tuple once it' emitted.

### Bolt ###

Bolts consume input streams, processes, and then emit new streams. Bolts can do anything from filtering, functions, aggregations, joins, talking to databases, and more. More complex transformations are better in multiple steps by connecting multiple bolts.

Bolts subscribe to the streams of other components upon creation time and is done for each stream individually. As with spouts, bolts can emit multiple streams and the API to do so is the same.

### Topologies ###

Networks of spouts ans bolts form a topology. When a spout or bolt emits a tuple to a stream, it sends the tuple to every bolt that is subscribed to that stream.

The idea is that a storm topology is analogous to a MapReduce job, the key difference being that while a MapReduce job eventually finishes, a Storm topology is intended to run forever.

When connecting spouts and bolts you also need to define which streams the bolts should receive as input.

A stream can be partitioned using various strategies. Storm has support for seven built in:

1. *Shuffle grouping*. Tuples are randomly distributed across the bolt's tasks in a way such that each bolt is guaranteed to get an equal number of tuples.
2. *Fields grouping*. The stream is partitioned by the fields specified in the grouping. For example, if the stream is grouped by the `user-id` field, tuples with the same `user-id` will always go to the same task, but tuples with different `user-id`'s may go to different tasks.
3. *All grouping*. The stream is replicated across all the bolt's tasks. Use this grouping with care.
4. *Global grouping*. The entire stream goes to a single one of the bolt's tasks. Specifically, it goes to the task with the lowest id.
5. *None grouping*. This grouping specifies that you don't care how the stream is grouped. Currently, none groupings are equivalent to shuffle groupings. Eventually though, Storm will push down bolts with none groupings to execute in the same thread as the bolt or spout they subscribe from (when possible).
6. *Direct grouping*. This is a special kind of grouping. A stream grouped this way means that the producer of the tuple decides which task of the consumer will receive this tuple. Direct groupings can only be declared on streams that have been declared as direct streams. Tuples emitted to a direct stream must be emitted using one of the emitDirect methods. A bolt can get the task ids of its consumers by either using the provided TopologyContext or by keeping track of the output of the emit method in OutputCollector (which returns the task ids that the tuple was sent to).
7. *Local or shuffle grouping*. If the target bolt has one or more tasks in the same worker process, tuples will be shuffled to just those in-process tasks. Otherwise, this acts like a normal shuffle grouping.

## Running a Storm Cluster ##

There are three main entities that are used to actually run a topology in a Storm cluster:

1. Worker processes
2. Executors (threads)
3. Tasks

### Worker Processes ###

Each _worker process_ is a physical JVM and executes a subset of (all the tasks for) the topology. A worker process belongs to a specific topology and may run one or more executors for one or more components (spouts or bolts) of this topology. A running topology consists of many such processes running on many machines within a Storm cluster.

For example, if the combined parallelism of the topology is 300 and 50 workers are allocated, then each worker will execute 6 tasks (as threads within the worker). Storm tries to spread the tasks evenly across all the workers.

### Executors ###

An _executor_ is a thread that is spawned by a worker process. It may run one or more tasks for the same component (spout or bolt). An executor always has one thread that it uses for all of its tasks, which means that tasks run serially on an executor.

The number of executor threads can be changed after the topology has been started.

### Task ###

A _task_ performs the actual data processing. Each spout or bolt that you implement executes as multiple tasks across the cluster. The number of tasks for a component is always the same throughout the lifetime of a topology, but the number of executors (threads) for a component can change over time.  This means that the following condition holds true: `#threads <= #tasks`. By default, the number of tasks is set to be the same as the number of executors, i.e. Storm will run one task per thread (which is usually what you want anyways).

The number of tasks of a topology is static. You set the parallelism for each spout or bolt in the setSpout and setBolt methods of TopologyBuilder.

## Configuration ##

### Configuring the parallelism of a topology ###

 In Storm’s terminology parallelism is specifically used to describe the so-called _parallelism hint_, which means the initial number of executors (threads) of a component.

In the code below Storm is configured to run the bolt `GreenBolt` with an initial number of two executors and four associated tasks. Storm will run two tasks per executor (thread). If you do not explicitly configure the number of tasks, Storm will run by default one task per executor.

	topologyBuilder.setBolt("green-bolt", new GreenBolt(), 2)
	               .setNumTasks(4)
	               .shuffleGrouping("blue-spout");

### Configuration precedence ###

Storm has the following precedence for configuration settings:

1. `defaults.yaml`
2. `storm.yaml`
3. topology-specific configuration
4. internal component-specific configuration
5. external component-specific configuration

## Running a cluster ##

### Multithreading ###

Its perfectly fine to launch new threads in bolts that do processing asynchronously. `OutputCollector` is thread-safe and can be called at any time.

### Reliable Systems ###

If you want a reliable system you need to

1. Give `messageId` to tuple: `collector.emit(tuple, messageId)`
2. Acknowledge the tuple in the bolt: `collector.ack(input)`

## Resources ##

- Storm, [Wiki](https://github.com/nathanmarz/storm/wiki)
- Michael G. Noll, [Understanding the Parallelism of a Storm Topology](http://www.michael-noll.com/blog/2012/10/16/understanding-the-parallelism-of-a-storm-topology/)
