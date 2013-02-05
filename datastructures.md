# Data Structures #

## Hashing ##

### Bloom Filter ###

A Bloom filter is a data structure designed to tell you, rapidly and memory-efficiently, whether an element is present in a set.

The price paid for this efficiency is that a Bloom filter is a probabilistic data structure: it tells us that the element either definitely is not in the set or may be in the set.
You ask the datastructure if an element exists:
- False positives are possible. You may get `true` for elements that are not in the set.
- False negatives are not possible. You never get `false` for elements that are in the set.


Taken from [here](http://llimllib.github.com/bloomfilter-tutorial/)

## Graphs ##

*   _undirected_ graph
*   _directed_ graph

For both forms there are variants allowing multiple edges, also called _multigraph_, permitting multiple edges between two nodes. A graph can also _weighted_ or unweighted. That means that each edge has been assigned a weight normally a number between zero and one.

A graph is a collection of nodes and edges.

### Vertex ###

A **vertex** or node is the fundamental unit of each graph. Nodes are (normally) connected with edges. These edges can have a direction or a weight. The _degree_ of a node describes how many edges are connected to a node. An _isolated node_ has no edges. A _leaf node_ is a node with just one edge. If the graph is directed, you distinguish between an _indegree_ for incoming edges and _outdegree_ for outgoing edges.

## Other structures ##

- [Judy Array](http://judy.sourceforge.net/) Sparse dynamic array, consumes memory only when it is populated.
