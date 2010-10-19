# Software Development #

## Data Structures ##

### Graphs ###

*   _undirected_ graph
*   _directed_ graph

For both forms there are variants allowing multiple edges, also called _multigraph_, permitting multiple edges between two nodes. A graph can also _weighted_ or unweighted. That means that each edge has been assigned a weight normally a number between zero and one.

A graph is a collection of nodes and edges.

#### Vertex ####

A **vertex** or node is the fundamental unit of each graph. Nodes are (normally) connected with edges. These edges can have a direction or a weight. The _degree_ of a node describes how many edges are connected to a node. An _isolated node_ has no edges. A _leaf node_ is a node with just one edge. If the graph is directed, you distinguish between an _indegree_ for incoming edges and _outdegree_ for outgoing edges.

## Modularity and Testability ##

### Dependency Injection ###

_Dependency Injection_ <sup class="footnote">[1](#fn1)</sup> or _DI_ in short is the process of defining dependencies of external objects.

Three types of DI:

*   _Type 1_ or _Interface Injection_: an interface is added to the
    class
*   _Type 2_ or _Setter Injection_: _Setter_-methods to inject a
    dependency
*   _Type 3_ or _Constructor Injection_ the constructor of the module
    contains the dependencies

## API/Framework Design ##

### Fluent Interfaces ###

Fluent interfaces <sup class="footnote">[2](#fn2)</sup> only make sense if a developer uses them but not if he has to write classes fulfilling the contract.

## Sources ##

<p class="footnote" id="fn1"><sup>1</sup> <a href="http://martinfowler.com/articles/injection.html#InversionOfControl">Inversion of Control</a> (Martin Fowler, 04-01-23)</p>

<p class="footnote" id="fn2"><sup>2</sup> <a href="http://martinfowler.com/bliki/FluentInterface.html">FluentInterface</a> (Martin Fowler, 05-12-20)</p>
