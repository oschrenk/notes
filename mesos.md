# Mesos

Mesos is about managing clusters. In some sense it is the opposite of virtualization. While virtualization is about splitting a virtual resource into multiple virtual resources,  mesos joins multiple physical resources into a single virtal resource.

A Mesos cluster is made out of four major components

1. Zookeeper. Zookeeper is a service for managing configuration in distributed systems. Mesos uses Zookeeper to elect a master.
2. Mesos Masters. A master is a Mesos instance in control of the cluster. A cluster wull normally have multiple masters to provide fault-tolerance, with one instance being the elected leading master.
3. Mesos Slave. A slave is a Mesos instance which provides resources to the cluster.
4. Frameworks. Also known as a Mesos application, is composed of a scheduler, which registers with the master to receive resource offers, and one or more executors, which launches tasks on slaves. Examples of Mesos frameworks include Marathon, Chronos, and Hadoop.

## Frameworks

* **Chronos** A scheduler. Schedule jobs, receive failure and completion notifications, and trigger other dependent jobs.
* **Marathon** is the equivalent of the Linux upstart or init daemons, designed for long-running applications. You can use it to start, stop and scale applications across the cluster. Allows to deploy and manage containers incl. Docker
*  **Aurora** Similar to Marathon but can handle recurring jobs, offers support for namespaces for different environments, is backed by Apache Foundation.
* **Spark** Cluster oriented processing engine for big data that offers support for Mesos.

## Vocabulary

* **Mesosphere** The company backing Mesos
* **DC/OS**
* **Mesos Agent** Synonym for a Slave.
* **Offer**  List of a slave node's available CPU and memory resources. All slave nodes send offers to the master, and the master provides offers to registered frameworks
* **Task** A unit of work that is scheduled by a framework, and is executed on a slave node. A task can be anything from a bash command or script, to an SQL query, to a Hadoop job

## Resources

* [A quick introduction to Apache Mesos](http://iankent.uk/blog/a-quick-introduction-to-apache-mesos/)
* [Intro to Mesosphere: Apache Mesos, Marathon, Chronos &amp; HAProxy | DigitalOcean](https://www.digitalocean.com/community/tutorials/an-introduction-to-mesosphere)

