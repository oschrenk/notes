# Spark

## Terminology:

* **Application** A program built on Spark, consisting of a driver program and executors in the cluster.
* **Driver** A process running the main() function of the application (and providing the SparkContext).
* **Executor** A process launched for an application on a worker node.

## Tuning Spark

### How to determine the value for spark.sql.shuffle.partitions

How do you programmatically determine the optimal number of partitions and cores in Spark, as a function of:
1. available memory per core
2. number of records in input data
3. average/maximum record size
4. cache configuration
5. shuffle configuration
6. serialization
7. etc?

Doing it programmatically is hard but...
The number of partitions is supposed to be at least roughly double of your number of cores (surprised to not see this point in your list), and can easily grow up to 10x, until you may notice a too large overhead

We did lots of experiences and I'd say that the ideal impact is hit when "number of records of input data * maximum record size / number of partitions" is far smaller than the available memory per core (to give Spark enough headroom to properly work, for serialization among others, as you mentioned in #6).

Since most of the time, we'll have many cores sharing not (and never) enough memory, it looks interested to partition aggressively. Shuffling overhead looks easier to observe and debug than the memory efficiency.

