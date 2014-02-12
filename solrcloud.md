# SolrCloud #

SolrCloud is flexible distributed search and indexing, without a master node to allocate nodes, shards and replicas. It offers

- **Central configuration** for the entire cluster
- Automatic **load balancing and fail-over** for queries
- **ZooKeeper integration** for cluster coordination and configuration

To start a Solr instance in Cloud mode

	java -DzkRun -DnumShards=2 -Dbootstrap_confdir=./solr/collection1/conf -Dcollection.configName=myconf -jar start.jar

Let's look at each of these parameters:

- `DzkRun` Starts up a ZooKeeper server embedded within Solr to manage the cluster configuration. In production you'll multiple ZooKeepers in an ensemble or stand-alone ZooKeeper instance. In that case, you'll replace this parameter with `zkHost=<ZooKeeper Host:Port>`, which is the `hostname:port` of the stand-alone ZooKeeper.
- `DnumShards` Determines how many pieces you're going to break your index into. In this case we're going to break the index into two pieces, or shards, so we're setting this value to 2. Note that **once you start up a cluster, you cannot change the number of shards**. So if you expect to need more shards later on, build them into your configuration now (you can do this by starting all of your shards on the same server, then migrating them to different servers later).
- `Dbootstrap_confdir` ZooKeeper needs to get a copy of the cluster configuration
- `Dcollection.configName` Arbitrary name under which the configuration information is stored by ZooKeeper




## Resources ##

- [Apache Solr Reference Guide](https://cwiki.apache.org/confluence/display/solr/SolrCloud)