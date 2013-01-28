# Spatial datastructures and algorithms optimized for highly parallelized computing environments #

Spatial data is data that lives in a certain spatial context. These characteristics can be points, lines, polygons, surfaces or volumes and exist in the context of an area (two dimensional data) or a volume (three or more dimensions).

While storing the spatial attributes might be an easy task, working with it and is quite a different task.

## Anwendungen und Probleme ##

### Partial Match Queries ###

Heraussuchend von Objekten in der Datenbank von dem ein oder mehrere Koordinaten mit der Suchanfrage übereinstimmen

### Orthogonal Range Queries ###

Gegeben ist ein d-dimensionaler Raum mit n Punkten. Eine "Orthogonal Range Query" versucht Punkte in einer d-dimensionalen Box in diesem Raum zu finden.

### Nearest Neighbor Queries ###

Suche den oder die n-nächsten Objekte.

### Generalization ###

- Douglas-Peucker

## Geometrie ##

Distanzberechnungen passieren auf Gleitkommazahlen. Um Distanzen zwischen zwei Punkten auf einem Ellipsoiden zu berechnen gibt es mehrere Möglichkeiten, die sich nach Genauigkeit und Schnelligkeit unterscheiden.

Übliche Metriken
- Euklid
- Haversine
- LawOfCosine
- Loxodrome
- Orthodrome

## Data Structures ##

### Hierachical data structures ###

- K-D-B-tree, hB-tree and Bkd-tree, Adaptive kdtree
- Building a static kd-tree from n points takes O(n log 2 n)
- OCTree
- Quad Tree
- PR Tree
- R-tree
- R+ tree
- R* tree
- X-tree
- M-tree
- Segment tree
- Fenwick tree
- Hilbert R-tree
- SKD Tree

### Grids ###

- Uniform Grid

### Hashing ###

- [Natural Area Code](http://en.wikipedia.org/wiki/Natural_Area_Code)
- [Military Grid Reference System](http://en.wikipedia.org/wiki/Military_grid_reference_system)
- Geohash

## Implementierungen ##

sind zu finden in

- Neo4J
- MongoDB

## Parallized ##

- Was muss man bei der Entwicklung von parallelen Algorithmen beachten
- Wie evaluiert man parallele Algorithmen? Offensichtliche Antwort ist Zeit. Aber was ist mit O Notation?

Normalerweise bewertet man Datenstrukturen mit Hilfe einer Komplexitäts tabelle
		Average		Worst
Space	O(n)		O(n)
Search	O(log n)	O(log n)
Insert	O(log n)	O(log n)
Delete	O(log n)	O(log n)

- Reorganisierend (balancing) oder nicht?
- Was für Daten wollen wir unterstützen 3d+?

## Resources ##

- [manifold](http://www.manifold.net/)
- [1](http://blogs.esri.com/Dev/blogs/apl/archive/2010/03/30/Computations-on-vector-data-using-a-GPU.aspx)
- [2](http://www.azavea.com/blogs/labs/2010/06/gpu-computing-for-gis/)
- [3](http://gisandscience.com/2010/06/10/azavea-awarded-nsf-grant-to-explore-use-of-graphics-processing-units-for-faster-geographic-data-processing/)
